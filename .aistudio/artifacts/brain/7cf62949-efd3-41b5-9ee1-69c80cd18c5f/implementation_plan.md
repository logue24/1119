# RedDrop — Nền Tảng Chuyển Dữ Liệu Tốc Độ Cao & Quét Mã QR

Hệ thống web chuyển file và văn bản trực tiếp đa nền tảng với phong cách thiết kế Ruby & Crimson Red sang trọng, hỗ trợ tải lên tới 1.2GB, quét mã QR camera khung vuông thời gian thực, nhập mã xác nhận 6 số, điều hướng liên kết `#code`, xem trước tệp tin, tải chọn lọc hoặc tải tất cả, trình soạn thảo văn bản đếm dòng/từ chi tiết, cùng bộ 5 ngôn ngữ (Tiếng Việt, English, 한국어, 日本語, 简体中文).

---

## User Review & Critical Decisions

> [!IMPORTANT]
> Các quyết định cốt lõi đã được thiết kế nhằm đảm bảo hoạt động 100% không giả lập, không lỗi kết nối:
> - **Lưu trữ & Truyền dữ liệu chuyển tiếp tốc độ cao**: Sử dụng API backend chuyển tiếp chia nhỏ dữ liệu (chunked buffer relay / streaming) trên Express/Vite để truyền file lớn tới 1.2GB ổn định giữa các thiết bị mà không phụ thuộc vào trạng thái mở mạng phức tạp của P2P NAT.
> - **Thời hạn truyền mặc định**: Mã 6 ký tự dễ nhớ và dữ liệu có thời hạn 24 giờ (hoặc tùy chọn hủy ngay sau khi nhận).
> - **Màu sắc chủ đạo**: Giao diện đỏ Ruby hiện đại (`#DC2626` / `#991B1B` / `#7F1D1D`) trên nền tối Carbon sang trọng, tương phản cao, chuẩn WCAG AA.
> - **Quét QR bằng Camera**: Tích hợp video stream trực tiếp vào khung ngắm hình vuông có laser scanline chuyển động, hỗ trợ cả camera sau/trước, lật camera, và tải ảnh QR nếu không có webcam.

---

## 1. Overview & Core Concept

- **Mục tiêu**: Cung cấp giải pháp gửi và nhận file/văn bản nhanh nhất giữa điện thoại và máy tính mà không cần cài đặt ứng dụng hay đăng nhập tài khoản.
- **Đối tượng sử dụng**: Người dùng cá nhân, nhóm làm việc văn phòng, lập trình viên và người dùng di động cần chuyển ảnh, video, tài liệu, snippet code giữa các thiết bị ngay lập tức.
- **Giá trị cốt lõi**:
  - Không cần cáp nối, không cần tài khoản, hoạt động trên mọi trình duyệt (Chrome, Safari, iOS, Android, macOS, Windows).
  - Tích hợp liên kết tức thì qua mã QR hoặc URL hash `/#123456` hoặc `/#code=123456`.

---

## 2. User Experience & Visual Design

### A. Luồng người dùng (User Flows)
1. **Gửi dữ liệu (Send Flow)**:
   - Chuyển đổi giữa 2 tab: **File Transfer** (Kéo thả/chọn file, thanh tiến trình tải lên, hiển thị kích thước file, hỗ trợ file lớn) và **Text Transfer** (Trình nhập văn bản trực tiếp với bộ đếm số dòng, số từ, số ký tự theo thời gian thực).
   - Nhấn **Tạo mã chuyển giao** -> Hệ thống sinh mã 6 chữ số ngẫu nhiên dễ đọc (ví dụ: `849-216`) và mã QR tương ứng.
   - Hiển thị QR Code rõ nét, nút Sao chép liên kết (`https://.../#code=849216`), nút Tải ảnh QR, và nút Sao chép mã số.

2. **Nhận dữ liệu (Receive Flow)**:
   - **Tự động nhận diện**: Nếu người dùng mở link chứa mã qua hash URL `#code=...`, giao diện tự động điền mã và mở phiên nhận.
   - **Nhập mã xác nhận**: Người dùng có thể gõ trực tiếp 6 chữ số vào ô nhập liệu phân cách thẩm mỹ.
   - **Quét mã QR qua Camera**: Nhấn nút "Quét mã QR" -> Xin quyền mở camera -> Hiển thị khung ngắm hình vuông với hiệu ứng quét laser đỏ. Khi đưa mã vào khung, hệ thống tự động nhận diện chuỗi hoặc mã `#code` và kích hoạt tải dữ liệu ngay lập tức.
   - **Xem trước & Tải về**:
     - Hiển thị danh sách file với thông tin dung lượng, định dạng, icon tương ứng.
     - Cho phép **Xem trước (Preview)**: ảnh (lightbox), văn bản (code/text viewer), âm thanh, video, tài liệu.
     - Cho phép **chọn từng file** bằng checkbox hoặc **chọn tất cả** ("Select All").
     - Tải về từng file hoặc tải toàn bộ tệp được chọn dưới dạng file ZIP tiện lợi.

3. **Chuyển đổi ngôn ngữ (i18n)**:
   - Bộ chọn ngôn ngữ nhanh ở thanh điều hướng: Tiếng Việt (VI), English (EN), 한국어 (KO), 日本語 (JA), 简体中文 (ZH).

### B. Nhận diện trực quan & Phong cách (Visual Identity)
- **Tông màu chủ đạo**: Nền tối Carbon (`#090A0F`, `#12141C`) kết hợp màu đỏ Ruby / Crimson sang trọng (`#EF4444`, `#DC2626`, `#B91C1C`).
- **Typography**: `Plus Jakarta Sans` / `Outfit` cho tiêu đề và nhãn thao tác; `JetBrains Mono` / `tabular-nums` cho mã 6 số, số dòng, số từ, dung lượng byte và đồng hồ thời gian.
- **Khung ngắm quét Camera**: Khung vuông góc bo tròn với 4 góc viền đỏ neon tinh tế, thanh quét laser chuyển động lướt lên xuống, chỉ báo rõ ràng khi camera đang hoạt động.

---

## 3. Key Product Decisions & Trade-Offs

- **Truyền nhận File qua Backend Chunked API**:
  - *Lựa chọn*: Tích hợp API trung chuyển trên máy chủ Express/Vite với cơ chế chunked upload & streaming download.
  - *Lý do*: P2P thuần (WebRTC không có STUN/TURN thương mại) thường xuyên thất bại khi 2 thiết bị ở 2 mạng 4G/Wifi khác nhau (Symmetric NAT). Chunked backend relay đảm bảo truyền file 1.2GB thành công 100% trong mọi điều kiện mạng.
- **Bộ máy quét mã QR**:
  - *Lựa chọn*: Kết hợp `BarcodeDetector` API tiêu chuẩn của trình duyệt và thư viện quét canvas hình ảnh `jsQR` dự phòng.
  - *Lý do*: Đảm bảo hoạt động trơn tru trên mọi hệ điều hành (Android, iOS Safari, Windows, macOS) với tốc độ 30fps mượt mà, khung ngắm hình vuông chuẩn xác.
- **Hỗ trợ URL Hash `#code`**:
  - *Lựa chọn*: Lắng nghe sự kiện `hashchange` và tự động đọc `window.location.hash` khi load trang. Bất kỳ máy nào quét QR bằng app camera mặc định của điện thoại sẽ mở link và tự động fetch dữ liệu ngay lập tức.

---

## 4. Technical Architecture & Data Strategy

```
┌────────────────────────────────────────────────────────────────────────┐
│                        RedDrop Web Interface                           │
│                                                                        │
│   ┌─────────────────────┐      ┌───────────────────────────────────┐   │
│   │   Language Switch   │      │   Top Bar (Wordmark, Status, Info)│   │
│   │ [VI | EN | KO...]   │      └───────────────────────────────────┘   │
│   └─────────────────────┘                                              │
│                                                                        │
│   ┌──────────────────────────────┬─────────────────────────────────┐   │
│   │        Tab: Gửi dữ liệu      │       Tab: Nhận dữ liệu         │   │
│   ├──────────────────────────────┼─────────────────────────────────┤   │
│   │ • Upload File (Tới 1.2GB)    │ • Nhập mã 6 chữ số              │   │
│   │ • Nhập Text (Đếm dòng, từ)   │ • Quét Camera Khung Vuông       │   │
│   │ • Sinh mã 6 số + QR Code     │ • Tự nhận link #code            │   │
│   │ • Copy Link / Tải ảnh QR     │ • Xem trước (Preview Modal)     │   │
│   │ • Quản lý thời hạn lưu trữ   │ • Chọn 1 hoặc Chọn tất cả       │   │
│   └──────────────────────────────┴─────────────────────────────────┘   │
└───────────────────────────────────┬────────────────────────────────────┘
                                    │ HTTP / Stream Chunks
                                    ▼
┌────────────────────────────────────────────────────────────────────────┐
│                  Express & Vite Full-Stack API                         │
│                                                                        │
│   • POST /api/transfer/create          (Tạo phiên gửi text/file)       │
│   • POST /api/transfer/:code/chunk     (Tải lên chunk dữ liệu lớn)     │
│   • GET  /api/transfer/:code/meta      (Lấy thông tin và danh sách tệp)│
│   • GET  /api/transfer/:code/file/:id  (Tải stream file riêng lẻ)      │
│   • GET  /api/transfer/:code/zip       (Tải toàn bộ file dạng ZIP)     │
│   • DELETE /api/transfer/:code         (Hủy phiên sau khi nhận)        │
└────────────────────────────────────────────────────────────────────────┘
```

### Chi tiết các mô-đun phát triển:
1. **`src/types/transfer.ts`**: Định nghĩa cấu trúc dữ liệu transfer session, file item, text payload, ngôn ngữ.
2. **`src/i18n/translations.ts`**: Hệ thống đa ngôn ngữ 5 thứ tiếng (Tiếng Việt, Tiếng Anh, Tiếng Hàn, Tiếng Nhật, Tiếng Trung) đầy đủ tất cả nhãn, thông báo, cảnh báo.
3. **`src/services/api.ts`**: Client service quản lý chunked upload, download stream, zip bundle.
4. **`src/components/QRScannerModal.tsx`**: Trình quét mã QR Camera khung vuông chuyên nghiệp với chuyển đổi camera trước/sau, đèn trợ sáng (nếu hỗ trợ) và canvas fallback.
5. **`src/components/QRCodeDisplay.tsx`**: Hiển thị QR Code đỏ/trắng tương phản cao sắc nét, kèm nút tải ảnh PNG/SVG và sao chép liên kết `#code`.
6. **`src/components/SendSection.tsx`**: Khu vực gửi dữ liệu với kéo thả file lớn (thanh progress, hiển thị MB/GB), trình soạn thảo văn bản với thống kê số dòng và số từ.
7. **`src/components/ReceiveSection.tsx`**: Khu vực nhận dữ liệu, xử lý mã 6 số, danh sách tệp với preview (hình ảnh, tài liệu, video, code), nút chọn tất cả và tải về.
8. **`src/components/FilePreviewModal.tsx`**: Hộp thoại xem trước file đa phương tiện.
9. **`server.ts` & `vite.config.ts`**: Cấu hình backend phục vụ các API lưu trữ và truyền stream file lớn không giới hạn/tới 1.2GB.
