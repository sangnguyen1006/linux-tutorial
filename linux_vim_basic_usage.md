# Tổng hợp cách sử dụng Vim cơ bản

## 1. Mở và thoát Vim

- Mở file với vim:  
  `vim ten_file.txt`
- Mở vim không file:  
  `vim`

**Thoát:**
- Thoát (không lưu):  
  `:q!`  
- Lưu và thoát:  
  `:wq` hoặc `ZZ`
- Lưu (không thoát):  
  `:w`

---

## 2. Các chế độ trong Vim

- **Normal (bình thường):** Mặc định khi mở Vim.
- **Insert (chèn):** Dùng để gõ/chỉnh sửa văn bản.
- **Visual (bôi đen để thao tác):** Dùng chọn văn bản.

**Chuyển đổi chế độ:**
- Vào chế độ insert: `i` (chèn trước con trỏ), `a` (chèn sau con trỏ), `o` (mở dòng mới phía dưới)
- Trở về normal: Nhấn `Esc`
- Vào visual: `v` (chữ thường), `V` (cả dòng), `Ctrl+v` (block/ký tự khối)

---

## 3. Di chuyển con trỏ

- Di chuyển từng ký tự: `h` (trái), `j` (xuống), `k` (lên), `l` (phải)
- Đầu dòng: `0` hoặc `^`
- Cuối dòng: `$`
- Đầu file: `gg`
- Cuối file: `G`
- Di chuyển theo từ: `w` (tới từ sau), `b` (tới từ trước)
- Tới dòng số n: `:n` (vd: `:10` tới dòng 10)

---

## 4. Chỉnh sửa văn bản

- Xóa ký tự: `x`
- Xóa từ: `dw`
- Xóa cả dòng: `dd`
- Xóa đến cuối dòng: `D`
- Thay thế ký tự: `r<ký tự>`
- Undo (hoàn tác): `u`
- Redo: `Ctrl+r`
- Copy dòng: `yy`
- Dán: `p` (sau), `P` (trước)
- Cắt dòng: `dd`
- Copy vùng chọn: Ở Visual mode, chọn xong rồi bấm `y`
- Cắt vùng chọn: Ở Visual mode, chọn xong rồi bấm `d`

---

## 5. Tìm kiếm và thay thế

- Tìm kiếm: `/chuỗi_cần_tìm` (gõ `/` rồi nhập từ khoá, nhấn Enter)
  - Tìm tiếp: `n`
  - Tìm lùi: `N`
- Thay thế trong dòng: `:s/old/new/`
- Thay thế toàn file: `:%s/old/new/g`

---

## 6. Một số thao tác hữu ích

- Lưu file khác: `:w ten_file_moi.txt`
- Hiển thị số dòng: `:set number`
- Ẩn số dòng: `:set nonumber`
- Bật/tắt highlight tìm kiếm: `:set hlsearch` / `:set nohlsearch`
- Chuyển sang đọc file khác: `:e ten_file_khac.txt`
- Xem danh sách lệnh vừa dùng: `:history`

---

## 7. Thoát nhanh các chế độ

- Luôn nhấn `Esc` để về Normal mode trước khi nhập lệnh (bắt đầu bằng dấu `:`).
- Khi bị "kẹt" ở chế độ nào đó, cứ nhấn `Esc` vài lần để chắc chắn trở về Normal mode.

---

## 8. Tài liệu tham khảo

- Học cơ bản bằng lệnh:  
  `vimtutor`
- Trang chủ: https://www.vim.org/docs.php
- Cheat sheet: https://vim.rtorr.com/  