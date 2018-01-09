

**Hệ thống tệp (File system – FS)**

Nhân hệ điều hành Linux có chứa một lớp hệ thống tập tin ảo (Virtual File System – VFS) được sử dụng trong suốt quá trình hệ thống gọi kích hoạt các tập tin. VFS là một lớp gián tiếp, quản lý các lời gọi hệ thống hướng tập tin và gọi những chức năng cần thiết trong mã hệ thống tập tin vật lý để thực hiện nhập/xuất.

Cơ chế gián tiếp này được sử dụng thường xuyên trong các hệ điều hành Linux để dễ dàng hoà nhập và sử dụng nhiều kiểu hệ thống tập tin khác nhau.

Khi một tiến trình phát sinh một lời gọi hệ thống hướng tập tin, nhân hệ điều hành gọi một chức năng được chứa trong VFS. Chức năng này quản lý các thao tác độc lập cấu trúc và chuyển hướng lời gọi tới một chức năng chứa trong mã hệ thống tập tin vật lý, nó có khả năng quản lý các thao tác phụ thuộc cấu trúc. Mã hệ thống tập tin sử dụng các chức năng trữ bộ đệm để yêu cầu nhập/xuất trên các thiết bị.

<div style="text-align:center"> <img src=https://raw.githubusercontent.com/lacoski/khoa-luan/master/Filesystem/PIC/phancap.png></div>


**Cấu trúc hệ thống tập tin ảo:**

**- VFS** định nghĩa một bộ các chức năng mà mọi hệ thống tập tin phải thực thi. Giao diện này được tạo bởi một tập hợp các thao tác liên kết với ba kiểu của đối tượng:

**hệ thống tập tin, i-node, và các tập tin mở.**

- VFS biết về các kiểu hệ thống tập tin được hỗ trợ trong nhân hệ điều hành. Nó sử dụng một bảng đã định nghĩa trong quá trình cấu hình nhân hệ điều hành. Mỗi mục từ trong bảng này mô tả một kiểu hệ thống tập tin: nó có chứa tên của kiểu hệ thống tập tin và một con trỏ trên một hàm đã gọi trong quá trình thao tác gắn vào (mount). Khi một hệ thống tập tin được gắn vào, hàm mount tương ứng được gọi. Chức năng này chịu trách nhiệm để đọc siêu khối (superblock) từ đĩa, khởi nạp các biến nội của nó, và trả về một bộ mô tả hệ thống tập tin được gắn vào cho VFS.

- Sau khi hệ thống tập tin được gắn vào, các hàm VFS có thể sử dụng bộ mô tả này để truy nhập hệ thống tập tin. Bộ mô tả hệ thống tập tin được gắn vào có chứa nhiều kiểu dữ liệu: kiểu dữ liệu thông tin là phổ biến với mọi kiểu hệ thống tập tin, kiểu con trỏ tới hàm được cung cấp bởi mã nhân hệ điều hành hệ thống tập tin vật lý, và dữ liệu riêng được bảo quản bởi mã hệ thống tập tin vật lý. Các con trỏ hàm có trong bộ mô tả hệ thống tập tin cho phép VFS truy nhập hệ thống tập tin.

- Hai kiểu khác của bộ mô tả được sử dụng bởi VFS: một bộ mô tả **i-node** và một bộ **mô tả tập tin mở**.

**Tổ chức logic của hệ thống tập tin Linux**

Các kiểu tập tin trong Linux

- Regular. Tập tin regular là tập tin chỉ chứa dữ liệu. Dữ liệu có thể là một chương trình, một tập tin text, mã nguồn hay bất kỳ nội dung nào khác

- Directory. Tương tự như regular, nó chứa một bộ các byte dữ liệu, như ở đây, dữ liệu chỉ giới hạn là một danh sách các tập tin khác, đó là nội dung của thư mục. Không có giới hạn về kiểu tập tin mà một thư mục có thể chứa đựng

- Charater device và block device: Các chương trình Linux giao tiếp với các thiết bị phần cứng thông qua hai kiểu tập tin đặc biệt gọi là character device và block device. Các tập tin character device tham chiếu đến các trình điều khiển thiết bị muốn thực hiện các thao tác nhập/xuất bộ đệm như là terminal. Các tập tin block device được liên kết với các trình điều khiển thiết bị thực hiện các thao tác nhập/xuất chỉ trong phạm vi những đoạn lớn 512 hay 1024 byte và muốn nhân hệ điều hành thực hiện bộ đệm cho chúng. Một số kiểu của phần cứng như là đĩa có thể đại diện cho cả hai kiểu tập tin character device và block device.

- Domain socket. Được kết nối giữa các tiến trình và cho phép chúng truyền thông với nhau một cách nhanh chóng và tin cậy. Có nhiều kiểu khác nhau của domain socket, phần lớn chúng được sử dụng trong giao tiếp mạng.

- Name pipes. Cho phép truyền thông giữa hai tiến trình không có quan hệ chạy trên cùng một máy.

- Hard link. Thực sự không phải là một kiểu tập tin, mà là một tập tin &#39;liên kết&#39; với tập tin khác. Mỗi tập tin có ít nhất một hard link. Khi một hard link mới được tạo cho một tập tin, một tên hiệu (alias) cho tập tin đó sẽ được tạo. Nội dung của hard link và tập tin nó liên kết tới luôn giống nhau. Khi thay đổi nội dung của hard link, nội dung của tập tin mà nó liên kết tới cũng thay đổi, và ngược lại.

- Symbolic link. Là một tập tin chỉ chứa tên của tập tin khác. Khi nhân hệ điều hành mở hoặc duyệt qua một symbolic link, nó được dẫn đến tập tin mà symbolic link chỉ đến thay vì chính bản thân của symbolic link. Sự khác nhau cơ bản giữa hard link và symbolic link là hard link là tham chiếu trực tiếp, trong khi đó symbolic link là một tham chiếu bởi tên tập tin
