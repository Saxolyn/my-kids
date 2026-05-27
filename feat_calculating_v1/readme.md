
# 🌟 Toán Vui Pro — Thi Đấu Tập Trung

**Toán Vui Pro** là một ứng dụng web dạng Single-Page Application (SPA) giúp người dùng (đặc biệt là học sinh) luyện tập và thi đấu tính nhẩm các phép toán cơ bản. Ứng dụng mang lại trải nghiệm học tập thú vị, tập trung và có hệ thống theo dõi tiến độ rõ ràng.

## 🎯 Mục đích chức năng

* **Luyện phản xạ Toán học:** Cung cấp môi trường thi đấu tính toán nhanh với các phép tính Cộng, Trừ, Nhân, Chia và Hỗn hợp.
* **Cá nhân hóa trải nghiệm học tập:** Hỗ trợ nhiều người chơi trên cùng một thiết bị thông qua hệ thống "Hồ sơ người dùng" (Profiles).
* **Theo dõi tiến độ:** Ghi nhận lại lịch sử thi đấu, tính toán tỷ lệ chính xác theo ngày để phụ huynh hoặc người dùng tự đánh giá sự tiến bộ.
* **Ứng dụng độc lập, nhẹ nhàng:** Hoạt động mượt mà không cần cài đặt phức tạp, toàn bộ dữ liệu được lưu trữ an toàn ngay trên trình duyệt của người dùng.

---

## ✨ Các chức năng chính

### 1. Quản lý Hồ sơ (Profile Management)

* Tạo nhiều hồ sơ với tên, hình đại diện (Avatar Emoji) và màu sắc cá nhân hóa.
* Lưu trữ dữ liệu độc lập cho từng hồ sơ (lịch sử, cài đặt cá nhân).

### 2. Chế độ Thi đấu Đa dạng

* **5 chế độ chơi:** Phép Cộng (➕), Phép Trừ (➖), Phép Nhân (✖️), Phép Chia (➗), và Hỗn Hợp (🎲).
* Thuật toán sinh câu hỏi thông minh, đảm bảo phép trừ không ra số âm, phép chia luôn chia hết không dư.

### 3. Trải nghiệm Làm bài Thi (Quiz Interface)

* **Bàn phím ảo (Numpad):** Hỗ trợ nhập liệu nhanh trên màn hình cảm ứng, có nút xóa và lưu đáp án.
* **Điều hướng thông minh:** Thanh tiến trình trực quan, cho phép nhảy đến câu hỏi bất kỳ, tiến/lùi giữa các câu hỏi.
* **Ràng buộc nộp bài:** Nút nộp bài chỉ kích hoạt khi 100% câu hỏi đã được điền đáp án, tránh tình trạng nộp nhầm/nộp thiếu.
* Đồng hồ đếm giờ tổng thời gian làm bài.

### 4. Báo cáo & Thống kê

* Chấm điểm tự động và trao danh hiệu (Cúp/Huy chương) dựa trên tỷ lệ % chính xác.
* Hiển thị chi tiết từng câu hỏi: Đáp án của người dùng vs. Đáp án đúng.
* Bảng thống kê lịch sử: Tính tổng số trận, số câu đúng và tỷ lệ chính xác trung bình theo từng ngày.

### 5. Cài đặt Nâng cao & Quản lý Dữ liệu

* Tùy chỉnh số lượng câu hỏi (10, 20, 30 hoặc tự nhập).
* Tùy chỉnh độ khó (Dải số có 2, 3, 4 chữ số, hoặc tự định nghĩa min-max).
* Tùy chỉnh thời gian đếm ngược trước khi vào trận.
* Chọn các phép tính cụ thể sẽ xuất hiện trong chế độ "Hỗn Hợp".
* **Xuất/Nhập file CSV:** Cho phép tải lịch sử thi đấu xuống máy tính dưới dạng `.csv` và khôi phục dữ liệu từ file `.csv` (rất hữu ích để sao lưu hoặc chuyển thiết bị).

---

## 🛠 Cấu trúc dự án

Dự án được xây dựng gộp toàn bộ trong một tệp duy nhất (`.html`) để tối ưu tính di động và dễ triển khai. Cấu trúc bên trong tệp được chia thành 3 phần rõ rệt:

1. **Khối `<style>` (CSS):**
* Sử dụng CSS Variables (`:root`) để quản lý theme màu sắc.
* Thiết kế giao diện dạng thẻ (Cards), lưới (Grid), và Flexbox để Responsive tốt trên nhiều kích thước màn hình.
* Có các hiệu ứng Animation mượt mà (chuyển động của khối background, hiệu ứng đếm ngược, pop-up kết quả).


2. **Khối Thẻ HTML (UI/DOM):**
* Hệ thống chuyển trang (Screen Routing) qua các `div` có class `.screen`:
* `#screen-home`: Màn hình tạo và chọn hồ sơ.
* `#screen-mode`: Màn hình chọn phép toán và xem lịch sử.
* `#screen-countdown`: Màn hình đếm ngược trước khi thi.
* `#screen-quiz`: Giao diện làm bài chính.
* `#screen-results`: Màn hình hiển thị điểm và chi tiết bài làm.


* Hệ thống Modal Overlay (Pop-up) dành cho cài đặt, tạo profile và xác nhận hành động.


3. **Khối `<script>` (Logic JavaScript):**
* **State Management:** Quản lý cấu hình `appSettings`, danh sách người dùng `profiles`, trạng thái câu hỏi hiện tại `questions`, `userAnswers`.
* **Core Logic:** Các hàm tạo câu hỏi (`generateQuestion`), luồng làm bài (`loadQuestion`, `validateInputState`), tính điểm (`processEvaluation`).
* **Local Storage:** Lưu và lấy dữ liệu JSON liên tục (`toanyui_profiles_v3`).
* **Xử lý File:** Logic đọc/ghi định dạng CSV bằng `FileReader` và `Blob`.



---

## 💻 Công nghệ sử dụng

Dự án này là một ứng dụng **Vanilla Web Development** (không sử dụng framework nặng nề như React hay Vue), bao gồm:

* **HTML5:** Xây dựng cấu trúc ngữ nghĩa, `inputmode` tối ưu bàn phím di động.
* **CSS3:** Flexbox, CSS Grid, Custom Properties, Keyframe Animations.
* **JavaScript (ES6+):** Xử lý logic nghiệp vụ, thao tác DOM trực tiếp, Template Literals, Destructuring.
* **Web Storage API:** Sử dụng `localStorage` để lưu trữ cơ sở dữ liệu phi quan hệ (NoSQL-like) ngay trên trình duyệt.
* **File API:** Xử lý nhập/xuất tệp tin dữ liệu cục bộ.
* **Phông chữ (Google Fonts):** Sử dụng `Baloo 2` (tạo sự vui tươi, phù hợp trẻ em) và `Nunito` (dễ đọc).