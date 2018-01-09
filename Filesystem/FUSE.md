# Filesystem in Userspace

---

__Filesystem in Userspace (FUSE)__ là một lõi module nạp được cho các hệ thống máy tính chạy hệ điều hành họ Unix, cho phép người dùng thông thường có khả năng tạo các hệ thống tệp mà không cần chỉnh sửa mã kernel

Đây là kết quả đạt được khi chạy các mã hệ thống tệp trong không gian người dùng, trong khi module FUSE chỉ cung cấp "cầu nối" đến giao diện kernel thực sự. FUSE đã chính thức được tích hợp vào nhân Linux từ phiên bản 2.6.14.

FUSE rất hữu dụng trong việc ghi các hệ thống tệp ảo. Không như các kiểu hệ thống tệp khác, ghi và nhận trực tiếp dữ liệu từ đĩa, hẹ thống tệp ảo không lưu trữ dữ liệu.

Phát hành dưới giấy phép GNU General Public License và GNU Lesser General Public License, FUSE là phần mềm tự do.

FUSE có các bản cho Linux, FreeBSD, NetBSD (cũng như PUFFS), OpenSolaris, và Mac OS X.
