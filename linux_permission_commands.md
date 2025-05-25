# Tổng hợp các lệnh phân quyền cho tệp/thư mục trên Linux (đặc biệt theo group)

## 1. Xem quyền truy cập

- Xem quyền file/thư mục:
  ```bash
  ls -l
  ```
  Kết quả ví dụ:
  ```
  -rw-rw-r-- 1 user group  4096 May 25  file.txt
  drwxr-x--- 2 user group  4096 May 25  dir1
  ```

---

## 1.1. Giải thích cách quy định quyền trong Linux cho file và folder

Khi dùng lệnh `ls -l`, bạn sẽ thấy một dãy ký tự như:
```
-rw-rw-r-- 1 user group 4096 May 25 file.txt
```
Dãy đầu tiên, ví dụ: `-rw-rw-r--`, thể hiện quyền truy cập.  
- **Ý nghĩa từng ký tự:**
  - Ký tự đầu tiên: 
    - `-` : là file thường
    - `d` : là thư mục (directory)
    - `l` : là link (liên kết mềm)
  - 9 ký tự tiếp theo chia thành 3 nhóm, mỗi nhóm 3 ký tự:
    - **[2-4]**: Quyền của **chủ sở hữu** (user/owner)
    - **[5-7]**: Quyền của **nhóm** (group)
    - **[8-10]**: Quyền của **người khác** (others)
  - Mỗi nhóm: r (read - đọc), w (write - ghi), x (execute - thực thi), dấu `-` nghĩa là không có quyền đó.

**Ví dụ:**
- `-rw-rw-r--`
  - `-` : file thường
  - `rw-` : user (owner) được đọc (r), ghi (w), không thực thi (x)
  - `rw-` : group được đọc (r), ghi (w), không thực thi (x)
  - `r--` : others chỉ được đọc (r), không ghi, không thực thi

- `drwxr-x---`
  - `d` : thư mục
  - `rwx` : user (owner) được đọc, ghi, thực thi
  - `r-x` : group được đọc, thực thi, không ghi
  - `---` : others không có quyền gì

**Lưu ý:**
- Với thư mục, quyền `x` nghĩa là được truy cập vào thư mục (cd vào, hoặc liệt kê file).
- Với file, quyền `x` nghĩa là có thể chạy file như một chương trình/script.

---

## 2. Thay đổi quyền với `chmod`

### **Cú pháp:**
```bash
chmod [tùy chọn] [quyền mới] file/dir
```

### **Phân quyền dạng ký tự:**
- **u**: user (owner)
- **g**: group
- **o**: others
- **a**: all (u+g+o)

#### **Ví dụ:**
- Thêm quyền ghi cho group:
  ```bash
  chmod g+w file.txt
  ```
- Xóa quyền đọc của others:
  ```bash
  chmod o-r file.txt
  ```
- Chỉ cho phép owner và group đọc/ghi, others không có quyền:
  ```bash
  chmod 660 file.txt
  ```
- Cho phép group thực thi folder:
  ```bash
  chmod g+x dir1
  ```

### **Phân quyền dạng số (quy định quyền theo số):**
- 4 = read (r)
- 2 = write (w)
- 1 = execute (x)

Mỗi quyền gồm ba số: `[user][group][others]`
- user: quyền của chủ sở hữu (owner)
- group: quyền của nhóm
- others: quyền của người khác

**Cách cộng:**
- r = 4, w = 2, x = 1.  
  VD: rwx = 4+2+1 = 7; rw- = 6; r-- = 4.

**Ví dụ:**
- `chmod 755 file.txt` → owner: rwx (7), group: r-x (5), others: r-x (5)
- `chmod 700 file.txt` → owner: rwx (7), group: --- (0), others: --- (0)
- `chmod 664 file.txt` → owner: rw- (6), group: rw- (6), others: r-- (4)
- `chmod 770 dir1` → owner & group: rwx (7), others: --- (0)

---

## 3. Thay đổi group sở hữu với `chgrp`

- Đổi group cho file:
  ```bash
  chgrp groupname file.txt
  ```
- Đổi group cho thư mục (và tất cả file con):
  ```bash
  chgrp -R groupname dir1
  ```

---

## 4. Thay đổi owner và group với `chown`

- Đổi owner:
  ```bash
  chown username file.txt
  ```
- Đổi owner và group:
  ```bash
  chown username:groupname file.txt
  ```
- Đổi owner/group cho thư mục và file con:
  ```bash
  chown -R username:groupname dir1
  ```

---

## 5. Tạo group và phân quyền cho group

### **Tạo group mới:**
```bash
sudo groupadd ten_group
```

### **Thêm người dùng vào group:**
```bash
sudo usermod -aG ten_group ten_user
```
- Đăng xuất và đăng nhập lại để user được nhận quyền group mới.

### **Xóa người dùng khỏi group:**
```bash
sudo gpasswd -d ten_user ten_group
```

### **Kiểm tra các group mà user thuộc về:**
```bash
groups ten_user
```
Ví dụ:
```bash
groups alice
```
### **Liệt kê các group hiện tại trên hệ thống:**

- **Liệt kê toàn bộ group:**
  ```bash
  cat /etc/group
  ```
  hoặc:
  ```bash
  getent group
  ```

- **Chỉ liệt kê tên các group:**
  ```bash
  cut -d: -f1 /etc/group
  ```

---

### **Đổi group sở hữu của file/thư mục:**
```bash
chown :ten_group file.txt
chown :ten_group dir1
```

### **Phân quyền cho group:**
- VD: Cho group quyền đọc, ghi, thực thi với thư mục:
  ```bash
  chmod 770 dir1
  ```
---

## 6. Cách xem user nào thuộc một group trên Linux

### **Cách 1: Dùng lệnh `getent`**
Liệt kê các user thuộc group `ten_group`:
```bash
getent group ten_group
```
**Ví dụ:**
```bash
getent group bioinfo
```
Kết quả trả về có dạng:
```
bioinfo:x:1002:alice,bob,carol
```
Các user thuộc group nằm ở cuối dòng, phân cách bằng dấu phẩy.

---

### **Cách 2: Xem trực tiếp trong file `/etc/group`**
Mở file `/etc/group` và tìm dòng chứa tên group:
```bash
cat /etc/group | grep ten_group
```
**Ví dụ:**
```bash
cat /etc/group | grep bioinfo
```
Kết quả:
```
bioinfo:x:1002:alice,bob,carol
```

---

### **Cách 3: Liệt kê tất cả user và group của họ**
Để biết user nào thuộc group nào:
```bash
groups ten_user
```
**Ví dụ:**
```bash
groups alice
```
Kết quả sẽ hiển thị tất cả các group mà user đó thuộc về.

---

## 7. Đặt quyền mặc định cho group khi tạo file mới (setgid với thư mục)

- Đặt setgid cho thư mục để file tạo trong đó tự thuộc group của thư mục:
  ```bash
  chmod g+s dir1
  ```
  Sau đó, mọi file/folder tạo trong `dir1` sẽ tự động thuộc group của `dir1`.

---

## 8. Một số ví dụ thực tế

- **Chỉ cho phép group đọc và ghi file, không cho others:**
  ```bash
  chmod 660 file.txt
  ```
- **Cho group thực thi thư mục và tất cả file con:**
  ```bash
  chmod -R g+x dir1
  ```
- **Đổi group sở hữu toàn bộ dự án thành `bioinfo`:**
  ```bash
  chgrp -R bioinfo /home/user/project
  ```
- **Tạo group mới và phân quyền cho group đó:**
  ```bash
  sudo groupadd data_team
  sudo usermod -aG data_team alice
  chown :data_team dataset.txt
  chmod 660 dataset.txt
  ```

---

## 9. Kiểm tra lại quyền sau khi đổi

```bash
ls -l
```
Hoặc kiểm tra group:
```bash
ls -l | grep file.txt
```

## 10. Đổi tên group trên Linux

### **Sử dụng lệnh groupmod**

Để đổi tên một group hiện có, sử dụng lệnh:

```bash
sudo groupmod -n ten_moi ten_cu
```
- `ten_cu`: Tên group cũ
- `ten_moi`: Tên group mới

**Ví dụ:**
```bash
sudo groupmod -n data_team_old data_team
```
Lệnh trên sẽ đổi tên group `data_team` thành `data_team_old`.

**Lưu ý:**  
- Khi đổi tên group, các user thuộc group cũ sẽ tự động thuộc group mới.
- Kiểm tra lại bằng lệnh:
  ```bash
  getent group ten_moi
  ```
  hoặc
  ```bash
  cat /etc/group | grep ten_moi
  ```
## 11. Xóa group trên Linux

### **Sử dụng lệnh groupdel**

Để xóa một group khỏi hệ thống, sử dụng lệnh sau:

```bash
sudo groupdel ten_group
```
- `ten_group`: Tên của group bạn muốn xóa.

**Ví dụ:**
```bash
sudo groupdel oldgroup
```
Lệnh trên sẽ xóa group `oldgroup` khỏi hệ thống.

**Lưu ý:**
- Nếu group vẫn còn là group sở hữu của file/thư mục nào đó, các file/thư mục đó sẽ vẫn tồn tại, nhưng group sẽ không còn trên hệ thống.
- Không thể xóa group nếu nó là group đăng nhập chính (primary group) của một user hiện tại.
- Kiểm tra lại danh sách group sau khi xóa:
  ```bash
  getent group
  ```
---  

**Ghi chú:**  
- Để quản lý nhóm, dùng lệnh `groups`, `adduser username groupname`, `newgrp groupname`,...
- Hãy cẩn trọng khi dùng quyền ghi/thực thi với group hoặc others để đảm bảo bảo mật.