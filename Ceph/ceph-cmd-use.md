# Tổng hợp câu lệnh trên Ceph
---
## Các câu lệnh thường dùng
### Kiểm tra Cluster health
Quick overview về tình trạng Cluster, các lỗi (nếu có), tình trạng các OSD, node.
```
ceph status
```
### Kiểm tra dung lượng Ceph Cluster
Kiểm tra dung lượng đã sử dụng trên Ceph Cluster
```
ceph df
```
### Xem CRUSH map
```
ceph osd tree
```

### Tạo hoặc xóa OSD
```
ceph osd create || ceph osd rm osd.<id>
```
### List cluster key
```
ceph auth list
```

## Tập lệnh ceph-deploy
### 1. Tạo mới Cluster
```
ceph-deploy new <node-monitor>
```
### 2. Cài đặt Ceph
```
ceph-deploy install <nodes>
```
### 3. Khởi tạo key monitor và gather
```
ceph-deploy mon create <node-monitor>
ceph-deploy gatherkeys <node-monitor>

# Tự động tạo theo ceph.conf

ceph-deploy mon create-initial
```

### 4. Chuẩn bị OSD
__Xóa bỏ dữ liệu cũ trên device disk__

```
ceph-deploy disk zap <nodes:/path/to/device>
```

__Tạo OSD__
```
ceph-deploy osd create <nodes:/path/to/device>
```
hoặc

```
# Chuẩn bị OSD
ceph-deploy osd prepare <nodes:/path/to/device>

# Kích hoạt OSD
ceph-deploy osd activate <nodes:/path/to/device>
```

### 5. Thiết lập management-key liên kết các node
```
ceph-deploy admin <nodes>

# Set lại quyền trên các node
sudo chmod 644 /etc/ceph/ceph.client.admin.keyring
```
### 6. Đẩy cấu hình tới các node
```
ceph-deploy --overwrite-conf config push ceph-admin ceph-node-1 ceph-node-2
```
