# I/O Systems
---
## Giới thiệu
1 trong những vai trò quan trọng OS là quản  trong hoạt động IO - điều kiến tác vụ IO, các thiết bị IO, quản lý và điều khiển các thao tác nhập/xuất và các thiết bị nhập/xuất.

__Các khái niệm cơ bản__
- Kiểm soát thiết bị kết nối PC là 1 nội dung chính của OS.
- IO device gồm nhiều loại (mouse, hdd, ..)
- Phương pháp để quản lý là tạo IO subsystem trong kernel, phân tách kernel khỏi sự phức tạp IO devices
- Để tăng tính linh hoạt OS, sử dụng device drivers cung cấp các phương thức cho phép OS giao tiếp thiết bị

Các thành phần phần cứng nhập/xuất cơ bản như cổng, bus và bộ điều khiển thiết bị chứa trong một dãy rộng các
thiết bị nhập/xuất.

## I/O Hardware
Device kết nối PC = gửi tín hiệu thông qua dây hoặc air. Để device kết nối PC => cần có connection point hoặc port. Nếu device chia sẽ = tập dây, kết nối gọi là bus. __BUS__ là tập dây và giao thức xác định, chỉ rõ các message có thể được gửi qua wires.

__Bus__ thường được sử dụng trong kiến trúc PC, với tốc độ, thông lượng, phương thức kết nối khác nhau.
- __PCI bus__ – kết nối vi xử lý với __memory subsystem__ với tốc độ cao
- __expansion bus__ – kết nối thiết bị chậm như keybroard User port.
- __Small Computer System Interface (SCSI)__ - Kết nối tới __SCSI controller__
- 1 số mở rộng __PCI Express (PCIe), HyperTransport__ tốc độ từ 16-25 gb/s

__Controller__ là tập các các linh kiện điện tử, hoạt động trên port, bus, device. serial-port controller is a simple device controller.

__Controller__ có 1 hoặc nhiều thanh ghi cho việc lưu __data__, kiểm soát signal. __Processor__ kết nối với __controller__ = đọc ghi các bit lên thanh ghi. Giao tiếp xảy ra = sử dụng các __chỉ thị IO__ xác định, truyền các byte hoặc word tới __IO port address__. __IO instruction__ kích hoạt bus lines để lựa chọn thiết bị mong muốn, chuyển bit tới device register.

<div style="text-align:center"> <img src=https://raw.githubusercontent.com/lacoski/khoa-luan/master/Filesystem/PIC/io-sys-1.png></div>

__Device controller__ có thể hỗ trợ __memory-mapped I/O__. Trong trường hơp __device-control registers__ được map tới không gian địa chỉ __processor__. CPU thực thi __IO request__ bằng __standard data-transfer instructions__ cho hoạt động đọc ghi __device-control registers__ đã mapped tới vị trí __physical memory__.

__I/O port__ bao gồm 4 loại thành ghi, sử dụng thể hiện status, control, data-in, data-out
- Data-in register = sử dụng khi host nhập input
- Data-out register = đọc bởi host khi gửi kết quả
- Status register = trạng thái của cmd, lỗi, .. thể hiện bởi các bit
- Control register = ghi bởi host khi bắt đầu cmd hoặc thay đổi trạng thái device.

### __Poling - Tìm kiếm, tham dò__

Giao thức tương tác giữ host và controller rất phức tạp, ta có thể hiểu đơn giản qua khái niệm bắt tay.

Giả sử 2 bits dùng để thể hiện quan hệ người cung cấp, người tiêu thụ (giữa controller và host). Controller thể hiện trạng thái thông qua busy bit trong status register. Controller set trạng busy bit khi nó đang thực hiện công việc khác và xóa bit này khi nó sẵn sàng thực hiện.

Host phát tín hiệu mong muốn sử dụng = __command-ready bit__ trong __command register__. Host thiết lập __command-ready bit__ khi commad có sẵn cho controller thực hiện.

Quá trình:
- 1) host lặp quá trình đọc busy bit cho đến khi bit ko tồn tại
- 2) host thiết lập write bit trong command = register và ghi byte vào data-out register.
- 3) Host thiết lập command-ready bit
- 4) Khi controller nhận thấy command-ready bit được set, nó thiết lập busy bit
- 5) Controller đọc command register và thấy write cmd. Nó sẽ đọc data-out register để lấy byte, thực hiện IO tói device
- 6) Controller xóa command-ready bit, xóa error bit trong status register để chỉ ra device I/O hoàn thành, xóa busy bit thể hiện tiến trình hoàn thành

### __Ngắt__

<div style="text-align:center"> <img src=https://raw.githubusercontent.com/lacoski/khoa-luan/master/Filesystem/PIC/io-sys-2.png></div>

Cơ chế hoạt đọng:
- CPU có 1 dây dòng yêu cầu ngắt (interrup-request line) – CPU sẽ chú ý mỗi khi thực hiện 1 chỉ thị
- Khi CPU phát hiện controller gửi 1 tín hiệu trên dòng yêu cầu ngắt – CPU lưu 1 số trạng thái (con trỏ lệnh hiện hành) – nhảy tới thủ tục ngắt thuộc (interrupt-handler) đc cố định trong bộ nhớ
- Bộ quản lý ngắt xác định nguyên nhân ngắt, thực hiện xử lý cần thiết, thực thi chỉ thị return from interrupt trả về CPU trạng thái thực thi trước khi ngắt

### __Truy xuất bộ nhớ trực tiếp (Direct memory-access-DMA)__

Đối với thiết bị có khối lượng truyền lớn như disk, nó sẽ lãng phí tài nguyên bộ xử lý để theo dõi bit trạng thái, đẩy dữ liễu vào thanh ghi theo từng byte.

PC giảm gánh nặng chu CPU = chuyển 1 số công việc tới Controller có mục đích đặc biệt - __bộ điều khiển truy xuất bộ nhớ trực tiếp (direct memory-access-DMA)__.

Để khởi tạo thao tác chuyển DMA:
- Máy tính tạo khối lệnh DMA vào bộ nhớ. Nó có con trỏ chỉ tới nguồn và đích chuyển, số byte được chuyển.
- CPU ghi địa chỉ khối lệnh tới bộ điều khiển DMA, rồi tiếp tục làm việc khác.
- Bộ điều kiển DMA xử lý để điều hành bus bộ nhớ trực tiếp, đặt các địa chỉ trên bus để thực hiện việc chuyển mà không có sự trợ giúp của CPU
- Một bộ điều khiển DMA đơn giản là một thành phần chuẩn trong PCs, và __bảng nhập/xuất bus chính (bus-mastering I/O boards)__ PC thường chứa phần cứng DMA tốc độ cao.

<div style="text-align:center"> <img src=https://raw.githubusercontent.com/lacoski/khoa-luan/master/Filesystem/PIC/io-sys-3.png></div>

### __Vùng đệm - Buffer__
Một vùng bộ nhớ lưu trữ dữ liệu tạm thời khi có hoạt động chuyển giao dữ liệu giữa hai thiết bị hay giữa thiết bị và ứng dụng.

Vùng đệm tồn tại bởi 3 lý dó:
- Đối phó với tính trạng không tương thích tốc độ giữa điểm gửi và điểm nhận
- Thích ứng giữa các thiết bị có kích thước truyền dữ liệu khác nhau (Tốc độ cao và thấp)
- Hỗ trợ ngữ nghĩa sao chép cho nhập/xuất ứng dụng.

#### __Vùng lưu trữ - Cache__
Là vùng bộ nhớ nhanh quản lý các bản sao dữ liệu. Truy xuất tới một bản sao được lưu trữ hiệu quả hơn truy xuất tới bản gốc

Sự khác nhau giữa __vùng đệm (Buffer)__ và __vùng lưu trữ (Cache)__:
- Buffer có thể giữ chỉ bản sao dữ liệu đã có
- Cache trữ giữ vừa một bản sao trên thiết bị cho phép truy xuất nhanh, ghi nhanh (dữ liệu chưa update tới phân vùng lưu trữ).

Vùng lưu trữ và vùng đệm có chức năng khác nhau nhưng đôi khi một vùng bộ nhớ có thể được dùng cho cả hai mục đích.
