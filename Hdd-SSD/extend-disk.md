# Tốc độ đọc ghi ổ cứng
----
## **Giới thiệu**

Khi disk được sử dụng, motor nằm trong sẽ quay với tốc độ nhanh. Tới 60-250 vòng mỗi giây. Tốc độ đia quay từ 5400, 7200, 10000, 15000 rpm.

Tốc độ đĩa chia thành 2 phần:
- Tốc độ truyền tải – tức tốc độ truyền giữa ổ đĩa và máy tính.
- Thời gian định vị hoặc thời gian truy cập ngẫu nhiên bao gồm 2 phần:
  - 1- Thời gian cần di chuyển (disk arm) tới cylinder mong muốn – thời gian tìm kiếm (seek time);
  - 2- Thời gian cần thiêt để sector mong muốn quay tới đầu đọc – gọi độ trễ quay (rotational latency).

Truyền tài dữ liệu trên bus được thực hiện bởi bộ xử lý điện tích đặc biệt, gọi ‘controllers’

## **Disk Scheduling – Lập lịch ổ đĩa**

OS có trách nhiệm phân bố, tối ưu khả năng sử dụng disk.
- Với hdd, thời gian truy cập gồm 2 phần:
 - seek time – thời gian tìm kiếm cylinder
 - rotational latency – thời gian đầu đọc đến chính xác sector.
- Công thức tính băng thông disk:
 - Tổng số byte được gửi / (tổng thời gian tính từ yêu cầu đầu tiên - truyền tải hoàn tất)

Có thể nâng cao hiệu năng Disk bằng lập lịch disk I/O requests.

Khi chương trình cần xử lý I/O tới ổ đĩa – nó gọi hàm hệ thống tới OS. Request có thông tin như sau:
- Hoạt động thuộc loại input or output.
- Đĩa chỉ ổ đĩa truyền tới.
- Địa chỉ memory sẽ chuyển tới.
- Số sector gửi tới.

Tiến trình này cần **Disk drive** và **Controller**. Nếu drive hoặc controller không sẵn sáng – yêu cầu sẽ được đưa vào hàng đợi, chờ để thực thi.

Queue thường có rất nhiều request => OS chọn tiền trình cần được thực thi trước => sử dụng thuật toán lập lịch

## **Các thuật toán**

**FCFS Scheduling**
- Thuật toán đơn giản – tới trước phục vụ trước.
- Chạy đúng bản chất nhưng không làm tăng hiệu năng.

VD:
- Disk queue yêu cầu sử dụng IO cho các cylinder: 98, 183, 37, 122, 14, 124, 65, 67
- Disk bắt đầu với cylinder 53 => 98 …. => 67. Tổng di chuyển hết 640 cylinders, không tối ưu được hoạt động xử lý.

<div style="text-align:center"> <img src=https://raw.githubusercontent.com/lacoski/khoa-luan/master/Hdd-SSD/PIC/ex-disk1.png></div>

**SSTF Scheduling**
- Phục vụ request gần đầu đọc cylinder vừa xử lý.
- Sử dụng thuật toán: shortest-seek-time-first (SSTF) algorithm.
- Thuật toán lựa chọn request có thời gian tìm kiếm thấp nhất (theo đầu đọc ghi) – cách khác, tt chọn request gần đầu đọc nhất.
- Tồn tại nhược điểm, request tới liên tục, cylinder xa sẽ không thể được thực hiện.

VD:
- Dãy cylinder: 98, 183, 37, 122, 14, 124, 65, 67 – bắt đầu 53
- TT chọn 65 đầu tiên -> 67 -> 37 (gần hơn 98) -> 98 – 122 – 124 – 183
- Chạy qua 236 cylinder
- tt tiêu biểu kỹ thuật lập lịch shortest-job-first (SJF).

<div style="text-align:center"> <img src=https://raw.githubusercontent.com/lacoski/khoa-luan/master/Hdd-SSD/PIC/ex-disk2.png></div>

**SCAN Scheduling**
- Hoạt động theo trục quay disk: Trục đỡ sẽ bắt đầu từ 1 mặt => mặt cuối và đảo ngược, phục vụ yêu cầu. Đĩa sẽ được quét liên tục
- Phục vụ theo đĩa quay và đầu đọc, đĩa quay tới đâu, check các request nằm phía trước, phục vụ, cứ tiếp tục cho đến khi đảo ngược.
- Nhược điểm, request phía sau sẽ phải chờ cho đến khi hoạt động đảo ngược.

<div style="text-align:center"> <img src=https://raw.githubusercontent.com/lacoski/khoa-luan/master/Hdd-SSD/PIC/ex-disk3.png></div>

**C-SCAN Scheduling**
- giải quyết vấn đề SCAN
- Giống SCAN, CSCAN chạy từ đầu tới cuối disk. Nhưng khi đầu đọc tới vị trí cuối, nó trực tiếp quay lại điểm bắt đầu để phục vụ bất kỳ các request phía trước.

<div style="text-align:center"> <img src=https://raw.githubusercontent.com/lacoski/khoa-luan/master/Hdd-SSD/PIC/ex-disk4.png></div>

**LOOK Scheduling**
- Bắt nguồn từ SCAN và CSAN, nhưng nó sẽ không tới cuối disk mà sẽ đảo hướng khi thỏa mãn.
- Thuật toán này gọi là LOOK và C-LOOK scheduling.
- TT tìm yêu cầu trước khi di chuyển.

VD:
<div style="text-align:center"> <img src=https://raw.githubusercontent.com/lacoski/khoa-luan/master/Hdd-SSD/PIC/ex-disk5.png></div>

## **Lựa chọn thuật toán**
- Đầu tiên phải phân loại thiết bị lưu trữ
- SSD khác Hdd => chủ yếu sử dụng FCFS vì chúng không có đỗ trễ cơ.
- Với Hdd, SCAN và C SCAN tốt khi system thường xuyên sử dụng disk. SSTF hoặc FCFS sử dụng khi ít request

**Các vấn đề ảnh hưởng**
- Request tới disk có thể bị ảnh hưởng bởi phân bố file. Nếu phân bố liền kề sẽ hạn chế sự di chuyển đầu đọc => sử dụng link, indexed file khiến khối phân bố, tăng khả năng di chuyển đầu đọc.
- Vị trí và chỉ mục các block khá quan trọng. Khi cần sử dụng 1 file, file sẽ được tìm kiếm trong direc. Nếu direc nằm tại cylinder đầu mà file data lại tại cylinder cuối => tăng độ trể vì đầu đọc di chuyển 1 vòng => ở trường hợp này cần sử dụng cache cho direc và index block vào RAM để tăng tốc độ.
- Các thuật toán vừa liệt kế chỉ tập trung vào seek-time (thời gian tìm kiếm). Giới hạn phần mềm không giải quyết được __rotational latency__.
 - => Nhà sản xuất để giảm bớt đã nhúng thuật toán lập lịch disk vào controller hardware bên trong disk drive. Nếu OS gửi 1 tập request tới controller, controller sẽ áp dụng thuật toán nsx nâng cao hiệu năng.

Thực tế, vấn đề tối ưu thuất toán thường được làm bởi disk controller. OS sẽ xử lý, lập lịch độ ưu tiên các service, dịch vụ
