# Pointers, Arrays, Memory
## Address vs value
$void *$ is a type that can point to anything.
word alignment.
## Pointer Arithmetic
```C
char *c;
char **d;
(c+5)-> c + sizeof(char)*5 ->c+5
(d+7)-> d + sizeof(char*)*7 ->d+28
```
* The actual value used by the compiler is the size what the pointer are pointing to.
## Conclusion on Pointers
![[Pasted image 20220924172540.png]]
![[Pasted image 20220825124403.png]]
## Struct
[结构体内存对齐](https://cloud.tencent.com/developer/article/1703257)
It's an instruction to C on how to arrange a bunch of bytes in a bucket.
Provides enough space and aligns the data with padding.
```C
struct foo{
int a;
char b;
struct foo *c;
}
```
So the real memory layout will be:
4 bytes for a,
1 byte for b,
3 bytes empty,
4 bytes for c.
## Unions
```C
union foo{
int a;
char b;
union foo *c
}
```
Provides enough space for the **largest** element.
## C Arrays 
```C
int ar[2];
```
The number of elements is static in the declaration, you can't do `int ar[x]` where x  is a variable
![[Pasted image 20220924173758.png]]
* Can use pointer variable to access arrays.
An array is passed into a function as a pointer.
## C Strings 
This can be modified.
```C
char string[] = "abc";
```
This can't. 字符串常量
```C
char *string = "abc";
```
Ends with a `\0` .
![[Pasted image 20220924174251.png]]
![[Pasted image 20220924174341.png]]
![[Pasted image 20220924174540.png]]
![[Pasted image 20220825130723.png]]
## Endianness
The network byte order is big-endian.
Endian conversion functions in C:
```C
ntohs()
htohs()
```
## C Memory Management
![[Pasted image 20220825132139.png]]
![[Pasted image 20220825132506.png]]
![[Pasted image 20220825132756.png]]
## Managing the Heap
![[Pasted image 20220825133630.png]]
## Pointer to a function
![[Pasted image 20220929173744.png]]
![[Pasted image 20220929174327.png]]
![[Pasted image 20220929175156.png]]