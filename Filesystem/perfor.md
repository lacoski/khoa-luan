# Nâng cao hiệu năng
---
## Giới thiệu
Để cải thiện hiệu năng, Disk controller thêm vào local memory nội tại để cache lại toàn bộ tracks. Khi hoạt động tìm kiếm diễn ra, track sẽ được đọc từ disk cache (giảm thời gian chờ).

Disk controller sau đó chuyển bất kỳ sector yêu cầu tới OS. Block được chọn sẽ truyền từ disk controller vào main memory (cache OS).

## Kỹ thuật Cache
2 loại cache của OS:
- buffer cache - system duy trì các phần khác nhau trong memory, các block được lưu theo tần số sử dụng với thời gian ngắn
- Page cache - Sử dụng kỹ thuật virtual memory, lưu lại file data như pages hơn là __file-system-oriented blocks__
 -  caching file data sử dụng virtual address đem lại hiệu năng cao hơn so với __cache physical blocks__.

Các OS mới nhất thường hỗ trợ cả 2 phương pháp. Kỹ thuật được biết __unified virtual memory.__

Với dòng Unix và Linux:
- Cung cấp kỹ thuật __unified buffer cache__
- Kỹ thuật sử dụng memory mapping
 - Memory mapping sử dụng 2 loại cache - __Page cache__ và __buffer cache__
 - Memory mapping thu được từ hoạt động đọc disk block bởi FS - Block sẽ được cache và buffer chace.
- Kỹ thuật sử dụng các hàm hệ thống chuẩn __read()__ và __write()__
 - Sử dụng chung trên page cache - giảm thiểu __double caching__, cho phép hệ thống virtual memory quản lý FS data.

> Hiện tượng __double caching__ xảy ra khi FS data được lưu lại 2 lần.

Ví dụ về hiện tượng __double caching__
- Data được lưu tại cả Buffer và Page cache (lãng phí tài nguyên hệ thống)

<div style="text-align:center"> <img src=https://raw.githubusercontent.com/lacoski/khoa-luan/master/Filesystem/PIC/perfor-1.png></div>

Phương pháp __unified buffer cache__

<div style="text-align:center"> <img src=https://raw.githubusercontent.com/lacoski/khoa-luan/master/Filesystem/PIC/perfor-2.png></div>

## Vấn đề ảnh hượng đến hiệu năng
Hoạt động đọc ghi đồng bộ và bắt đồng bộ ảnh hượng đến hiệu năng FS
- Synchronous writes – quả trình này sẽ diễn ra trực tiếp tới disk, sẽ không được lưu lại bộ đệm
 - Ảnh hướng đến hiệu năng vì không thế nhiều hơn 1 tiến trình sử dụng
 - Các hoạt động thường thấy: Chỉnh sửa, update metadata.
- Asynchronous write – data sẽ được cache lại để đồng bộ.
 - Hầu như hoạt động đọc ghi sử dụng kỹ thuật bất đồng bộ để nâng cao hiệu năng

## Cải thiện tương tác Cache, FS, Disk driver
Khi data được ghi tới disk, page được lưu vào bộ cache. Disk driver sắp xếp đầu ra hàng đợi theo disk address.
Hoạt động trên cho phép Disk driver tối ưu việc đầu đọc tìm kiếm, ghi data (vì queue tối ưu theo vòng quay).
