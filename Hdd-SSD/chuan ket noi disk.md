**Chuẩn kết nối ổ đĩa**

**Các chuẩn giao tiếp dùng trong thiết bị lưu trữ**

- Chuẩn giao tiếp dữ liệu là tập hợp các tập lệnh giúp trao đổi dữ liệu giữa máy tính và thiết bị lưu trữ.

- So sánh các giao diện kết nối ổ cứng ra bên ngoài. Các loại giao diện chính bao gồm IDE, SATA, SCSI, SAS và FC.

- Mỗi loại giao diện có khả năng truyền dữ liệu với tốc độ nhất định (throughput – tốc độ truyền dữ liệu data transfer speed, đơn vị Gbps, Mbps…).

**Giao tiếp IDE**
- Chuẩn IDE sử dụng phương thức truyền tải dữ liệu song song.

Ưu điểm:
- Truyền tải song song là tốc độ cao. Trong cùng một thời điểm có thể truyền tải nhiều bit dữ liệu hơn so với truyền tải nối tiếp.

Nhược điểm:
- Sử dụng nhiều dây dẫn để truyền các bit dữ liệu đi nên gây ra hiện tượng tạp âm nhiễu. Đó là lý do vì sao ATA-66 sử dụng lên đến 80 dây dẫn. Bởi vì giữa các dây truyền tín hiệu là các dây đất nằm xen kẽ dây tín hiệu để chống nhiễu

**Chuẩn EIDE:**
- Chia làm 2 kênh (primary và secondary) và 2 kênh này sử dụng 2 đường BUS riêng. Trên mỗi kênh lại chia làm 2 cấp (master và slaver) trên cùng 1 kênh. Vì cả 2 thiết chỉ được phép sử dụng 1 đường BUS trong cùng 1 thời điểm.
+ EIDE không có khả năng cho phép nhiều thiết bị sử dụng nhiều thiết bị trên cùng 1 BUS trong cùng 1 thời điểm. Nên các thiết bị sẽ được cấp phép để sử dụng tuần tự đường BUS.

**Chuẩn ATA (còn được gọi là PATA –Parallel ATA)**
- Chuẩn ATA có dung lượng thấp và tốc độ truyền thấp. Được phát triển và mở rộng dần dần từ ATA1 đến ATA 7. Để nâng cao hiệu suất của EIDE mà không làm tăng chi phí, không còn cách nào khác người ta phải thay thế kiểu giao diện song song bằng kiểu nối tiếp. Kết quả là chuẩn SATA ra đời.

**Chuẩn SATA**
- Chuẩn SATA 3.0 với băng thông dữ liệu lên đến 6Gb/s và nó cho phép tương thích ngược với các chuẩn cũ (SATA 1.0, SATA 2.0). Sử dụng công nghệ cho phép truyền tải theo chế độ nối tiếp.
- Một giao diện SATA chỉ cho phép gắn 1 ổ cứng duy nhất.
- Các chip điều khiển SATA đều hỗ trợ chuẩn giao tiếp AHCI (Advanced Host Controller Interface) làm giao tiếp chuẩn, hỗ trợ các tính năng nâng cao. Tháo lắp nóng. Tính năng này chỉ hỗ trợ khi thiết bị chạy chế độ AHCI.
- Thường sử dụng cho các thiết bị máy tính cá nhân. Sử dụng cable 7 chân và chỉ có 7 sợi nên gọn hơn nhiều so với ATA. Điều này giúp ích rất nhiều đến khía cạnh tỏa nhiệt của máy tính, vì sử dụng nhiều cáp mỏng hơn sẽ làm cho không khí lưu thông bên trong case của máy tính được dễ dàng hơn.
