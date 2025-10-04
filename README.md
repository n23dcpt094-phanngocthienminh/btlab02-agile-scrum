
# Lab 02 – Phân tích yêu cầu & Thiết kế UML (Hotel Booking System)
👥 Thành viên nhóm
Phan Ngọc Thiên Minh _ N23DCPT094 -Leader
Nguyễn Ngọc Bảo Linh _ N23DCPT089
Lê Minh Khang _ N23DCPT083

## 🎯 Mục tiêu
- Học cách mô tả yêu cầu phần mềm bằng UML.  
- Thiết kế cơ sở dữ liệu (ERD).  
- Vẽ Use Case và Sequence Diagram.  
- Ứng dụng phương pháp Agile – Scrum với Jira.  
- Đồng bộ artefact lên GitHub.  

---

## 📝 1. Phân tích yêu cầu hệ thống

### Các thực thể (Entity chính)
- Guest (Khách hàng)  
- RoomType (Loại phòng)  
- Room (Phòng cụ thể)  
- Reservation (Đặt phòng)  
- Payment (Thanh toán)  
- Staff (Nhân viên: lễ tân, quản lý)  

### Chức năng hệ thống
- Khách hàng: tìm phòng, xem chi tiết, đặt phòng, thanh toán online.  
- Lễ tân: quản lý đặt phòng, check-in, check-out.  
- Quản lý: quản lý phòng & giá, báo cáo doanh thu.  
- Buồng phòng: cập nhật trạng thái phòng.  

---

## 🖼 2. Use Case Diagram

### Actor
- Guest  
- Receptionist  
- Manager  
- Payment Gateway  
- Housekeeping  

### Use Cases chính
- Tìm phòng, Xem chi tiết phòng  
- Đặt phòng online (Booking)  
- Thanh toán online  
- Check-in / Check-out  
- Quản lý phòng & giá  
- Quản lý đặt phòng (cho lễ tân)  
- Công việc buồng phòng  
- Báo cáo doanh thu  

```plantuml
@startuml
actor Guest
actor Receptionist
actor Manager
actor PaymentGateway
actor Housekeeping

Guest --> (Tìm phòng & Xem chi tiết)
Guest --> (Đặt phòng online)
Guest --> (Thanh toán online)

PaymentGateway --> (Thanh toán online)

Receptionist --> (Check-in)
Receptionist --> (Check-out)
Receptionist --> (Quản lý đặt phòng)

Manager --> (Quản lý phòng & giá)
Manager --> (Báo cáo doanh thu)

Housekeeping --> (Công việc buồng phòng)

@enduml
```

---

## 🔄 3. Sequence Diagram

### a) Luồng đặt phòng online
```plantuml
@startuml
actor Guest
participant "Booking System" as System
participant "Payment Gateway" as Pay

Guest -> System: Chọn phòng, nhập thông tin
System -> Pay: Gửi yêu cầu thanh toán
Pay --> System: Trả kết quả (OK/Fail)
System -> Guest: Xác nhận đặt phòng, gửi email
@enduml
```

### b) Luồng Check-in/Check-out
```plantuml
@startuml
actor Receptionist
participant "Booking System" as System
participant "Room" as Room

Receptionist -> System: Tra cứu mã đặt phòng
System -> Room: Gán phòng (Check-in)
Receptionist -> System: Cập nhật trạng thái

Receptionist -> System: Yêu cầu Check-out
System -> Room: Tổng hợp chi phí, cập nhật buồng phòng
Receptionist -> System: Thu tiền, xác nhận
@enduml
```

---

## 🗂 4. Thiết kế cơ sở dữ liệu (ERD)

### Các bảng
- **Guest(GuestID, Name, Phone, Email, Address)**  
- **RoomType(TypeID, Name, Price, Capacity, Description)**  
- **Room(RoomID, TypeID, Status, Floor)**  
- **Reservation(ResvID, GuestID, RoomID, StaffID, CheckInDate, CheckOutDate, Status)**  
- **Payment(PaymentID, ResvID, Amount, Method, Status, Date)**  
- **Staff(StaffID, Name, Role, Username, PasswordHash)**  

### ERD với PlantUML
```plantuml
@startuml
!theme spacelab

entity "Guest" as Guest {
    * GuestID : INT <<PK>>
    --
    Name : VARCHAR(100)
    Phone : VARCHAR(15)
    Email : VARCHAR(100)
    Address : VARCHAR(255)
}

entity "RoomType" as RoomType {
    * TypeID : INT <<PK>>
    --
    Name : VARCHAR(50)
    Price : DECIMAL(10,2)
    Capacity : INT
    Description : TEXT
}

entity "Room" as Room {
    * RoomID : INT <<PK>>
    --
    TypeID : INT <<FK>>
    Status : ENUM('Available','Booked','Cleaning')
    Floor : INT
}

entity "Reservation" as Reservation {
    * ResvID : INT <<PK>>
    --
    GuestID : INT <<FK>>
    RoomID : INT <<FK>>
    StaffID : INT <<FK>>
    CheckInDate : DATE
    CheckOutDate : DATE
    Status : ENUM('Pending','Confirmed','CheckedOut')
}

entity "Payment" as Payment {
    * PaymentID : INT <<PK>>
    --
    ResvID : INT <<FK>>
    Amount : DECIMAL(10,2)
    Method : ENUM('Cash','Credit','Online')
    Status : ENUM('Pending','Paid','Refunded')
    Date : DATETIME
}

entity "Staff" as Staff {
    * StaffID : INT <<PK>>
    --
    Name : VARCHAR(100)
    Role : ENUM('Receptionist','Manager')
    Username : VARCHAR(50)
    PasswordHash : VARCHAR(255)
}

Guest ||--o{ Reservation : "1–N"
RoomType ||--o{ Room : "1–N"
Room ||--o{ Reservation : "1–N"
Reservation ||--o{ Payment : "1–N"
Staff ||--o{ Reservation : "1–N"

@enduml
```

---

## 📌 5. Agile – Scrum trên Jira

### Product Backlog
- Đăng ký/Đăng nhập khách hàng  
- Tìm phòng & Xem chi tiết phòng  
- Đặt phòng & thanh toán online  
- Check-in/Check-out (Lễ tân)  
- Quản lý phòng & giá (Manager)  
- Báo cáo doanh thu  
- Công việc buồng phòng  

### Sprint Plan
- **Sprint 1**: Auth, Tìm phòng, Xem chi tiết phòng  
- **Sprint 2**: Đặt phòng & giữ chỗ  
- **Sprint 3**: Thanh toán & Check-in/out  
- **Sprint 4**: Báo cáo, housekeeping, tối ưu & release  

### Jira Board
- To Do → In Progress → Code Review → Testing → Done  

Mỗi user story mô tả theo format:  
```
As a [role], I want [function], so that [benefit].
```

---

## ✅ Artefact cần nộp
- Use Case Diagram (.png / .drawio / PlantUML)  
- 2 Sequence Diagram (Đặt phòng, Check-in/out)  
- ERD diagram (PlantUML / PNG)  
- README.md (tài liệu mô tả)  
