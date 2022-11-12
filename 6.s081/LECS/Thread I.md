# 6.1810 2022 Lecture 11: Thread switching

Topic: more "under the hood" with xv6
  Previously: system calls, interrupts, page tables, locks
  Today: process/thread switching

**Why** support multiple tasks?
  time-sharing: many users and/or many running programs.
  program structure: prime number sieve.
  parallel speedup: exploit multi-core hardware.

The process abstraction is part of the solution.
  A program can be written as if had a dedicated CPU.
  No need to worry about how to share CPU with others.

Process = address space + thread(s)
  we've already looked at address spaces (via page tables)
  thread = an independent serial execution -- registers, pc, stack
  usually many more threads than CPUs
  a threading system switches from thread to thread to multiplex each CPU
    the switching is today's main topic

threads can share memory, or not
  linux: supports multiple threads sharing a user process's memory
    as well as lots of threads in the kernel
  xv6 user processes: one thread per process
  xv6 kernel: thread per process, they share kernel memory (thus locks)

there are other techniques for interleaving multiple tasks
  look up event-driven programming, or state machines
  threads are not the most efficient, but they are usually the most convenient

a thread might be executing, or not
  if executing
    the thread is using the CPU and registers
    and it has a stack and memory
  if not executing ("suspended", "blocked", "waiting")
    registers must be saved somewhere in memory
    stack and memory need not be saved, just preserved
  when a thread is suspended, and then resumed
    must save state, and then restore it

xv6 process saved state
  [diagram]
  user memory
  user stack (C function call chain &c)
  user saved registers (in trapframe (TF))
  kernel stack
  kernel saved registers ("context" (CTX))
  kernel p->state

if an xv6 process is not executing
  it's typically waiting for some event -- console, pipe, disk, &c
    because it made a system call into the kernel, e.g. read()
  may be waiting midway through complex kernel system call implementation
    needs to resume in kernel system call code where it left off
    thus the saved kernel registers
    as well as saved user registers

it may look like each xv6 process has two threads
  one user, one kernel
  but really there's just one
    if executing in kernel, user thread is waiting for trap return
    if executing in user space, kernel thread is empty
  I'll use process and thread interchangeably

p->state values
  RUNNING -- executing on a CPU right now (might be either in user or kernel)
  RUNNABLE -- wants to execute, but isn't (suspended, in kernel)
  SLEEPING -- waiting for an event (suspended, in kernel read(), wait(), &c)
  p->state tells the scheduler how to handle the process
  guards against mistakes like executing on two CPUs at same time

how xv6 switches from one process to another -- "context switch"
  e.g. P1 calls read() and waits, P2 is RUNNABLE
  [P1, TF1, KSTACK1, CTX1, swtch(), CTXs, STACKs, swtch(), CTX2, KSTACK2, TF2, P2]
  TF = trapframe = saved user registers
  CTX = context = saved RISC-V registers
  getting from one user process to another involves multiple transitions:
    user -> kernel; saves user registers in trapframe
    kernel thread -> scheduler thread; saves kernel thread registers in context
    scheduler thread -> kernel thread; restores kernel thread registers from ctx
    kernel -> user; restores user registers from trapframe

why a separate scheduler thread?
  so that there's always a stack on which to run scheduler loop
  e.g. switching away from an exiting process
  e.g. fewer processes than CPUs

scheduler thread details
  one per CPU; each has a stack and a struct context
  a kernel thread always switches to the current CPU's scheduler thread
    which switches to another kernel thread, if one is RUNNABLE
    there are never direct kernel thread to kernel thread switches
  each CPU's scheduler thread keeps scanning the process table until
    it finds a RUNNABLE thread
  if there is no RUNNABLE thread, the scheduler is "idle"

what if user code computes without ever making a system call?
  will that stop other processes from running?
  CPU's timer hardware forces periodic interrupt
    xv6 timer handler switches ("yields")
  "pre-emption" or "involuntary context switch"

struct proc in proc.h
  p->trapframe holds saved user thread's registers
  p->context holds saved kernel thread's registers
  p->kstack points to the thread's kernel stack
  p->state is RUNNING, RUNNABLE, SLEEPING, &c
  p->lock protects p->state (and other things...)

# Code

pre-emptive switch demonstration
  vi user/spin.c
  two CPU-bound processes
  my qemu has only one CPU
  we'll watch xv6 switch between them

make qemu-gdb
gdb
(gdb) c
show user/spin.c
spin

you can see that they alternate, despite running continuously.
xv6 is switching its one CPU between the two processes.
  driven by timer interrupts
  we can see how often the timer ticks
how does the switching work?

I'm going to cause a break-point at the timer interrupt.
(gdb) b tra.c:81
(gdb) c
(gdb) where

we're in usertrap(), handling a device interrupt from the timer
(timerinit() in kernel/start.c configures the RISC-V timer hardware).

what was running when the timer interrupt happened?

(gdb) print p->name
(gdb) print p->pid
(gdb) print/x p->trapframe->epc

let's look for the saved epc in user/spin.asm
timer interrupted user code in the increment loop, no surprise

(gdb) step ... into yield() in proc.c
(gdb) next
(gdb) print p->state

change p->state from RUNNING to RUNNABLE -> give up CPU but want to run again.
note: yield() acquires p->lock
  to prevent another CPU from running this RUNNABLE thread!
  we're still using its kernel stack
  and we have not yet saved its kernel registers in its context

(gdb) next 3
(gdb) step (into sched())

sched() makes some sanity checks, then calls swtch()

(gdb) next 9

this is the context switch from a process's kernel thread to the scheduler thread
  swtch will save the current RISC-V registers in first argument (p->context)
  and restore previously-saved registers from 2nd argument (c->context)
  c->context are saved registers from this CPU's scheduler thread
let's see what register values swtch() will restore

(gdb) print/x c->context

where is c->context.ra?
  i.e. where will swtch() return to?
  (gdb) x/i c->context.ra
  swtch() will return to the scheduler() function in proc.c

(gdb) tbreak swtch
(gdb) c

we're in kernel/swtch.S
swtch() saves current registers in xx(a0)
  a0 is the first argument, p->context
swtch() then restores registers from xx(a1)
  a1 is the second argument, c->context
then swtch returns

Q: swtch() neither saves nor restores $pc (program counter)!
   so how does it know where to start executing in the target thread?

Q: why does swtch() save only 14 registers (ra, sp, s0..s11)?
   the RISC-V has 32 registers -- what about the other 18?
     zero, gp, tp
     t0-t6, a0-a7
   note we're talking about kernel thread registers
     all 32 user register have already been saved in the trapframe

registers at start of swtch:
(gdb) print $pc  -- swtch
(gdb) print $ra  -- sched
(gdb) print $sp

registers at end of swtch:
(gdb) stepi 28   -- until ret
(gdb) print $pc  -- swtch
(gdb) print $ra  -- scheduler
(gdb) print $sp  -- stack0+??? -- entry.S set this up at boot
(gdb) where
(gdb) stepi

we're in scheduler() now, in the "scheduler thread",
  on the scheduler's stack

scheduler() just returned from a call to swtch()
  it made that call a while ago, to switch to our process's kernel thread
  that previous call saved scheduler()'s registers
  our processes's call to swtch() restored scheduler()'s saved registers
  p here refers to the interrupted process

(gdb) print p->name
(gdb) print p->pid
(gdb) print p->state

remember yield() acquired the process's lock
  now scheduler releases it
  the scheduler() code *looks* like an ordinary acquire/release pair
    but in fact scheduler acquires, yield releases
    then yield acquires, scheduler releases
  unusual: the lock is released by a different thread than acquired it!

Q: why hold p->lock across swtch()?
   yield() acquires
   scheduler() releases
   could we release p->lock just before calling swtch()?

p->lock makes these steps atomic:
  * p->state=RUNNABLE
  * save registers in p->context
  * stop using p's kernel stack
  so other CPU's scheduler won't start running p until all steps complete

scheduler()'s loop looks at all processes, finds one that's RUNNABLE
  keeps looping until it finds something -- may be idle for a while
  in this demo, will find the other spin process

Q: does scheduler() give proc[0] an unfair advantage?
   since its for loop always starts by looking at proc[0]?

let's fast-forward to when scheduler() finds a RUNNABLE process

(gdb) tbreak proc.c:465
(gdb) c

scheduler() locked the new process, then set state to RUNNING
  now another CPUs' scheduler won't run it

it's the other "spin" process:

(gdb) print p->name
(gdb) print p->pid
(gdb) print p->state

let's see where the new thread will start executing after swtch()
  by looking at $ra (return address) in its context

(gdb) print/x p->context->ra
(gdb) x/4i p->context->ra

new thread will return into sched()

look at kernel/swtch.S (again)

(gdb) tbreak swtch
(gdb) c
(gdb) stepi 28 -- now just about to execute swtch()'s ret
(gdb) print $ra
(gdb) where

now we're in a timer interrupt in the *other* spin process
  in the past it was interrupted, called yield() / sched() / swtch()
  but now it will resume, and return to user space

note: only swtch() writes contexts (except for initialization)
      only sched() and scheduler() call swtch()
      so for a kernel thread, context.ra always points into sched()
      and for a scheduler thread, context.ra always points into scheduler()

sched() and scheduler() are "co-routines"
   each knows what it is swtch()ing to
   each knows where swtch() return is coming from
   e.g. yield() and scheduler() cooperate about p->lock and p->state
   different from ordinary thread switching, where neither
     party typically knows which thread comes before/after

Q: what is the "scheduling policy"? 
   i.e. how does xv6 decide what to run next if multiple threads are RUNNABLE?
   is it a good policy?

Q: is there pre-emptive scheduling of kernel threads?
   yes -- timer interrupt and yield() can occur while in kernel.
   yield() called by kerneltrap() in kernel/trap.c
   where to save registers of interrupted kernel code?
     not in p->trapframe, since already has user registers.
     not in p->context, since we're about to call yield() and swtch()
     kernelvec.S pushes them on the kernel stack (since already in kernel).
   is pre-emption in the kernel useful?
     not critical in xv6.
     valuable if some system calls have lots of compute.
     or if we need a strict notion of thread priority.

Q: why does scheduler() enable interrupts, with intr_on()?
   There may be no RUNNABLE threads
     They may all be waiting for I/O, e.g. disk or console
   Enable interrupts so device has a chance to signal completion
     and thus wake up a thread
   Otherwise, system will freeze

Q: why does sched() forbid locks from being held when yielding the CPU?
   (other than p->lock)
   i.e. sched() checks that noff == 1
   suppose process P1 holding lock L1, yields CPU
   process P2 runs, calls acquire(L1)
   P2's acquire spins with interrupts turned off
     so timer interrupts won't occur
     so P2 won't yield the CPU
     so P1 can't execute
     so P1 won't release L1, ever

Q: can we get rid of the separate per-cpu scheduler thread?
   could sched() directly swtch() to a new thread?
   that would be faster -- avoids one of the swtch() calls
   we'd move scheduler()'s loop into sched()
   maybe -- but:
     scheduling loop would run in sched() on a thread's kernel stack
     what if that thread is exiting?
     what if another cpu wants to run the thread?
     what if there are fewer threads than CPUs -- i.e. too few stacks?
     can be dealt with -- give it a try!

Summary
  xv6 provides a convenient thread model for kernel code
  pre-emptive via timer interrupts
  transparent via switching registers and stack
  multi-core requires careful handling of stacks, locks
  next lecture: mechanisms for threads to wait for each other