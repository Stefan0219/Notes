# Notes about writing RV
## Save the registers before/after call function.
because little endian.
**the number add to stack pointer should be same to the number subtract from it!** 
**before** 
```R
addi sp, sp, -4
sw ra, 0(sp)
```
**after** 
```R
lw ra, 0(sp)
addi sp,sp,4
```
## Recursion is equal to function call
So just call itself at the end of the function to where the function label starts.

## Hof
pass a function into a function can set `a0` or `a1` to be the address of the function. 
```R
la a1, foo
```
and to execute it use `jalr` which 
```R
jalr ra,a1,0
```

## Return 
```R
jr ra
```
