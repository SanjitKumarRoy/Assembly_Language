# Tutorial on Assembly Language Programming

## Run `Assembly Program`
### Step 1: Install NASM and Build Tools
Open a terminal and run the following commands to install `NASM` and the required build tools:
```bash
sudo apt update
sudo apt install nasm build-essential
```

### Step 2: Create the Assembly File
Create a file name `hello.asm`


```bash
section .data                 ; Data section — used for initialized data like strings
    msg db "Hello, Linux!", 0xA  ; Define a string followed by newline (0xA = LF = '\n')
    len equ $ - msg              ; Compute length of the string: current position ($) - start of msg

section .text                ; Code section — where the actual program instructions go
    global _start            ; Let the linker know that the entry point is called _start

_start:                      ; Entry point of the program

    ; --- write(1, msg, len) syscall ---
    mov rax, 1               ; syscall number for write (Linux x86-64)
    mov rdi, 1               ; first argument: file descriptor 1 (stdout)
    mov rsi, msg             ; second argument: address of the message to print
    mov rdx, len             ; third argument: number of bytes to write
    syscall                  ; make the system call (invoke write)

    ; --- exit(0) syscall ---
    mov rax, 60              ; syscall number for exit
    xor rdi, rdi             ; exit code 0 (xor rdi, rdi is a fast way to set rdi = 0)
    syscall                  ; make the system call (terminate the program)
```

### Step 3: Assemble and Link the Program
```bash
nasm -f elf64 hello.asm -o hello.o
ld hello.o -o hello
```

### Step 4: Run the Program
```bash
./hello
```

## 🐞 Debug with GDB
### 1. Assemble and Link with Debug Info:
```bash
nasm -f elf64 -g main.asm -o main.o
ld main.o -o main
```
### 2. GDB command:
| Action                       | GDB Command              |
|-----------------------------|--------------------------|
| Start GDB                   | `gdb ./main`         |
| Set breakpoint              | `break _start`           |
| Run program                 | `run`                    |
| Step one instruction        | `stepi`                  |
| Show all registers          | `info registers`         |
| Print register value (e.g. `al`) | `print $al`             |
| Show memory value at `A`    | `x/b &A`                 |
| Show memory address of `A`  | `info address A`         |
---

## 📄 Example Code Snippet (`main.asm`)

```nasm
section .data
    A db 2
    B db 3

section .text
    global _start

_start:
    mov al, [A]
    add al, [B]
    mov rax, 60
    xor rdi, rdi
    syscall
```


## Register Naming by Bit Width
|64-bit|32-bit|16-bit|8-bit|Notes|
|------|------|------|-----|-----|
|`rax`|`eax`|`ax`|`al`|Accumulator|
|`rbx`|`ebx`|`bx`|`bl`|Base Register|
|`rcx`|`ecx`|`cx`|`cl`|Loop Counter|
|`rdx`|`edx`|`dx`|`dl`|Data Register|
|`rsi`|`esi`|`si`|`sil`|Source Index|
|`rdi`|`edi`|`di`|`dil`|Destination Index|
|`rsp`|`esp`|`sp`|`spl`|Stack Pointer|
|`rbp`|`ebp`|`bp`|`bpl`|Base Pointer|
|`r8`|`r8d`|`r8w`|`r8b`|Extended General<br>Purpose Registers|
|...|...|...|...|Up to `r15`|
---
💡 The `r8` – `r15` registers are only available in 64-bit mode.


