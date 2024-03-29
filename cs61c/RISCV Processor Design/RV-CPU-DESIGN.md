
# Single-Core Processor
![[Pasted image 20220907100901.png]]
## CPU
The active part of the computer that does all the work.
* Data manipulation.
* Decision making.
### Datapath
Portion of the processor that contains hardware necessary to perform operations required by the processor(the body)
### Control
Portion of the processor that tells the datapath what needs to be done.(the brain)
# One-Instruction-Per-Cycle RISC-V Machine
## Stage of the Datapath
break up the process of “executing an instruction” into stages, and then connect the stages to create the whole datapath.
![[Pasted image 20220907101849.png]]
These five stages are executed in one clock cycle, which is called One-instruction-Per-cycle.
![[Pasted image 20220907102124.png]]
Register write will be executed at the next rising edge of clock.
### Datapath components
#### Combinational
* Combinational elements
![[Pasted image 20220907102259.png]]
#### State and Sequencing
##### Register
![[Pasted image 20220907102845.png]]
##### Register File
Consists of 32 registers
![[Pasted image 20220907103100.png]]
##### Memory
![[Pasted image 20220907103611.png]]

For **read** operation we **don't** need to wait for the clock, just put the address of register or memory, the data will automatically pop up.
For **write** operation we need to wait for the rising edge of the clock to write data.
Each instruction during execution reads and updates the state of
1. registers
2. pc
3. Memory

# Datapath
## R-Format Datapath
### Review
![[Pasted image 20220907104657.png]]
### Implementing the add instruction
![[Pasted image 20220907104826.png]]
Instruction does two changes
1. **Reg[rd] = Reg[rs1] + Reg[rs2]** 
2. **PC +=4** 
#### Datapath for add
![[Pasted image 20220907105733.png]]
![[Pasted image 20220907105907.png]]
#### Datapath for sub/add
* sub almost the same as add, except now we need to subtract operands.
![[Pasted image 20220907110234.png]]

## I-Format Datapath
### Addi
![[Pasted image 20220907110633.png]]
Add a Mux at there
![[Pasted image 20220907111024.png]]
![[Pasted image 20220907111256.png]]
![[Pasted image 20220907111309.png]]
![[Pasted image 20220907111355.png]]
### load
![[Pasted image 20220908095327.png]]
![[Pasted image 20220908095428.png]]
![[Pasted image 20220908095834.png]]
![[Pasted image 20220908100056.png]]
## S-Format
#### Adding sw instruction
![[Pasted image 20220908100327.png]]
![[Pasted image 20220908100505.png]]
![[Pasted image 20220908100522.png]]
![[Pasted image 20220908100613.png]]

## B-Format
![[Pasted image 20220908100827.png]]
* Need to compute **PC+IMMEDICATE**  and to compare values of **rs1 and rs2** 
![[Pasted image 20220908101412.png]]
![[Pasted image 20220908101646.png]]
![[Pasted image 20220908101836.png]]
![[Pasted image 20220908102255.png]]
