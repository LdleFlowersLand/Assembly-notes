# Assembly-notes
学习汇编的笔记
# 第一章

## 1.5.1 8086的功能结构

![img](https://gimg2.baidu.com/image_search/src=http%3A%2F%2Ffile.elecfans.com%2Fweb1%2FM00%2F4F%2F23%2Fo4YBAFrUKVKAH5V1AACKvZ-E2ak765.jpg&refer=http%3A%2F%2Ffile.elecfans.com&app=2002&size=f9999,10000&q=a80&n=0&g=0n&fmt=jpeg?sec=1637154264&t=e6c50458bcdc25110beffa358c9e85a4)

## 1.5.2 8086的寄存器

### 1.通用寄存器

#### （1）数据寄存器

AX累加器：使用频率最高，用于算数，逻辑运算，以及鱼外设传送信息。

BX基址寄存器：常用于存放储存器地址。

CX计数器：作为循环和串操作等指令中的隐含计数器。

DX数据寄存器：用来放双字长数据的高16位，或存放外设端口地址。



#### （2）变址及指针寄存器

### 2.标志寄存器

![img](https://gimg2.baidu.com/image_search/src=http%3A%2F%2Fimg-blog.csdnimg.cn%2F20190522092627892.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzIwMzMwNTk1%2Csize_16%2Ccolor_FFFFFF%2Ct_70&refer=http%3A%2F%2Fimg-blog.csdnimg.cn&app=2002&size=f9999,10000&q=a80&n=0&g=0n&fmt=jpeg?sec=1637153439&t=21389a230fd5e5488ed90b74c33f22e3)



#### （1）状态标志

状态控制标志位是用来控制CPU操作的，它们要通过专门的指令才能使之发生改变。

##### 1、追踪标志TF(Trap Flag)

当追踪标志TF被置为1时，CPU进入单步执行方式，即每执行一条指令，产生一个单步中断请求。这种方式主要用于程序的调试。

指令系统中没有专门的指令来改变标志位TF的值，但程序员可用其它办法来改变其值。

##### 2、中断允许标志IF(Interrupt-enable Flag)

中断允许标志IF是用来决定CPU是否响应CPU外部的可屏蔽中断发出的中断请求。但不管该标志为何值，CPU都必须响应CPU外部的不可屏蔽中断所发出的中断请求，以及CPU内部产生的中断请求。具体规定如下：

(1)、当IF=1时，CPU可以响应CPU外部的可屏蔽中断发出的中断请求；

(2)、当IF=0时，CPU不响应CPU外部的可屏蔽中断发出的中断请求。

CPU的指令系统中也有专门的指令来改变标志位IF的值。

##### 3、方向标志DF(Direction Flag)

方向标志DF用来决定在串操作指令执行时有关指针寄存器发生调整的方向。具体规定在第5.2.11节——字符串操作指令——中给出。在微机的指令系统中，还提供了专门的指令来改变标志位DF的值。

#### （2）控制标志

##### 1、进位标志CF(Carry Flag)

进位标志CF主要用来反映运算是否产生进位或借位。如果运算结果的最高位产生了一个进位或借位，那么，其值为1，否则其值为0。

使用该标志位的情况有：多字(字节)数的加减运算，无符号数的大小比较运算，移位操作，字(字节)之间移位，专门改变CF值的指令等。

##### 2、奇偶标志PF(Parity Flag)

奇偶标志PF用于反映运算结果中“1”的个数的奇偶性。如果“1”的个数为偶数，则PF的值为1，否则其值为0。

利用PF可进行奇偶校验检查，或产生奇偶校验位。在数据传送过程中，为了提供传送的可靠性，如果采用奇偶校验的方法，就可使用该标志位。

##### 3、辅助进位标志AF(Auxiliary Carry）

在发生下列情况时，辅助进位标志AF的值被置为1，否则其值为0：

(1)、在字操作时，发生低字节向高字节进位或借位时；
(2)、在字节操作时，发生低4位向高4位进位或借位时。

对以上6个运算结果标志位，在一般编程情况下，标志位CF、ZF、SF和OF的使用频率较高，而标志位PF和AF的使用频率较低。

##### 4、零标志ZF(Zero Flag)

零标志ZF用来反映运算结果是否为0。如果运算结果为0，则其值为1，否则其值为0。在判断运算结果是否为0时，可使用此标志位。

##### 5、符号标志SF(Sign Flag)

符号标志SF用来反映运算结果的符号位，它与运算结果的最高位相同。在微机系统中，有符号数采用补码表示法，所以，SF也就反映运算结果的正负号。运算结果为正数时，SF的值为0，否则其值为1。

##### 6、溢出标志OF(Overflow Flag)

溢出标志OF用于反映有符号数加减运算所得结果是否溢出。如果运算结果超过当前运算位数所能表示的范围，则称为溢出，OF的值被置为1，否则，OF的值被清为0。

“溢出”和“进位”是两个不同含义的概念，不要混淆。如果不太清楚的话，请查阅《计算机组成原理》课程中的有关章节。

### 3.指令指针寄存器

指令指针寄存器IP（X86型CPU）相当于ARM型CPU中的[程序计数器](https://baike.baidu.com/item/程序计数器/3219536)PC，用于控制程序中指令的执行顺序。正常运行时，IP中含有BIU要取的下一条指令（字节）的偏移地址，一般情况下，每从内存中存取一次指令码，IP就自动加1，从而保证指令的顺序执行。IP实际上是指令机器码存放内存单元的地址指针，IP的内容可以被转移类指令（如JMP）强迫改写，以改变程序执行的顺序。

# 第二章

## 2.1 数据传送类指令

### 2.1.1 通用数据传送指令

#### 1.传送指令MOV

#### 2.交换指令XCHG

XCHG中的操作数可以是字也可以是字节，可以在通用寄存器鱼通用寄存器或储存器之间对换数据，但**不能在储存器和储存器之间交换数据**。

```assembly
mov ax , 1234h	;ax = 1234h
mov bx , 5678h	;bx = 5678h
xchg ax , bx	 ;ax = 5678h , bx = 1234h
xchg ah , al	 ;ax =7856h
```



####  3.换码指令XLAT

英文全称：translate

换码指令用于将BX指定的缓冲区中，al指定的位移处的数据取出赋值给al，如：

```assembly
xlat label
xlat	; al <- ds : [ bx + al ]
```

换码指令常用于将一种代码转换为另一种代码：

*使用前，在主存中建立1字节表格，表格内容是要转换成的目的代码，表格的首地址存放于BX寄存器中，需要转换的代码存放于AL寄存器中，要求被转换的代码应是相对表格首地址的位移量。设置好后，执行换码指令，即将AL寄存器的内容转换为目标代码。*

例如：

```assembly
mov bx,100h	;将表首地址给bx
mov al,03	;将表内的偏移地址给al
xlat		;将表内偏移al个位置的数据赋值给al
```



### 2.1.2 堆栈操作指令

#### 1.进栈指令PUSH

​	格式：

```assembly
push r16/m16/seg;压入16位寄存器，16位内存，或段寄存器到栈顶
```



#### 2.出栈指令POP

​	格式：

```assembly
pop r16/m16/seg;弹出栈顶数据到16位寄存器，16位内存，或段寄存器
```

### 2.1.3 标志传送指令

#### 1.标志寄存器传送

##### （1）标志送AH指令LAHF

英语全称：load AH from FLAGS

```assembly
lahf ;从FLAGS中的低四位载入AH中
```

##### （2）AH送标志指令SAHF

英文全称：save AH to FLAGS

```assembly
sahf ;将AH中的数据保存到FLAGS的低四位
```

##### （3）标志进栈指令PUSHF

英文全称：push FLAGS

```assembly
pushf
```

##### （4）标志出栈指令

英文全称：pop FLAGS

```assembly
popf 
```

**注意没有对高位FLAGS的操作，并且对FLAGS的操作载入和保存都是与AH之间进行的。**

#### 2.标志位操作

这一类指令能对CF，DF， IF三个标志为进行设置，除影响去其所设置的标志外，均不影响其他标志。

```assembly
clc ;clear carry，将进位标志CF复位为0
stc ;set carry，将将进位标志CF置位为1
cmc ;carry make change，使CF发生变化
cld ;clear direction，将方向标志DF复位为0
std ;set direction，将方向标志置位为1
cli ;clear interrupt，将中断标志复位为0
sti ;set interrupt，将中断标志置位为1
```

## 2.2 算数运算类指令

### 2.2.1  状态标志

见1.5.1

### 2.2.2 加法指令

#### 1.加法指令ADD

格式：

```assembly
add reg,imm/reg/mem ;
add mem,imm/reg ;
;第一个操作数必然是通用寄存器或内存
```

例子：

```assembly
mov al,0fbh
add al,07h

mov word prt [200h],4652
mov bx,1feh
add al,bl
```

#### 2.带进位加法指令ADC

英文全称：add with carry

格式：

```assembly
adc reg,imm/reg/mem ;
adc mem,imm/reg ;
;第一个操作数必然是通用寄存器或内存
```

例子：

```assembly
mov ax,4652h ;ax=4652h
add ax,0f0f0h ;ax=3742h,CF=1
mov dx,0234h ;dx=0234h
adc dx,0f0f0h ;dx=f325h,CF=0
```

上述程序段完成DX.AX=0234 4652H + F0F0 F0F0H = F325 3742H

#### 3.增量指令INC

英文全称：increase 1

格式：

```assembly
inc reg/mem ;reg/mem <-reg/mem+1
```

该指令不影响CF标志

### 2.2.3 减法指令

#### 1.减法指令SUB

英文全称：substract

格式：

```assembly
sub reg,imm/reg/mem ;
sub mem,imm/reg ;
```

例子：

```assembly
mov al,0fbh ;al=0fbh
sub al,07h ;al=0f4h,CF=0
mov bx,1feh ;bx=1feh
sub al,bl ;al=0f6h,CF=1
```

真值表：待补

设进位为C，其中借位标志CF=C$\bigoplus$​​1

#### 2.带错位减法指令SBB

英文全称：substract with borrow

格式：

```assembly
sbb reg,imm/reg/mem ;
sbb mem,imm/reg ;
```

例子：

```assembly
mov ax,4652h
sub dx,0f0f0h
mov dx,0234h
sbb dx,0f0f0h
```

上述程序段完成DX.AX=0234 4652H - F0F0 F0F0H = F325 3742H

#### 3.减量指令 DEC

英文全称：decrease 1

#### 4.求补指令NEG

英文全称：待补



# 实验

## 2.实验2

```assembly
        .MODEL SMALL
        .STACK
        .DATA
SOURCE  DB 33H,34H,35H,36H
TARGET  DB 80 DUP(?)
        .CODE
        .STARTUP
        MOV SI,OFFSET SOURCE
        MOV DI,OFFSET TARGET
        MOV CX,80
AGAIN1: MOV AL,BYTE PTR [SI]
        MOV [DI],AL
        INC SI
        INC DI
        LOOP AGAIN1
        MOV DI,0
AGAIN2: MOV DL,TARGET[DI]
        MOV AH,02H
        INT 21H
        INC DI
        CMP DI,80
        JB AGAIN2
        .EXIT
        END
```



## 3.实验3

```assembly
MODEL TINY
        .DATA
        .CODE
        .STARTUP
        MOV DX,OFFSET STRING
        MOV AH,09H
        INT 21H
        MOV AH,01H 
        INT 21H 
        MOV AH,02H 
        MOV DL,07H
        INT 21H 
        .EXIT 0
STRING  DB 'PRESS KEY TO CONTINUE $'
        END
```



## 4.实验4

```assembly
TACKSEG    SEGMENT STACK
            DB 256 DUP(?)
STACKSEG    ENDS
DATA1       SEGMENT WORD PUBLIC 'CONST'
CONST1      DW 100
DATA1       ENDS
DATA2       SEGMENT WORD PUBLIC 'VAR'
VAR1        DW ?
DATA2       ENDS
DATAGROUP   GROUP DATA1,DATA2
CALLDOS     EQU <INT 21H>
CODESEG     SEGMENT PARA PUBLIC 'CODE'
            ASSUME CS:CODESEG,DS:DATAGROUP,SS:STACKSEG
START:      MOV AX,DATAGROUP
            MOV DS,AX
            MOV AX,CONST1
            MOV AX,4C00H
            CALLDOS
CODESEG     ENDS
            END START
```



## 5.实验5

```assembly
STACK       SEGMENT WORD STACK 'STACK'
STACK       ENDS
DATA        SEGMENT WORD PUBLIC 'VAR'
VAR1        DB 100
VAR2        DB 200
TOP         EQU THIS WORD
DATA        ENDS
CODE        SEGMENT PARA PUBLIC 'CODE'
            ASSUME CS:CODE,DS:DATA,SS:STACK
START:      MOV AX,DATA
            MOV DS,AX
            MOV BX,OFFSET TOP
            MOV AX,4C00H
            INT 21H
CODE        ENDS
            END START
```



## 6.实验6

```assembly
DSEG        SEGMENT WORD PUBLIC 'DATA'
            ORG 100H
            DW 200 DUP(?)
TOP         EQU THIS WORD
ARRAY       DW 100 DUP(5678H)
DSEG        ENDS
CSEG        SEGMENT PARA PUBLIC 'WORD'
            ASSUME CS:CODESEG,DS:DSEG,SS:DSEG
START:      MOV AX,DSEG
            MOV DS,AX
            MOV SS,AX
            MOV SP,OFFSET TOP
            MOV CX,100
            XOR SI,SI
AGAIN:      PUSH ARRAY[SI]
            INC SI
            INC SI
            LOOP AGAIN
            MOV AX,4C00H
            INT 21H
CSEG        ENDS
            END START
```



## 7.实验7

```assembly
STUDENT     STRUCT
SID         DW ?
SNAME       DB 'ABCDEFG'
MATH        DB 0
ENGLISH     DB 0
STUDENT     ENDS
;定义student结构
```



## 8.实验8

```assembly
            .MODEL SMALL
            .STACK
            .DATA
PERSON      STRUCT
NUMBER      DW 0
PNAME       DB '________'
SEX         DB 0
BIRTHDAT    DB 'MM/DD/YYYY'
FATHER      DB '   '
MOTHER      DB '   '
HAVE_SON    DB ?
PERSON      ENDS
STUDENT     RECORD YEAR:4,SEX:1=0,MARRIAGE:1=1
ZHANG       PERSON <1000B,1,0>
WANG        PERSON <1001B,,0>
ARRAY       PERSON 100 DUP(<>)
            .CODE
            .STARTUP
            MOV BX,OFFSET ARRAY
            MOV AX,1
            SUB SI,SI
            MOV CX,LENGTH ARRAY
            MOV DX,TYPE ARRAY
AGAIN:      MOV [BX+SI].PERSON.NUMBER,AX
            INC AX
            ADD SI,DX
            LOOP AGAIN
            .EXIT
            END
```



# 字典

## 常用表示

### 1.常用字母

1. r：通用寄存器
2. m：内存·
3. imm：立即数
4. reg：通用寄存器
5. seg：段寄存器
6. r8：8位通用寄存器
7. r16：16位通用寄存器
8. r32：32位通用寄存器
9. m8：8位内存
10. m16：16位内存
11. m32：32位内存
12. mem：8/9/10
13. imm8：8位立即数
14. imm16：16位立即数
15. imm32：32位立即数













