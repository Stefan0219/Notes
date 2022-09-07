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
These five stages are executed in one clock cycle, that is called One-instruction-Per-cycle.
![[Pasted image 20220907102124.png]]
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
