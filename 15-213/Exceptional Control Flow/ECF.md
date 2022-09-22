# Exceptional Control Flow
## Control Flow
![[Pasted image 20220921090602.png]]
### Altering the Control Flow
* Jump and branches
* Call and return
Change **program state.**
**But insufficient for a useful system. Difficult to react to changes in system state.**
![[Pasted image 20220921090950.png]]
So, system needs mechanisms for **ECF**
## ECF
![[Pasted image 20220921091132.png]]
# Exceptions
An exception is a **transfer of control to the OS kernel** in response to some **event** (i.e., change in processor state)
![[Pasted image 20220921091257.png]]
![[Pasted image 20220921091341.png]]
## Asynchronous Exceptions (Interrupts)
![[Pasted image 20220921091440.png]]
![[Pasted image 20220921091540.png]]
## Synchronous Exceptions
![[Pasted image 20220921091724.png]]
![[Pasted image 20220921091755.png]]
### Syscall
![[Pasted image 20220921091923.png]]
![[Pasted image 20220921092000.png]]
![[Pasted image 20220921092035.png]]
# Processes
![[Pasted image 20220921092241.png]]
![[Pasted image 20220921092358.png]]
# Process Control
![[Pasted image 20220921092504.png]]
![[Pasted image 20220921092545.png]]
![[Pasted image 20220921092630.png]]
![[Pasted image 20220921092906.png]]
![[Pasted image 20220921092919.png]]
![[Pasted image 20220921092954.png]]
![[Pasted image 20220921093009.png]]
![[Pasted image 20220921093059.png]]
![[Pasted image 20220921093133.png]]
# Signal
![[Pasted image 20220922204119.png]]
Back ground process will not be terminated after running, **kernel** need to send signals to process which will be terminated.
A signal is a small message which **notifies** a process that an **event** of some type have occurred in the system.** 
![[Pasted image 20220922204436.png]]
## Sending signal
**Kernel** sends a signal to a destination process by **updating** some **state** in the **context** of the destination process.
### Why sending signal 
![[Pasted image 20220922210303.png]]
![[Pasted image 20220922222432.png]]
**Sending signals(SIGINT/SIGTSTP) form the Keyboard will only act on the foreground process group.**


## Receiving a Signal
![[Pasted image 20220922212857.png]]
![[Pasted image 20220922222837.png]]
![[Pasted image 20220922222857.png]]
![[Pasted image 20220922222957.png]]

## Pending(挂起) and Blocked Signals 
Pending: sent but not yet received.
![[Pasted image 20220922221746.png]]
![[Pasted image 20220922221925.png]]
A pending signal is received at most once.
## Pending / Blocked Bits
![[Pasted image 20220922222048.png]]
* Every process belongs to exactly one process group.
![[Pasted image 20220922222400.png]]
## Default Actions of signal
 Each signal type has a predefined **default action**, which is one of:
* The process terminates.
* The process terminates and dumps core(take a snap shot of the memory).
* The process stops until restarted by a SIGCONT signal.
* The process ignores the signal.
## Install Signal Handlers
![[Pasted image 20220922223519.png]]
**A signal handler is a separate logical flow(not process) that runs concurrently with the main program.**
![[Pasted image 20220922223827.png]]
![[Pasted image 20220922223903.png]]
## Blocking and unblocking signals
### Implicit
* Kernel blocks any pending signals of type currently being handled.
![[Pasted image 20220922225352.png]]
 ![[Pasted image 20220922225432.png]]