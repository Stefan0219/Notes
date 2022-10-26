# 6.1810 2022 Lecture 8: Q&A

Plan: **answering your questions** 
  Approach:
    walk through staff solutions
    start with pgtbl lab because it was the hardest
    your questions are at bottom of this file

Pgtbl lab comments
  few lines of code, but difficult-to-debug bugs
  - worst case: qemu/xv6 stops running
  - "best" case: kernel panic
  hard to debug for staff too
  - there are so many possible reasons why

Part 1 of pgtbl lab
  Avoid kernel transitions
  Expose kernel state to user space
    augment user space diagram with USYSCALL
  Which system calls can be sped up?  (see kernel/syscall.h)
    can we move system call to user space that write kernel state?
    can we move system call to user space that read kernel state related to other processes?
  Candidate are:
    getpid()
    uptime()
    fstate()?
      maybe possible, but too much state?
  Demo
    pgtbltest.c
    ulib.c
    kernel/memlayout.h
    kernel/proc.c:
      proc_pagetable()
      freeproc()
  Linux has vDSO (https://lwn.net/Articles/615809/)
    vDSO: virtual dynamic shared object
    cat /proc/<pid>/maps
    read-only, shared memory region
    vdso library mapped into each user program
      library interprets data in the shared region
    example timer measurements
      kernel posts time to shared region
      vDSO code adds TSC to latest time
    calls implemented using vDSO (virtual system calls)
      clock_gettime(), getcpu(), getpid(), getppid(), gettimeofday(), set_tid_address()
      
Part 2 of pgtbl lab
  Explain vm output in terms of fig 3-4
       L0, L1, L2/PTE
       page table 0x0000000087f6b000
       ..0: pte 0x0000000021fd9c01 pa 0x0000000087f67000
       .. ..0: pte 0x0000000021fd9801 pa 0x0000000087f66000
       .. .. ..0: pte 0x0000000021fda01b pa 0x0000000087f68000  (text)
       .. .. ..1: pte 0x0000000021fd9417 pa 0x0000000087f65000  (data)
       .. .. ..2: pte 0x0000000021fd9007 pa 0x0000000087f64000  (guard)
       .. .. ..3: pte 0x0000000021fd8c17 pa 0x0000000087f63000  (stack)
       ..255: pte 0x0000000021fda801 pa 0x0000000087f6a000
       .. ..511: pte 0x0000000021fda401 pa 0x0000000087f69000
       .. .. ..509: pte 0x0000000021fdcc13 pa 0x0000000087f73000  (usyscall)
       .. .. ..510: pte 0x0000000021fdd007 pa 0x0000000087f74000  (trapframe)
       .. .. ..511: pte 0x0000000020001c0b pa 0x0000000080007000  (trampoline)
       init: starting sh
    what is entry 0, 1, 2, and 3?
    what is 509, 510, and 511?
    - 511: trampoline
    - 510: trapframe
    - 509: USYSCALL
    why are the protection bits as they are?
      bottom 2 hex digits in PTE/L1/L0
    - 509: URV
    - 510: WRV (no U bit)
    - 511: XRV (no U bit)
    why are XRW = 0 in L0 and L1 ptes?
      indicate that it is a not a leaf PTE
    are the physical addresses contiguous?
  Demo
    structure follows walk()
    vm.c:vmprint()
    vm.c:print_level
      PTE2PA()
      why can we use the output of PTE2PA as a virtual address?
    
Part 3 of pgtbl lab
  Goal: efficiently detect which pages have been accessed
  Idea: exploit hardware page-table walk:
    it sets PTE_A when page is accessed (read or write)
    it sets PTE_D when page is written ("made dirty")
  Approach: syscall w. bitmask indicating which pages are accessed
  Demo
    pgtbltest.c
    sys_pgaccess
      draw: 1 PGSIZE buffer with 1 bit per page
    pgaccess
      argaddr(2, &ubitmask)
        can it fail?
      copyout
        the physical page for a user page is also in kernel AS
  How does Linux use access bits?
    Used for implementing LRU
      Move least accessed page to swap
    Use PTE_D to detect if copy on disk is stale
    Scanning all pages is expensive, so more clever
      Hard problem; many changes/tweaks over time
      https://www.usenix.org/legacy/publications/library/proceedings/usenix05/tech/general/full_papers/jiang/jiang_html/html.html
      https://lwn.net/Articles/495543/
      https://linux-mm.org/PageReplacementDesign
      https://www.kernel.org/doc/gorman/html/understand/understand013.html
      mm/swap.c
  Linux doesn't expose access bits information to user space!
    How would could you detect page access *without* PTE_A?

Syscall lab
  systems calls look like functions calls
  BUT they are not function calls
  trace
    proc.h
    fork()
  sysinfo
    copyout()

Util lab
  primes
    close fd's to terminate correctly
  xargs
    setting up argv 

=== Questions ===

=== util

I was a little confused during the lab-util lab about pipes -- do we
have to close all the write ends of the pipe in order for the program
to terminate (if it was just continuously doing if (read(...)  +> 0))?

===

2) For the very first lab, I had a hard time closing th pipes
correctly. Is there some convention for when to close pipes

===

I was also wondering if the prime sieve question from lab 1 could be
explained, particularly the concurrent sieve/pipeline part.

=== syscall

When a trap saves all the existing registers, is there any limit on
which of those registers the kernel will use? Or is it possible it
will use all of them?

====

I was wondering why there was no error handling for argparsing in
syscalls. The function signature for argint is `void argint(int n, int
*ip)` and the internals of the function appear to panic if a bad input
was supplied. Is there a more graceful way to handle this?

====

When a system call occurs, what's the difference between
"trapframe->a0 = 0;" and "return 0;" in a sys_XXX function? In my
opinion both put 0 to the a0 register (when we are back in user
mode).

===

In the syscall labs, why did we need to use a perl script to generate
usys.S? Why don't we just write all the user syscall stubs in C code
instead of generating all of the code using assembly? Is it just more
compact and efficient to use a script to create assembly code?

=== pgtabl

Also, I got a little confused about page tables.  Are PTEs and virtual
addresses equivalent or related? Is there one PTE per 4096-byte page
(if so is there multiple VAs per page or just one)?

===

In the pagetable lab, the following was mentioned: "Some garbage
collectors (a form of automatic memory management) can benefit from
information about which pages have been accessed (read or write)."

Assuming that the garbage collector for some program is running in
userland, is there a syscall exposed in Linux that achieves this?

===

I didn't understand the last question on lab-pgtbl: What does the
third to last page contain?

From inspecting the PTE, it looked like the PTE_R and PTE_U bits were
set (and not PTE_W) which didn't seem to correspond to any part of the
process's user address space from Figure 3.4?

===

I would like to discuss how copyout works, I mostly understand the
function/code, but it would be good to go over, when exactly copyout
should happen and what actually happens on a high level.

===

Where are page tables themselves stored in memory?

===

How would we implement a LRU paging out system? How do we keep track
of which pages have been most recently used?

===

In the last part of the page table lab, I only mark a page as
"accessed" if the user requesting access information has read
permissions for the given +page.  However, I can't think of anything
harmful that a user could do with access information for a page they
don't have permission to read. Was this permission check totally
unnecessary or am I overlooking something? Would the access bit still
be set by the hardware even if the physical page is accessed via a
different page table?

===

1) for the lab pgtbl, the second question's last part was the
following: What does the third to last page contain?
My answer was this:
    If I understand this correctly, the third to last page is the 509.
The pte ends in 13 which is 00010011 which means the pages is
accessible to user mode, is readable and valid. I suspect that it is
either trampoline or trapframe because vmprint is printing all the
pages and trampoline and trapframe should have been printed. I think
it's trapframe since it's below trampoline.

I am a little confused about the bits though. I have a feeling I'm
encoding this wrong because 00010011 doesn't match with neither the
RX--- nor the R-W-- that's written next to the trampoline and
trampfrane pages in figure 3.4.

Could you please explain the answer to this question?

===

Why do we design page tables with multiple levels?

===

I was a bit unsure about the discussion questions for lab 3 (pgtbl).
In particular, I was wondering about what other system calls could be
optimized via shared memory in user space, and also about the output
of vmprint.

===

How to use gdb to debug page-table? Is it possible to print out the
content of physical address?

===

How does the hardware set the bit for access for pagetable?

===

For lab pgtbl, we printed the initial page table, I am wondering
why there is little relationship between the pa.  For example, 509
0x0000000087f76000 for USYSCALL is close to 510 0x0000000087f77000 for
TRAPFRAME. While 511 0x0000000080007000 for TRAMPOLINE is far from
them.  ow are they allocated?

===

In the page table lab there was an optional challenge exercise for
using super-pages to reduce the number of PTEs. How do we determine
when the page walk should end early (to obtain a super-page)? I assume
that this is chosen by the OS upon a call to sbrk() as a function of
the parameter. How do we then combine ideas for lazy memory allocation
and super-pages? (lazy allocation allocates on a page by page basis,
but how do we know at which granularity to allocate?)

===

In the page table lab, it is written that pgaccess is used by garbage
collectors. I'd like to know in what way this information is used.
Also, doesn't using pgaccess unset the access bits of all pages, thus
causing it to only be usable once before it wipes the slate?

===

I got the logic for PTE_A but I'm not sure about the process, is this
a CPU thing that the page gets marked when it's accessed or it's
somewhere in the kernel?

=== Other

With regards to multiple of the labs, how come we need to hold a lock
when accessing some fields of the proc struct, but not others? What
does it mean for the ones we don't need a lock for to be "private to
the process" (i.e. what exactly is the difference between these two
sets of fields)?

====

Why does xv6 yield the CPU on every single timer interrupt? Isn't this
extremely performance-inefficient? How do real operating systems
handle the yielding of processes?

===

I'm a bit interested in how gdb actually works.

===

This is a more general question, but can we go over visually (maybe
thru a diagram or something) all the important registers for page
table/ trapframe sorts of things, along with important memory
addresses, and how those are different between physical and virtual
memory?  I know there's a diagram 3.3 that shows exactly that, but
it's still confusing to me. We've mentioned a lot of register names/
memory locs and they're all starting to blur together.