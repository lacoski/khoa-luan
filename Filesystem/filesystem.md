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

**Các tùy chọn file system:**

file hệ thống Linux Ext

 - Ext – Extended file system: là định dạng file hệ thống đầu tiên được thiết kế dành riêng cho Linux. Có tổng cộng 4 phiên bản và mỗi phiên bản lại có 1 tính năng nổi bật. Phiên bản đầu tiên của Ext là phần nâng cấp từ file hệ thống Minix được sử dụng tại thời điểm đó, nhưng lại không đáp ứng được nhiều tính năng phổ biến ngày nay. Và tại thời điểm này, chúng ta không nên sử dụng Ext vì có nhiều hạn chế, không còn được hỗ trợ trên nhiều distribution.

 - Ext2 thực chất không phải là file hệ thống journaling, được phát triển để kế thừa các thuộc tính của file hệ thống cũ, đồng thời hỗ trợ dung lượng ổ cứng lên tới 2 TB. Ext2 không sử dụng journal cho nên sẽ có ít dữ liệu được ghi vào ổ đĩa hơn. Do lượng yêu cầu viết và xóa dữ liệu khá thấp, cho nên rất phù hợp với những thiết bị lưu trữ bên ngoài như thẻ nhớ, ổ USB... Còn đối với những ổ SSD ngày nay đã được tăng tuổi thọ vòng đời cũng như khả năng hỗ trợ đa dạng hơn, và chúng hoàn toàn có thể không sử dụng file hệ thống không theo chuẩn journaling.

 - Ext3 về căn bản chỉ là Ext2 đi kèm với journaling. Mục đích chính của Ext3 là tương thích ngược với Ext2, và do vậy những ổ đĩa, phân vùng có thể dễ dàng được chuyển đổi giữa 2 chế độ mà không cần phải format như trước kia. Tuy nhiên, vấn đề vẫn còn tồn tại ở đây là những giới hạn của Ext2 vẫn còn nguyên trong Ext3, và ưu điểm của Ext3 là hoạt động nhanh, ổn định hơn rất nhiều. Không thực sự phù hợp để làm file hệ thống dành cho máy chủ bởi vì không hỗ trợ tính năng tạo disk snapshot và file được khôi phục sẽ rất khó để xóa bỏ sau này.

 - Ext4: cũng giống như Ext3, lưu giữ được những ưu điểm và tính tương thích ngược với phiên bản trước đó. Như vậy, chúng ta có thể dễ dàng kết hợp các phân vùng định dạng Ext2, Ext3 và Ext4 trong cùng 1 ổ đĩa trong Ubuntu để tăng hiệu suất hoạt động. Trên thực tế, Ext4 có thể giảm bớt hiện tượng phân mảnh dữ liệu trong ổ cứng, hỗ trợ các file và phân vùng có dung lượng lớn... Thích hợp với ổ SSD so với Ext3, tốc độ hoạt động nhanh hơn so với 2 phiên bản Ext trước đó, cũng khá phù hợp để hoạt động trên server, nhưng lại không bằng Ext3.

 - BtrFS – thường phát âm là Butter hoặc Better FS, hiện tại vẫn đang trong giai đoạn phát triển bởi Oracle và có nhiều tính năng giống với ReiserFS. Đại diện cho B-Tree File System, hỗ trợ tính năng pool trên ổ cứng, tạo và lưu trữ snapshot, nén dữ liệu ở mức độ cao, chống phân mảnh dữ liệu nhanh chóng... được thiết kế riêng biệt dành cho các doanh nghiệp có quy mô lớn.

Mặc dù BtrFS không hoạt động ổn định trên 1 số nền tảng distro nhất định, nhưng cuối cùng thì nó vẫn là sự thay thế mặc định của Ext4 và cung cấp chế độ chuyển đổi định dạng nhanh chóng từ Ext3/4. Do vậy, BtrFS rất phù hợp để hoạt động với server dựa vào hiệu suất làm việc cao, khả năng tạo snapshot nhanh chóng cũng như hỗ trợ nhiều tính năng đa dạng khác.

Bên cạnh đó, Oracle cũng đang cố gắng phát triển 1 nền tảng công nghệ nhằm thay thế cho NFS và CIFS gọi là CRFS với nhiều cải tiến đáng kể về mặt hiệu suất và tính năng hỗ trợ. Những cuộc kiểm tra trên thực tế đã chỉ ra BtrFS đứng sau Ext4 khi áp dụng với các thiết bị sử dụng bộ nhớ Flash như SSD, server database...

 - ReiserFS: có thể coi là 1 trong những bước tiến lớn nhất của file hệ thống Linux, lần đầu được công bố vào năm 2001 với nhiều tính năng mới mà file hệ thống Ext khó có thể đạt được. Nhưng đến năm 2004, ReiserFS đã được thay thế bởi Reiser4 với nhiều cải tiến hơn nữa. Tuy nhiên, quá trình nghiên cứu, phát triển của Reiser4 khá “chậm chạp” và vẫn không hỗ trợ đầy đủ hệ thống kernel của Linux. Đạt hiệu suất hoạt động rất cao đối với những file nhỏ như file log, phù hợp với database và server email.

XFS được phát triển bởi Silicon Graphics

 - XFS được phát triển bởi Silicon Graphics từ năm 1994 để hoạt động với hệ điều hành riêng biệt của họ, và sau đó chuyển sang Linux trong năm 2001. Khá tương đồng với Ext4 về một số mặt nào đó, chẳng hạn như hạn chế được tình trạng phân mảnh dữ liệu, không cho phép các snapshot tự động kết hợp với nhau, hỗ trợ nhiều file dung lượng lớn, có thể thay đổi kích thước file dữ liệu... nhưng không thể shrink – chia nhỏ phân vùng XFS. Với những đặc điểm như vậy thì XFS khá phù hợp với việc áp dụng vào mô hình server media vì khả năng truyền tải file video rất tốt. Tuy nhiên, nhiều phiên bản distributor yêu cầu phân vùng /boot riêng biệt, hiệu suất hoạt động với các file dung lượng nhỏ không bằng được khi so với các định dạng file hệ thống khác, do vậy sẽ không thể áp dụng với mô hình database, email và một vài loại server có nhiều file log. Nếu dùng với máy tính cá nhân, thì đây cũng không phải là sự lựa chọn tốt nên so sánh với Ext, vì hiệu suất hoạt động không khả thi, ngoài ra cũng không có gì nổi trội về hiệu năng, quản lý so với Ext3/4.

JFS được IBM phát triển lần đầu tiên năm 1990

 - JFS được IBM phát triển lần đầu tiên năm 1990, sau đó chuyển sang Linux. Điểm mạnh rất dễ nhận thấy của JFS là tiêu tốn ít tài nguyên hệ thống, đạt hiệu suất hoạt động tốt với nhiều file dung lượng lớn và nhỏ khác nhau. Các phân vùng JFS có thể thay đổi kích thước được nhưng lại không thể shrink như ReiserFS và XFS, tuy nhiên nó lại có tốc độ kiểm tra ổ đĩa nhanh nhất so với các phiên bản Ext.

ZFS hiện tại vẫn đang trong giai đoạn phát triển bởi Oracle

 - ZFS hiện tại vẫn đang trong giai đoạn phát triển bởi Oracle với nhiều tính năng tương tự như Btrfs và ReiserFS. Mới xuất hiện trong những năm gần đây vì có tin đồn rằng Apple sẽ dùng nó làm file hệ thống mặc định. Phụ thuộc vào thỏa thuận điều khoản sử dụng, Sun CDDL thì ZFS không tương thích với hệ thống nhân kernel của Linux, tuy nhiên vẫn hỗ trợ toàn bộ Linux’s Filesystem in Userspace – FUSE để có thể sử dụng được ZFS. Người sử dụng có thể gặp khó khăn khi cài đặt hệ điều hành Linux vì có yêu cầu FUSE và có thể không được hỗ trợ bởi distributor.

 - Swap có thể coi thực sự không phải là 1 dạng file hệ thống, bởi vì cơ chế hoạt động khá khác biệt, được sử dụng dưới 1 dạng bộ nhớ ảo và không có cấu trúc file hệ thống cụ thể. Không thể kết hợp và đọc dữ liệu được, nhưng lại chỉ có thể được dùng bởi kernel để ghi thay đổi vào ổ cứng. Thông thường, nó chỉ được sử dụng khi hệ thống thiếu hụt bộ nhớ RAM hoặc chuyển trạng thái của máy tính về chế độ Hibernate.

Trên đây là một số thông tin cơ bản về cấu trúc file hệ thống trong Linux, hy vọng rằng có thể giúp các bạn hiểu rõ hơn về hệ điều hành mã nguồn mở này cũng như kinh nghiệm lựa chọn file system sao cho phù hợp với nhu cầu, mục đích sử dụng. Chúc các bạn thành công!
