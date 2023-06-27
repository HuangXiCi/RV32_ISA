## R-type



| [31:25] | [24:20] | [19:15] | [14:12] | [11:7] | [6:0]  |
| ------- | ------- | ------- | ------- | ------ | ------ |
| funct7  | rs2     | rs1     | funct3  | rd     | opcode |
| 7-bit   | 5-bit   | 5-bit   | 3-bit   | 5-bit  | 7-bit  |

opcode: partially specifies what instruction is

the reg - reg op is **7'b0110011**

funct7 + funct3: combined with opcode, these two fields describe what operation to perform

rs1 & rs2: source register

rd: destination register 

each reg field holds a 5-bit unsigned integer, because there are 32 regs (x0-x31)

for example:

in ASM:

```assembly
add x18, x19, x10
```

| [31:25] | [24:20] | [19:15] | [14:12] | [11:7] | [6:0]      |
| ------- | ------- | ------- | ------- | ------ | ---------- |
| funct7  | rs2     | rs1     | funct3  | rd     | opcode     |
| 7-bit   | 5-bit   | 5-bit   | 3-bit   | 5-bit  | 7-bit      |
| 0000000 | 01010   | 10011   | 000     | 10010  | 0110011    |
| add     | rs2=10  | rs1=19  | add     | rd=18  | reg-reg op |

---

## All RV32 R-format instructions



| 0000000 | rs2  | rs1  | 000  |  rd  | 0110011 |   add    |
| :-----: | :--: | :--: | :--: | :--: | :-----: | :------: |
| 0100000 | rs2  | rs1  | 000  |  rd  | 0110011 | **sub**  |
| 0000000 | rs2  | rs1  | 001  |  rd  | 0110011 | **sll**  |
| 0000000 | rs2  | rs1  | 010  |  rd  | 0110011 | **slt**  |
| 0000000 | rs2  | rs1  | 011  |  rd  | 0110011 | **sltu** |
| 0000000 | rs2  | rs1  | 100  |  rd  | 0110011 | **xor**  |
| 0000000 | rs2  | rs1  | 101  |  rd  | 0110011 | **srl**  |
| 0100000 | rs2  | rs1  | 101  |  rd  | 0110011 | **sra**  |
| 0000000 | rs2  | rs1  | 110  |  rd  | 0110011 |  **or**  |
| 0000000 | rs2  | rs1  | 111  |  rd  | 0110011 | **and**  |

- add：加法操作
- sub：减法操作
- sll：逻辑左移操作，空出来的位置使用0填充
- slt、sltu：有符号和无符号的比较指令，即 rs1 小于 rs2 则把rd置 1，否则置 0
- xor：异或操作
- srl：逻辑右移操作，空出来的位置使用0填充
- sra：算数右移操作，空出来的位置使用符号位填充
- or：或操作
- and：与操作

RISC-V中没有算数左移，因为算数左移和逻辑左移是一样的
