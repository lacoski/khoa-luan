# Kế hoạch xây dựng hạ tầng ceph
---
## Kế hoạch phần cứng cho Ceph Cluster
Ceph là software-based storage system, được thiết kế chạy tương thích với các phần cứng chung. Tính năng này của Ceph khiến nó đáp ứng được các yêu cầu về chi phí, tính mở rộng, giải pháp của các nhà cung cấp.

Cluster hardware config yêu cầu phải có kế hoạch khi xây dựng storage. Loại hardware sử dụng cho thiết kế cluster cần được cân nhắc trước khi khởi tạo project. Kế hoạch càng chi tiết giúp cho thiết kế tránh khởi các vấn đề về nghẽn cổ chai, tính bảo đảm của cluster. Chọn phần cứng dựa trên sự đa dạng của các nhà cung cấp, túi tiền nhưng các yếu tố phải xem xét cẩn trọng là hiệu năng, sức chứa hoặc cả 2, mức chịu lỗi, các phương pháp xử lý.

## Yêu cầu giám sát
Ceph monitor chịu trách nhiệm theo dõi sức khỏe của toàn cluster bằng các duy trì cluster map. Nó thuộc 1 phần lưu trữ cluster data. Vì thế nó sẽ không đòi hỏi CPU cũng như Memo, yêu cầu tài nguyên thấp, thiết bị phần cứng cơ bản + vài gb ram là đủ cho monitor node.

Có thể tận dụng các tài nguyên server sẵn có để cài Ceph Monitor. (Cài lên server đang chạy dịch vụ khác). Trong môi trường nonproduction, khi đang cân nhắc phần cứng và túi tiền, ta có thể sử dụng Ceph monitor trên máy vật lý cách biệt với các máy ảo.

Nếu cần cấu hình cluster lưu log trên local monitor node, chắc chắn ổ đĩa vật lý máy chủ đủ dụng lượng để monitor node lưu log. Trên các healthy cluster, log có thể tăng lên tầm vài gb, nhưng đôi với unhealthy cluster khi đang chạy debugging level, nó có thể tăng lên vài gb trong thời gian  ngắn. Vì vậy cần cân nhắc hạ tằng, thiết kế monitor node phù hợp

Network cho monitor node cần được cung cấp đủ hoặc thừa. NIC tổi thiểu 1 Gbs được khuyên dùng. Vì khi xuất hiện lỗi trên cluster, node report rất nhiều. Đối với môi trường test, đây không phải vấn đề đang lưu tâm nhưng môi trường produc thì sẽ là 1 vấn đề lớn

## Yêu cầu trên OSD
Ceph cơ bản sẽ tạo 1 OSD cho mỗi physical disk trong cluster node. Tuy nhiên OSD hỗ trợ triển khai linh hoạt trên OSD/disk và OSD/Raid.

Ceph OSD yêu cầu:
+ CPU, memory
+ OSD journal (block device or a file)
+ underlying filesystem (XFS, ext4, and Btrfs)
+ separate OSD or cluster redundant network (recommended)
Cấu hình khuyến cáo: CPU 1 GHz, RAM 2gb/OSD để đáp ứng cho môi trường cluster, và khả năng chịu lỗi của cluster. Tiến trình khôi phục yêu cầu nhiều tài nguyên.
OSD là đơn vị lưu trữ chính trong Ceph, vì vậy yêu cầu nhiều disk để tăng khả năng lưu trữ, nhưng vấn đề cần quan tầm là tăng memory để tăng hiểu quả sử dụng disk.

Từ góc độ hiệu năng, ta cần cân nhắc tách biệt OSD journal disk trên OSD. Hiệu năng sẽ được cải thiện khi OSD journal được tạo trên SSD disk và OSD data tạo trên Hdd. SSD disk sử dụng như journal sẽ cải thiện hiệu năng cluster và giải quyết các công việc nặng nhanh chóng, hiệu quả. Tuy nhiên sử dụng SSD sẽ gia tăng chi phí. Vì vậy nên thiết kế 1 SSD phục nhiều nhiều OSD disk, nhưng đổi lại, nếu SSD journal disk lỗi, toàn bộ data trên các OSD liên kết sẽ mất => tránh sử dụng quả tải SSD, cơ bản dùng 1 SSD/ 2-4 OSD.

+ Yêu cầu Network

Từ khía cạnh network, ceph yêu cầu cluster cần ít nhất 2 mạng riêng, 1 cho public network (front-side data network) và 1 cho clustered network (backside data network).

2 mạng sẽ tách biệt client data với ceph cluster traffic. Vì hầu như mọi thời điểm, Ceph cluster sẽ sử dụng cluster traffic để thực hiện quá trình nhân bản object, phục hồi trong trường hợp phát sinh lỗi. Nếu sử dụng chung 2 đường có thể sẽ giảm hiệu năng. Tuy nhiên ta có thể sử dụng chung 1 đường vật lý của cả 2 mạng. Về cơ bản, khuyến cáo 1 Gb cho mỗi đường. Tuy nhiên để có hiệu năng tốt nhất  nên sử dụng đường mạng 10 gb (trong trường hợp thiết kế cluster mở rộng trong tương lai, tránh tính trạng nghẽn cổ chai).

Cần tính toán sự dư thừa cho mỗi tầng mạng như network controller, ports, switches, router. Public network cung cấp các liên kết giữa client – ceph cluster. Mỗi quá hđ I/O giữa client và Ceph cluster sẽ sử dụng kết nối. Cần chắc chắn có đủ băng thông cho client. Với cluster network, Ceph OSD sẽ sử dụng mạng này. Chúng sẽ nhân bản, sử dụng đường truyền, tất cả hoạt động này diễn ra qua cluster network, đồng thời sử dụng cho quá trình tái cân bằng data, khôi phục.

Vì vậy cần dựa theo hạ tầng, đánh giá để thiết kế đường truyền phù hợp, nâng cao hiệu năng, chất lượng.

## Yêu cầu MDS

Ceph MDS sử dụng  nhiều tài nguyên. Yêu cầu nhiều CPU process (>= 4 core). Ceph MDS phụ thuộc vào data caching, cũng như để nhanh chóng đáp ứng yêu cầu => sử dụng nhiều RAM. Ram cao trên Ceph MDS sẽ cải thiện hiệu năng CephFS,  vì vậy cần đặt Ceph MDS trên máy vật lý với nhiều RAM, CPU (core). Yêu cầu network cho Ceph MDS = 1 GB.
