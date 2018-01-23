# Khôi phục
---
## Giới thiệu
File và direc được lưu trên main memory và disk => khi xảy ra lỗi cần khả năng khôi phục.

Lỗi hệ thống có thể từ xảy ra từ xung đột trên disk file system data __(directory structures, free-block pointers, free FCB pointers)__.

__Vấn đề__
- Khi tạo 1 file, sẽ thay đổi 1 phần FS trên disk.
- Directory structure được chỉnh sửa, FCBs được cấp phát, data block cấp phát, bộ đếm trên block tăng hoặc giảm.
- Đây là 1 chuỗi các hoạt động => có thể dẫn đến lỗi nếu quá trình xử lý bị ảnh hưởng.
- 1 lỗi phổ biến là tính năng caching của OS để nâng cao hiệu năng IO. 1 số thay đổi tới disk, trong khi 1 số được cache. Nếu cache chưa được đưa xuống disk mà hệ thống lỗi => xảy ra lỗi về data đang xử lý.
- 1 số vấn đề bên cạnh như thực thi FS, disk controller, user app cũng có thể gây lỗi FS.

## Các phương pháp khôi phục
__Consistency Checking__
- Khi xảy ra lỗi, FS cần tìm kiếm lỗi và sửa. Để phát hiện, pp = scan toàn bộ metadata trên mỗi file và kiếm tra tính nhất quáng hệ thống. Quá trình diễn ra rất lâu và xảy ra khi hệ thống khởi đông lại.
- pp bên cạnh, FS có thể lưu trạng thái trong FS metadata. Khi metadata thay đổi, các trạng thái metadata sẽ đánh dấu.
- __Consistency checker__ – chương trình hệ thống như fsck (Unix) sẽ đối chiếu data trong direc struc với data block trên disk, cố gắng sửa các mâu thuẫn tìm thấy.
- __allocation và free-spacemanagement algorithms__ sẽ xác định loại lỗi mà check tim thấy, sử dụng pp tương ứng sửa. Đối với hđ có thể gây lỗi cao Unix sẽ sử dụng cơ chế đồng bộ, tránh việc lỗi ko thể sửa.

__Log-Structured File Systems__
- Giải quyết 1 số vấn đề consistency checking đang gặp phải, áp dụng __log-based transaction-oriented (or journaling) file systems__.
- Giải pháp tập trung vào kỹ thuật log-based recovery khi fs metadata update.

Tổng quát, tất cả metadata thay đổi được ghi tuần tự tới log. Mỗi tập hoạt động được thực hiện 1 nhiệm vụ xác đinh = __transaction__. Khi thay đổi được ghi vào log, chúng sẽ được cân nhắc để commited, system call có thể trả lại user process, cho phép nó tiếp tục thực hiện => Log entry sẽ lưu lại hoạt động thực sự trên FS structure.

Khi thay đổi thực hiện, con trỏ sẽ update hđ đã hoàn thành, hoặc chưa hoàn thành. Khi hoạt động transaction hoàn thành => quá trình ghi log sẽ được xóa khỏi log file (1 bộ đệm xử lý). __circular buffer__ ghi tới vị trí đánh dấu lưu trữ, ghi đè vào giá trị cũ nếu có. Log có thể chia thành các phần trong FS hoặc trên đĩa.

Nếu hệ thống lỗi, log sẽ tồn tại 0 hoặc nhiều __transactions__. Mỗi __transaction__ tồn tại các nhiệm vụ chưa hoàn thành trên FS kể cả đã được committed bởi OS => chúng sẽ tiếp tục thực hiện (thông tin về con trỏ, FS được lưu lại để có thể tiếp tục). Trong trường hợp Transaction bị hủy => nó ko được committed trước khi sys crash => mọi hoạt động trên tập tin bởi transactions sẽ bị loại bỏ, khổi phục trạng thái ban đầu (trước khi thay đổi).
