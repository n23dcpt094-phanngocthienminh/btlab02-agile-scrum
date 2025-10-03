# LAB02 - Hotel Booking System (Use Case & Sequence)

## Mô tả ngắn
Hệ thống quản lý đặt phòng khách sạn (mini project) bao gồm: tìm phòng, đặt phòng online, thanh toán, check-in/check-out, quản lý phòng, và công việc housekeeping.

## Actors (tác nhân)
- Guest (khách hàng)
- Receptionist (lễ tân)
- Manager (quản lý)
- Payment Gateway (hệ thống thanh toán bên thứ 3)
- Housekeeping (buồng phòng)

## Use Cases chính
1. Search rooms: Khách tìm phòng theo ngày, loại, số người.
2. View room details: Xem mô tả, giá, tiện nghi.
3. Hold room: Hệ thống giữ phòng tạm khi khách điền thông tin.
4. Book room (Online): Khách đặt phòng (gồm hold + thông tin khách + payment).
5. Make payment: Thanh toán online qua Payment Gateway.
6. Check-in / Check-out: Lễ tân xử lý đến/đi, gán phòng, tính tiền.
7. Manage rooms & rates: Quản lý loại phòng, giá, trạng thái.
8. Manage reservations: Lễ tân duyệt/hủy/sửa đặt phòng.
9. Housekeeping tasks: Đánh dấu dọn/dirty/ready.
10. Revenue reports: Báo cáo doanh thu cho quản lý.

## Sequence Diagrams (mô tả)
-luồng Đặt phòng online:
  1. Guest chọn phòng → hệ thống hold room.
  2. Guest điền thông tin + thanh toán.
  3. Payment Gateway trả kết quả → nếu success confirm reservation và gửi email; nếu fail hủy hold.

-luồng Check-in/Check-out bởi Receptionist:
  - Check-in: Tra cứu mã đặt phòng → gán phòng → update trạng thái → notify housekeeping.
  - Check-out: Tính chi phí → thu tiền → finalize & mark room dirty → generate receipt.

