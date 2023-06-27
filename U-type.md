## U-type



U-type for "Upper Immediate" instruction

| [31:12]          | [11:7] | [6:0]  |
| ---------------- | ------ | ------ |
| [31:12]upper-imm | rd     | opcode |
| 20-bit           | 5-bit  | 7-bit  |

Used for two instructions:

- **LUI : Load Upper Immediate**

LUI opcode is **7'b0110111**

LUI指令能将立即数的高20位写入寄存器，而低12位清零。**LUI**通常与**ADDI**指令一起使用，从而将一个32位的立即数写入目的寄存器。例如：

```assembly
lui x10, 0x87654		#x10 = 0x87654000
addi x10, x10, 0x321	#x10 = 0x87654321
```

同理，我们如何写入立即数0xDEADBEEF到寄存器？

```assembly
lui x10, 0xDEADB		#x10 = 0xDEADB000
addi x10, x10, 0xEEF	#x10 = 0xDEADAEEF
```

为何高20位的最低位发生了改变，没能达到预期的结果呢？

因为低12位立即数的最高位为1，经由符号扩展后为0xffffeef

相当于结果变成了高20位值-1再与低12位相拼接

So how to set 0xDEADBEEF ?

```assembly
lui x10, 0xDEADC		#x10 = 0xDEADC000
addi x10, x10, 0xEEF	#x10 = 0xDEADBEEF
```

我们在汇编语言中可以使用li伪指令直接载入32的立即数，但是在编译时还是会被编译成lui和addi两条指令

---

- **AUIPC : Add Upper Immediate to PC**

AUIPC opcode is **7'b0010111**

Adds upper immediate value to PC and places result in rd

Used for PC-relative addressing

for example:

in ASM:

```assembly
Lable: auipc x10, 0		# puts address of lable in x10
```

