
# GBDev Cheatsheet
## Table of contents

<!-- MDTOC maxdepth:8 firsth1:0 numbering:1 flatten:0 bullets:0 updateOnSave:1 -->

1. [Table of contents](#table-of-contents)   
2. [COMPILER INSTRUCTIONS](#compiler-instructions)   
&emsp;2.1. [Compiler REPEAT](#compiler-repeat)   
&emsp;2.2. [Finishes REPEAT instruction.](#finishes-repeat-instruction)   
&emsp;2.3. [Section declaration](#section-declaration)   
&emsp;2.4. [Global label](#global-label)   
&emsp;2.5. [Local label](#local-label)   
3. [OPCODES](#opcodes)   
&emsp;3.1. [Interrupt operations](#interrupt-operations)   
&emsp;&emsp;3.1.1. [Disable interrupts](#disable-interrupts)   
&emsp;&emsp;3.1.2. [Enable interrupts](#enable-interrupts)   
&emsp;&emsp;3.1.3. [Static value](#static-value)   
&emsp;3.2. [Flow control operations](#flow-control-operations)   
&emsp;&emsp;3.2.1. [Jump](#jump)   
&emsp;&emsp;3.2.2. [Jump relative](#jump-relative)   
&emsp;&emsp;3.2.3. [Function call](#function-call)   
&emsp;&emsp;3.2.4. [Return to previous point](#return-to-previous-point)   
&emsp;&emsp;3.2.5. [Return to previous point and enable interrupts](#return-to-previous-point-and-enable-interrupts)   
&emsp;3.3. [Data management](#data-management)   
&emsp;&emsp;3.3.1. [Load](#load)   
&emsp;&emsp;3.3.2. [Push](#push)   
&emsp;&emsp;3.3.3. [Pop](#pop)   
&emsp;3.4. [Aritmetic operations](#aritmetic-operations)   
&emsp;&emsp;3.4.1. [Compares a and <value> and sets flags](#compares-a-and-value-and-sets-flags)   
&emsp;3.5. [Ors something with something](#ors-something-with-something)   
4. [Interrupts](#interrupts)   

<!-- /MDTOC -->

-----
## COMPILER INSTRUCTIONS
-----
### Compiler REPEAT
> Repeats code for a memory range.
> - `<offset to repeat>`

```RGBASM
	REPT <high memory direction> - <low memory direction>
```

### Finishes REPEAT instruction.
> Tells the compiler to stop repeating.

```RGBASM
    ENDR
```

### Section declaration
> Declares a section to the compiler.
> - ``<name string>``, ``<memory area>``

```RGBASM
	SECTION <name string>, <mem area>
```

### Global label
> Declares a global label for the execution to jump to.

```RGBASM
	<global label>:
```

### Local label
> Declares a local label for the execution to jump to, only works inside a global label.

```RGBASM
	.<local label>
```

----
OPCODES
---
----

### Interrupt operations

#### Disable interrupts
> Disables all interrupts until `ei` is called.
> - no parameters

```RGBASM
	di
```

#### Enable interrupts
> Enables interrupts according to the `IE` register until `di` is called.
> - no parameters

```RGBASM
	ei
```

#### Static value
> Compiler instruction, can use strings as value can place a comma and another `<value>`>.
> - ``<value>``, ``[<value>]``

```RGBASM
	db <value>, [<value>]
```

### Flow control operations

#### Jump
> Jumps flow of the program to an address.
> - ``<condition>``, ``<address>``

| conditions | Explanation |
|------------|-------------|
| C			 | (carry)	   |
| NC 	     | (no carry)  |
| Z          | (zero)      |
| NZ         | (not zero)  |

```RGBASM
    jp [<condition>], <address>
```

#### Jump relative
> Jumps flow of the program to an address relative to the current.
> - ``[<condition flag> if not present always jumps]``, ``<address to jump>``

| conditions | Explanation |
|------------|-------------|
| C			 | (carry)	   |
| NC 	     | (no carry)  |
| Z          | (zero)      |
| NZ         | (not zero)  |

```RGBASM
   jr [<condition flag>], <address>
```

#### Function call
> Calls a function by jumping to an address and pushes the current address + 1 to the stack.
> - ``[<condition flag> if not present always jumps]``, ``<function name>``

| conditions | Explanation |
|------------|-------------|
| C			 | (carry)	   |
| NC 	     | (no carry)  |
| Z          | (zero)      |
| NZ         | (not zero)  |

```RGBASM
    call [<condition flag>], <function name>
```

#### Return to previous point
> Returns execution flow to the previous point by popping the stack and jumping there. To be used after a call statement.
> - ``[<condition flag> if not present always jumps]``

| conditions | Explanation |
|------------|-------------|
| C			 | (carry)	   |
| NC 	     | (no carry)  |
| Z          | (zero)      |
| NZ         | (not zero)  |

```RGBASM
    ret [<condition flag>]
```

#### Return to previous point and enable interrupts
> Returns execution flow to the previous point by popping the stack and jumping there and then enables system interrupts again. To be used after a call statement.
> - ``[<condition flag> if not present always jumps]``

| conditions | Explanation |
|------------|-------------|
| C			 | (carry)	   |
| NC 	     | (no carry)  |
| Z          | (zero)      |
| NZ         | (not zero)  |

```RGBASM
    reti [<condition flag>]
```

### Data management

#### Load
> Loads the contents of `<source>` into `<destination>`.
> - ``<destination>``, ``<source>``

```RGBASM
    ld <destination>, <source>
```

#### Push
> Pushes a 16 bits register contents to the stack.  
> SLOOOOOOOW
> - ``<register>``
```RGBASM
	push <hl/de/bc>
```

#### Pop
> Pops the stack value to a 16 bits register.  
> SLOOOOOOOW
> - ``<register>``
```RGBASM
	pop <hl/de/bc>
```

### Aritmetic operations

#### Compares a and <value> and sets flags
- ``<value>``

	| Flag		 | Condition it proves |
	|------------|---------------------|
	| C reset    | a >= ``<value>``    |
	| C set      | a < ``<value>``     |
	| Z reset    | a != ``<value>``    |
	| Z set      | a == ``<value>``    |

	```RGBASM
	cp <value>
	```

### Ors something with something
- ``[<destination> if not present a]``, ``<value>``
```RGBASM
    or [<destination>], <value>
	```

### Xors something with something
- ``[<destination> if not present a]``, ``<value>``
```RGBASM
    xor [<destination>], <value>
	```

### Ands something with something
- ``[<destination> if not present a]``, ``<value>``
```RGBASM
    and a
	```

### Increment
- Increments by 1, can be used with 16 bit registers
- ``<destination>``
```RGBASM
    inc <destination>
	```

### Decrement
- Decrements by 1, can be used with 16 bit registers
- ``<destination>``
```RGBASM
    dec <destination>
	```

### Add
- Adds something to something, can be used with 16 bit registers
- ``[<destination> if not present a]``, ``<value>``
```RGBASM
    add [<destination>], <value>
	```

### Add with carry
- Adds something to something with carry
- ``[<destination> if not present a]``, ``<value>``
```RGBASM
    adc [<destination>], <value>
	```

### Substract
- Substracts something to something
- ``[<destination> if not present a]``, ``<value>``
```RGBASM
    sub [<destination>], <value>
	```

### Substract with carry
- Substracts something to something with carry
- ``[<destination> if not present a]``, ``<value>``
```RGBASM
    sbc [<destination>], <value>
	```

### Rotate accumulator left with carry
- Rotates register a left one bit, placing bit 7 at bit 0 and in the carry flag
```RGBASM
	rlca
	```

### Rotate left with carry
- Rotates any register left one bit, placing bit 7 at bit 0 and in the carry flag  
- ``<value (register or [hl])>``
```RGBASM
	rlc <value>
	```

### Rotate accumulator left
- Rotates register a left one bit, placing bit 7 at in the carry flag and the carry flag in bit 0
```RGBASM
	rla
	```

### Rotate left
- Rotates any register left one bit, placing bit 7 at in the carry flag and the carry flag in bit 0  
- ``<value (register or [hl])>``
```RGBASM
	rl <value>
	```

### Rotate accumulator right with carry
- Rotates register a right one bit, placing bit 0 at bit 7 and in the carry flag
```RGBASM
	rrca
	```

### Rotate right with carry
- Rotates any register right one bit, placing bit 0 at bit 7 and in the carry flag  
- ``<value (register or [hl])>``
```RGBASM
	rrc <value>
	```

### Rotate accumulator right
- Rotates register a right one bit, placing bit 0 at in the carry flag and the carry flag in bit 7
```RGBASM
	rra
	```

### Rotate right
- Rotates any register right one bit, placing bit 0 at in the carry flag and the carry flag in bit 7
- ``<value (register or [hl])>``
```RGBASM
	rr <value>
	```

### Shift left arithmetically
- Shift lefts 1 bit, placing 0 into bit 0 and placing bit 7 in the carry flag
- ``<value (register or [hl])>``
```RGBASM
	sla <value>
	```

### Shift right arithmetically
- Shift right 1 bit, leaving bit 7 untouched and placing bit 0 in the carry flag
- ``<value (register or [hl])>``
```RGBASM
	sra <value>
	```

### Shift right logically
- Shift right 1 bit, placing 0 into bit 7 and placing bit 0 in the carry flag
- ``<value (register or [hl])>``
```RGBASM
	srl <value>
	```

## Bit test
- Copies the bit ``<b>`` in ``<value (register or [hl])>`` inverted to the Z flag
```RGBASM
	bit <b>, <value>
	```

## Bit set
- Sets the bit ``<b>`` in ``<value (register or [hl])>`` to 1
```RGBASM
	set <b>, <value>
	```

## Bit reset
- Resets the bit ``<b>`` in ``<value (register or [hl])>`` to 0
```RGBASM
	res <b>, <value>
	```

-----
Syntax
---
-----

| Symbol | Meaning |
|------------|-------------|
|	[]  |pointer|
|	$   |hex|
|	%   |bin|
|	""  |string|
|	i   |increment register|
|	d	|decrement register|

-----
Registers
---
-----

| Register | Use |
|------------|-------------|
	a | 8 bit register accumulator
	h | 8 bit register
	l | 8 bit register
	d | 8 bit register
	e | 8 bit register
	b | 8 bit register
	c | 8 bit register
	f | 8 bit register for flags
	hl | h + l as a 16 bit register
	de | d + e as a 16 bit register
	bc | b + c as a 16 bit register
	af | a + f as a 16 bit register, special use
	sp | Stack register
	pc | Program register, contains the next instruction to be executed
	IF | Interrupt flag register
	IE | Interrupt enable register
	rLCDC | LCD control byte. uses masks to control how the LCD behaves
	rLY | Current y coordinate for screen rendering above 144 is VBlank
	rBGP | Background color pallete
	rSCY | Screen scroll Y position
	rSCX | Screen scroll X position
	rNR52 | rAUDENA = audio enable/disable

-----
Memory areas
---
-----

|Begins at	|Ends at		|Name
|------------|-------------|----------|
|$0000		|$7FFF		|ROM|
|$0000		|$3FFF		|ROM0|
|$4000		|$7FFF		|ROMX|
|$8000		|$9FFF		|VRAM|
|$A000		|$BFFF		|SRAM|
|$C000		|$DFFF		|WRAM|
|$E000		|$FDFF		|Echo RAM|
|$FE00		|$FE9F		|OAM|
|$FEA0		|$FEFF		|"FEXX"|
|$FF00		|$FF7F		|IO|
|$FF80		|$FFFE		|HRAM|

-----
Documentation example
---
-----

```RGBASM
; Copy a string up to but not including the first NUL character
; @param de A pointer to the string to be copied
; @param hl A pointer to the beginning of the destination buffer
; @return de A pointer to the byte after the source string's terminating byte
; @return hl A pointer to the byte after the last copied byte
; @return a Zero
; @return flags C reset, Z set
Strcpy:
    ld a, [de]
    inc de
    and a
    ret z
    ld [hli], a
    jr Strcpy
```
-----
Interrupts
---
-----

|Bit in IF	|Interrupt name	|Handler address	|Notes|
|------------|-------------|----------|---------|
0 ($01)		|VBlank			|$0040			|highest priority
1 ($02)		|STAT			|$0048			|also called LCD or sometimes abusively LCDC
2 ($04)		|Timer			|$0050
3 ($08)		|Serial			|$0058
4 ($10)		|Joypad			|$0060			|lowest priority; rarely used
