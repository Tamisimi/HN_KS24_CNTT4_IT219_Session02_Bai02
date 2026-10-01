# Bài 2 — User thường `devops` và Sudoers

## Mục tiêu

Tạo user `devops`, thêm vào nhóm `sudo`, copy SSH key từ root, phân quyền `.ssh` đúng để đăng nhập SSH và chạy `sudo`.

## Các lệnh đã chạy (trên Droplet, user root)

### 1. Tạo user và đặt mật khẩu

```bash
adduser devops
```

Nhập mật khẩu khi được hỏi (dùng cho `sudo`).

### 2. Thêm vào nhóm sudo

```bash
usermod -aG sudo devops
```

Kiểm tra:

```bash
groups devops
```

Kỳ vọng có `sudo` trong danh sách nhóm.

### 3. Sao chép SSH key từ root sang devops

```bash
rsync --archive --chown=devops:devops ~/.ssh /home/devops/
```

### 4. Phân quyền thư mục / file SSH

```bash
chmod 700 /home/devops/.ssh
chmod 600 /home/devops/.ssh/authorized_keys
chown -R devops:devops /home/devops/.ssh
```

### 5. (Tuỳ chọn) Kiểm tra nội dung key

```bash
ls -la /home/devops/.ssh
cat /home/devops/.ssh/authorized_keys
```

## Kiểm tra từ máy cá nhân

### Đăng nhập bằng user devops

```bash
ssh -i ~/.ssh/id_ed25519 devops@<IP_ADDRESS_DROPLET>
```

### Kiểm tra sudo

```bash
sudo whoami
```

Kỳ vọng: yêu cầu mật khẩu user `devops` (nếu chưa cấu hình NOPASSWD), sau đó in ra `root`.

## Log minh chứng (dán sau khi chạy thật)

```text
# ssh devops@IP — thành công

# groups devops
# devops : devops sudo

# sudo whoami
# root
```

## Ghi chú

| Hạng mục | Giá trị |
|----------|--------|
| User | devops |
| Nhóm quản trị | sudo |
| SSH | copy từ `/root/.ssh` → `/home/devops/.ssh` |
| Quyền `.ssh` | 700 |
| Quyền `authorized_keys` | 600 |
| IP Droplet | `<điền IP>` |
