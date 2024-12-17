Notes on a simple x86 assembly program

- - -

```bash
sudo apt install nasm
```

The first 2 lines of code will define the entry point for our program

```asm
global _start
_start: 
```

- The **global** keyword is used to make an identifier accessible to the linker
- Identifier followed by a column (`_start:`) will create a label
- Labels are used to name locations in our code

```asm
mov eax, 1
```

- This is a `move` instruction.
- In the example above, we `move` the `integer 1` into the `general purpose` register `eax`

```asm
mov ebx, 42
```

- Similarly, we will move 42 into the register `ebx`

```asm
int 0x80
```

- Finally, this line, perform an `interrupt`
- This means the processor will transfer control to an `interrupt handler` that we specified by the following value (`0x80`).
- We're using the hex value 80 which is the interrupt handler for `system calls`
- The `system call` that it makes is determined by the `eax` register

- - -

In our case 

```asm
global _start
_start:
    mov eax, 1
    mov ebx, 42
    int 0x80
```

- the value 1, means we're making an exit system call, that means it signals the end of our program.
- the value stored in `ebx` will be the exit status for a program, we used 42 but it can be any integer.

In order to compile with `nasm` we run 

```bash
nasm -f elf32 ex1.asm -o ex1.o
```

- We build an elf32 object file.
- elf32 = executable and linking format and it's the executable format used by linux

Next we will build an executable from that object file

```bash
ld -m elf_i386 ex1.o -o ex1
```

Similarly we pass de the `-m` flag to specify it's an x86 elf program.
Then we simply run our program

```bash
./ex1
echo $?
42
```

- - - 

By adding another line we can perform a subtraction

```asm
sub ebx, 29
```

- This line will substract 29 from 42 stored into the `ebx` register
- The subtraction is done in place, therefore in alters `ebx` meaning the exit status is now 13

```asm
global _start
_start:
    mov eax, 1
    mov ebx, 42
    sub ebx, 29
    int 0x80
```

```bash
nasm -f elf32 ex1.asm -o ex1.o
ld -m elf_i386 ex1.o -o ex1
./ex1
echo $?
13
```