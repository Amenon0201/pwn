RIP (instruction pointer) chứa địa chỉ của instruction tiếp theo. Trong x86-64 thông thường không thể dùng mov để trực tiếp đọc/ghi RIP như một general-purpose register; nó thay đổi thông qua các instruction điều khiển luồng như jmp, call, ret, sysret,...

movsx không phải cách duy nhất để "đưa register nhỏ thành register lớn". Nó specifically thực hiện sign extension.

RSP (stack pointer) chứa địa chỉ đỉnh hiện tại của stack.

Stack không phải một vùng register riêng. Stack thực chất là một vùng memory, còn RSP chỉ là register dùng để trỏ tới vị trí hiện tại trên stack.

_start là symbol thường được linker sử dụng làm entry point của một Linux executable khi link chương trình assembly thủ công.

Một process có thể có rất nhiều memory mapping khác:

.text
.rodata
.data
.bss
heap
stack
shared libraries
mmap regions
vdso
...

Một layout đơn giản có thể mô tả:

High address
+------------------+
|      stack       |
|        ↓         |
+------------------+
|                  |
|   mmap/shared    |
|     libraries   |
|                  |
+------------------+
|        ↑         |
|      heap        |
+------------------+
|      .bss        |
|      .data       |
|     .rodata      |
|      .text       |
+------------------+
Low address

Không nên coi đây là layout tuyệt đối của mọi executable vì Linux process layout còn phụ thuộc ASLR, loader, architecture, executable type...

Không nên hiểu là lea "đặc biệt được phép truy cập RIP như một operand register thông thường". lea có thể sử dụng RIP trong RIP-relative addressing để tính địa chỉ.