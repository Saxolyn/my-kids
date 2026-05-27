# Tài Liệu Dự Án: Ứng Dụng Web Luyện Tập "Toán Vui Pro"

Ứng dụng **Toán Vui Pro** là một nền tảng Web Single-Page (SPA) gọn nhẹ, giúp người dùng (đặc biệt là trẻ em) luyện tập các phép tính toán học cơ bản (Cộng, Trừ, Nhân, Hỗn hợp) một cách trực quan, vui nhộn thông qua cơ chế trò chơi hóa (Gamification).

## 1. Yêu Cầu Nghiệp Vụ (Requirements)

### 1.1. Quản lý Hồ Sơ (Profile Management)
- Hỗ trợ tạo nhiều hồ sơ người dùng độc lập (Tên, Nhân vật Avatar biểu cảm, Màu sắc chủ đề).
- Tự động lưu giữ Profile hoạt động gần nhất. Khi người dùng truy cập lại, hệ thống tự động đăng nhập thẳng vào profile đó mà không cần chọn lại từ đầu.

### 1.2. Cơ Chế Trò Chơi & Bộ Đếm Thời Gian
- **Màn hình đếm ngược (Countdown):** Hiển thị hiệu ứng đếm ngược trực quan trước khi chính thức bắt đầu bài tập nhằm tạo sự tập trung.
- **Bộ bài tập:** Mỗi lượt chơi gồm đúng 20 câu hỏi ngẫu nhiên dựa theo phép tính đã chọn.
- **Thời gian:** Đo lường chính xác thời gian hoàn thành từng câu hỏi (đơn vị giây) và tổng thời gian của cả lượt chơi.
- **Phản hồi thời gian thực:** Hiển thị hiệu ứng hoạt họa (Animation) khi trả lời Đúng (🎉/✨) hoặc Sai (💥) kèm theo việc hiển thị đáp án chính xác ngay lập tức.

### 1.3. Quản Lý Độ Khó & Cấu Hình Cao Cấp (Settings)
Cung cấp bảng cài đặt riêng biệt cho từng Profile để tùy chỉnh trải nghiệm:
- **Độ khó (Dải số):**
  - Mức 2 chữ số: Sinh số ngẫu nhiên trong khoảng [10 - 99].
  - Mức 3 chữ số: Sinh số ngẫu nhiên trong khoảng [100 - 999].
  - Mức 4 chữ số: Sinh số ngẫu nhiên trong khoảng [1000 - 9999].
  - Mức Tùy chọn: Người dùng tự điền khoảng số [Min - Max] theo nhu cầu.
- **Thời gian đếm ngược:** Cho phép chọn giữa các mốc 3s, 5s, 10s, 15s hoặc nhập số giây tùy biến.

### 1.4. Thống Kê Lịch Sử & Đồng Bộ Dữ Liệu
- **Gom nhóm theo ngày:** Hệ thống tự động nhóm tất cả các lượt chơi phát sinh trong cùng một ngày (`DD/MM/YYYY`).
- **Tỷ lệ đúng trung bình:** Nếu một ngày chơi nhiều lần, tỷ lệ chính xác được tính bằng công thức:
  $$\text{Tỷ lệ đúng trung bình} = \left( \frac{\sum \text{Số câu đúng}}{\sum \text{Tổng số câu hỏi}} \right) \times 100\%$$
- **Xuất dữ liệu (Export CSV):** Hỗ trợ xuất toàn bộ lịch sử chi tiết của profile hiện tại ra file `.csv` (tích hợp chuẩn mã hóa BOM UTF-8 để tránh lỗi font hiển thị trên Microsoft Excel).
- **Nhập dữ liệu (Import/Restore CSV):** Cho phép nạp lại file `.csv` lịch sử đã xuất trước đó vào hệ thống để khôi phục dữ liệu khi người dùng chuyển thiết bị hoặc xóa cache trình duyệt.

---

## 2. Phân Tích Kiến Trúc Kỹ Thuật (Architecture Analysis)

Mặc dù ứng dụng được đóng gói trọn gói trong **1 file HTML duy nhất (Vanilla HTML/CSS/JS)** để tối ưu tốc độ tải và tính cơ động, cấu trúc mã nguồn vẫn đảm bảo phân tách tư duy rõ ràng tương tự các kiến trúc hiện đại:

### 2.1. Tầng Giao Diện (Frontend - View Layer)
- **CSS Variables (:root):** Quản lý tập trung hệ màu sắc tươi sáng (Pastel) giúp dễ dàng thay đổi theme toàn cục.
- **Single-Page Navigation:** Sử dụng kỹ thuật ẩn/hiện các khối thuộc tính `.screen` thông qua class `.active` do JavaScript điều khiển, mang lại trải nghiệm mượt mà không bị reload trang:
  - `screen-home`: Màn hình danh sách hồ sơ.
  - `screen-countdown`: Màn hình đếm ngược chuẩn bị vào trận.
  - `screen-mode`: Màn hình trung tâm chọn phép tính, xem cấu hình và bảng lịch sử theo ngày.
  - `screen-quiz`: Không gian tương tác trả lời câu hỏi và bàn phím số (Numpad) ảo.
  - `screen-results`: Màn hình báo cáo tổng kết hiệu suất sau 20 câu.

### 2.2. Tầng Logic Xử Lý & Sinh Dữ Liệu (Controller & Business Layer)
- **Thuật toán sinh số ngẫu nhiên:** Đảm bảo tuân thủ nghiêm ngặt dải biên `[Min - Max]` từ cấu hình. Đối với phép trừ, thuật toán tự động đảo vị trí để số bị trừ luôn lớn hơn hoặc bằng số trừ ($a \ge b$), tránh sinh ra kết quả âm không phù hợp với lứa tuổi tiểu học. Đối với phép nhân dải lớn, hệ thống tự động tối ưu hóa cấu trúc một thừa số nhỏ kết hợp một thừa số lớn để bài toán giữ được tính thực tế cao.

### 2.3. Tầng Lưu Trữ & Đồng Bộ (Data & Storage Layer)
- **Local Storage State:** Toàn bộ trạng thái bao gồm thông tin cá nhân, cấu hình bài tập, và mảng danh sách lịch sử (`sessions`) được đồng bộ hóa tức thời xuống Trình duyệt thông qua JSON Stringify.
- **CSV Parser/Generator:** Sử dụng đối tượng `Blob` kết hợp URL Object để sinh file download ở phía Client-side mà không cần máy chủ (Backend server). Tầng đọc file sử dụng `FileReader` để phân tách dòng (split) và đưa ngược dữ liệu vào mảng State.

---