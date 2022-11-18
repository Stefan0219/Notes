**主要是你问问题， 我能解答的都会解答， 我可能把握不好速度**


# CPU
* 数据运算 （加减乘除）
* 控制流决策（decision making）分支指令

把CPU类比做提线木偶
![[Pasted image 20221118164500.png]]

1 Datapath（身体）
**ALU** 等
 ![[Pasted image 20221118164532.png]]
2 Control （绳子）告诉datapath 怎么做（控制）
control unit
![[Pasted image 20221118164545.png]]


![[Pasted image 20221118164806.png]]

**CPU 怎么知道指令对应的操作？**
![[Pasted image 20221118181043.png]]
![[Pasted image 20221118165515.png]]
人为规定协议。
CPU查找指令对应的字段。
获得指令对应的操作 激活对应的电路。
ALU中所有的操作都是**并行产生**，不论什么指令，最后的输出由控制器决定（MUX）。

![[Pasted image 20221118170516.png]]


addi $4, $0, 42 mips
addi x4, x0, 2 (riscv) 简单起见
翻译为二进制指令

000000000010   00000  000 00100  0010011
		imm
	![[Pasted image 20221118171643.png]]

IF(INSTRUCTION FETCH)


