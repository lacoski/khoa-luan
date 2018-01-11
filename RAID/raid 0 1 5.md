# RAID
---

**Tìm hiểu về RAID**

RAID là viết tắt của Redundant Array of Inexpensive Disks (Hệ thống đĩa dự phòng).

Đây là hệ thống hoạt động bằng cách kết nối một dãy các ổ cứng có chi phí thấp lại với nhau để hình thành một thiết bị nhớ đơn có dung lượng lớn hỗ trợ hiệu quả cao và đáng tin cậy.

RAID được sử dụng và triển khai thành phương pháp lưu trữ trong doanh nghiệp và các máy chủ, nhưng trong 5 năm sau đó RAID đã trở nên phổ biến đối với mọi người dùng.

**Lợi thế của RAID**

Có 3 lý do chính để áp dụng RAID:

+ Dự phòng

+ Hiệu quả cao

+ Giá thành thấp

Sự dự phòng là nhân tố quan trọng nhất trong RAID. Cho phép sao lưu dữ liệu bộ nhớ khi gặp sự cố. Nếu một ổ cứng trong dãy bị trục trặc thì có thể thay thế sang ổ cứng khác mà không cần tắt cả hệ thống. Phương pháp dự phòng phụ thuộc vào phiên bản RAID được sử dụng.

Khi áp dụng các phiên bản RAID mạnh, hiệu năng có thể tăng cao. Hiệu quả cũng tùy thuộc vào số lượng ổ cứng được liên kết với nhau và các mạch điều khiển.

Raid được chia làm nhiều loại. Các Raid được sử dụng có thể là Raid 0, Raid 1, Raid 3, Raid 4, Raid 5, RAID 01, RAID 03, RAID 10, RAID 50, RAID 60,...

**1. RAID 0:**

<div style="text-align:center"> <img src="https://raw.githubusercontent.com/lacoski/khoa-luan/master/RAID/PIC/Raid%200.png"></div>

Raid 0 là loại Raid khá phổ biến, bởi có khả năng nâng cao hiệu suất tốc độc đọc ghi trao đổi dữ liệu của ổ cứng.
Để tiến hành setup Raid 0 thì cần tối thiểu 2 ổ đĩa (Disk 0, Disk 1).

Cách lưu trữ dữ liệu: Có 1 file A dung lượng 100MB. Khi tiến hành lưu trữ thay vì file A sẽ được lưu vào 1 ổ cứng duy nhất, Raid 0 sẽ lưu vào 2 ổ đĩa disk 0, disk 1 mỗi ổ 50MB (Striping), giảm thời gian đọc ghi xuống 1 nửa theo lý thuyết.

Ưu điểm:
   - Tốc độ đọc ghi nhanh (gấp đôi bình thường theo lý thuyết).

Nhược điểm:
- Tiềm ẩn rủi ro về dữ liệu. Lý do dữ liệu được chia đôi lưu trên 2 ổ đĩa.Trường hợp 1 trong 2 ổ đĩa bị hỏng thì nguy cơ mất dữ liệu rất cao. Về ổ cứng yêu cầu phải 2 ổ cùng dung lượng, nếu 2 ổ khác dung lượng thì lấy ổ thấp nhất.

Đối tượng sử dụng:
 - Thích hợp với những dịch vụ cần lưu trữ và truy xuất với tốc độ cao. Chẳng hạn như dịch vụ video streaming, chạy cơ sở dữ liệu.

**2. RAID 1:**

<div style="text-align:center"> <img src="https://raw.githubusercontent.com/lacoski/khoa-luan/master/RAID/PIC/Raid%201.png"></div>

Raid 1 là loại Raid cơ bản. Tăng độ an toàn về dữ liệu. Tiến hành setup Raid 1 cần tối thiểu 2 ổ cứng để lưu trữ.

Raid 1 đảm bảo an toàn dữ liệu do dữ liệu được ghi vào 2 ổ giống hệt nhau (Mirroring).

Ưu điểm:
- An toàn về dữ liệu, trường hợp 1 trong 2 ổ đĩa bị hỏng thì dữ liệu vẫn có khả năng đáp ứng dịch vụ.

Nhược điểm:
- Hiệu suất không cao, Nâng cao chi phí (giả sử khách hàng sử dụng 2 ổ cứng 500GB. Khi sử dụng Raid 1 thì dung lượng lưu trữ có thể sử dụng chỉ được 500GB). Về ổ cứng yêu cầu phải 2 ổ cùng dung lượng, nếu 2 ổ khác dung lượng thì lấy ổ thấp nhất.

Đối tượng sử dụng:
- Các dịch vụ lưu trữ, các website vừa và nhỏ không yêu cầu quá cao về tốc độ đọc ghi (in/out) của ổ cứng. Các đối tượng yêu cầu sự an toàn về dữ liệu như các dịch vụ kế toán,lưu trữ thông tin khách hàng, bất động sản v.v…

**   3. RAID 10:**

<div style="text-align:center"><img src="https://raw.githubusercontent.com/lacoski/khoa-luan/master/RAID/PIC/raid10.png"></div>

Raid 10 là sự kết hợp giữa 2 loại raid phổ biến và Raid 1 và Raid 0. Để setup Raid 10 cần sử dụng tối thiểu 4 ổ cứng (Disk 0, Disk 1, Disk 2, Disk 3).

Dữ liệu sẽ được lưu đồng thời vào 4 ổ cứng. 2 ổ dạng Striping (Raid 0) và 2 ổ (Mirroring) Raid 1.

Ưu điểm:
- Đây là 1 hình thức lưu trữ nhanh nhẹn và an toàn, vừa nâng cao hiệu suất mà lại đảm bảo dữ liệu không bị thất thoát khi 1 trong số 4 ổ cứng bị hỏng.

Nhược điểm:
- Chi phí cao. Đối với Raid 10 dung lượng sẵn sàng sử dụng chỉ bằng ½ dung lượng của 4 ổ. (giống như raid 1).

Đối tượng sử dụng:
- Raid 10 thích hợp với tất cả các đối tượng sử dụng (từ những yêu cầu về hiệu suất đến việc đảm bảo an toàn dữ liệu). Về ổ cứng yêu cầu phải 4 ổ cùng dung lượng, nếu 4 ổ khác dung lượng thì lấy ổ thấp nhất.

**4. RAID 5:**

<div style="text-align:center"> <img src="https://raw.githubusercontent.com/lacoski/khoa-luan/master/RAID/PIC/r5.png"></div>

Nguyên tắc của Raid 5 gần giống với 2 loại raid lưu trữ truyền thống là Raid 1 và Raid 0. Tách ra lưu trữ các ổ cứng riêng biệt và có phương án dự phòng khi có sự cố phát sinh đối với 1 ổ cứng bất kì trong cụm.

Setup Raid 5 cần tối thiểu 3 ổ cứng. Giả sử có 1 file A thì khi lưu trữ sẽ tách ra 3 phần A1, A2, A3. Ba phần nãy sẽ tương ứng lưu trên ổ đĩa Disk 0, Disk 1, Disk 2, còn ổ đĩa Disk 3 sẽ giữ bản sao lưu backup của 3 phần này. Tương tự các file sau cũng vậy và tùy theo tiến trình thực hiện mà bản sao lưu có thể được lưu ở bất kì 1 trong những ổ trong cụm Raid.

Ưu điểm:
- Nâng cao hiệu suất, an toàn dữ liệu, tiết kiệm chi phí hơn so với hình thức lưu trữ Raid 10.

Nhược điểm:
- Chi phí phát sinh thêm 1 ổ so với hình thức lưu trữ thông thường. (tổng dung lượng ổ cứng sau cùng sẽ bằng tổng dung lượng đĩa sử dụng trừ đi 1 ổ. Giả sử bạn có 4 ổ 500GB thì dung lượng sử dụng sau cùng khi triển khai Raid 5 bạn chỉ còn 1500GB).

Đối tượng sử dụng:
- Tất cả những website, dịch vụ, ứng dụng có số lượng truy cập và yêu cầu tài nguyên từ nhỏ đến vừa và lớn.

**5. RAID 6**
<div style="text-align:center"> <img src="https://raw.githubusercontent.com/lacoski/khoa-luan/master/RAID/PIC/r6.png"></div>

Mở rộng, và tương tự RAID 5. Lặp lại nhiều hơn số lần sự phân tách dữ liệu để ghi vào các đĩa cứng khác nhau
RAID 6 yêu cầu tối thiểu 4 ổ cứng.

Ưu điểm:
- Nâng cao hiệu suất, an toàn dữ liệu, tiết kiệm chi phí hơn so với hình thức lưu trữ Raid 10.

Nhược điểm:
- Chi phí phát sinh sẽ cao hơn RAID 5.

**5. JBOD**


JBOD (Just a Bunch Of Disks) thực tế không phải là một dạng RAID chính thống, nhưng lại có một số đặc điểm liên quan tới RAID và được đa số các thiết bị điều khiển RAID hỗ trợ. JBOD cho phép bạn gắn bao nhiêu ổ đĩa tùy thích vào bộ điều khiển RAID của mình (dĩ nhiên là trong giới hạn cổng cho phép). Sau đó chúng sẽ được &quot;tổng hợp&quot; lại thành một đĩa cứng lớn hơn cho hệ thống sử dụng. Ví dụ bạn cắm vào đó các ổ 10GB, 20GB, 30GB thì thông qua bộ điều khiển RAID có hỗ trợ JBOD, máy tính sẽ nhận ra một ổ đĩa 60GB. Tuy nhiên, lưu ý là JBOD không hề đem lại bất cứ một giá trị phụ trội nào khác: không cải thiện về hiệu năng, không mang lại giải pháp an toàn dữ liệu, chỉ là kết nối và tổng hợp dung lượng mà thôi.

6.
