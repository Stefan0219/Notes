# Binary Prefix
![[Pasted image 20220914142413.png]]
**Kissing me gives ten percent extra zeal & youth!**
## The way to remember numbers
![[Pasted image 20220914142616.png]]
# Memory Hierarchy Basis
* Temporal locality: If we use it now, chances are we'll want to use it again soon.
> Keep most recently accessed data items closer to the processor.
* Spatial locality: If we use a piece of memory, chances are we'll use the neighboring pieces soon.
> Move blocks consisting of contiguous words closer to the processor.
# Cache
**Caches provide an illusion to the processor that the memory is infinitely large and infinitely fast**
## Direct Mapped Caches
**In a direct-mapped cache, each memory address is associated with one possible block within the cache**
![[Pasted image 20220914155950.png]]
![[Pasted image 20220914160006.png]]
![[Pasted image 20220914160021.png]]
![[Pasted image 20220914160034.png]]
![[Pasted image 20220915154547.png]]
# Writes, Block, Sizes, Misses
## Write
### Write-through
* Update both cache and memory.
### Write-back
* Update word in cache block.
* allow memory word to be stale
* add **dirty** bit to block
	* Memory and cache inconsistent.
	* needs to be updated when block is replaced.
* Operating system flushes cache before I/O.
## Block Size Tradeoff
![[Pasted image 20220916084924.png]]
![[Pasted image 20220915155031.png]]
![[Pasted image 20220915155208.png]]
![[Pasted image 20220915155246.png]]
## Types of Cache Missed
### Compulsory(forced) Misses
* occur when a program is first started
* cache does not contain any of that program’s data yet, so misses are bound to occur
* can’t be avoided easily.
* **Every block of memory will have one compulsory miss (NOT only every block of the cache)** 
### Conflict Misses
![[Pasted image 20220915155639.png]]
### Fully Associative Caches
![[Pasted image 20220915160335.png]]
* Any block can go anywhere in the cache.
* must compare with all tags in **entire cache** to see if data is there.
![[Pasted image 20220915160517.png]]
![[Pasted image 20220915160635.png]]
### Capacity Misses
![[Pasted image 20220915160852.png]]
# Conclusion
![[Pasted image 20220915161017.png]]

# Set-Associative Cache
![[Pasted image 20220916084239.png]]
![[Pasted image 20220916084322.png]]
![[Pasted image 20220916084339.png]]
![[Pasted image 20220916084424.png]]
![[Pasted image 20220916084506.png]]
![[Pasted image 20220916084532.png]]
![[Pasted image 20220916084545.png]]
# Average Memory Access Time
![[Pasted image 20220916084632.png]]
![[Pasted image 20220916084653.png]]
![[Pasted image 20220916084726.png]]
![[Pasted image 20220916084741.png]]