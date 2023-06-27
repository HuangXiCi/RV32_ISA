## S-type



| [31:25]   | [24:20] | [19:15] | [14:12] | [11:7]   | [6:0]  |
| --------- | ------- | ------- | ------- | -------- | ------ |
| [11:5]imm | rs2     | rs1     | funct3  | [4:0]imm | opcode |
| 7-bit     | 5-bit   | 5-bit   | 3-bit   | 5-bit    | 7-bit  |



Store needs to read two regs, rs1 for base memory address, and rs2 for data to be stored,as well immediate offset.源寄存器rs1提供基地址，rs2提供需要存储的数据，立即数为偏移量。

由于我们需要两个源寄存器，所以不能像Load装载指令那样，将rs2的位置腾出得到连续的12-bit立即数

the S-type opcode is **7'b0100011**

for example:

in ASM:

```assembly
sw x14, 8(x2)
```

| [31:25]        | [24:20] | [19:15] | [14:12] | [11:7]        | [6:0]   |
| -------------- | ------- | ------- | ------- | ------------- | ------- |
| [11:5]imm      | rs2     | rs1     | funct3  | [4:0]imm      | opcode  |
| 7-bit          | 5-bit   | 5-bit   | 3-bit   | 5-bit         | 7-bit   |
| 0000000        | 01110   | 00010   | 010     | 01000         | 0100011 |
| [11:5]offset=0 | rs2=14  | rs1=2   | sw      | [4:0]offset=8 | store   |

---

## All RV32 Store Instructions



| [11:5]imm | rs2  | rs1  | 000  | [4:0]imm | 0100011 | sb   |
| --------- | ---- | ---- | ---- | -------- | ------- | ---- |
| [11:5]imm | rs2  | rs1  | 001  | [4:0]imm | 0100011 | sh   |
| [11:5]imm | rs2  | rs1  | 010  | [4:0]imm | 0100011 | sw   |

RISC-V中只能保存有符号数，不能保存无符号数
