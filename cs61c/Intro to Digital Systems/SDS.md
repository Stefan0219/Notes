# Transistors
Three terminals : Drain ,Gate, Source
![[Pasted image 20220905113134.png]]
![[Pasted image 20220905113320.png]]
![[Pasted image 20220905113340.png]]
# Signals and Waveform
![[Pasted image 20220905113619.png]]
![[Pasted image 20220905113642.png]]
* A nibble adder which treat $X_0$ as the LSB and $X_3$  as MSB, thus gives the $X$ below.
![[Pasted image 20220905113837.png]]
# Register
![[Pasted image 20220905113916.png]]
clk enables the register to store the input.
Clock control pulse of our circuits.
![[Pasted image 20220905114046.png]]
# Register Details: Flip-Flops
n-bit wide register is nothing but n instance of **flip-flop**.
![[Pasted image 20220905152404.png]]
Input bits is $d_i$ and output is $q_i$ , d stands for data, q stands for quiet a.k.a. stable
## edge-triggered d-type flip-flop
Only consider positive edge-triggered.
![[Pasted image 20220905152804.png]]
On the rising edge of the clock, the input d is sampled and transferred to the output. At all other times, the input d is ignored.
> limitations 
> 	ff cannot change their outputs instantaneously.
> 	Time is need to transfer inputs internally

Therefore, the input $d$ should be stable before the rising edge and remain stable for a short amount of time after the edge.
These two called: **setup time**  and **hold time** .
Setup time mainly prevent sampling during the input is at rising edge.
During this time window, the input shall **not change** .
Once the flip-flop captures the new input, it also takes a small amount time to transfer the new value to output. 
This delay is called *clk-to-q* delay.
![[Pasted image 20220905153730.png]]

# Accumulator
![[Pasted image 20220905154924.png]]
![[Pasted image 20220905155212.png]]

![[Pasted image 20220905155316.png]]