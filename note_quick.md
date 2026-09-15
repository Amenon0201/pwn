Kiến trúc máy tính
Mọi con đường giao tiếp máy tính đều dẫn về cpu
4 cổng logic cơ bản: and, or, not, xor (và nhiều phức tạp lẫn nâng cao hơn) -> build nên computer
cpu có register và cache (và cu và alu) giao tiếp (qua trung gian) với memory, disk, network + other
cpu (cache -> register) -> memory -> disk

kiến trúc máy tính tạo từ 3 jonh (1 kỹ sư điện, 1 nhà vật lý và 1 nhà toán học)

~~~#

Assembly = binary
các kiến trúc cpu khác nhau cần bộ assembly khác nhau
intel assembly des cho x86 architec
có 3 loại data: data trực tiếp, data register, data memory


~~~#

tương tác data vào register bằng assembly
register có nhiều loại nhiều mục đích
size 64bit (thấp hơn tùy vào architect)
rax eax ax (ah al)
all partical register x86 (check lại video)

mov rax, 0x539
moving 0x539 vào rax
32-bit caveat (check lại video)

mov rax, 0x539
mov rbx, rax
mov doesn't move, it's copies it instead

extending data
mov eax, -1
movsx rax, eax

register arithmetic (check vid)

speacial: rip, rsp (check vid)

other register (check vid)

~~~#

register chứa data. CPU đặt data vào các thanh ghi và di chuyển chúng giữa các thanh ghi
có khoảng 10 đến 20 loại register phục vụ cho các mục đích phổ biến, và nhiều hơn với các loại chuyên dụng
.s là phần mở rộng phổ biến đối với file assembly (không bắt buộc)

program tương tác với máy tính thông qua syscall (system call)
syscall 60 exit
muốn dùng syscall nào thì nạp số hiệu syscall đó vào rax rồi gọi syscall
mov rax, 60
syscall

có nhiều thanh ghi chứa nhiều tham số
thanh ghi chứa đối số đầu tiên rdi


object file (đuôi o) file của trình biên dịch
dùng as để hợp assembly code thành object file
dùng ld để link object file với executable
.global _start
_start:

mov hoạt động như lệnh copy