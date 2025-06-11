# Tutorial on Assembly Language Programming

### Step 1: Install NASM and Build Tools
Open a terminal and run the following commands to install NASM and the required build tools:
```bash
sudo apt update
sudo apt install nasm build-essential
```

### Step 2: Create the Assembly File
Create a file name `hello.asm`
```bash
section .data
    msg db "Hello, Linux!", 0xA    ; message + newline
    len equ $ - msg                ; calculate length of message

section .text
    global _start

_start:
    ; write(1, msg, len)
    mov rax, 1       ; syscall number for write
    mov rdi, 1       ; file descriptor (stdout)
    mov rsi, msg     ; address of the message
    mov rdx, len     ; length of the message
    syscall

    ; exit(0)
    mov rax, 60      ; syscall number for exit
    xor rdi, rdi     ; status code 0
    syscall
```

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

