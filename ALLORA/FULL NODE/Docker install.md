# Cài đặt Full Node Allora trên Ubuntu
Xem hướng dẫn chính thức tại: 
```
https://docs.allora.network/devs/validators/run-full-node
```

# Thông tin
Full node Allora hỗ trợ sẵn `snapshot`, không cần đồng bộ từ đầu nên đồng bộ khá nhanh.

## Cài đặt Git
### Cập nhật danh sách gói:
```
sudo apt update
```

### Cài đặt Git:
```
sudo apt install git
```

### Kiểm tra Git đã cài đặt thành công
Kiểm tra phiên bản Git:
```
git --version
```

## Cài đặt Docker
```
sudo apt-get install apt-transport-https ca-certificates curl software-properties-common jq -y
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"
sudo apt-get update
sudo apt-get install docker-ce -y
```


### Kiểm tra Docker đã cài đặt thành công
Kiểm tra trạng thái Docker:
```
sudo systemctl status docker
```

Ấn q để thoát.

## Chạy Full Node Allora với Docker
### Clone repository Allora:
```
git clone https://github.com/allora-network/allora-chain.git
```

### Chuyển vào thư mục Allora:
```
cd allora-chain
```

### Checkout branch:
```
git branch -a
```

### Checkout về phiên bản trước:
```
git checkout 812ab2d855f164b9798b09ca043d769dbddffd7e
```

### Sửa file `docker-compose.yaml`:
```
nano docker-compose.yaml
```

Lưu và thoát bằng cách nhấn `Ctrl+X, Y và nhấn Enter`.


### Thay đổi `Moniker` của bạn và đổi phiên bản chain từ `v0.2.14` thành `v0.3.0`: (có thể tham khảo hình ảnh dưới đây)

![image](https://github.com/user-attachments/assets/ac237ee1-5ecd-4763-b3c5-6927008e1798)


### Sửa file l1_node.sh để chạy v0.3.0:
```
nano scripts/l1_node.sh
```



### Sửa dòng `allorad \` thành `/cosmovisor/upgrades/v0.3.0/bin/allorad \` khoảng dòng `80` như hình dưới:

![image](https://github.com/user-attachments/assets/b04647df-502f-4f4d-b9f0-6414943a11d3)


### Cập nhật hình ảnh Docker trong file `docker-compose.yaml` mới nhất:
```
docker compose pull
```

### Chạy Docker Compose:
```
docker compose up -d
```

### Xem log
Xem log Docker Compose:
```
docker compose logs -f
```

### Kiểm tra tình trạng của Full Node
Kiểm tra trạng thái Full Node:
```
curl -s http://localhost:26657/status | jq .
```

### Kiểm tra đồng bộ hóa xong chưa, trả về false nếu đã đồng bộ hóa xong
Kiểm tra trạng thái đồng bộ:
```
curl -so- http://localhost:26657/status | jq .result.sync_info.catching_up
```


## thông tin
Dùng rpc này cho worker thì dùng ip của server thay cho `localhost`: `http://<ip server>:26657 thay cho http://localhost:26657`

# END
