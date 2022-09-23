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
