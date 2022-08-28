# GDB Manual

  

The more specific notes can be found [here](https://inst.eecs.berkeley.edu/~cs61c/sp22/labs/lab01/) .
Use -g  flag during the complication process.
Use cgdb for my own tools.
## Basic operations
```typescript

cgdb exe

```
- Start
```typescript

start

```
- Step over
```typescript

next

```
or
```typescript

n

```
- Step into
```typescript

step

```
or
```typescript

s

```
- Exit from the function which we step into.
```typescript

finish

```
- Print the value
The things that you can print out is way more than the simple variables.
```typescript

print variable

print I_am_a_ptr->I_am_its_member

print foo(bar)

print *i_am_a_ptr

```
or
```typescript

p variable

```
- Break Points
```typescript

break foo.c:bar

```
Set a break point at foo.c's function bar
or just set the number following the break or b
```typescript

b foo.c:bar

```
- Conditional break point
It only stops when the condition is satisfied.
```typescript

b 1 if letter=='1'

```
- Run the program
```typescript

run

```
or
```typescript

r

```
- Backtrace
Allow you to see the call stack at the time of the ::crash:: .
```typescript

backtrace

```
or
```typescript

bt

```
- Command `command`
Executes a list of commands every time a break point is reached.
1. set a break point

command 1
```typescript

p var1

p var2

end

```
- Delete the specified breakpoint.