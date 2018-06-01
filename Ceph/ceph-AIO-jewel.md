# Cài đặt Ceph All in one Jewel - CentOS 7
---
## Chuẩn bị

### Về tài nguyên
__Yêu cầu sử dụng 1 node, chạy CentOS 7 64bit__
```
CPU         4 core
RAM         4 GB

Disk        sba: os
            sbd: 1 disk journal
            sdc,sde,sdd: 3 disk osd

Network     ens33: 1 replicate data
            ens34: 1 access ceph
```


## Cài đặt
### Phần 1 - Cấu hình chuẩn bị trên node
#### Bước 1: Tạo Ceph User
Tạo Ceph user 'cephuser' trên tất các các nodes.
```
useradd -d /home/cephuser -m cephuser
passwd cephuser
```
Cấp quyền root cho user vừa tạo
```
echo "cephuser ALL = (root) NOPASSWD:ALL" | sudo tee /etc/sudoers.d/cephuser
chmod 0440 /etc/sudoers.d/cephuser
sed -i s'/Defaults requiretty/#Defaults requiretty'/g /etc/sudoers
```
#### Bước 2: Cấu hình NTP
Sử dụng NTP đồng bộ thời gian trên tất cả các Node.
> Ở đây sử dụng NTP pool US.

```
yum install -y ntp ntpdate ntp-doc
ntpdate 0.us.pool.ntp.org
hwclock --systohc
systemctl enable ntpd.service
systemctl start ntpd.service
```

#### Bước 3 (Tùy chọn): Nếu sử dụng VMware, cần sử dụng công cụ hỗ trợ
```
yum install -y open-vm-tools
```

#### Bước 4: Hủy bỏ SELinux
```
sed -i 's/SELINUX=enforcing/SELINUX=disabled/g' /etc/selinux/config
```

#### Bước 5: Cấu hình Host File
```
vim /etc/hosts
```
Nội dung
```
# content vim
192.168.2.133 ceph-aio
```
> Ping thử tới host, kiếm tra Network

### Phần 2: Cấu hình SSH Server
> __Cấu hình trên ceph-aio node__

#### Bước 1: Truy cập ceph-aio
```
ssh root@ceph-aio
```

#### Bước 2: Tạo ssh-key
```
ssh-keygen
```
> Để khoảng trắng trên các lựa chọn


#### Bước 3: Cấu hình ssh file
```
vim ~/.ssh/config
```
Nội dung
```
# content vim

Host ceph-aio
        Hostname ceph-admin
        User cephuser
```
Thay đổi quyền trên file
```
chmod 644 ~/.ssh/config
```

Chuyển ssh-key tới known_hosts
```
ssh-keyscan ceph-aio >> ~/.ssh/known_hosts
```

> Yều cầu nhập passwd trong lần đầu tiền truy cập

### Phần 3: Cấu hình Firewalld
#### Tùy chọn 1: Cấu hình dựa theo lab
Bỏ qua cấu hình firewall
```
systemctl stop firewalld
systemctl disable firewalld
```

#### Tùy chọn 2: Cấu hình firewalld (Chưa kiểm chứng)
##### Xem thêm
[Cài đặt Ceph trên CentOS 7](ceph-install-lab.md)

### Phần 4: Thiết lập Ceph Cluster
Tại phân này, ta sẽ cài đặt tât cả các thành phần ceph lên 1 node duy nhất.

#### Bước 1: Truy cập ceph-admin node
```
ssh root@ceph-aio
```

#### Bước 2: Cài đặt ceph-deploy trên ceph-aio
Thêm Ceph repo và cài đặt gói thiết lập Ceph với yum cmd
```
sudo rpm -Uhv http://download.ceph.com/rpm-jewel/el7/noarch/ceph-release-1-1.el7.noarch.rpm
sudo yum update -y && sudo yum install ceph-deploy -y
```

#### Bước 3: Tạo mới Ceph Cluster config
Tạo cluster directory
```
mkdir cluster
cd cluster/
```

Tạo mới cluster config với 'ceph-deploy' command, thiết lập monitor node = 'ceph-admin'
```
ceph-deploy new ceph-aio
```

Cấu hình ceph.conf
```
vim ceph.conf
```
Nội dung
```
# Content vim (Sửa lại theo lab)
[global]
fsid = 60643eb6-a568-42ae-b665-114807627e09
auth_cluster_required = cephx
auth_service_required = cephx
auth_client_required = cephx

# Cấu hình Network
public network = 192.168.2.0/24
cluster network = 192.168.3.0/24

# Cấu hình monitor node
[mon]
mon host = ceph-aio
mon initial members = ceph-aio

[mon.ceph-aio]
host = ceph-aio
mon addr = 192.168.2.133
```

#### Bước 4: Cài đặt Ceph
Cài đặt Ceph trên Ceph AIO.
```
ceph-deploy install ceph-aio
```

Thiết lập ceph mon
```
ceph-deploy mon create-initial
```

#### Bước 5: Thiết lập OSD

```
ceph-deploy disk list ceph-aio
```

Xóa partition tables trên tất cả ổ đĩa với zap option
```
ceph-deploy disk zap ceph-aio:/dev/sdb
ceph-deploy disk zap ceph-aio:/dev/sdc
ceph-deploy disk zap ceph-aio:/dev/sdd
ceph-deploy disk zap ceph-aio:/dev/sde
```
> cmd trên sẽ xóa toàn bộ dữ liệu hiện tại trên ổ

Thiết lập OSD node
```
ceph-deploy osd prepare ceph-aio:sdc:/dev/sdb
ceph-deploy osd prepare ceph-aio:sdd:/dev/sdb
ceph-deploy osd prepare ceph-aio:sde:/dev/sdb
```
> Ở đây, ta sẽ sử dụng /dev/sdb là ổ đĩa journal (cache dữ liệu)

Kích hoạt OSD
```
ceph-deploy osd activate ceph-aio:/dev/sdc1:/dev/sdb1
ceph-deploy osd activate ceph-aio:/dev/sdd1:/dev/sdb2
ceph-deploy osd activate ceph-aio:/dev/sde1:/dev/sdb3
```
> Ở đây, ta sẽ sử dụng /dev/sdb là ổ đĩa journal (cache dữ liệu)


Kết quả, /dev/sdb sẽ tách làm 3 phần vùng
- /dev/sdb1 - Ceph Journal của /dev/sdc1
- /dev/sdb2 - Ceph Journal của /dev/sdd1
- /dev/sdb3 - Ceph Journal của /dev/sde1

Kiểm tra tại OSD node
```
[root@ceph-aio ~]# lsblk
NAME        MAJ:MIN RM  SIZE RO TYPE MOUNTPOINT
fd0           2:0    1    4K  0 disk
sda           8:0    0   20G  0 disk
├─sda1        8:1    0    1G  0 part /boot
└─sda2        8:2    0   19G  0 part
  ├─cl-root 253:0    0   17G  0 lvm  /
  └─cl-swap 253:1    0    2G  0 lvm  [SWAP]
sdb           8:16   0   20G  0 disk
├─sdb1        8:17   0    5G  0 part
├─sdb2        8:18   0    5G  0 part
└─sdb3        8:19   0    5G  0 part
sdc           8:32   0   20G  0 disk
└─sdc1        8:33   0   20G  0 part /var/lib/ceph/osd/ceph-0
sdd           8:48   0   20G  0 disk
└─sdd1        8:49   0   20G  0 part /var/lib/ceph/osd/ceph-1
sde           8:64   0   20G  0 disk
└─sde1        8:65   0   20G  0 part /var/lib/ceph/osd/ceph-2
sr0          11:0    1  4.1G  0 rom  

```

Kiểm tra ceph
```
[root@ceph-aio ~]# ceph -s
    cluster bad5f60e-804d-404d-a87e-96e9618b888c
     health HEALTH_WARN
            too few PGs per OSD (21 < min 30)
     monmap e1: 1 mons at {ceph-aio=192.168.2.148:6789/0}
            election epoch 4, quorum 0 ceph-aio
     osdmap e23: 3 osds: 3 up, 3 in
            flags sortbitwise,require_jewel_osds
      pgmap v50: 64 pgs, 1 pools, 0 bytes data, 0 objects
            101548 kB used, 61307 MB / 61406 MB avail
                  64 active+clean

```
