# MEMORY

## Signed bit và unsigned bit

Signed bit: bit đầu 1 là âm, 0 là dương.
Quy tắc: Đảo bit và cộng 1
Ví dụ: 
0100 = 4
đảo bit: 1011
1100 = -4

Trong hệ hex, từ 0 đến 7 là dương, 8 đến f là âm.

# Các lệnh assembly

## Disassembling Programs
Dùng lệnh objdump để đảo ngược file executable thành file assembly.

```
objdump -d -M intel /program

Disassembly of section .text:

0000000000401000 <_start>:
  401000:	48 c7 c7 39 05 00 00 	mov    rdi,0x539
  401007:	48 c7 c7 00 00 00 00 	mov    rdi,0
  40100e:	48 c7 c0 3c 00 00 00 	mov    rax,0x3c
  401015:	0f 05                	syscall
```

Lệnh objdump không chuẩn intel syntax nên cần `-M intel`

Lệnh objdump hiển thị các byte thô của memory bên cạnh assembly dưới dạng hexadecimal. Đồng thời data được lưu trong register cũng được biểu diễn dưới dạng hexadecimal.

*Ý nghĩa:* Bằng cách đọc mã, chúng ta có thể tìm ra những thông tin thú vị (liên quan tới bảo mật).
Chẳng hạn như chúng ta biết được rdi chứa data 0x539 trước khi bị set thành 0 trong ví dụ trên.

## Trace syscall

Dùng lệnh `strace` + đường dẫn chương trình sẽ đưa ra mọi syscall đã mà chương trình đã chạy cng với kết quả của nó.

```
strace /tmp/your-program
execve("/tmp/your-program", ["/tmp/your-program"], 0x7ffd48ae28b0 /* 53 vars */) = 0
exit(42)                                 = ?
+++ exited with 42 +++
```

## GDB
Dùng lệnh `gdb` + đường dẫn chương trình để mở trình debug chương trình.
Dùng lệnh `starti` để bắt đầu chạy chương trình.
Dùng lệnh `stepi` hoặc `si` để chạy từng lệnh.
Dùng lệnh `disassemble` để disassembling chương trình.
Dùng lệnh `quit` hoặc `q` để thoát trình debug

tên register trong gdb cần prefix $
Dùng lệnh `print` + tên register để in ra data trong register
Dùng lệnh `set` + tên register để ghi đè data trong register

Đối với các data runtime state như argc, disassembling không đọc được mà cần debug để đọc.
```
pop    rdi          <- reads argc from the stack into rdi
mov    rdi,0x0      <- overwrites rdi with 0!
mov    rax,0x3c
syscall             <- exit(0) --- the secret is gone!
```

Dùng lệnh `x` + tên register để đọc nội dung địa chỉ trong register
x/d để đọc nội dung dưới dạng demical
x/a để đọc nội dung dưới dạng adressing
x/s để đọc nội dung dưới dạng string

Đặt init vào file assembly để làm breakpoint và dùng lệnh `run` hoặc `r` để chạy chương trình tới breakpoint. Có thể truyền tham số bằng cách viết sau lệnh `run`. Có thể đặt input vào gdb bằng cách viết đường dẫn sau lệnh `run < ` + đường dẫn file.

# SYSCALL

## Một số syscall

0 là read: read(int fd, void *buf, size_t count)

1 là write: write(int fd, void *buf, size_t count)

2 là open: open(const char *pathname, int flags)

note: Một số syscall cần hằng số cổ. Chẳng hạn như open cần flag argument để quyết định cách file được mở (O_RDONLY, O_WRONLY, O_RDWR)

## String arguments

String arguments là một chuỗi byte tiếp diễn trong bộ nhớ.

![String_argument](/picture/String_arguments.png)

note: Có thể viết filename vào rsp từng byte một và dùng 0 kết thúc string argument.

## File descriptors (FDs)

FD 0: Standard Input
FD 1: Standard Output
FD2: Standard Error

Ví dụ:
```
mov rdi, 1
mov rsi, [rsp+16]
mov rdx, 1
mov rax, 1
syscall
```
tương đương với write(1, [rsp+16], 1)

note: Kết quả của read (số byte đọc khả dụng) nói riêng và chương trình nói chung thường được reuturn vào rax.

## RIP

Có thể ném string argument vào sau syscall exit để lưu trong memory mà không sợ crash out
```
_start:
...
lea rdi, [rip+path]
...

path:
.asciz "/flag" 
```

note: dùng lea thay vì mov vì open cần adress thay vì data. Syntax rip+path là bỏ qua toàn bộ delta giữa rip và path

## Control flow

### Một số ệnh

Dùng lệnh `jmp` để skip lệnh (bản chất skip bytes)

Conditional jumps:
![Conditional_jumps](/picture/Conditional_jumps.png)

Dùng lệnh `setz` (hoặc `setnz`) để set giá trị register dựa trên ZF.
 
### Register Rflags

Register Rflags dùng để lưu trạng thái conditional.

Một số flag quan trọng:

Carry Flag: carry bit vượt giới hạn (unsigned)
Zero Flag: bit có bằng 0 không
Overflow Flag: Tương tự CF (signed)
Signed Flag: bit cao nhất

Một số pattern:

sub: thực hiện phép trừ, lưu kết quả vô register, cập nhật flag
cmp: thực hiện phép trừ, cập nhật flag
test: thực hiện phép AND, cập nhật flag

note: Có thể thực hiện loop
```
mov rax, 0
LOOP_HEADER:
inc rax
cmp rax, 10
jb LOOP_HEADER
```