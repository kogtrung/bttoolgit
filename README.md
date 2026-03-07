# Mini Task Manager

Hệ thống quản lý công việc cá nhân, cho phép người dùng theo dõi, tạo, cập nhật và hoàn thành công việc hàng ngày.

## Giới thiệu dự án
- Tên dự án: Mini Task Manager
- Mô tả: Hệ thống quản lý công việc cá nhân.
- Mục đích: Cho phép người dùng quản lý công việc hàng ngày thông qua các chức năng cơ bản.

## Thành viên nhóm
- Thái Nguyễn Công Trung - 2280603472 - Công Trung
- Huỳnh Đình Hưng - 2280601292 - 2280601292_HuỳnhĐìnhHưng
- Trần Minh Đức - 2280600743 - Minh Đức
- Nguyễn Đình An - 2280603898 - 3898_Nguyễn ĐÌnh An
- Phạm Minh Hải - 2280600820 - phamminhhai2503
- Trần Thái Thông - 2280603134 - 2280603134-Trần Thái Thông
- Nguyễn Văn Ngọc Anh - 2280600073 - Nguyen Van Ngoc Anh

## Chức năng hệ thống
1. Đăng ký tài khoản
2. Đăng nhập
3. Tạo công việc
4. Xem danh sách công việc
5. Cập nhật công việc
6. Đánh dấu hoàn thành công việc
7. Xóa công việc

## Công nghệ sử dụng
- Java 17
- Spring Boot
- Spring MVC
- Spring Data JPA
- Thymeleaf (render giao diện phía server)
- MySQL
- Maven

## Cấu trúc dự án
```
mini-task-manager/
├─ src/
│  ├─ main/
│  │  ├─ java/
│  │  │  └─ com/example/mini_task/
│  │  │     ├─ controller/
│  │  │     ├─ service/
│  │  │     ├─ repository/
│  │  │     ├─ model/
│  │  │     └─ config/
│  │  └─ resources/
│  │     ├─ templates/
│  │     └─ static/
├─ pom.xml
└─ README.md
```
- controller: Xử lý HTTP request, ánh xạ URL, trả về view hoặc dữ liệu.
- service: Chứa logic nghiệp vụ, giao tiếp giữa controller và repository.
- repository: Tầng truy cập dữ liệu dùng Spring Data JPA.
- model: Định nghĩa thực thể (entity), DTO nếu có.
- config: Cấu hình ứng dụng (bảo mật, Web/MVC, JPA...).
- resources/templates: Template Thymeleaf.
- resources/static: Tài nguyên tĩnh (CSS, JS, hình ảnh).

## Mô hình dữ liệu
- User
  - Thuộc tính: id, username, password
  - Ghi chú: username duy nhất; password lưu dưới dạng băm.
- Task
  - Thuộc tính: id, title, description, status, user, createdAt
  - Ghi chú: status là enum (PENDING, IN_PROGRESS, COMPLETED); ManyToOne tới User; createdAt dùng LocalDateTime.

## Quy trình phát triển
- Nhánh:
  - main: Nhánh ổn định, đã qua kiểm thử và sẵn sàng triển khai.
  - develop: Nhánh tích hợp, hợp nhất các nhánh tính năng trước khi lên main.
  - feature/*: Nhánh triển khai từng chức năng độc lập.
- Nguyên tắc:
  - Không phát triển trực tiếp trên main.
  - Các chức năng được triển khai trên feature branch.
  - develop được dùng để tích hợp trước khi đưa lên main.

## Phân chia chức năng
- feature/register
- feature/login
- feature/task-create
- feature/task-list
- feature/task-update
- feature/task-complete
- feature/task-delete

```
main
└─ develop
   ├─ feature/register
   ├─ feature/login
   ├─ feature/task-create
   ├─ feature/task-list
   ├─ feature/task-update
   ├─ feature/task-complete
   └─ feature/task-delete
```

## Triển khai & vận hành
- Yêu cầu môi trường:
  - Java 17
  - Maven 3.8+
  - MySQL 8.x
- Cấu hình database và biến môi trường (.env):
  - Ứng dụng sử dụng thư viện java-dotenv để nạp biến môi trường từ file .env ở thư mục gốc dự án.
  - Tạo file .env tại đường dẫn c:/Workspace/Congcu/Baitap/mini-task/.env với nội dung tối thiểu:
    ```
    DB_URL=jdbc:mysql://localhost:3306/mini_task_db?useSSL=false&serverTimezone=Asia/Ho_Chi_Minh
    DB_USERNAME=<DB_USERNAME_THAT_SU_DUNG>
    DB_PASSWORD=<DB_PASSWORD_THAT_SU_DUNG>
    SERVER_PORT=8080
    ```
  - Lưu ý:
    - .env đã được cấu hình trong .gitignore, không commit lên repository.
    - Có thể thay thế bằng biến môi trường hệ điều hành (nếu không dùng .env).
  - Cấu hình tương ứng trong src/main/resources/application.yaml:
```yaml
spring:
    datasource:
      url: ${DB_URL}
      username: ${DB_USERNAME}
      password: ${DB_PASSWORD}
      driver-class-name: com.mysql.cj.jdbc.Driver
    jpa:
      hibernate:
        ddl-auto: update
      show-sql: true
      properties:
        hibernate:
          format_sql: true
  server:
    port: ${SERVER_PORT:8080}
```
- Khởi tạo database:
  - Tạo schema mini_task_manager trong MySQL.
  - Cấp quyền truy cập cho user tương ứng.
- Cách chạy ứng dụng:
```bash
mvn clean install
mvn spring-boot:run
```
- Truy cập:
  - Ứng dụng chạy tại http://localhost:8080.
 - Kiểm thử:
   - Môi trường test dùng H2 in-memory, cấu hình trong src/test/resources/application-test.yaml, không phụ thuộc .env.

## Kết luận
Mini Task Manager là hệ thống quản lý công việc cá nhân đơn giản, ổn định và dễ mở rộng. Kiến trúc phân lớp, quy trình nhiều nhánh và công nghệ chuẩn doanh nghiệp giúp đội ngũ dễ dàng bổ sung tính năng mới (nhắc việc, phân loại, ưu tiên, lịch, thông báo) theo nhu cầu thực tế.
