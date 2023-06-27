## B-type



for example:

in ASM:

```assembly
beq x1, x2, Label
```

意思是判断x1和x2是否相等，如果相等则跳转到Label处，否则继续执行下面的代码

可以发现，B-type指令与S-type指令有些类似，不需要将结果写入rd

那么我们如何对分支指令进行编码呢？

分支指令通常用于判断或循环(if-else, while, for)

Loops are generally small (<50 instructions)

Instructions stored in a localized area of memory (Code/text)

So the largest branch distance limited by size of code

Address of current instruction stored in the program counter (PC)

通常分支转移的跨越的指令地址空间有限，转移后PC值往往只发生了较小量级的变化

因此使用PC地址相对寻址的方法来得到分支转移的指令地址

只要将立即数段作为相对PC值的补码偏移量，就能转移到PC值的±2^11的地址

- 那么这个偏移量的地址单位应该怎么选取呢？
- 能不能和存储器地址单位一样选择字节呢？

不能，因为指令都是32-bit (4-byte)，我们需要跳转到对应指令的首字节位置，不会跳转到指令的中间字节位置，所有我们使用字节为单位，也只需要以4为倍数的字节偏移量，这无疑会让偏移量编码产生浪费，因此将偏移量以字(32-bit/4-byte)为单位

- To improve the reach of a single branch instruction, multiply the offset by four bytes before adding to PC
- This would allow one branch instruction to reach ±2^11 × 32-bit instructions either side of PC

**Four times greater reach than using byte offset**

在实际的RISC-V架构中，为了扩展16-bit的压缩指令编码格式和以16-bit为倍数的可变指令编码格式，分支转移的偏移量是以2-byte为单位的

So RISC-V conditional branches can only reach ±2^10 × 32-bit instructions on either side of PC

| [31:25]       | [24:20] | [19:15] | [14:12] | [11:7]       | [6:0]  |
| ------------- | ------- | ------- | ------- | ------------ | ------ |
| [12\|10:5]imm | rs2     | rs1     | funct3  | [4:1\|11]imm | opcode |
| 7-bit         | 5-bit   | 5-bit   | 3-bit   | 5-bit        | 7-bit  |

the B-type opcode is **7'b1100011**

---

C code:

```c
...
//得到1+2+3+...+50
for(i=1;i<51;i++)
    sum = sum + i;
...
```

in ASM:

```assembly
addr	instruction
		Loop:
		...
1000	bge x19, x10, End
1004	add x18, x18, x19
1008	addi x19, x19, 1
1012	j	Loop
		End:
1016	...
```

| [31:25]       | [24:20] | [19:15] | [14:12] | [11:7]       | [6:0]   |
| ------------- | ------- | ------- | ------- | ------------ | ------- |
| [12\|10:5]imm | rs2     | rs1     | funct3  | [4:1\|11]imm | opcode  |
| 7-bit         | 5-bit   | 5-bit   | 3-bit   | 5-bit        | 7-bit   |
| 0000000       | 01010   | 10011   | 101     | 10000        | 1100011 |
| imm=16        | rs2=10  | rs1=19  | bge     | imm=16       | branch  |

---

## All RISC-V Branch Instruction

| [12\|10:5]imm | rs2  | rs1  | 000  | [4:1\|11]imm | 1100011 | beq  |
| ------------- | ---- | ---- | ---- | ------------ | ------- | ---- |
| [12\|10:5]imm | rs2  | rs1  | 001  | [4:1\|11]imm | 1100011 | bne  |
| [12\|10:5]imm | rs2  | rs1  | 100  | [4:1\|11]imm | 1100011 | blt  |
| [12\|10:5]imm | rs2  | rs1  | 101  | [4:1\|11]imm | 1100011 | bge  |
| [12\|10:5]imm | rs2  | rs1  | 110  | [4:1\|11]imm | 1100011 | bltu |
| [12\|10:5]imm | rs2  | rs1  | 111  | [4:1\|11]imm | 1100011 | bgeu |

