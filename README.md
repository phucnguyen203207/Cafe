# Mộc Café — Website bán hàng

Website POS tiếng Việt, chạy không cần cài thư viện hoặc build. Đây là bản dùng thử chức năng trên một trình duyệt, chưa phải hệ thống vận hành nhiều thiết bị có máy chủ.

## Chạy tại máy

Cần Node.js 20 trở lên:

```sh
node server.mjs
```

Mở http://127.0.0.1:4173. Không mở index.html trực tiếp bằng file:// vì ES modules, IndexedDB và Service Worker cần HTTP/HTTPS.

## Dùng thử

Mặc định vào tài khoản chủ quán để xem giao diện. Bấm tên người dùng để đổi tài khoản bằng PIN. PIN mẫu: chủ quán **123456**, quản lý **222222**, thu ngân **111111**. Có thể đổi PIN bằng Cài đặt. Phân quyền này chỉ nằm trong trình duyệt, không bảo vệ khỏi DevTools hoặc can thiệp dữ liệu thiết bị.

1. Mở ca, nhập tiền mặt đầu ca.
2. Chọn Tại bàn / Mang đi / Giao hàng trước khi thêm món. Nút + thêm món mặc định; chạm ảnh để tùy chỉnh.
3. Gửi pha chế. Tab Pha chế có Bắt đầu / Đã xong; các tab cùng trình duyệt cập nhật bằng BroadcastChannel.
4. Thanh toán tiền mặt, hoặc ghi nhận chuyển khoản, thẻ, MoMo, ZaloPay đã thực hiện bên ngoài. Có thể chia nhiều hình thức, tính tiền thừa, giảm giá và phụ thu.
5. Đơn đã thanh toán vào lịch sử, trừ kho theo công thức; hàng đợi in giữ bill đến khi nhân viên xác nhận đã in.
6. Kết ca nhập tiền thực đếm. Báo cáo có doanh thu ngày/giờ, phương thức, món bán chạy, số bill, trung bình đơn, xuất Excel .xlsx.

## Chức năng có trong bản này

- 16 món với ảnh chụp minh họa lưu offline, tìm không dấu, danh mục, ghim món, thêm/sửa thực đơn, tải ảnh thật, đánh dấu hết món, combo dưới dạng một món. Nguồn ảnh được ghi trong photo-sources.json; ảnh mặc định là ảnh tham khảo, không phải ảnh chụp tại quán. Ảnh tự tải lên luôn được ưu tiên.
- Size M/L, đường/đá, topping, ghi chú, giá cộng thêm theo mang đi/giao hàng. Giá trong đơn cũ được giữ nguyên khi sửa thực đơn.
- 12 bàn, giữ đơn mở, chuyển/gộp bàn, tách số lượng từng món. Bỏ giảm giá/phụ thu trước khi tách/gộp; thao tác chọn số lượng thay cho kéo thả.
- Tiền VND nguyên, giảm %/tiền, voucher mẫu MOC10, phụ thu, làm tròn 1/100/500/1000 đồng. Thanh toán và trừ kho trong một transaction IndexedDB, từ chối thu tiền lần hai cho đơn đã hoàn tất.
- Hủy món/bill bắt buộc quản lý/chủ quán và lý do. Hoàn toàn bộ tiền trong ca gốc còn mở, ghi nhật ký; hoàn tiền sau ca cần xử lý kế toán, chưa hỗ trợ tại giao diện. Hoàn tiền không tự hoàn nguyên liệu vì đồ uống đã pha.
- Ca, đối chiếu két, lịch sử theo người mở ca; tiền thừa không tính vào doanh thu.
- In bill K80/K58 qua trình duyệt, xem hàng đợi và thử in lại. Chưa xác nhận tự động trạng thái máy in.
- Nhập kho, công thức từng món, cảnh báo sắp hết. Định lượng một đơn vị cố định theo món, chưa có công thức riêng cho size/topping.
- Ghi thông tin khách, yêu cầu MST/đơn vị/địa chỉ/email và yêu cầu xử lý sai sót hóa đơn; **chưa phát hành HĐĐT**.
- Logo, màu chủ đạo, sáng/tối, chế độ chỉ chữ, giao diện responsive, phím / và F8, focus trap hộp thoại, reduced motion.
- IndexedDB, Service Worker cache tài nguyên sau lần chuẩn bị đầu tiên, BroadcastChannel giữa tab cùng origin. Sao lưu luân phiên 24 vị trí theo giờ khi có thay đổi; tải JSON, khôi phục có kiểm tra định dạng. Nhật ký không có thao tác xóa trên UI.

## Phần cần triển khai tiếp trước khi dùng vận hành thật

| Yêu cầu | Trạng thái / phần cần thêm |
|---|---|
| FR-09 VietQR, thẻ, ví | Hiện chỉ ghi nhận thủ công. Cần tài khoản quán, API/SDK và webhook xác thực tiền vào. Không lưu thông tin thẻ. |
| FR-16/17 HĐĐT | Lưu yêu cầu, chưa tích hợp. Cần nhà cung cấp, máy chủ giữ khóa API, điều kiện pháp lý của quán, luồng phát hành/điều chỉnh/thay thế. Phiếu in không thay HĐĐT. |
| FR-18/19/20 bảo mật | PIN và kiểm tra quyền tại giao diện, log IndexedDB. Cần đăng nhập máy chủ, xác thực quyền ở mọi API, log append-only và chính sách dữ liệu để đạt bảo mật thực tế. |
| FR-23 xem từ xa | Trang web mở được trên điện thoại nhưng mỗi thiết bị có dữ liệu riêng. Cần backend và quyền chủ quán để xem số liệu từ xa. |
| FR-25 khách thân thiết | Chưa triển khai tích điểm, mua X tặng Y, khung giờ khuyến mãi. |
| FR-27 ứng dụng giao đồ ăn | Chưa kết nối GrabFood/ShopeeFood; cần hợp đồng/quyền đối tác. |
| FR-29 QR gọi món | Chưa triển khai cổng khách gọi món và xác thực QR theo bàn. |
| NFR-01 nhiều thiết bị / UI-01 | Chưa kiểm thử tải cao, p95 dưới 100 ms hoặc hàng trăm món; cần đo trên thiết bị mục tiêu. |
| NFR-02 đồng bộ offline | Bán offline cục bộ sau khi cache thành công, chưa có đồng bộ máy chủ. Cần outbox sự kiện, idempotency phía máy chủ, kiểm soát xung đột và kiểm thử ngắt mạng. |
| NFR-03 sao lưu / cập nhật | Bản lưu tự động cùng thiết bị không chống hỏng máy. Cần sao lưu ra máy chủ và thử khôi phục, phiên bản schema, rollout có kiểm soát. Service Worker mới chờ tab cũ đóng để hạn chế thay tài nguyên giữa ca. |
| NFR-04 tương thích | Chưa nghiệm thu Windows/Android/iPad thực tế và xoay màn hình. |
| NFR-06 hỗ trợ | Chưa có hotline hoặc SLA. Cần tổ chức vận hành thực tế. |
| UI-05 kéo thả | Dùng bảng chọn số lượng để tách đơn; chưa có kéo thả. |
| UI-08 ảnh nền | Có logo/màu, chưa có tải ảnh nền. |

## Dữ liệu & offline

Địa chỉ localhost, 127.0.0.1 và địa chỉ đã triển khai là các origin riêng, không tự chia sẻ đơn. Chuyển môi trường bằng tải sao lưu JSON rồi khôi phục ở nơi đích. Xóa dữ liệu trình duyệt hoặc dùng chế độ riêng tư có thể làm mất dữ liệu; tải bản sao lưu ra ngoài thiết bị thường xuyên. Khi khôi phục, giữ tài khoản và nhật ký hiện tại, không nhập mật khẩu từ JSON.

Lệnh in có trạng thái chờ / nhân viên xác nhận. Không có bridge máy in, tự mở két, tem ly hoặc tự thử lại qua phần cứng. Browser print không báo chính xác tình trạng máy in.

## Kiểm tra

```sh
node --check dist/src/app.js
node --test tests/core.test.mjs
```

Kiểm thử nghiệp vụ bao gồm tìm tiếng Việt, giảm/làm tròn, thanh toán kết hợp, từ chối tiền thiếu/lẻ, trừ kho, từ chối thanh toán lặp, yêu cầu mở ca, đối chiếu tiền mặt, quyền và kiểm tra sao lưu. Kiểm thử trình duyệt được ghi trong QA.md.
