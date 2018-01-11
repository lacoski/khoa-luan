# BIOS
----
**Giới thiệu**
- BIOS viết tắt (Basic Input/Output System) - Hệ thống xuất nhập cơ bản, còn có thể gọi là chuẩn legacy.
- Nằm trên PC, bo mạch chính.
- Chạy đầu tiên, chuẩn bị cho các hoạt động khác.
- Là chương trình nằm trong chip có sẵn trên bản mạch chính (PROM, EPROM). Chuẩn bị khởi động, tìm kiếm ổ đĩa, hệ điều hành.
- Thành phần không thể thiếu trong máy tính

**Nguyên lý hoạt động:**
- BIOS nằm trong chip PROM (programmable Read-only memory - chip bộ nhớ chỉ đọc), EPROM hay bộ nhớ flash.
- Khi khởi động, BIOS sẽ tìm kiếm các thành phần còn lại của PC (ổ đĩa, bộ nhớ, ..).
-  BIOS có thể setup CMOS. CMOS nơi lưu trữ dữ liệu chuyển biệt người dùng: time, thông tin ổ đĩa). CMOS truy cập thông qua BIOS

**1 số vấn đề liên quan:**
- BIOS ngày nay có thể được chứa trong (Electrically Erasable Programmable Read-Only Memory) hoặc bộ nhớ flash. Cho phép user cập nhật.
- Quá trình cập nhất BIOS xảy ra lỗi có thể gây lỗi, khiến máy tính không sử dụng được. Để giải quyết vấn đề này, cung cấp thêm 1 phiên bản lưu trữ backup BIOS, chịu trách nhiệm kiểm tra BIOS.

**Hạn chế BIOS**
- Hiện BIOS đã trở nên lỗi thời, không tương thích với các công nghệ mới
- Gặp hạn chế bởi sử dụng BIOS MBR, không thể khởi động ổ cứng lớn hơn 2TB
