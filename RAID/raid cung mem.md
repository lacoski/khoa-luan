** Tổng quan Raid cứng, raid mềm **

** 1. Raid cứng **

RAID cứng thiết lập mảng đĩa cho hệ điều hành sẵn trước khi cài đặt hệ điều hành

RAID cứng: khi hệ điều hành sử dụng không tốn tài nguyên cho việc quản lý đĩa... liên quan đến RAID

RAID cứng chỉ hỗ trợ một định dạng ổ cứng hay khi thiết lập yêu cầu phần cứng khắt khe hơn và không thực hiện được với các ổ cứng bình thường như ATA


Ưu điểm: Độ ổn định cao, có ưu điểm là đẩy cao tốc độ của ổ cứng và có thể thay thế khi card raid hư vì nó không tích vào vào mainboard mà nó được cắm vào cổng PCI Express, nhưng vẫn đảm bảo được data khi chỉ hư card raid mà ổ cứng chứa dữ liệu vẫn còng nguyên không bị hư hại, bên cạnh đó còn có trình điều khiển và phần mềm quản lý, và phục hồi raid.

Nhược điểm: Giá thành cao, không sử dụng được cho các máy tính phổ thông (ví dụ: 1 pc bình thường)

** 2. Raid mềm **

RAID mềm cài đặt hệ điều hành rồi thiết lập RAID

RAID mềm: Phụ thuộc vào hệ điều hành nên bị tốn một phần tài nguyên hệ thống cho việc quản lý RAID

RAID mềm thực hiện được với nhiều loại ổ cứng khác nhau, dung lượng khác nhau... Và trên một đĩa vật lý có thể có những partition với RAID 0, có những partition với RAID 1, RAID 5.


Ưu điểm: Sử dụng cho các máy tính sử dụng các ổ cứng phổ thông nhưng vẫn nâng cao được hiệu năng cho máy tính.

Nhược điểm: Raid mềm chỉ là tập hợp con các tính năng của raid cứng, và nói về độ hư hại của raid mềm là rất lớn không có khả năng phuc hồi, vì raid mềm tích hợp trên mainboard, khi hư là hư main.
