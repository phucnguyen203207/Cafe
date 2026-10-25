# Kiểm tra bản web

Ngày kiểm tra: 25/10/2026. Dữ liệu kiểm thử chỉ ở origin localhost, không được đóng gói trong nguồn website.

## Đã chạy

- Kiểm tra cú pháp Node cho app.js, core.js, db.js, art.js, excel.js, sw.js.
- 10 kiểm thử nghiệp vụ trong tests/core.test.mjs; 1 kiểm thử định dạng xuất Excel trong tests/excel.test.mjs.
- Máy chủ cục bộ trả HTTP 200.
- Trình duyệt hiển thị thực đơn 16 món và giao diện tiếng Việt.
- Thêm nhanh cà phê sữa đá giá 35.000.
- Tùy chỉnh bạc xỉu size L, 50% đường, trân châu: giá 57.000. Tổng đơn 92.000.
- Khi chưa mở ca, Thanh toán yêu cầu mở ca trước; mở ca 500.000 rồi tiếp tục đúng đơn.
- Khách đưa 100.000, hiển thị tiền thừa 8.000.
- Hoàn tất ghi nhận đơn, tạo lệnh in chờ; báo cáo ngày hiển thị 92.000 doanh thu, 1 bill, trung bình 92.000 và 2 món bán.

## Cần nghiệm thu thêm

- Thiết bị Windows POS / Android / iPad thật, màn dọc/ngang và bàn phím ảo.
- Độ trễ p95 dưới 100 ms, hàng trăm món, peak load nhiều thiết bị.
- Máy in thật, giấy K58/K80 và font driver. Chưa có kết nối két tiền.
- Service Worker và IndexedDB trong từng chính sách trình duyệt, ngắt kết nối rồi tải lại.
- Mất điện, đầy ổ đĩa, xóa cache, restore backup trong các tình huống lỗi.
- Tích hợp máy chủ, bảo mật, hóa đơn và thanh toán bên thứ ba trước vận hành thật.
