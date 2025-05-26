# Giải nén file nén trên Linux

## 1. Giải nén file `.gz`

- **Giải nén file.gz thành file gốc (tại thư mục hiện tại):**
  ```bash
  gunzip file.txt.gz
  ```
  (Sau lệnh này, bạn sẽ có file `file.txt` và file gốc `.gz` bị xóa)

- **Giải nén nhưng GIỮ LẠI file gốc:**
  ```bash
  gzip -dk file.txt.gz
  ```
  (Tùy chọn `-k` giúp giữ lại file `.gz` gốc)

- **Giữ lại file gốc, giải nén ra stdout (không ghi ra file):**
  ```bash
  zcat file.txt.gz
  ```

- **Giải nén vào thư mục khác:**
  - Di chuyển file `.gz` đến thư mục cần, hoặc sau khi giải nén thì di chuyển file kết quả:
    ```bash
    gunzip -c file.txt.gz > /path/to/otherdir/file.txt
    ```
    (Tùy chọn -c giải nén ra stdout, cho phép bạn chuyển hướng kết quả)

  - **Nếu muốn giữ lại file gốc khi giải nén vào thư mục khác:**
    ```bash
    gzip -dc file.txt.gz > /path/to/otherdir/file.txt
    ```

---

## 2. Giải nén file `.tar` và `.tar.gz` (hoặc `.tgz`)

- **Giải nén file `.tar` vào thư mục hiện tại:**
  ```bash
  tar -xvf file.tar
  ```

- **Giải nén file `.tar.gz` hoặc `.tgz` vào thư mục hiện tại:**
  ```bash
  tar -xzvf file.tar.gz
  # hoặc
  tar -xzvf file.tgz
  ```
  (File nén gốc sẽ ĐƯỢC GIỮ LẠI, không bị xóa)

- **Giải nén vào thư mục khác** (ví dụ `/tmp/testdata`):
  ```bash
  tar -xzvf file.tar.gz -C /tmp/testdata
  ```
  (File nén gốc sẽ ĐƯỢC GIỮ LẠI)

---

## 3. Giải nén file `.zip`

- **Giải nén vào thư mục hiện tại:**
  ```bash
  unzip file.zip
  ```
  (File nén gốc sẽ ĐƯỢC GIỮ LẠI)

- **Giải nén vào thư mục khác:**
  ```bash
  unzip file.zip -d /path/to/otherdir
  ```
  (File nén gốc sẽ ĐƯỢC GIỮ LẠI)

---

## 4. Giải nén file `.rar`

- **Cài đặt unrar** (nếu chưa có):
  ```bash
  sudo apt-get install unrar   # Ubuntu/Debian
  sudo yum install unrar       # CentOS/RHEL
  ```

- **Giải nén vào thư mục hiện tại:**
  ```bash
  unrar x file.rar
  ```
  (File nén gốc sẽ ĐƯỢC GIỮ LẠI)

- **Giải nén vào thư mục khác:**
  ```bash
  unrar x file.rar /path/to/otherdir/
  ```
  (File nén gốc sẽ ĐƯỢC GIỮ LẠI)

---

## 5. Giải nén file `.7z`

- **Cài đặt p7zip** (nếu chưa có):
  ```bash
  sudo apt-get install p7zip-full
  ```

- **Giải nén vào thư mục hiện tại:**
  ```bash
  7z x file.7z
  ```
  (File nén gốc sẽ ĐƯỢC GIỮ LẠI)

- **Giải nén vào thư mục khác:**
  ```bash
  7z x file.7z -o/path/to/otherdir
  ```
  (Không có dấu cách giữa `-o` và đường dẫn; file nén gốc sẽ ĐƯỢC GIỮ LẠI)

---

## 6. Một số lệnh tổng hợp nhanh

| Định dạng   | Giải nén thư mục hiện tại              | Giải nén sang thư mục khác                      | Giải nén giữ lại file gốc |
|------------|----------------------------------------|------------------------------------------------|--------------------------|
| .gz        | `gunzip file.gz`                      | `gunzip -c file.gz > /path/other/file`         | `gzip -dk file.gz`       |
| .tar.gz    | `tar -xzvf file.tar.gz`               | `tar -xzvf file.tar.gz -C /path/other`         | Luôn giữ file gốc        |
| .zip       | `unzip file.zip`                      | `unzip file.zip -d /path/other`                | Luôn giữ file gốc        |
| .rar       | `unrar x file.rar`                    | `unrar x file.rar /path/other/`                | Luôn giữ file gốc        |
| .7z        | `7z x file.7z`                        | `7z x file.7z -o/path/other`                   | Luôn giữ file gốc        |

---

**Lưu ý:**  
- Chỉ riêng `gunzip` mặc định xóa file `.gz` sau khi giải nén. Để giữ lại file gốc hãy dùng `gzip -dk file.gz`.
- Các định dạng khác (`tar`, `zip`, `rar`, `7z`) GIỮ file nén gốc sau khi giải nén.
- Đối với giải nén vào thư mục khác, hãy chắc chắn thư mục đích đã tồn tại hoặc tạo trước:  
  ```bash
  mkdir -p /path/to/otherdir
  ```
- Đọc thêm các tùy chọn của từng lệnh qua `man tar`, `man unzip`, `man unrar`, `man 7z`, `man gzip`

---