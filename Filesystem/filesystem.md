## **File System  - Hệ thống quản lý tập tin** ##
**Giới thiệu**

Trong máy tình, file system hoặc filesystem được sử dụng để kiểm soát dữ liệu, lưu trữ và lấy lại. Nếu không có file system, thông tin được lưu trong nhưng khối lớn sẽ không có cách nào để tìm thấy vị trí bắt đầu và vị trí kết thúc, vị trí tiếp theo của dữ liệu. Bởi dữ liệu được chia thành các mảnh và mỗi mảnh được đặt tên, vì thế thống tin dễ dàng được đánh dấu và xác thực. File system được bắt nguồn từ hệ thống lưu trữ trên giấy, nhóm các dữ liệu được gọi là “file”. Cấu trúc và luật logic được sử dụng để quản lý các nhóm thống tin và chúng được gọi là “file system”.

Có rất nhiều loại file system. Mỗi loại đều có cấu trúc và logic khác nhau, tính chất về tốc độ, tính linh hoạt, bảo mật, size và .. 1 số file system được thiết kể để sử dụng cho 1 số ứng dụng đặc biệt. Ví dụ, ISO 9660 file system được thiết đặc biệt cho đĩa quang.
File system có thể sử dụng nhiều trên loại thiết bị lưu trữ và các loại phương tiên truyền thống khác nhau. Nổi bật và thông thường nhất là ổ đĩa cứng. Bên cạnh đó là bộ nhợ flash, đĩa quang. Trong 1 số trường hợp, như tmpfs, computer main memory (RAM) sử dụng cả file đệm, sử dụng trong thời gian ngắn.
1 số file system được sử dụng cho local data storage devices, bên cạnh đó cung cấp truy chế truy cập file thông qua giao thức mạng (NFS, SMB, 9P). Một số file system là ảo, có nghĩa cung cấp “file” ảo sử dụng cho những yêu cấu tính toán hoặc ánh xạ vào nhưng file system khác. File system quản lý truy cập trên cả nội dung của file và metadata của nhưng file này. Nó chịu trách nhiệp sắp xếp không gian lưu trữ đảm bảo, tin cậy, rõ ràng, có hệ thống.

**Kiến trúc**

File system bao gồm 2 hoặc 3 lớp. Đôi khi các lớp được chia rõ ràng, đôi lúc được kết hợp lại.
Lớp thứ nhất – “logical file system”:
“logical file system” chịu trách nhiệm tương tác với ứng dụng người dùng. Nó cung cấp API cho các hoạt động cơ bản – Mở, đóng, đọc, .. và truyền các yêu cầu xuống lớp dưới cho việc xử lý. “Logical file system” quản lý hoạt động mở đối tượng “file table” và “per-process file descriptors”. Lớp này cung cấp “file access, directory operations, và security and protection”.
Lớp thứ 2 (không bắt buộc) - virtual file system.
Là lớp giao diện cho phép hỗ trợ đồng thời nhiều loại file system vật lý, còn được gọi là thực thi file system.
Lớp thứ 3 - physical file system.
Đây là lớp liên quan đến hoạt động vật lý của thiết bị lưu trữ (disk). Nó xử lý các khối vật lý cho việc đọc hoặc ghi. Nó xử lý các buffer, memory management và chịu trách nhiệm bố trí các khối vật lý trong những ví trí được chỉ định. Physical file system tương tác với device drivers hoặc các kênh tới thiết bị lưu trữ.

**Các khía cạnh của file system (Chỉ áp dụng với storage devices)**

File system chịu trách nhiệm tổ chức file và directories, chỉ định các phân vùng để lưu trữ hay không lưu trữ. Hiện tượng dư thừa các không gian lưu trữ mà không thể tận dụng được gọi là “slack space”. Độ lớn cấp phát cho các đối tượng được chọn khi file system được tạo. Việc chọn độ lớn cấp phát phụ thuộc vào độ lớn trung bình mà file cần cho việc lưu trữ. Thông thướng sẽ cấp phát hợp lý nhất cho việc lưu trữ.
Khái niệm phân mảnh xảy ra khi các không gian dư thừa, không lữu trữ hoặc các file đơn không tiếp giáp. Việc này xuất hiện trong quá trình sử dụng, tạo, sửa, xóa file. Khi file được tạo mới cần không gian lớn hơn các phân mảnh hiện có(hợp lý nhất cho việc lưu trữ). Các file được xóa nhưng không gian đang sử dụng lại không được cấp phát cho việc lưu trữ


 1. Filename
Filename được sử dụng để xác thực vị trí lưu trữ trong file system. Hầu hết file system có hạn chế về độ dài, quy tắc đặt tên “filename”.

 2. Directories
Filesystem còn có dạng directory, cho phép user nhóm các file riêng lẻ thành 1 tập hợp. Có thể được thực hiện bằng cách gán file với với số thứ tự trong table of content hoặc inode trong hệ điều hành nhân Unix. Directory có thể cho phép tổ chức dạng phân cấp chứ nhiều file và direc bên trong.

 3. Metadata
Thông tin đi kèm với mỗi file system. Như độ dài của dữ liệu file được lưu với số block được phân phát hoặc dung lượng. Thời gian chỉnh sửa cuối cùng. Thời gian tạo, khối, character, socket, .. Chủ sở hữu userID và group ID, quyển truy cập

**Multiple file systems chạy trên 1 hệ thống**

Thông thường, nhà phân phối thường cấu hình với 1 file system duy nhất trên toàn thiết bị lưu trữ. 1 số file system với thuộc tính khác nhau có thể cùng được sử dụng. VD, browser cache, được cấu hình để lưu trong nhưng bộ đệm cấp phát nhỏ, cho phép xóa, tạo mới liên tục mà không ảnh hưởng đến hệ thống lưu trữ.
Bên cạnh, 1 số hệ thống đám mây sử dụng "disk images" sử dụng file system khác nhau. Ví dụ dễ thấy nhất là ảo hóa, user có thể chạy định dạng ext4 của Linux trên máy ảo, mà máy ảo đó được lưu trữ trên định dạng NTFS windows. Ext4 file sustem được định dạng lại disk image, mà disk image được lưu trên NTFS host.

**Các loại file systems**

Disk file systems
VD: FAT (FAT12, FAT16, FAT32), exFAT, NTFS, HFS and HFS+, HPFS, APFS, UFS, ext2, ext3, ext4, XFS, btrfs, ISO 9660, Files-11, Veritas File System, VMFS, ZFS, ReiserFS and UDF. Some disk file systems are journaling file systems or versioning file systems.
Database file systems
Chuyển đổi file system
Cho phép chuyển đổi định dạng từ loại filesystem a thành 1 loại filesystem khác, cùng hoặc khác thuộc tính.
Network file systems
Shared disk file systems
