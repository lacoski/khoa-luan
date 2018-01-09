Các loại file system trong Linux

File hệ thống Linux Ext – Extended file system:
Giới thiệu:
- là định dạng file hệ thống đầu tiên được thiết kế dành riêng cho Linux.
- Có tổng cộng 4 phiên bản và mỗi phiên bản lại có 1 tính năng nổi bật.

Ext:
- Phiên bản đầu tiên của Ext là phần nâng cấp từ file hệ thống Minix
- Có thể xử lý các hệ thống file có kích thước 2 Gigabyte (GB)
- Không đáp ứng được nhiều tính năng phổ biến ngày nay. Tồn tại nhiều hạn chế
- Không còn được hỗ trợ trên nhiều distribution.

**Ext2:**
- Được thiết kế để giải quyết vấn đề Ext đang gặp phải, xây dựng fs mạnh mẽ.
- Phiển bản thương mai đâu tiên của linux.
- Ext2 thực chất không phải là file hệ thống journaling, sự lựa chọn cho usb, sdcard do tính năng đọc ghi ít (do không sử dụng journal).
- Hỗ trợ dung lượng ổ cứng lên tới 2 TB.

**Ext3**
- Phiên bản Ext2 đi kèm với journaling.
- Mục đích chính của Ext3 là tương thích ngược với Ext2, và do vậy những ổ đĩa, phân vùng có thể dễ dàng được chuyển đổi giữa 2 chế độ mà không cần phải format.
- Vấn đề, những giới hạn của Ext2 vẫn còn nguyên trong Ext3
- Ưu điểm của Ext3 là hoạt động nhanh, ổn định hơn Ext2.
- Nhược điểm, Không thực sự phù hợp để làm file hệ thống dành cho máy chủ bởi vì không hỗ trợ tính năng tạo disk snapshot và file được khôi phục sẽ rất khó để xóa bỏ sau này.

**Ext4**
- Mang ưu điểm Ext3, lưu giữ được những ưu điểm và tính tương thích ngược với phiên bản trước.
- Dễ dàng kết hợp các phân vùng định dạng Ext2, Ext3 và Ext4 trong cùng 1 ổ đĩa trong Ubuntu để tăng hiệu suất hoạt động.
- Ext4 có thể giảm bớt hiện tượng phân mảnh dữ liệu trong ổ cứng, hỗ trợ các file và phân vùng có dung lượng lớn...
- Thích hợp với ổ SSD so với Ext3, tốc độ hoạt động nhanh hơn so với 2 phiên bản Ext trước đó, cũng khá phù hợp để hoạt động trên server, nhưng lại không bằng Ext3.

**BtrFS**
- Hiểu là là Butter hoặc Better FS
- Hiện tại đang trong giai đoạn phát triển bởi Oracle
- Có nhiều tính năng giống với ReiserFS.
- Đại diện cho B-Tree File System, hỗ trợ tính năng pool trên ổ cứng, tạo và lưu trữ snapshot, nén dữ liệu ở mức độ cao, chống phân mảnh dữ liệu nhanh chóng... được thiết kế riêng biệt dành cho các doanh nghiệp có quy mô lớn.
- BtrFS không hoạt động ổn định trên 1 số nền tảng distro nhất định, được coi là sự thay thế mặc định cho Ext4. Cho phép chuyển đổi định dạng nhanh chóng từ Ext3/4.
- BtrFS rất phù hợp để hoạt động với server dựa vào hiệu suất làm việc cao, khả năng tạo snapshot nhanh chóng cũng như hỗ trợ nhiều tính năng đa dạng khác.
- BtrFS đứng sau Ext4 khi áp dụng với các thiết bị sử dụng bộ nhớ Flash như SSD, server database...

**ReiserFS**
- 1 trong những bước tiến lớn nhất của file hệ thống Linux, được công bố vào năm 2001 với nhiều tính năng mới mà file hệ thống Ext khó có thể đạt được.
- Nhưng đến năm 2004, ReiserFS đã được thay thế bởi Reiser4 với nhiều cải tiến hơn nữa. Tuy nhiên, quá trình nghiên cứu, phát triển của Reiser4 khá “chậm chạp” và vẫn không hỗ trợ đầy đủ hệ thống kernel của Linux. Đạt hiệu suất hoạt động rất cao đối với những file nhỏ như file log, phù hợp với database và server email.

**XFS**
- XFS được phát triển bởi Silicon Graphics
- Khá tương đồng với Ext4, chẳng hạn như hạn chế được tình trạng phân mảnh dữ liệu, không cho phép các snapshot tự động kết hợp với nhau, hỗ trợ nhiều file dung lượng lớn, có thể thay đổi kích thước file dữ liệu...
- Không thể shrink – chia nhỏ phân vùng XFS.
- XFS khá phù hợp với việc áp dụng vào mô hình server media vì khả năng truyền tải file video rất tốt.
- Tuy nhiên, nhiều phiên bản distributor yêu cầu phân vùng /boot riêng biệt, hiệu suất hoạt động với các file dung lượng nhỏ không bằng được khi so với các định dạng file hệ thống khác, do vậy sẽ không thể áp dụng với mô hình database, email và một vài loại server có nhiều file log.
- Nếu dùng với máy tính cá nhân, thì đây cũng không phải là sự lựa chọn tốt nên so sánh với Ext, vì hiệu suất hoạt động không khả thi, ngoài ra cũng không có gì nổi trội về hiệu năng, quản lý so với Ext3/4.

**JFS**
- JFS được IBM phát triển lần đầu tiên năm 1990
- Điểm mạnh rất dễ nhận thấy của JFS là tiêu tốn ít tài nguyên hệ thống, đạt hiệu suất hoạt động tốt với nhiều file dung lượng lớn và nhỏ khác nhau.
- Các phân vùng JFS có thể thay đổi kích thước được nhưng lại không thể shrink như ReiserFS và XFS, tuy nhiên nó lại có tốc độ kiểm tra ổ đĩa nhanh nhất so với các phiên bản Ext.

**ZFS**
- ZFS hiện tại vẫn đang trong giai đoạn phát triển bởi Oracle với nhiều tính năng tương tự như Btrfs và ReiserFS.
- Phụ thuộc vào thỏa thuận điều khoản sử dụng, Sun CDDL thì ZFS không tương thích với hệ thống nhân kernel của Linux, tuy nhiên vẫn hỗ trợ toàn bộ Linux’s Filesystem in Userspace – FUSE để có thể sử dụng được ZFS.
- Người sử dụng có thể gặp khó khăn khi cài đặt hệ điều hành Linux vì có yêu cầu FUSE và có thể không được hỗ trợ bởi distributor.

**Swap**
- Không phải là 1 dạng file hệ thống, cơ chế hoạt động khá khác biệt
- Được sử dụng dưới 1 dạng bộ nhớ ảo và không có cấu trúc file hệ thống cụ thể.
- Không thể kết hợp và đọc dữ liệu được, nhưng lại chỉ có thể được dùng bởi kernel để ghi thay đổi vào ổ cứng.
- Thông thường, nó chỉ được sử dụng khi hệ thống thiếu hụt bộ nhớ RAM hoặc chuyển trạng thái của máy tính về chế độ Hibernate.
