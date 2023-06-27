## J-type



- JAL (jump and link)

The JAL opcode is **7'b1101111**

JAL saves PC+4 in rd (the return addr), and set PC = PC + offset (PC relative jump)

| 31      | [30:21]   | 20      | [19:12]    | [11:7] | [6:0]  |
| ------- | --------- | ------- | ---------- | ------ | ------ |
| [20]imm | [10:1]imm | [11]imm | [19:12]imm | rd     | opcode |
| 1-bit   | 10-bit    | 1-bit   | 8-bit      | 5-bit  | 7-bit  |

可以看到，JAL指令与B-type一样采用相对PC值进行跳转的方法，跳转空间为±2^19。

同样以2-bit作为最小单位进行跳转，所以能够跳转到PC值±2^18 32-bit的指令

- J (jump)

在RISC-V中，J指令是由JAL组成的伪指令，只是令rd = x0，就没有写入PC+4的操作了

例如:

```assembly
j Label
```

会被编译成:

```assembly
jal x0, Label		#Discard return addr
```

而jal指令使用的最多的还是:

```assembly
jal ra, FuncName
```

将PC+4保存到ra中，并跳转到FunName处，用于实现函数调用

---

- JALR (I-type)

The JALR opcode is **7'b1100111**

JALR writes PC+4 to rd, and sets PC = rs1 + imm

注意JALR是一条I型指令，其与I型指令中的Load指令非常相似

**同时它的跳转没有以2-bit作为最小单位**，这点与branch和jal不同

| [31:20] | [19:15] | [14:12] | [11:7] | [6:0]  |
| ------- | ------- | ------- | ------ | ------ |
| imm     | rs1     | funct3  | rd     | opcode |
| 12-bit  | 5-bit   | 3-bit   | 5-bit  | 7-bit  |
| offset  | base    | funct3  | dest   | JALR   |

- JR

在RISC-V中，JR指令是由JALR组成的伪指令

```assembly
jr ra
```

会被编译成：

```assembly
jalr x0, ra, 0
```

这样，在函数调用完成后就能返回原地址执行下面的代码了

如何跳转到任何32-bit的绝对地址？

```assembly
lui x1, <hi20bits>
jalr ra, x1, <lo12bits>
```

如何相对PC值跳转32-bit偏移量

```assembly
auipc x1, <hi20bits>
jalr x0, x1, <lo12bits>
```

