# Cấu trúc Filesystem
---
## **Giới thiệu**
- Tất cả các cấu trúc dữ liệu được đặt kích cỡ dựa trên kích thước một khối (block). Kích thước của khối phụ thuộc vào kích thước của hệ thống tập tin.
- Hệ thống tập tin được chia thành những nhóm khối (block group), số lượng các nhóm khối cũng phụ thuộc vào kích thước của hệ thống tập tin.
- Mỗi nhóm khối có chứa các thông tin xác định vị trí, số khối và các thông tin mô tả trạng thái hệ thống tập tin hiện hành, bao gồm các thông tin sau:
 - **superblock**: Chứa các thông tin cơ bản nhất và các thuộc tính của hệ thống tập tin, thí dụ như tổng số i-node, tổng số khối, trạng thái hệ thống tập tin …
 - **group descriptors**: Là một mảng cấu trúc, mỗi cấu trúc mô tả một nhóm khối, vị trí bảng i-node của nó, bản đồ khối và i-node ….
 - **block bitmap**: Block Bitmap được thường đặt tại khối đầu tiên của nhóm khối. Mỗi bit đại diện cho trạng thái hiện hành của khối trong nhóm khối đó; giá trị 1 có nghĩa là khối đang được sử dụng và 0 là chưa được sử dụng.
 - **i-node bitmap**: chức năng tương tự như block bitmap, mỗi bit đại diện cho một i-node trong bảng i-node (i-node table). Mỗi một nhóm khối có một inode bitmap
 - **i-node table**: sử dụng để lưu vết tất cả các tập tin; vị trí, kích thước, kiểu và các quyền truy nhâp của tập tin đều được lưu trữ trong các i-node. Trong bảng i-node tất cả các tập tin đều được tham chiếu bởi số i-node (i-node number - Số i-node là một chỉ số trong bảng i-node chỉ tới một cấu trúc i-node). Mỗi i-node có chứa thông tin về một tập tin vật lý riêng rẽ trên hệ thống
 - **data block**: Được sử dụng để lưu trữ nội dung của các tập tin, bao gồm danh sách các thư mục, các thuộc tính mở rộng, các symbolic link …

<div style="text-align:center"> <img src=https://raw.githubusercontent.com/lacoski/khoa-luan/master/Filesystem/PIC/metadata-1.PNG></div>

## **I-node**
- Mỗi tập tin được đại diện bởi một cấu trúc được gọi là một i-node.
- I-node là một bảng có kích thước cố định được sử dụng để lưu trữ tất cả các thông tin về một tập tin, và mỗi tập tin chỉ có một i-node duy nhất.
- Thông tin bao gồm: chủ nhân của tập tin, thời điểm thay đổi nội dung tập tin, thời gian tập tin được truy nhập sau cùng, kích thước, các quyền trên tập tin, số lượng tập tin liên kết v.v…
- I-node có chứa một tập hợp các con trỏ.
 - 10 con trỏ đầu tiên, được tìm nạp từ đĩa tới bộ nhớ chính khi tập tin được mở
 - Tập tin lớn hơn, con trỏ tiếp theo trong i-node sẽ chỉ đến một khối trên đĩa, được gọi là single indirect block.
 - Con trỏ thứ 12 trong trong i-node trỏ tới double indirect block, khối này có chứa địa chỉ của khối có chứa danh sách các single indirect block.
 - *Một single indirect block trỏ tới hàng trăm khối dữ liệu. Nếu vẫn không đủ, một triple indirect block cũng có thể được sử dụng*

 <div style="text-align:center"> <img src=https://raw.githubusercontent.com/lacoski/khoa-luan/master/Filesystem/PIC/metadata-2.PNG></div>
