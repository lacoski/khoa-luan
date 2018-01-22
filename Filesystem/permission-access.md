# Kiểm soát truy xuất
---
## Giới thiệu
Truy xuất dựa trên định danh người dùng. Cơ chế thông dụng nhất để cài đặt truy xuất phụ thuộc định danh là gắn với mỗi tập tin và thư mục một danh sách kiểm soát truy xuất (access-control list-ACL) xác định tên người dùng và kiểu truy xuất được phép cho mỗi người dùng

Khi một người dùng yêu cầu truy xuất tới một tập tin cụ thể, hệ điều hành kiểm tra danh sách truy xuất được gắn tới tập tin đó. Nếu người dùng đó được liệt kê cho truy xuất được yêu cầu, truy xuất được phép. Ngược lại, sự vi phạm bảo vệ xảy ra và công việc của người dùng đó bị từ chối truy xuất tới tập tin.

Để tạo được danh sách, hệ thống phân loại người dùng theo mỗi tập tin:
+ Người sở hữu (Owner): người dùng tạo ra tập tin đó 
+ Nhóm (Group): tập hợp người dùng đang chia sẻ tập tin và cần truy xuất tương tự là nhóm hay nhóm làm việc
+ Người dùng khác (universe): tất cả người dùng còn lại trong hệ thống

Để tạo và cài đặt danh sách, trên Unix chỉ có người quản trị mới có quyền tạo.
