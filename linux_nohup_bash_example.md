# Tổng hợp lệnh `nohup` và ví dụ bash giả lập thực hiện tác vụ | Created by AI

## 1. Tổng quan về `nohup`
- `nohup` (No Hang Up) dùng để chạy một lệnh hoặc script mà không bị dừng lại khi đóng terminal hoặc thoát khỏi SSH.
- Khi kết hợp với `&`, lệnh sẽ chạy nền (background).

**Cú pháp chung:**
```bash
nohup <command> [options] &
```
- Đầu ra mặc định sẽ được ghi vào file `nohup.out` nếu không chỉ định file khác.

---

## 2. Các lệnh nohup cơ bản

| Mục đích                           | Lệnh ví dụ                                                        |
|------------------------------------|-------------------------------------------------------------------|
| Chạy script bash nền               | `nohup bash script.sh &`                                          |
| Chạy python nền                    | `nohup python myscript.py &`                                      |
| Ghi log ra file khác               | `nohup bash script.sh > mylog.txt 2>&1 &`                         |
| Chạy nhiều lệnh                    | `nohup sh -c 'cmd1; cmd2' > log.txt 2>&1 &`                       |
| Chạy nền không in output           | `nohup bash script.sh > /dev/null 2>&1 &`                         |
| Kiểm tra tiến trình                | `ps aux | grep nohup`                                             |
| Dừng tiến trình                    | `kill <pid>`                                                      |
| Dừng tiến trình mạnh (bắt buộc)    | `kill -9 <pid>`                                                   |

**Giải thích:**
- `2>&1`: Gộp lỗi và chuẩn đầu ra chung vào một file log.
- `> /dev/null 2>&1`: Không ghi bất kỳ output (stdout, stderr) nào ra file.
- `&`: Đưa tiến trình chạy nền.
- `kill -9 <pid>`: Dừng tiến trình ngay lập tức, dùng khi `kill <pid>` không hiệu quả.

---

## 3. Bash script giả lập một tác vụ dài

Ví dụ: Một script giả lập xử lý dữ liệu lớn (sleep và ghi log nhiều lần).

### Tạo file bash script:
```bash
#!/bin/bash
echo "Bắt đầu xử lý dữ liệu: $(date)"
for i in {1..5}
do
    echo "Đang xử lý bước $i..."
    sleep 10
done
echo "Hoàn thành: $(date)"
```
Lưu lại với tên: `process_data.sh`  
Cấp quyền thực thi:  
```bash
chmod +x process_data.sh
```

---

### Chạy script với `nohup` và ghi log:
```bash
nohup ./process_data.sh > process.log 2>&1 &
```

- Script sẽ chạy nền, log ra file `process.log`.
- Bạn có thể đóng terminal, script vẫn tiếp tục chạy đến khi xong.

---

### Chạy script với `nohup` không ghi output:
```bash
nohup ./process_data.sh > /dev/null 2>&1 &
```
- Script chạy nền, không ghi bất kỳ log nào ra file.

---

## 4. Kiểm tra & quản lý tiến trình

- Kiểm tra quá trình đang chạy:
  ```bash
  ps aux | grep process_data.sh
  ```
- Kiểm tra log trực tiếp:
  ```bash
  tail -f process.log
  ```
- Dừng tiến trình (bình thường):
  ```bash
  kill <pid>
  ```
- Dừng tiến trình mạnh (bắt buộc, nếu tiến trình "cứng đầu"):
  ```bash
  kill -9 <pid>
  ```

---

## 5. Lưu ý
- Không nên chạy nhiều tác vụ nặng trên cùng một server nếu không kiểm soát được tài nguyên.
- Luôn redirect log ra file để tiện theo dõi và debug khi cần.
