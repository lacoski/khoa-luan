**Tổng quan về HDD**

**Giới thiệu**

Ổ đĩa cứng hay còn là Hard Disk Drive, viết tắt: HDD. Thiết bị dùng để lưu trữ dữ liệu trên bề mặt các tấm đĩa hình tròn phủ vật liệu từ tính.

Ổ đĩa cứng là loại bộ nhớ "không thay đổi" (non-volatile), không bị mất dữ liệu khi ngừng cung cấp nguồn điện cho chúng

Ổ đĩa cứng là một khối duy nhất, các đĩa cứng được lắp ráp cố định trong ổ ngay từ khi sản xuất nên không thể thay thế được các "đĩa cứng" như với cách hiểu như đối với ổ đĩa mềm hoặc ổ đĩa quang.

**Cấu tạo**

Ổ đĩa cứng gồm các thành phần, bộ phận có thể liệt kê cơ bản và giải thích sơ bộ như sau:

**1. Cụm đĩa: Bao gồm toàn bộ các đĩa, trục quay và động cơ.**

+ Đĩa từ.
+ Trục quay: truyền chuyển động của đĩa từ.
+ Động cơ: Được gắn đồng trục với trục quay và các đĩa.

**2. Cụm đầu đọc:**
+ Đầu đọc (head): Đầu đọc/ghi dữ liệu
+ Cần di chuyển đầu đọc (head arm hoặc actuator arm).

**3. Cụm mạch điện**
+ Mạch điều khiển: có nhiệm vụ điều khiển động cơ đồng trục, điều khiển sự di chuyển của cần di chuyển đầu đọc để đảm bảo đến đúng vị trí trên bề mặt đĩa.
+ Mạch xử lý dữ liệu: dùng để xử lý những dữ liệu đọc/ghi của ổ đĩa cứng.
+ Bộ nhớ đệm (cache hoặc buffer): là nơi tạm lưu dữ liệu trong quá trình đọc/ghi dữ liệu. Dữ liệu trên bộ nhớ đệm sẽ mất đi khi ổ đĩa cứng ngừng được cấp điện.
+ Đầu cắm nguồn cung cấp điện cho ổ đĩa cứng.
+ Đầu kết nối giao tiếp với máy tính.
+ Các cầu đấu thiết đặt (tạm dịch từ jumper) thiết đặt chế độ làm việc của ổ đĩa cứng: Lựa chọn chế độ làm việc của ổ đĩa cứng (SATA 150 hoặc SATA 300) hay thứ tự trên các kênh trên giao tiếp IDE (master hay slave hoặc tự lựa chọn), lựa chọn các thông số làm việc khác...

**4. Vỏ đĩa cứng:**

Phần đế chứa các linh kiện gắn trên nó, phần nắp đậy lại để bảo vệ các linh kiện bên trong.

chức năng:

+ Định vị các linh kiện và đảm bảo độ kín khít để không cho phép bụi được lọt vào bên trong của ổ đĩa cứng.
+ chịu đựng sự va chạm (ở mức độ thấp) để bảo vệ ổ đĩa cứng.

**5. Đĩa từ:**

<div style="text-align:center"> <img src=https://raw.githubusercontent.com/lacoski/khoa-luan/master/Hdd-SSD/PIC/track-sector-cylinder-cluster.png></div>

Đĩa từ (platter): Đĩa thường cấu tạo bằng nhôm hoặc thuỷ tinh, trên bề mặt được phủ một lớp vật liệu từ tính là nơi chứa dữ liệu. Sử dụng một hoặc cả hai mặt trên và dưới. Số lượng đĩa có thể nhiều hơn một, phụ thuộc vào dung lượng và công nghệ của mỗi hãng sản xuất khác nhau.

**Track**
  Trên một mặt làm việc của đĩa từ chia ra nhiều vòng tròn đồng tâm thành các track.

**Sector**

Trên track chia thành những phần nhỏ bằng các đoạn hướng tâm thành các sector. Các sector là phần nhỏ cuối cùng được chia ra để chứa dữ liệu. Theo chuẩn thông thường thì một sector chứa dung lượng 512 byte.

Số sector trên các track là khác nhau từ phần rìa đĩa vào đến vùng tâm đĩa, các ổ đĩa cứng đều chia ra hơn 10 vùng mà trong mỗi vùng có số sector/track bằng nhau.

**Cylinder**

Tập hợp các track cùng bán kính (cùng số hiệu trên) ở các mặt đĩa khác nhau thành các cylinder

Trên một ổ đĩa cứng có nhiều cylinder bởi có nhiều track trên mỗi mặt đĩa từ.

**6. Trục quay**

Trục quay là trục để gắn các đĩa từ lên nó, chúng được nối trực tiếp với động cơ quay đĩa cứng. Trục quay có nhiệm vụ truyền chuyển động quay từ động cơ đến các đĩa từ

**7. Đầu đọc/ghi**

Đầu đọc đơn giản được cấu tạo gồm lõi ferit (trước đây là lõi sắt) và cuộn dây (giống như nam châm điện)

Đầu đọc trong đĩa cứng có công dụng đọc dữ liệu dưới dạng từ hoá trên bề mặt đĩa từ hoặc từ hoá lên các mặt đĩa khi ghi dữ liệu.

Số đầu đọc ghi luôn bằng số mặt hoạt động được của các đĩa cứng, có nghĩa chúng nhỏ hơn hoặc bằng hai lần số đĩa (nhỏ hơn trong trường hợp ví dụ hai đĩa nhưng chỉ sử dụng 3 mặt).

**8. Cần di chuyển đầu đọc/ghi**

Cần di chuyển đầu đọc/ghi là các thiết bị mà đầu đọc/ghi gắn vào nó. Cần có nhiệm vụ di chuyển theo phương song song với các đĩa từ ở một khoảng cách nhất định, dịch chuyển và định vị chính xác đầu đọc tại các vị trí từ mép đĩa đến vùng phía trong của đĩa (phía trục quay).
