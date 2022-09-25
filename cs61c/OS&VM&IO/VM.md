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
![[Pasted image 20220924155101.png]]
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
### TLB Flush
![[Pasted image 20220924150543.png]]
![[Pasted image 20220924150645.png]]
![[Pasted image 20220924150812.png]]

## Multi-level Page Tables
![[Pasted image 20220924150913.png]]
![[Pasted image 20220924150947.png]]
![[Pasted image 20220924151057.png]]
## Caches and Virtual Memory
### Physically Indexed, Physically Tagged Cache
![[Pasted image 20220924151346.png]]
![[Pasted image 20220924151622.png]]
**Synonyms** : Different virtual address can map to same physical address.
**Homonyms** : Same virtual address can map to different physical address.
### Virtually Indexed, Virtually Tagged Cache
![[Pasted image 20220924151816.png]]
![[Pasted image 20220924151844.png]]
### Virtually Indexed, Physically Tagged Caches
![[Pasted image 20220925214959.png]]
![[Pasted image 20220924152339.png]]
![[Pasted image 20220924152622.png]]
Start form the index to find the location
![[Pasted image 20220914160021.png]]
![[Pasted image 20220924152823.png]]
![[Pasted image 20220925215031.png]]
## PIPT vs VIVT
![[Pasted image 20220925214928.png]]
## Thrashing(抖动)
![[Pasted image 20220925215145.png]]
