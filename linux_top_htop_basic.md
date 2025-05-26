# Cách sử dụng lệnh `top` và `htop` cơ bản | Created by AI

## 1. Lệnh `top`

- **Mục đích:** Theo dõi tiến trình, mức sử dụng CPU, RAM, swap, load hệ thống theo thời gian thực.

**Cách dùng:**
```bash
top
```
- Nhấn `q` để thoát.

### Một số phím thao tác trong top:
- `P`: Sắp xếp theo CPU (default)
- `M`: Sắp xếp theo RAM
- `T`: Sắp xếp theo thời gian chạy tiến trình
- `k`: Nhập PID để kill tiến trình
- `1`: Hiển thị chi tiết từng core CPU
- `h` hoặc `?`: Hiện trợ giúp

### Lọc thông tin với top:
- Chỉ xem tiến trình của user:
  ```bash
  top -u <tên_user>
  ```
- Cập nhật sau n giây:
  ```bash
  top -d 2
  ```

---

## 2. Lệnh `htop`

- **Mục đích:** Như top nhưng giao diện đẹp hơn, hiển thị màu sắc, dễ thao tác với phím mũi tên.

**Cách dùng:**
```bash
htop
```
- Nhấn `F10` hoặc `q` để thoát.

### Một số thao tác trong htop:
- Dùng phím mũi tên để di chuyển.
- `F3`: Tìm kiếm tiến trình
- `F4`: Lọc tiến trình theo tên
- `F6`: Chọn trường để sắp xếp (CPU, MEM, TIME, ...)
- `F9`: Kill tiến trình (chọn rồi Enter)
- `F2`: Cấu hình giao diện

### Một số lệnh mẫu:
- Xem chi tiết cho user cụ thể:
  ```bash
  htop -u <tên_user>
  ```
- Không hiển thị các tiến trình đã kết thúc:
  ```bash
  htop --no-kill --no-color
  ```

---

## 3. Lưu ý

- `htop` có thể cần cài đặt:  
  ```bash
  sudo apt install htop     # Ubuntu/Debian
  sudo yum install htop     # CentOS/RedHat
  ```

- Nếu chỉ có thể dùng dòng lệnh (không giao diện màu), hãy dùng `top`.

---

**Tài liệu tham khảo:**
- `man top`
- `man htop`
- https://htop.dev/
