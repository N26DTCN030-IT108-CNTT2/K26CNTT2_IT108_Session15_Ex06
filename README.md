# K26CNTT2_IT108_Session15_Ex06
Bài tập số 6
# BÁO CÁO BẢN ĐẶC TẢ KHÔNG GIANG CỘNG TÁC DOANH NGHIỆP (GREEN-TECH WORKSPACE)
> **Dự án:** Trang trại thông minh Green-Tech  
---

## 1. Cấu trúc Thư mục & Kênh thảo luận (Workspace Architecture)

### 1.1. Cấu trúc Thư mục Lưu trữ (Folder Structure)
Dữ liệu dự án được phân chia rõ ràng trên Cloud Storage để tránh lẫn lộn quyền hạn:

```text
GREEN-TECH_SMART_FARM/
├── 01_Reports_Public/         # Báo cáo tiến độ & chỉ số cho Nhà đầu tư
├── 02_Hardware_Docs/          # Tài liệu thông số & bình luận từ Đối tác thiết bị
├── 03_Sensor_SourceCode/      # Thư mục sensors: Mã nguồn cảm biến (Dev Team)
│   ├── .env.example           # File cấu hình mẫu (KHÔNG chứa secret/API key)
│   └── src/                   # Code điều khiển cảm biến
└── 04_Internal_Config/        # Cấu hình nội bộ bảo mật
    └── .env                   # CHỨA MẬT KHẨU CẢM BIẾN (Bảo mật tuyệt đối)
```

1.2. Cấu trúc Kênh thảo luận (Channel Structure)Hệ thống kênh trên Slack/Lark/Teams được chia theo đúng chức năng công việc:Tên Kênh / TopicLoại KênhQuy định Nội dung & Phạm vi Sử dụng#announcementsRead-OnlyThông báo quan trọng từ Lead, lịch họp, thay đổi tiến độ dự án.#tech-sensor-bugsPublicBáo lỗi kỹ thuật cảm biến, luồng dữ liệu IoT và khởi tạo Task xử lý.#dev-code-syncPrivateĐội Dev trao đổi về mã nguồn, PR, Review Code và Hybrid Sync Workflow.#general-discussionPublicTrao đổi chung giữa các bộ phận, thảo luận bất đồng bộ ngoài kỹ thuật.

2. Quy định Phân quyền Truy cập & Bảo mật File .env2.1. Ma trận Phân quyền Truy cập (Access Control Matrix)Đối tượng / Vai tròThư mục / Tài liệuMức quyền (Role)Mô tả Chi tiếtNhà đầu tư (Investors)01_Reports_Public/ViewerChỉ xem báo cáo tài chính & chỉ số nông nghiệp; không có quyền sửa, bình luận hay tải file.Đối tác thiết bị (Hardware Partners)02_Hardware_Docs/CommenterXem thông số phần cứng và gửi nhận xét/góp ý qua Suggesting Mode / Comment mà không làm hỏng file gốc.Đội ngũ Lập trình (Dev Team)03_Sensor_SourceCode/EditorToàn quyền chỉnh sửa, phát triển mã nguồn cảm biến.Trưởng dự án (Project Lead)Entire Workspace

---

### ✂️ PHẦN 2: Bảo mật File, Hybrid Workflow & Quy trình Task (Copy và dán nối tiếp vào file)

```markdown
### 2.2. Cơ chế Bảo mật File `.env` (Chứa Mật khẩu Cảm biến)
* **Giới hạn phạm vi lưu trữ:** File `.env` chứa mật khẩu cảm biến được đặt riêng trong thư mục `04_Internal_Config/`, chỉ phân quyền đích danh cho Project Lead và Lead Dev.
* **Tắt chia sẻ công khai:** Chuyển trạng thái liên kết sang **Restricted** (Chỉ những người được thêm trực tiếp mới truy cập được).
* **Khóa nâng cao:** Bật 2 tùy chọn bảo mật trong Cài đặt nâng cao của Cloud Storage:
  * ❌ *Disable options to download, print, and copy for commenters and viewers.*
  * ❌ *Editors cannot change permissions and share.*
```
---

## 3. Mô hình Hybrid Workflow cho Đội Dev (Chức năng 1)

Để mã nguồn cảm biến luôn an toàn, không bị mất mát khi kết nối mạng chập chờn tại trang trại:

1. **Lưu trữ Cục bộ (Local First):** Dev viết code và kiểm thử trực tiếp trên SSD máy tính cá nhân.
2. **Đồng bộ Tự động (Background Sync):** Tiến trình ngầm tự động đẩy file từ SSD lên Cloud Storage khi có Internet.
3. **Bộ đệm Ngoại tuyến (Offline Buffer):** Khi mất mạng tại trang trại, mọi thao tác lưu file vẫn ghi nhận bình thường ở Local Cache và tự động sync lên Cloud ngay khi có lại mạng.
4. **Quản lý Phiên bản (Version History):** Bật tính năng theo dõi phiên bản trên Cloud để sẵn sàng Restore về bản code ổn định trước đó nếu phát sinh lỗi.

---

## 4. Quy trình Chuyển đổi Lỗi Cảm biến thành Task (Chức năng 3)

Chuyển đổi các thảo luận về lỗi cảm biến thành Task có Deadline rõ ràng trên Enterprise Workspace:

```text
[ Phát hiện Lỗi Cảm biến ]
           │
           ▼
[ Nhắn vào kênh #tech-sensor-bugs ] ──(Tuân thủ quy tắc No-Hello + [URGENT])
           │
           ▼
[ Tạo Task / Issue chính thức ] ──────(Gắn Assignee + Deadline + Priority)
           │
           ▼
[ Cập nhật Tiến độ trong Thread ] ────(Giữ kênh chat chính luôn gọn gàng)

```
Gửi tin nhắn Asynchronous: Thành viên nhắn báo lỗi vào #tech-sensor-bugs theo mẫu: [URGENT] [Tên Cảm biến] + Hiện trạng lỗi + File Log/Ảnh chụp + Hướng đề xuất.

Tạo Task trực tiếp: Sử dụng tính năng tích hợp (Lark Task / Slack Integration) chuyển tin nhắn thành Task:

Task Name: Fix Bug: Cảm biến độ ẩm Zone A mất kết nối

Assignee: @Dev_Hardware_Lead

Deadline: 17:00 PM - Cùng ngày

Thảo luận qua Thread: Mọi cập nhật tiến độ và trao đổi kỹ thuật chỉ thực hiện trong Thread của Task đó.


---

### ✂️ PHẦN 3: Bàn giao Dự án & Kịch bản Khôi phục File (Copy và dán nối tiếp ở cuối file)

```markdown
---

## 5. Bàn giao Dự án & Kịch bản Kiểm thử Khôi phục File

### 5.1. Quy trình Bàn giao Dự án (Transfer Ownership)
Khi bài tập nhóm hoặc dự án kết thúc, Trưởng dự án bàn giao toàn bộ quyền sở hữu cho Quản lý mới:
1. Mở menu **Share (Chia sẻ)** của thư mục gốc `GREEN-TECH_SMART_FARM/`.
2. Tìm tài khoản của Quản lý mới -> Chuyển trạng thái từ *Editor* sang **Transfer Ownership** (Chuyển quyền sở hữu).
3. Xác nhận chuyển giao. Quản lý cũ tự động chuyển thành quyền *Editor*.

### 5.2. Kịch bản Kiểm thử: Khôi phục nhanh khi bị xóa sạch folder `sensors`
* **Tình huống:** Thành viên lỡ tay xóa sạch thư mục `03_Sensor_SourceCode/` (hoặc folder `sensors`) trên Cloud.
* **Quy trình Khôi phục Nhanh nhất (Fastest Recovery Procedure):**

```text
[ Phát hiện mất File ] ──> [ Mở Cloud Trash / Recycle Bin ] ──> [ Chọn Folder / Files ] ──> [ Bấm Restore ]
```
- Giả định một thành viên lỡ tay xóa sạch folder sensors, hãy trình bày quy trình khôi phục nhanh nhất.
Bước 1 (Vào Thùng rác Cloud): Project Lead hoặc Editor mở ứng dụng Cloud Storage trên trình duyệt Web -> Chọn mục Trash / Recycle Bin (Thùng rác).

Bước 2 (Restore): Tìm thư mục 03_Sensor_SourceCode/ (hoặc sensors) vừa bị xóa -> Chọn Restore (Khôi phục). Thư mục cùng toàn bộ file bên trong và phân quyền cũ sẽ lập tức xuất hiện lại vị trí ban đầu.

Bước 3 (Khôi phục từ Local SSD): Trong trường hợp thùng rác Cloud đã bị dọn dẹp, mở máy tính của Lead Dev (nơi chưa đồng bộ lệnh xóa) và đẩy lại bản sao cục bộ gần nhất lên Cloud.

