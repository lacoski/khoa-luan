# Thực thi File system
---
## Tổng quan
Các cấu trúc và các thao tác được dùng để thực thi các thao tác của hệ thống tập tin.
1 vài on-disk và Cấu trúc bộ nhớ trong (in-memory structures) được sử dụng để thực thi FS. Cấu trúc phụ thuộc vào OS và FS và 1 số nguyên tắc chung.

Trên đĩa, FS chứa thông tin về cách khởi động hệ điều hành được lưu trữ ở đó, tổng số khối, số và vị trí của các khối trống, cấu trúc thư mục, các tập tin riêng biệt.

## Các cấu trúc trên đĩa gồm
+ __Khối điều khiển khởi động__ (boot control block): chứa thông tin được yêu cầu bởi hệ thống để khởi động một hệ điều hành từ volume đó. Nếu đĩa không chứa hệ điều hành thì khối này là rỗng. Điển hình, nó là khối đầu tiên của đĩa. Trong UFS, khối này được gọi là boot block; trong NTFS, nó là (partition boot sector).
+ __Khối điều khiển phân khu__ (partition control block): chứa chi tiết về volume (or partition), như số lượng khối trong volume, kích thước khối, bộ đếm khối trống và con trỏ khối trống, bộ đếm FCB trống và con trỏ FCB. Trong UFS khối này được gọi là siêu khối (superblock); trong NTFS, nó là bảng tập tin chính (Master File Table)
+ __Directory structure__ – Sử dụng cho hđ tổ chức File. Trong UFS, bao gồm file name + inode liên kết. Trong NTFS, lưu trong master file table.
+ __per-file FCB (file control block)__ chưa nhiều thông tin về file. Nó có số định danh riêng cho phép liên kết với directory entry. Trong NTFS, thông tin nay lưu trong master file table

__In-memory information (Thông tin bộ nhớ trong)__ được sử dụng cho hoạt động quản lý FS và nâng cao hiệu năng = caching. Được cập nhật trong quá trình hoạt động FS. Một vài loại cấu trúc được thêm vào:
+ __in-memory mount table__ bao gồm thông tin về mỗi ổ đĩa được mounted volume.
+ __in-memory directory-structure cache__ giữ thông tin các directory thường được truy cập
+ __system-wide open-file table__ chứa bản sao FCB (file control block) cho mỗi file được mở, cững như các thông tin bên cạnh
+ __per-process open-file table__ chứa con trỏ tới mục tương ứng trong system-wide open-file table cũng như thông tin bên cạnh
+ Buffers lưu file system block khi chúng được đọc từ disk hoặc ghi tới disk

Để tạo file mới, App program gọi logical file system. logical file system nhận biết định dạng directory structures. Nó sẽ cấp phát FCB mới, đọc thư mục tương ứng vào bộ nhớ, cập thư mục với tập tin mới, ghi lại vào đĩa. FCB bao gồm:

<div style="text-align:center"> <img src=https://raw.githubusercontent.com/lacoski/khoa-luan/master/Filesystem/PIC/fs-imp-1.png></div>

> - 1 số OS (như Linux/Unix) đối xử direc như file. 1 trường phân biệt nó là directory.
> - 1 số OS (như Windows) phân biệt file và direc.

Tuy thuộc vào kiến trúc, __logical file system__ có thể gọi __file-organization module__ để __map directory I/O__ tới __disk-block number__, chuyển vào FS cơ bản, IO control system.

Khi file được tạo, để sử dụng cho hoạt động IO - Nó phải cần được mở. Lời gọi __Open()__ truyển filename tới logical file system – hàm được gọi đầu tiên, tìm kiếm __system-wide open-file table__ để kiểm tra file có đang được sử dụng bởi tiến trình khác.
- Nếu mở, __per-process open-file table entry__ tạo con trỏ tới __existing system-wide open-file table__ (tiết kiệm không gian bộ nhớ)
- Nếu không, tìm kiếm trong direc structure theo file name. 1 phần direc structure được cached trong memo để tăng tốc hđ OS.

Khi file tìm thấy, FCB tạo bản sao tới __system-wide open-file table__ trong memory. Table không chỉ lưu FCB, nó còn theo dõi số process đang sử dụng file.

Tiếp theo 1 entry được tạo trong __per-process open-file table__, với con trỏ tới entry trong __system-wide open-file table__ và 1 số trường khác (có thể con trở tới vị trí hiện tại file sử dụng cho hd read() write(), trạng thái truy cập nếu file đang mở). Open() call trả lại contror tới entry tương ứng trong __per-process file-system table__.

Tất cả hđ trên file thực hiện thông qua con trỏ. File name có thể ko là 1 phần open-file table, system sẽ ko sử dụng nó khi FCB tương ứng được định vị trên disk. Nó có thể được cached, tiết kiệm thời gian cho hđ tiếp theo trên file. Unix gọi chúng là __file descriptor__

Khi process đóng file, __per-process table entry__ được xóa. __system-wide entry__ đang mở sẽ giảm. Khi tất cả user mở và đóng lại, metadata sẽ được update, tạo bản copy tới disk-based direc structure, system-wide open-file table entry được xóa.

Caching cho FS structure rất quan trọng. Hầu hết sys giữ tất thông tin file đang mở trong memory, trừ datablock.

<div style="text-align:center"> <img src=https://raw.githubusercontent.com/lacoski/khoa-luan/master/Filesystem/PIC/fs-imp-2.png></div>
