# Hướng dẫn thiết lập Landing Page Cao Ban Long

## 1. Thiết lập Google Sheets và Google Apps Script

### 1.1. Tạo Google Sheets
1. Truy cập [Google Sheets](https://sheets.google.com)
2. Tạo bảng tính mới
3. Đặt tên các cột trong hàng đầu tiên:
   ```
   Timestamp | Họ và tên | Số điện thoại | Tỉnh/Thành phố | Quận/Huyện | Phường/Xã | Địa chỉ | Gói sản phẩm
   ```

### 1.2. Thiết lập Google Apps Script
1. Trong Google Sheets, chọn Extensions > Apps Script
2. Tạo file script mới với nội dung sau:
   ```javascript
   function doPost(e) {
     const sheet = SpreadsheetApp.getActiveSpreadsheet().getActiveSheet();
     const data = JSON.parse(e.postData.contents);
     
     // Map treatment codes to readable names
     const treatmentNames = {
       '1_hop': 'Liệu trình dùng thử 1 hộp (199k)',
       '2_hop': 'Liệu trình 1 tháng 2 hộp (320k)',
       '4_hop': 'Liệu trình 2 tháng 4 hộp (600k)'
     };
     
     // Add row to sheet
     sheet.appendRow([
       data.timestamp,
       data.name,
       data.phone,
       data.province,
       data.district,
       data.ward,
       data.address,
       treatmentNames[data.treatment]
     ]);
     
     return ContentService.createTextOutput(JSON.stringify({ 'result': 'success' }))
       .setMimeType(ContentService.MimeType.JSON);
   }
   ```
3. Lưu script
4. Chọn Deploy > New deployment
5. Chọn "Web app"
6. Thiết lập:
   - Execute as: Me
   - Who has access: Anyone
7. Click Deploy
8. Cấp quyền truy cập khi được yêu cầu
9. Sao chép URL web app được cung cấp

### 1.3. Cập nhật Landing Page
1. Mở file `index.html`
2. Tìm biến `GOOGLE_SCRIPT_URL`
3. Thay thế `'YOUR_GOOGLE_SCRIPT_URL'` bằng URL web app đã sao chép

## 2. Thiết lập Facebook Pixel

1. Truy cập [Facebook Ads Manager](https://business.facebook.com/adsmanager)
2. Tạo Pixel mới hoặc sử dụng Pixel hiện có
3. Sao chép Pixel ID
4. Trong file `index.html`, thay thế `YOUR_PIXEL_ID` bằng ID thật

## 3. Tối ưu hóa hình ảnh

### 3.1. Chuẩn bị hình ảnh
1. Chuẩn bị các hình ảnh theo kích thước yêu cầu:
   - `caobanlong.jpg`: 800x600px
   - `caobanlong1.jpg` đến `caobanlong6.jpg`: 100x100px
   - `process.jpg`: 800x600px
   - `customer1.jpg` đến `customer6.jpg`: 100x100px
   - `offer.gif`: 150x100px

2. Tối ưu hóa hình ảnh:
   - Sử dụng [TinyPNG](https://tinypng.com) để nén ảnh
   - Giữ dung lượng dưới 200KB/ảnh

3. Đặt tất cả hình ảnh vào thư mục `/images`

### 3.2. Tạo GIF ưu đãi với Canva
1. Mở [Canva](https://www.canva.com)
2. Tạo thiết kế mới với kích thước 150x100px
3. Thêm:
   - Nền màu cam (#F5A623)
   - Text "ĐẶT HÀNG NGAY" (font đậm, màu trắng)
   - Animation "Blink"
4. Xuất dưới dạng GIF
5. Lưu là `offer.gif` trong thư mục `/images`

## 4. Triển khai trên GitHub Pages

### 4.1. Tạo Repository
1. Truy cập GitHub và tạo repository mới: `caobanlong-landing`
2. Clone repository về máy:
   ```bash
   git clone https://github.com/yourusername/caobanlong-landing.git
   ```

### 4.2. Push code
1. Copy tất cả file vào thư mục repository
2. Commit và push:
   ```bash
   git add .
   git commit -m "Initial commit"
   git push origin main
   ```

### 4.3. Kích hoạt GitHub Pages
1. Vào repository Settings > Pages
2. Source: chọn branch `main`
3. Folder: chọn `/ (root)`
4. Save

Trang web sẽ có sẵn tại: `https://yourusername.github.io/caobanlong-landing`

## 5. Kiểm tra và tối ưu

### 5.1. Kiểm tra chức năng
- Form đặt hàng
- Popup thành công
- Đồng hồ đếm ngược
- Select tỉnh/thành phố
- Facebook Pixel events

### 5.2. Kiểm tra responsive
- Desktop (>1024px)
- Tablet (768px-1024px)
- Mobile (<768px)

### 5.3. Kiểm tra SEO
Sử dụng [Google Mobile-Friendly Test](https://search.google.com/test/mobile-friendly)
