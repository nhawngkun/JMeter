# BÁO CÁO BÀI TẬP: CẤU HÌNH VÀ KIỂM THỬ HIỆU NĂNG API BẰNG APACHE JMETER

## 1. Thông tin sinh viên
- **Họ và tên:** [Điền họ tên của bạn]

---

## 2. Mục tiêu bài tập
- Biết cách tìm kiếm, khảo sát thông tin cấu hình API (Method, URL, Path, Body) từ các công cụ quản lý API (Apidog).
- Sử dụng thành thạo Apache JMeter để thiết lập kịch bản kiểm thử hiệu năng cho các API vừa khảo sát.
- Hiểu cách cấu hình HTTP Request Sampler dựa trên các dữ liệu thực tế.

---

## 3. Nhật ký khảo sát dữ liệu API trên Apidog để đưa vào JMeter

Để tiến hành giả lập tải bằng JMeter, trước tiên em sử dụng công cụ Apidog để khởi tạo và lấy thông tin kịch bản mẫu của hệ thống Cửa hàng thú cưng (Pet Store).

![Khởi tạo dự án Pet Store](anh1.png)
*Hình 1: Khởi tạo dự án mẫu trên Apidog để lấy thông tin kịch bản API*

![Giao diện quản lý API Endpoints](anh2.png)
*Hình 2: Khảo sát các thư mục chức năng (Thú cưng, Cửa hàng, Người dùng...) để chọn API kiểm thử*

---

## 4. Chi tiết các bước cấu hình kịch bản trong phần mềm JMeter (Ảo)

Dựa trên các thông số thu thập được từ Apidog, em tiến hành tạo các cấu hình tương ứng trong **Apache JMeter** như sau:

### Bước 4.1: Cấu hình Nhóm người dùng ảo (Thread Group) trong JMeter
- **Number of Threads (users):** Thiết lập `50` (Giả lập 50 người dùng ảo truy cập đồng thời).
- **Ramp-up period (seconds):** Thiết lập `10` giây (Cứ mỗi giây hệ thống tự động kích hoạt thêm 5 người dùng ảo).
- **Loop Count:** Thiết lập `1` (Mỗi người dùng thực hiện kịch bản 1 lần).

---

### Bước 4.2: Cấu hình kịch bản lấy danh sách thú cưng (GET Request)
Từ thông tin khảo sát trên Apidog (Hình 3, Hình 4, Hình 5), API trả về trạng thái mã thành công `200 Passed`.

![Khảo sát thông tin API GET](anh3.png)
*Hình 3: Xác định Phương thức GET và Path /pets trên Apidog*

![Menu lựa chọn môi trường](anh4'.png)
*Hình 4: Xác định môi trường chạy và tên miền hệ thống*

![Kết quả chạy thử trên Apidog](anh5.jpg)
*Hình 5: Kết quả phản hồi thành công (200 Passed) làm cơ sở đối chiếu số liệu*

👉 **Thông số nhập tương ứng vào ô HTTP Request trong JMeter:**
- **Protocol [http]:** `https`
- **Server Name or IP:** `api.petstoreapi.com` (Hoặc địa chỉ IP/Domain của server mock)
- **HTTP Request [Method]:** Chọn `GET` từ menu thả xuống của JMeter.
- **Path:** Điền `/v1/pets`

---

### Bước 4.3: Cấu hình kịch bản tạo mới dữ liệu (POST Request)
Khảo sát từ Hình 6, chức năng này yêu cầu gửi kèm dữ liệu cấu trúc vật nuôi dưới định dạng JSON với mã thành công `201 Passed`.

![Kết quả cấu hình POST trên Apidog](anh6.jpg)
*Hình 6: Khảo sát cấu trúc Body JSON của phương thức POST*

👉 **Thông số nhập tương ứng vào ô HTTP Request trong JMeter:**
- **HTTP Request [Method]:** Chọn `POST`
- **Path:** Điền `/v1/pets`
- **Mục Body Data trong JMeter:** Copy và dán toàn bộ đoạn mã JSON cấu trúc chú chó "Max" từ ảnh vào:
```json
{
  "name": "Max",
  "species": "DOG",
  "breed": "Golden Retriever",
  "ageMonths": 24,
  "size": "LARGE",
  "color": "Golden"
}