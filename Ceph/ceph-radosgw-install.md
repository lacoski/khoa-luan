# Triển khai Radosgw trên Ceph Cluster
---
## Chuẩn bị
Đọc các tài liệu
Sử dụng lab 3 node ceph

## Cài đặt
### Phần 1: Thiết lập Ceph Radosgw
#### Bước 1: Thiết lập tại ceph-admin (Node có quyền admin)
> Thực hiện trên ceph-admin

Truy cập ceph-admin
```
ssh root@ceph-admin
```

Truy cập ceph cluster directory
```
cd cluster/
```
#### Bước 2: Tạo rados gateway
Tạo 2 rados gateway:
- 1 trên ceph-node-1
- 1 trên ceph-node-2

```
ceph-deploy rgw create ceph-node-1 ceph-node-2
```

> API gateways chạy trên port 7480.

> Sử dụng 2 gateway trong trường hợp cần tới HA, load balancing

__Kết quả__

pic 1

### Phần 2: Tạo radosgw user
#### Tổng quát
Để có thể sử dụng Ceph object storage, ta cần tạo user truy cập Radosgw. User được định danh bằng access và secret key, sử dụng cho mục đích truy cập, thực hiện các hoạt động trên  Ceph object storage.

#### Bước 1: Truy cập node admin
Truy cập node có quyền admin trong Ceph cluster
```
ssh root@ceph-admin
```
#### Bước 2: Tạo user

```
[root@ceph-admin cluster]# radosgw-admin user create --uid=lacoski --display-name="my user lacoski"
{
    "user_id": "lacoski",
    "display_name": "my user lacoski",
    "email": "",
    "suspended": 0,
    "max_buckets": 1000,
    "auid": 0,
    "subusers": [],
    "keys": [
        {
            "user": "lacoski",
            "access_key": "UDW5NH3UZ83CK1W0U2PW",
            "secret_key": "BjPiwiRRdTmgK50SjeDCmgVgfNWjfPgTIRTTr4Zq"
        }
    ],
    "swift_keys": [],
    "caps": [],
    "op_mask": "read, write, delete",
    "default_placement": "",
    "placement_tags": [],
    "bucket_quota": {
        "enabled": false,
        "max_size_kb": -1,
        "max_objects": -1
    },
    "user_quota": {
        "enabled": false,
        "max_size_kb": -1,
        "max_objects": -1
    },
    "temp_url_keys": []
}

```
#### Kiểm tra lại thông tin user
```
[root@ceph-admin cluster]# radosgw-admin user info --uid=lacoski
{
    "user_id": "lacoski",
    "display_name": "my user lacoski",
    "email": "",
    "suspended": 0,
    "max_buckets": 1000,
    "auid": 0,
    "subusers": [],
    "keys": [
        {
            "user": "lacoski",
            "access_key": "UDW5NH3UZ83CK1W0U2PW",
            "secret_key": "BjPiwiRRdTmgK50SjeDCmgVgfNWjfPgTIRTTr4Zq"
        }
    ],
    "swift_keys": [],
    "caps": [],
    "op_mask": "read, write, delete",
    "default_placement": "",
    "placement_tags": [],
    "bucket_quota": {
        "enabled": false,
        "max_size_kb": -1,
        "max_objects": -1
    },
    "user_quota": {
        "enabled": false,
        "max_size_kb": -1,
        "max_objects": -1
    },
    "temp_url_keys": []
}
```
### Phần 3: Kiểm tra user cung cấp bởi Radosgw
> Thực hiện trên ceph-admin (có thể trên ceph-client)

__Cài đặt gói__
```
yum -y install python-boto
```

__Tạo python script "/root/cephs3test.py"__
```
[root@ceph-admin ~]# vim cephs3test.py3

# content
import boto
import boto.s3.connection

access_key = 'UDW5NH3UZ83CK1W0U2PW'
secret_key = 'BjPiwiRRdTmgK50SjeDCmgVgfNWjfPgTIRTTr4Zq'
conn = boto.connect_s3(
   aws_access_key_id = access_key,
   aws_secret_access_key = secret_key,
   host = 'ceph-node-1', port = 7480,is_secure=False, calling_format = boto.s3.connection.OrdinaryCallingFormat(),)

bucket = conn.create_bucket('my-new-bucket')
for bucket in conn.get_all_buckets():
   print "{name} {created}".format(name = bucket.name,created = bucket.creation_date,)
```
> Chú ý: 2 key access và secret

> Chú ý: Host = 'radosgw host' lựa chọn phù hợp

__Chạy script__
```
python /root/cephs3test.py
```
__Kết quả (tương tự)__
```
my-new-bucket 2018-02-27T04:56:33.729Z
lacoski 2018-02-27T04:48:46.508Z
```

__Kiểm tra bằng node ceph-admin__
```
[root@ceph-admin ~]# radosgw-admin bucket list
[
    "my-new-bucket",
    "lacoski"
]
```

__Xóa bucket test__
```
radosgw-admin bucket rm my-new-bucket
```

### Phần 3: Truy cập Ceph object storage
Ở đây sẽ truy cập Ceph object thông qua S3 API tương thích.

#### 
