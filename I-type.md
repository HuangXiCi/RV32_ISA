## I-type



Only one field is different from R-format

R-format:

| [31:25] | [24:20] | [19:15] | [14:12] | [11:7] | [6:0]  |
| ------- | ------- | ------- | ------- | ------ | ------ |
| funct7  | rs2     | rs1     | funct3  | rd     | opcode |
| 7-bit   | 5-bit   | 5-bit   | 3-bit   | 5-bit  | 7-bit  |

I-format:

| [31:20] | [19:15] | [14:12] | [11:7] | [6:0]  |
| ------- | ------- | ------- | ------ | ------ |
| imm     | rs1     | funct3  | rd     | opcode |
| 12-bit  | 5-bit   | 3-bit   | 5-bit  | 7-bit  |

the op-imm is **7'b0010011**

Remaining fields(rs1, rd, funct3, opcode) same as before

**imm[11:0] can hold values in range (-2048,+2047)**

for example:

in ASM:

```assembly
addi x15, x1, -50
```

| [31:20]      | [19:15] | [14:12] | [11:7] | [6:0]   |
| ------------ | ------- | ------- | ------ | ------- |
| imm          | rs1     | funct3  | rd     | opcode  |
| 12-bit       | 5-bit   | 3-bit   | 5-bit  | 7-bit   |
| 111111001110 | 00001   | 000     | 01111  | 0010011 |
| imm=-50      | rs=1    | add     | rd=15  | op-imm  |

## All RV32 I-format Arithmetic Instructions



| imm  | rs1  | 000  | rd   | 0010011 | addi  |
| ---- | ---- | ---- | ---- | ------- | ----- |
| imm  | rs1  | 010  | rd   | 0010011 | slti  |
| imm  | rs1  | 011  | rd   | 0010011 | sltiu |
| imm  | rs1  | 100  | rd   | 0010011 | xori  |
| imm  | rs1  | 110  | rd   | 0010011 | ori   |
| imm  | rs1  | 111  | rd   | 0010011 | andi  |

| 0000000 | shamt | rs1  | 001  | rd   | 0010011 | slli |
| ------- | ----- | ---- | ---- | ---- | ------- | ---- |
| 0000000 | shamt | rs1  | 101  | rd   | 0010011 | srli |
| 0100000 | shamt | rs1  | 101  | rd   | 0010011 | srai |

**"Shift-by-immediate" instructions only use lower 5 bits of the immediate value for shift amout(can only shift by 0-31 bit positions)**

---

## Load instructions are also I-format



| [31:20] | [19:15] | [14:12] | [11:7] | [6:0]  |
| ------- | ------- | ------- | ------ | ------ |
| imm     | rs1     | funct3  | rd     | opcode |
| 12-bit  | 5-bit   | 3-bit   | 5-bit  | 7-bit  |
| offset  | base    | funct3  | dest   | LOAD   |

for example:

in ASM:

```assembly
lw x14, 8(x2)
```

| [31:20]      | [19:15] | [14:12] | [11:7] | [6:0]   |
| ------------ | ------- | ------- | ------ | ------- |
| imm          | rs1     | funct3  | rd     | opcode  |
| 12-bit       | 5-bit   | 3-bit   | 5-bit  | 7-bit   |
| 000000001000 | 00010   | 010     | 01110  | 0000011 |
| imm=+8       | rs1=2   | lw      | rd=14  | LOAD    |

---

## All RV32 Load Instructions



| imm  | rs1  | 000  | rd   | 0000011 | lb   |
| ---- | ---- | ---- | ---- | ------- | ---- |
| imm  | rs1  | 010  | rd   | 0000011 | lh   |
| imm  | rs1  | 011  | rd   | 0000011 | lw   |
| imm  | rs1  | 100  | rd   | 0000011 | lbu  |
| imm  | rs1  | 110  | rd   | 0000011 | lhu  |

lb is "load byte(8-bit)", lbu is load unsigned byte

lh is "load halfword(16-bit or 2-byte)", lhu is load unsigned halfword

they need to extend to fill dest 32-bit reg



**and we'll later see how to handle imm > 12-bit ......**

