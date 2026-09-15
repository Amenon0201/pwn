# Computer architecture
## CPU
CPU là trung tâm xử lý dữ liệu của máy tính. Có thể ví nó như bộ não của máy tính. Mọi hoạt động và tương tác của máy tính đều đi từ input tới CPU xử lý rồi đưa ra output.

CPU có cấu tạo chủ yếu gồm:
- Register: Lưu trữ để CPU truy cập nhanh
- CU (Control Unit): thực thi và điều khiển hoạt động trong CPU
- ALU (Arithmetic Logic Unit): thực hiện các phét toán số học và thao tác với bit (bitwise opertation)

Một bộ phận có khả năng lưu trữ càng nhỏ thì có tốc độ xử lý càng nhanh (CPU truy cập càng nhanh): Register > RAM > Disk (tốc độ xử lý)

## Register
Register có khả năng lưu trữ rất ít nhưng lại có tốc độ xử lý rất nhanh, được CPU sử dụng để lưu trữ tạm thời để xử lý dữ liệu.

Trong một máy tính có khoảng từ 10 đến 20 register để dùng cho các mục đích phổ biến, và có thể thêm nhiều hơn khoảng một chục đối với máy tính chuyên dụng

## RAM
RAM là bộ nhớ khả biến của máy tính, dùng để lưu trữ dữ liệu tạm thời trong quá trình máy tính hoạt động. Dữ liệu trong RAM sẽ mất đi sau khi tắt má

## Disk
Disk nơi có kích thước lưu trữ lớn nhất, nhưng cũng có tốc độ xử lý chậm nhất. Khác với RAM, dữ liệu trong disk không bị mất đi khi tắt máy. Disk được dùng làm nơi lưu trữ dữ liệu lâu dài.

Disk có hai loại chính:
- HDD (Hard Disk Drive): Sử dụng bộ phận cơ học để lưu trữ và truy xuất dữ liệu. Dễ bị hỏng khi va đập. Giá thành rẻ.
- SSD (Solid State Drive): Sử dụng mạch điện để lưu trữ và truy xuất dữ liệu. Khó bị hỏng hơn và có tốc độ truy xuất nhanh hơn HDD. Giá thành đắt hơn HDD

# Register

Register thường có chung kích thước với kích thước của architecture. Ví dụ: x86_64 có register 64 bits (8 bytes)

Register có thể acess từng phần.

![các phần của register](/picture/register.png)

Có thể thực hiện các phép toán và phép logic với register nếu có data bên trong chúng.

Một số register đặc biệt:
- rip: chứa địa chỉ của lệnh tiếp theo được thực thi (không thể read hay write trực tiếp)
- rsp: chứa địa chỉ của vùng nhớ dữ liệu tạm thời (cẩn thận)

Ngoài ra, x86 architecture còn có nhiều register khác để dùng cho: operating system, floating point, xử lý data lớn (zmm register 512 bit),...

# Memory layout
## Kiến thức chung
Memory có hai phần chính là text và data. Data được chia làm hai phần là static và dynamic. Static có thể chia làm hai phần là initialize và uninitialized. Dynamic được chia làm hai phần gồm heap và stack.

![memory layout](/picture/memory_layout.png)

Memory:
- Text (hay còn gọi là code segment): lưu trữ lệnh thực thi chương trình. Text segment read only, sharable và fixed size.
- Data:
    - Static:
        - Initialized (DS): lưu trữ statics và global variable đã được khởi tạo giá trị, có read/write accesible và fixed size.
        - Uninitialized (BSS): lưu trữ statics và global variable chưa được khởi tạo giá trị hoặc được khởi tạo giá trị băng 0, có read/write accesible và fixed size.
    - Dynamic:
        - Heap: Có dynamic size và có thể điều chỉnh size qua các lệnh như malloc, free, new, delete,... Thường dùng cấp phát bộ nhớ động cho các dạng dữ liệu động như danh sách liên kết. Sau khi dùng xong mà không release bộ nhớ có thể gây ra lỗi memory leaks và các memory errors khác
        - Stack: Lưu trữ function call, input arguments và local varialbe. Có cơ chế LIFO (Last In First Out). Dùng push và pop để đẩy dữ liệu vào và lấy dữ liệu ra. Address của stack được lưu trong register rsp.

## Stack
Chúng ta có thể push register và immediates vào stack và pop ra register.

```
mov rax, 0xc001ca75
push rax
pop rbx
```
Qua đoạn mã trên rbx có value 0xc001ca75. Đồng thời tương tự mov, push chỉ copy chứ không di chuyển dữ liệu.

## LSB và MSB
LSB (Least Significant Bit) là bit nằm ngoài cùng bên phải của một dãy bit, là bit có giá trị nhỏ nhất. Thay đổi bit này chỉ thay đổi giá trị số đó một lượng rất nhỏ.

MSB (Most Significant Bit) là bit nằm ngoài cùng bên trái của một dãy bit, là bit có giá trị lớn nhất. Thay đổi bit này sẽ làm thay đổi giá trị số đó rất nhiều.

## Little Endian và Big Endian

Kiến trúc x86_64 sử dụng Little Endian: Lưu trữ ngược. Little Endian lưu byte thấp nhất ở địa chỉ nhỏ nhất. Little Endian thay đổi thứ tự byte nhưng không thay đổi thứ tự bit.

![Endianess](/picture/Endianess.png)

Ngược lại, Big Endian lưu byte có trọng số cao nhất ở địa chỉ nhỏ nhất, giống như cách viết số từ trái sang phải của con người.

# Assembly
## Kiến thức chung
Assembly (hợp ngữ) là ngôn ngữ bậc thấp gần nhất với machine code (mã máy). Assembly thông qua quá trình assembling (dịch hợp ngữ) thành machine code:

`Assembly -> Assembler (Trình dịch hợp ngữ) -> Machine code`

Các kiến trúc CPU khác nhau cần bộ assembly khác nhau. Đối với x86 (và x86_64) thì dùng và AT&T assembly Intel assembly (phổ biến hơn).

Đuôi phổ biến của file assembly là '.s' hoặc '.S'. Đuôi của file object là '.o'.

## Một số lệnh

Lệnh mov (move):
- mov rax, 0x539: đưa giá trị 0x539 vào rax
- mov rbx, rax: đưa giá trị trong rax vào rbx
- mov rbx, [rax]: trong trường hợp rax chứa một address, đưa giá trị tại address bên trong rax vào rbx

Lệnh mov hoạt động như copy, nó không xóa đi dữ liệu cũ ở vị trí cũ mà chỉ sao chép dữ liệu sang vị trí mới.

*Lưu ý:* nếu như write một register 32 bit sẽ zero-out toàn bộ phần còn lại của register. Đây là điểm đặc trưng của kiến trúc x86_64.

Ví dụ:
Mov register 16 bit
```
mov rax, 0xffffffffffffffff
mov ax, 1111
kết quả: rax = 0xffffffffffff1111
```

Mov register 32 bit
```
mov rax, 0xffffffffffffffff
mov eax, 11111111
kết quả: rax = 0x0000000011111111
```

Lệnh movsx (move with sign extension): hoạt động tương tự mov nhưng copy sign bit (bit có trọng số lớn nhất dùng để xác định âm hay dương) để mở rộng kích thước.

Ví dụ:
Không dùng movsx
```
mov rax, 0xffffffffffffffff
mov eax, 0xffffffff
kết quả: rax = 0x00000000ffffffff
```

Dùng movsx
```
mov rax, 0xffffffffffffffff
mov eax, 0xffffffff
movsx rax, eax
kết quả rax, 0xffffffffffffffff
```

## Syscall và Build Executables
Program tương tác với máy tính thông qua syscall (system call). Muốn dùng syscall nào thì nạp số hiệu syscall đó vào rax rồi gọi syscall.

Ví dụ: 60 là exit program
```
mov rax, 60
syscall
```

Lệnh as dùng để dịch assembly file thành object file. Lệnh ld để link object file với executable (tệp thực thi). Có thể link nhiều object file với một executable cùng lúc.

Ví dụ:
```
as helloWorld.s -o helloWorld.o
ld helloWorld.o -o helloWorld
./helloWorld
```

Dòng lệnh .intel_syntax noprefix (viết trong file assembly) cho assembler biết rằng chúng ta dùng syntax của Intel assembly và không cần bổ sung tiền tố trước các lệnh ('%' trước các register). Lệnh này không nằm trong kiến trúc x86 nên nó không được dịch qua executable.

Trong the shell 'echo $?' lấy exit code cuối của executed command.

## Start - điểm bắt đầu của chương trình

Khi thực hiện ld có thể sẽ hiện thông báo warning 'entry symbol _start'. Tức thiếu start symbol, một thứ để xác định điểm bắt đầu chương trình. Không có nó, chương trình sẽ bắt đầu từ đầu executable.

Xác định _start symbol:
```
.global _start
_start:
```

Trong đó `_start:` là label đánh dấu nơi bắt đầu. `.global _start` giúp _start symbol visible ở linked level, tức hoạt động cả ở executable thay vì chỉ ở object file.

## Address caculator and RIP

Có thể thực hiện vài lệnh tính toán đối với địa chỉ memory:

Sử dụng rax làm index để tính địa chỉ memory.
```
mov rax, 0
mov rbx, [rsp+rax*8] //đọc phần tử nằm trên cùng của stack tại rsp
inc rax //tăng rax lên 1
mov rbx, [rsp+rax*8] //đọc phần tử nằm kế tiếp cái trên cùng của stack
```

Sử dụng lệnh lệnh lea (Load Effective Address) để lấy địa chỉ thay vì data trong địa chỉ
```
mov rax, 0
lea rbx, [rsp+rax*8] //rbx chứa địa chỉ của đầu stack
mov rbx, [rbx] //rbx chứa dữ liệu đầu stack
```

lea là một trong số ít lệnh có thể access trực tiếp tới register rip
```
lea rax, [rip] //load địa chỉ của lệnh tiếp theo
lea rax, [rip+8] //load địa chỉ của lệnh tiếp theo cộng thêm 8 bytes
```

Đồng thời vẫn có thể dùng mov để đọc hoặc ghi trực tiếp
```
mov rax, [rip] //đọc dữ liệu từ địa chỉ tương đối rip
mov [rip], rax //ghi đè lệnh tiếp theo
```

Ta có thể ghi immediate value trực tiếp vào memory
```
mov rax, 0x133337
mov DWORD PTR [rax], 0x1337 //ghi trực tiếp value 0x1337 vào địa chỉ 0x133337
```
Tùy thuộc vào assembler mà có thể viết DWORD thay vì DWORD PTR

BYTE: 1 byte
WORD: 2 bytes
DWORD: 4 bytes
QWORD: 8 bytes