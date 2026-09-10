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

![các phần của register](/register.png)

Có thể thực hiện các phép toán và phép logic với register nếu có data bên trong chúng.

Một số register đặc biệt:
- rip: chứa địa chỉ của lệnh tiếp theo được thực thi (không thể read hay write trực tiếp)
- rsp: chứa địa chỉ của vùng nhớ dữ liệu tạm thời (cẩn thận)

Ngoài ra, x86 architecture còn có nhiều register khác để dùng cho: operating system, floating point, xử lý data lớn (512 zmm register),...

# Assembly
Assembly (hợp ngữ) là ngôn ngữ bậc thấp gần nhất với machine code (mã máy). Assembly thông qua quá trình assembling (dịch hợp ngữ) thành machine code:

'Assembly -> Assembler (Trình dịch hợp ngữ) -> Machine code'

Các kiến trúc CPU khác nhau cần bộ assembly khác nhau. Đối với x86 (và x86_64) thì dùng Intel assembly (phổ biến hơn) và AT&T assembly.

