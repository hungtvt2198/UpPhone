# UpPhone – Official Installer & Releases

Kho phát hành chính thức bộ cài đặt và cập nhật tự động của phần mềm **UpPhone – Phone Farm Shopee Video Uploader** (Windows x64).

- **Phiên bản mới nhất:** **1.0.5** *(phát hành ngày 14/11/2026)*
- **Tải trực tiếp bộ cài đặt:** **[UpPhone_Setup.exe (v1.0.5)](https://github.com/hungtvt2198/UpPhone/releases/download/v1.0.5/UpPhone_Setup.exe)**

---

## Điểm mới trong phiên bản 1.0.5

- **Cho phép sửa các dòng chưa chạy khi đang chạy tự động:** Khi đã bấm chạy, phần mềm chỉ khóa các dòng đang thực sự Đang chạy; các dòng Chờ, Lỗi, Thành công, Đã dừng vẫn chỉnh sửa trực tiếp được 6 cột (ID KÊnh, SERIAL, TÊN WORKFLOW, ĐƯỜNG DẪN VIDEO, LINK AFFILIATE, TIÊU ĐỀ) và nút chọn file video 📁. Nếu đổi SERIAL của dòng đang Chờ trong hàng chạy, dòng sẽ tự chuyển sang luồng của máy mới.
- **Sửa giao diện trình cài đặt & cập nhật (UpPhone_Setup.exe):** Chuyển sang nền trắng chữ đậm có độ tương phản cao, khắc phục hoàn toàn tình trạng chữ nút bấm và checkbox bị tối mờ khó đọc khi cài đặt / cập nhật tự động.
- **Cải tiến hộp thoại Thêm dòng & danh sách ID kênh từ Google Sheet:**
  - Loại bỏ dòng Hashtag và Link air khỏi hộp thoại **Thêm**.
  - Danh sách **Serial** hiển thị kèm ghi chú của thiết bị (ví dụ 192.168.10.32:5555 (Trend)).
  - Ô **ID Kênh** vừa cho phép gõ tay vừa chọn từ danh sách dropdown tự động đồng bộ từ Google Sheet Data Master (cột B từ ô B7 trở xuống) ở cả hộp thoại Thêm và khi sửa trực tiếp trên bảng.
- **Phân biệt 2 trường hợp link_air theo Workflow:**
  - **shopee_AI_link & shopee_link (lấy link):** kiểm tra URL thật ở cột link_air khi mở Excel và bắt buộc phải lấy được link_air khi chạy xong.
  - **Các workflow khác (không lấy link):** không kiểm tra link_air khi chạy; tự động đánh dấu "Done" vào cột link_air trên file Excel sau khi hoàn thành để lần mở sau không chạy trùng lại.

## Cài đặt lần đầu
1. Tải file **[UpPhone_Setup.exe](https://github.com/hungtvt2198/UpPhone/releases/latest)** ở mục **[Releases](https://github.com/hungtvt2198/UpPhone/releases)**.
2. Mở UpPhone_Setup.exe → bấm **Yes** khi Windows hỏi quyền Quản trị viên (UAC).
3. Chọn thư mục cài đặt (mặc định **C:\Program Files\UpPhone**) và bấm **Cài đặt ngay**.

## Cập nhật phiên bản mới
- Trong phần mềm UpPhone, bấm nút **Update** (*Check for Update*) ở góc dưới cùng thanh chọn tab bên trái để tự động kiểm tra, tải và nâng cấp lên phiên bản mới nhất.
- Toàn bộ workflows/, 	emplates/, ghi chú thiết bị và cấu hình số máy của bạn được giữ nguyên 100% khi cập nhật.
