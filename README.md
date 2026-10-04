# Bài 2: Khởi tạo User thường và thiết lập đặc quyền quản trị

## Mục tiêu
- Tạo tài khoản người dùng thường (Non-root user) trên Ubuntu Server.
- Cấp đặc quyền quản trị qua nhóm `sudo` và sao chép cấu hình SSH Key để đăng nhập.

## Các lệnh Linux đã chạy

### 1. Tạo tài khoản người dùng mới
Sử dụng lệnh `adduser` để tạo tài khoản `devops` và làm theo các hướng dẫn trên màn hình để thiết lập mật khẩu cũng như thông tin user:
```bash
sudo adduser devops
```

### 2. Thêm user vào nhóm quản trị sudo
Sử dụng lệnh `usermod` để thêm tài khoản `devops` vào nhóm `sudo`:
```bash
sudo usermod -aG sudo devops
```

### 3. Sao chép cấu hình SSH Key cho user mới
Sử dụng lệnh `rsync` để sao chép toàn bộ thư mục `.ssh` từ user root sang thư mục home của user `devops`, đồng thời thay đổi chủ sở hữu một cách an toàn để user này có thể đăng nhập bằng SSH Key:
```bash
sudo rsync --archive --chown=devops:devops ~/.ssh /home/devops/
```

### 4. Phân quyền thư mục SSH (Xác nhận lại)
Cơ chế bảo mật SSH yêu cầu quyền hạn khắt khe. Phân quyền chính xác cho thư mục `.ssh` và file `authorized_keys` của user devops:
```bash
sudo chmod 700 /home/devops/.ssh
sudo chmod 600 /home/devops/.ssh/authorized_keys
```

## Kết quả kiểm tra
Đăng nhập từ máy cá nhân bằng SSH dưới tên user mới tạo:
```bash
ssh -i /path/to/private_key devops@<IP_ADDRESS_DROPLET>
```

Sau khi đăng nhập thành công, chạy lệnh kiểm tra quyền sudo:
```bash
sudo whoami
```
*(Yêu cầu nhập mật khẩu của user `devops` vừa tạo)*

**Log Terminal kết quả lệnh `sudo whoami`:**
```text
[Học viên chèn log Terminal chạy lệnh sudo whoami trả về kết quả "root" tại đây]
```
