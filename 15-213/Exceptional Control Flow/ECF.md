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
## Receiving a Signal