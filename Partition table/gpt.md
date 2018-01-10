# GPT - GUID Partition Table
---
Giới thiệu:
- Mỗi phân vùng trên ổ đĩa có một "globally unique identifier," hay viết tắt là GUID.
- Hỗ trợ ổ cứng tới 1 ZB ( 1 tỷ TB)
- Hỗ trợ tối đa 128 phân vùng ổ đĩa
- Chỉ hỗ trợ các phiên bản Windows 7, 8, 8.1, 10 64bit
- Chỉ hỗ trợ các máy tính dùng chuẩn UEFI
- GPT lưu trữ nhiều bản sao của các dữ liệu này trên đĩa, có thể khôi phụcdữ liệu nếu dữ liệu này bị lỗi.
- GPT lưu giá trị Cyclic Redundancy Check (CRC), kiểm tra các dữ liệu này còn nguyên vẹn hay không. Nếu lỗi, GPT cố gắng khôi phục các dữ liệu bị hư hỏng từ một vị trí khác trên ổ đĩa.
