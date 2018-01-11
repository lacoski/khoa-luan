# UEFI
---
Giới thiệu
- UEFI - viêt tắt Unified Extensible Firmware Interface
- Công cụ thay thế BIOS đã dần lỗi thời
- UEFI là hệ điều hành tối giản trên phần cứng và firmware.
- Được lưu tại /EFI/ trong bộ nhớ non-volatile, có thể được lưu trên bộ nhớ flash NAND trên bo mạch chính (mainboard) hoặc 1 ổ đĩa cứng, hay tại 1 phân vùng mạng chia sẻ.
- Hỗ trợ tính năng khởi động an toàn - Secure Boot.

Ưu điểm
- Khởi động nhanh, rút ngắn thời gian khắc phục sự cố, tích hợp khả năng kết nối mạng, công cụ sửa lỗi cơ bản
- Có khả năng quản lý thiết bị lưu trữ dung lượng lớn. (Vượt qua giới hạn BIOS)
- UEFI sử dụng phân vùng GUID, thay thế MBR. Đem lại khả năng khởi động ổ cứng lớn tới 9,4ZB (zetabyte).
- Cho phép các nhà sản xuất tích hợp thêm công cụ.
