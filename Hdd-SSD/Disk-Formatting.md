# Disk Formatting
---
## Kiến trúc
Ổ đĩa cứng mới sản xuất chỉ đơn giản là các platter chất liệu ghi từ, cần phần chia sector, cho phép disk controller có thể đọc ghi – tiền trình này gọi là __low-level formatting or physical formatting__.
__Low-level formatting__ tạo data structure cho mỗi sector.

- Data stucture trên mỗi sector bao gồm header và data area (thông thường 512 byte/size), trailer.
 - header và trailver chứa thông tin sử dụng cho Disk controller VD: sector number, error-correcting code (ECC).
- Khi controller ghi data vào sector, suốt quá trình I/O, ECC sẽ update các giá trị byte trong data area. Khi sector được đọc, ECC sẽ tính toán và đối chiếu giá trị hiện tại
 - Nếu dữ liệu hiện tại và giá trị ECC khác => sector có thể lỗi hoặc disk sector có thể bad. Nếu chỉ 1 vài bit lỗi, controller có thể xác định bit thay đổi và sửa lỗi.
 - Controller tự động chạy tiến trình ECC chõ mỗi sector khi đọc, ghi.
- __Low-level-formatted__ thường tiến hành bởi nhà sản xuất. Cho phép nsx khởi tạo, test disk, map __logical block__ tới __free sector__ trên disk.
- Trong quá trình LLF, có thể định nghĩa số byte trong data space giữa header và trailer trên sector. Mặc định: 256, 512, và 1,024 bytes.

Trước khi có thể lưu trữ user data, OS cần setup cấu trúc dữ liệu riêng lên disk.
+ 1) phân vùng đĩa trên 1 or nhiều cylinder (OS đối xử với mỗi partition như 1 ổ đĩa riêng biệt)
+ 2) logical formatting hoặc khởi tạo file system – OS cấu trúc file system lên disk. Cấu trúc dữ liệu có thể bao gồm maps of free và phân bố không gian, khởi tạo direc trống.

Để nâng cao hiệu năng, FS nhóm block thành cluster. Disk IO làm việc với blocks, FS IO làm việc với cluster. Bảo đảm hiệu năng, giảm tính truy cập ngẫu nhiên.
> Một số OS cho phép chương trình đặc biệt sử dụng disk partition như 1 chuỗi các logical blocks, bỏ qua file system data structure. Array logical block = RAW DISK.
 - VD: 1 số database cần 1 số phân vùng raw IO cho phép quản trị các database record. RAW I/O vượt qua fs service như buffer cache, file locking, prefetching, space allocation, file names, and directories.

## Boot Block
Để khởi động PC, PC cần chương trình khởi tạo – chương trình setup hệ thống từ CPU tới device controller, ram, OS. Để thực hiện quá trình này, bootstrap program cần tìm OS kernel trên disk, load kernel để khởi động OS.

Được điểm chương trình Bootstrap:
- Được lưu trên ROM, được fix cứng ko sợ ảnh hưởng viruss.
- Thiếu sự linh động nên hiện tại nó chỉ chứa chức năng khởi tạo bootstrap program trên disk => tối ưu linh hoạt của chương trình bootstrap (dễ thay đổi).
- Boot partition nằm trên disk gọi là __boot disk hoặc system disk.__
- Code trong boot ROM chi thị Disk controller đọc boot block lên memory sau đó thực thi code.

Full bootstrap program khá phức tạo, nó sử dụng để load toàn bộ OS từ disk và chạy OS.

<div style="text-align:center"> <img src=https://raw.githubusercontent.com/lacoski/khoa-luan/master/Hdd-SSD/PIC/disk-format-1.png></div>

## Bad Blocks
Khi các sector bị lỗi, Càng nhiều bad sector càng khiến cho quá trình đọc ghi dữ liệu của đầu đọc bị gián đoạn nên gây chậm và treo máy.
=> Disk cần nhận diện, thay thế dữ liệu đang lưu trữ sang 1 phân vùng khác.

Dựa trên Disk và Controller sử dụng, block lỗi sẽ được xử lý bởi nhiều cách.
- VD: IDE controllers, bad blocks xử lý thủ công. Khi phát hiện, bad block sẽ bị gắn cờ đánh đấu phân vùng ko đảm bảo.
- OS cung cấp 1 số chương trình đặc biêt (badblocks trong linux), cho phép tự quét và lock các phân vùng xác định. Data trong bad block đã phần sẽ mất.
- 1 số ổ đĩa thông minh hỗ trợ bad-block recovery. Controller duy trì list các bad block, được update suốt quá trình sử dụng disk. Từ list, controller sẽ thay thế bad block = sector khác. Tên bảng: sector sparing or forwarding.
- Fix lỗi trên sẽ ảnh hưởng đến thuật toán tối ưu disk.
- Quá trình thay thế bad block khỗng diễn ra mặc định => 1 số soft tự động thực hiện check và thay thế, xử lý bad block.

## Swap-Space Management
Hoạt động swap xảy ra khi RAM tới ngưỡng tràn => yêu cầu chuyển memory tới không gian trống có sẵn. 1 số hệ thống sử dụng thuật ngữ “swapping” and “paging” thay thế nhau.

Swap-space management là tiền trình mức OS. Virtual memory sử dụng trên disk space như mở rộng RAM.
- Disk access chậm hơn Ram access => sử dụng swap space giảm hiệu năng đăng kể.

__Swap-Space Use__
- Swap space được OS sử dụng theo nhiều cách, tùy thuộc vào thuật toán memory-management sử dụng.
- VD: 1 số OS sử dụng swap space lưu process image như code, data segment. Paging systems đơn giản lưu trữ page đẩy ra khởi main memory
- Số swap space có thể từ vài mb tới vài gb. Phụ thuộc vào dung lượng RAM
- Các tiến tình được lưu trên swap space có thể lỗi.

__Swap-Space Location__

Lưu tại 1 trong 2 nơi:
+ Normal file system
+ Separate disk partition

Nếu swapspace là file lớn trong FS thì những phương pháp này không đem đến hiệu năng cao.
Khi sử dụng swapping sẽ phải chấp nhận đánh đổi giữa hiệu năng và phân mảnh (Phân mảnh sẽ tăng thời gian trải đổi swaping).

Có thể nâng cao hiệu năng = caching thông tin vị trí block trên RAM và 1 số công cụ đặc biệt để phân bố block cho swap file.

Giải pháp khác là sử dụng __raw partition__. Hơn hết, __separate swap-space storage manager__ sẽ quản lý block thuộc raw partition (cấp phát, thu hồi), sử dụng thuật toán để tối ưu lưu trữ trên phân vùng. Phương pháp này sẽ tăng sự phân mảnh nhưng có thể chấp nhận vì data trong swap space có chu kỳ lưu ngắn.

<div style="text-align:center"> <img src=https://raw.githubusercontent.com/lacoski/khoa-luan/master/Hdd-SSD/PIC/disk-format-2.png></div>
