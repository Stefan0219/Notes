# Three Problems with Memory without Translation
**VM can be considered as a special type of cache which uses RAM to be the cache of the virtual memory** 
1. Not enough space.
![[Pasted image 20220923191835.png]]
2. Holes in Address Space
![[Pasted image 20220923192045.png]]
3. Ensuring protection from other programs.
![[Pasted image 20220923192132.png]]
![[Pasted image 20220923192205.png]]
# VM
## Indirection
![[Pasted image 20220923192442.png]]
![[Pasted image 20220923192510.png]]
![[Pasted image 20220923192645.png]]
![[Pasted image 20220923192736.png]]
![[Pasted image 20220923192759.png]]
## How does VM work ?
![[Pasted image 20220923192845.png]]
**Translation**
![[Pasted image 20220923193310.png]]
## Page Table
![[Pasted image 20220923193825.png]]
![[Pasted image 20220923193842.png]]
![[Pasted image 20220923194112.png]]
![[Pasted image 20220923194242.png]]
![[Pasted image 20220923194305.png]]
![[Pasted image 20220923194333.png]]
![[Pasted image 20220923194403.png]]
## Page Faults
How do I know if the page is in disk?
* The PTE points to disk.
![[Pasted image 20220923194529.png]]
## Page Replacement Policies
* FIFO
	*  Easy to implement.
* LRU
	* Most efficient but expensive to implement.
* RANDOM
![[Pasted image 20220923194805.png]]


## Memory Protection
## Additional Page Table Metadata
![[Pasted image 20220924143608.png]]
Just like cache.
### Page Protection Bits
* Can specify how a process can use each page
	* Read
	* Write
	* execute
**If don't have permission to do what you are trying, you'll get segfault or memory protection fault.**
![[Pasted image 20220924145756.png]]
![[Pasted image 20220924143848.png]]
![[Pasted image 20220924144040.png]]
**With virtual memory, it takes one more time to access to memory because you need to look up in page table first.**
To make this faster, we can implement a **Page Table Cache**
## Translation Lookaside Buffer(TLB)
![[Pasted image 20220924150001.png]]
![[Pasted image 20220924150037.png]]
![[Pasted image 20220924150104.png]]