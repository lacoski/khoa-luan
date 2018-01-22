# Cấu trúc thư mục
---
## Giới thiệu
Hệ thống lưu trữ hàng triệu tập tin trên các terabytes đĩa. Để quản lý tất cả dữ liệu này, chúng ta cần tổ chức lại chúng

1 số phương pháp:
- Cần chia thành một hay nhiều __phân khu (partition)__ hay __phân vùng (volumes)__.
- Phân khu này là cấu trúc cấp thấp mà các tập tin và thư mục định vị
- Các phân khu được dùng để cung cấp nhiều vùng riêng rẻ trong một đĩa, mỗi phân khu được xem như một thiết bị lưu trữ riêng
- Bên cạnh, các hệ thống khác cho phép các phân khu có dung lượng lớn hơn một đĩa để nhóm các đĩa vào một cấu trúc luận lý và cấu trúc tập tin
- Phân khu có thể được xem như các đĩa ảo, có thể lưu trữ nhiều hệ điều hành
- Mỗi phân khu chứa thông tin về các tập tin trong nó
- Thông tin này giữ trong những mục từ trong một thư mục thiết bị hay bảng mục lục phân vùng (volume table of contents)
- Thư mục thiết bị (được gọi đơn giản là thư mục)

<div style="text-align:center"> <img src=https://raw.githubusercontent.com/lacoski/khoa-luan/master/Filesystem/PIC/direc-1.png></div>

## Cấu trúc thư mục
Thư mục có thể được hiển thị như một bảng danh biểu dịch tên tập tin thành các mục từ thư mục. Các thư mục có thể được tổ chức trong nhiều cách. Chúng ta có thể chèn mục từ, xoá mục từ, tìm kiếm một mục từ và liệt kê tất cả mục từ trong thư mục.

__Cấu trúc thư mục dạng đơn cấp__
- Cấu trúc thư mục đơn giản nhất
- Tất cả tập tin được chứa trong cùng thư mục
- Có nhiều hạn chế khi số lượng tập tin tăng hay khi hệ thống có nhiều hơn một người dùng. Xung đột khi có nhiều người sử dụng, khó quản lý khi tập tin tăng cao.

<div style="text-align:center"> <img src=https://raw.githubusercontent.com/lacoski/khoa-luan/master/Filesystem/PIC/direc-2.png></div>

__Cấu trúc thư mục dạng hai cấp__
- Giải pháp mở rộng pp 1.
- Mỗi người dùng có thư mục tập tin riêng cho họ __(user file directory-UFD)__
- UFD có một cấu trúc tương tự nhưng các danh sách chứa các tập tin của một người dùng
- Khi công việc của người dùng bắt đầu hay người dùng đăng nhập, __thư mục tập tin chính của hệ thống (master file directory)__
- MFD được lập chỉ mục bởi tên người dùng hay số tài khoản và mỗi mục từ chỉ tới UFD cho người dùng đó
- Cho phép các người dùng khác nhau có thể có các tập tin với cùng một tên, với điều kiện là tất cả tên tập tin trong mỗi UFD là duy nhất
- Nhược điểm, cấu trúc này cô lập User (các user riêng biệt), hoàn toàn độc lập nhau, không thể chia sẽ.

<div style="text-align:center"> <img src=https://raw.githubusercontent.com/lacoski/khoa-luan/master/Filesystem/PIC/direc-3.png></div>

__Cấu trúc thư mục dạng cây__
- Tổng quát của thư mục hai cấp
- Cho phép người dùng tạo thư mục con và tổ chức các tập tin của họ
- Cây có thư mục gốc, Mỗi tập tin trong hệ thống có tên đường dẫn duy nhất
- Tên đường dẫn là đường dẫn từ gốc xuống tất cả thư mục con tới tập tin xác định
- Một thư mục (hay thư mục con) chứa tập hợp các tập tin hay thư mục con
- Mỗi người dùng có thư mục hiện hành, chứa hầu hết các tập tin người dùng hiện đang quan tâm. Người dùng có thể thay đổi đường dẫn hiện hành (xác định đường dẫn).
- Thư mục hiện hành khởi đầu của người dùng được gán khi người dùng đăng nhập vào hệ thống. Hệ điều hành tìm tập tin tính toán để xác định mục từ cho người dùng này.
- Trong tập tin tính toán là con trỏ chỉ tới thư mục khởi đầu của người dùng (Con trỏ - một biến cục bộ cho người dùng xác định thư mục hiện hành khởi đầu).
- Đường dẫn có hai kiểu: __tên đường dẫn tuyệt__ đối và __tên đường dẫn tương đối__
- Hệ thống thư mục cấu trúc cây, cho phép user truy xuất tới các tập tin của họ và các tập tin của người dùng khác

<div style="text-align:center"> <img src=https://raw.githubusercontent.com/lacoski/khoa-luan/master/Filesystem/PIC/direc-4.png></div>

__Cấu trúc thư mục dạng đồ thị không chứa chu trình__
- VD: cần chia sẻ dữ liệu chung giữa 2 người cùng cấp. Cấu trúc cây ngăn cản việc chia sẻ các tập tin và thư mục
=> đồ thị không chứa chu trình (acyclic graph) cho phép thư mục chia sẻ thư mục con và tập tin
- Cùng tập tin và thư mục con có thể ở trong hai thư mục khác nhau
- Một đồ thị không chứa chu trình là trường hợp tổng quát của cơ chế thư mục có cấu trúc cây.
- Một tập tin (hay thư mục) được chia sẻ không giống như hai bản sao của một tập tin. Với một tập tin được chia sẻ, chỉ một tập tin thực sự tồn tại vì thế bất cứ sự thay đổi được thực hiện bởi một người này lập tức nhìn thấy bởi người dùng khác
- Các UFD của tất cả thành viên trong nhóm chứa thư mục của tập tin được chia sẻ như một thư mục con
- Các tập tin và thư mục con được chia sẻ có thể được cài đặt trong nhiều cách. Trong Unix sử dụng pp liên kết - con trỏ chỉ tới một tập tin hay thư mục con khác
- Những liên kết được xác định dễ dàng bởi định dạng trong OS. Hệ điều hành bỏ qua các liên kết này khi duyệt cây thư mục, bảo đảm tính nhất quáng cây.
- Khi xóa, hệ thống chia sẻ sử dụng liên kết biểu tượng. Việc xoá một liên kết không cần tác động tập tin nguồn, chỉ liên kết bị xoá. Nếu chính tập tin bị xoá, không gian cho tập tin này được thu hồi, để lại các liên kết chơi vơi. Yêu cầu quan lý các liên kết tới file, chúng ta thật sự không cần giữ toàn bộ danh sách-chúng ta chỉ cần giữ số đếm của số tham chiếu. Một liên kết mới hay mục từ thư mục mới sẽ tăng số đếm tham chiếu; xoá một liên kết hay mục từ sẽ giảm số đếm. Khi số đếm là 0, tập tin có thể được xoá; không còn tham chiếu nào tới nó.
- Hệ điều hành UNIX sử dụng pp trên cho liên kết cứng - giữ một số đếm tham chiếu trong khối thông tin tập tin (hay inode)

<div style="text-align:center"> <img src=https://raw.githubusercontent.com/lacoski/khoa-luan/master/Filesystem/PIC/direc-5.png></div>

__Cấu trúc thư mục dạng đồ thị tổng quát__
- Cấu trúc đồ thị không chứa chu trình là đảm bảo rằng không có chu trình trong đồ thị. Vấn đề khi chúng ta liên kết một thư mục cấu trúc cây đã có, cấu trúc cây bị phá vỡ hình thành một đồ thị đơn giản
- Vì sử dụng pp liên kết => phá bỏ đặc tính của cây thư mục – tạo đồ thị cơ bản.

<div style="text-align:center"> <img src=https://raw.githubusercontent.com/lacoski/khoa-luan/master/Filesystem/PIC/direc-6.png></div>
