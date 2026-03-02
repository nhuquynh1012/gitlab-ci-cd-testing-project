
**HealthyApp – Ứng dụng GitLab CI/CD, Kiểm thử Chức năng, API và Hiệu năng**

# Giới thiệu đề tài
**Đề tài:** Ứng dụng GitLab CI/CD và kiểm thử hiệu năng vào dự án Spring Boot
HealthyApp là một ứng dụng backend được xây dựng bằng Spring Boot với các chức năng cơ bản:
* Đăng ký tài khoản
* Đăng nhập
* Tính chỉ số BMI

# Mục tiêu dự án
* Quản lý mã nguồn bằng Git
* Triển khai CI pipeline bằng GitLab
* Thực hiện kiểm thử chức năng (Manual Testing)
* Thực hiện kiểm thử API (API Testing với Postman)
* Thực hiện kiểm thử hiệu năng bằng JMeter
* Đánh giá khả năng chịu tải của hệ thống

# Công nghệ sử dụng
* Java 17
* Spring Boot
* Maven
* H2 Database
* Git
* GitLab CI/CD
* Apache JMeter
* Postman

# Hướng dẫn chạy dự án trên máy cá nhân

### 1️⃣ Clone project
```bash
git clone <(https://github.com/nhuquynh1012/gitlab-ci-cd-testing-project.git)>
cd healthyapp
```
### 2️⃣ Build project
Chạy dự án trên IntelliJ 

### 3️⃣ Chạy ứng dụng

```bash
./mvnw spring-boot:run
```
Sử dụng **Postman** để biết ứng dụng chạy thành công.
# API chính
### Đăng ký
POST /api/auth/register
### Đăng nhập
POST /api/auth/login
### Tính BMI
GET /api/bmi/calculate?weight=70&height=1.75


# Kiểm thử chức năng (Manual Testing)
Kiểm thử chức năng được thực hiện thủ công bằng Postman trước khi đưa mã nguồn lên GitLab.

# Các Test Case chính
| STT | Chức năng                   | Kết quả          |
| --- | --------------------------- | ---------------- |
| TC1 | Đăng ký hợp lệ              | Thành công       |
| TC2 | Đăng ký trùng username      | Báo lỗi          |
| TC3 | Đăng nhập sai mật khẩu      | Báo lỗi          |
| TC4 | Tính BMI hợp lệ             | Trả kết quả đúng |
| TC5 | Nhập ký tự sai định dạng    | Báo lỗi          |
| TC6 | Password để trống           | Báo lỗi          |
| TC7 | Tính BMI khi chưa đăng nhập | Báo lỗi          |
| TC8 | Đăng nhập khi chưa đăng ký  | Báo lỗi          |
Tất cả chức năng cơ bản được kiểm thử và đảm bảo hoạt động đúng theo yêu cầu.


#  Kiểm thử API (API Testing)
Kiểm thử API được thực hiện bằng **Postman** để đảm bảo các RESTful API hoạt động đúng logic và đúng chuẩn HTTP.

# Nội dung kiểm thử:
* Kiểm tra HTTP Methods: `GET`, `POST`
* Kiểm tra Status Code: `200`, `400`, `401`
* Kiểm tra Request Body hợp lệ và không hợp lệ
* Kiểm tra Response JSON structure
* Kiểm tra Authentication (nếu có)
* Kiểm tra các trường hợp Positive & Negative

### Ví dụ kiểm thử:
 Register thành công → 200 OK
 Register trùng username → 400 Bad Request
 Login sai mật khẩu → 401 Unauthorized
 BMI hợp lệ → Trả đúng giá trị tính toán
Postman Collection được tạo để phục vụ kiểm thử lặp lại và regression test.
 Lưu ý:
Dự án không sử dụng kiểm thử tự động (JUnit/TestNG), chỉ thực hiện kiểm thử thủ công (Manual & API Testing).


# CI Pipeline với GitLab
 Lưu ý quan trọng:
* Dự án được phát triển và chạy trên máy cá nhân (Windows).
* CI pipeline triển khai trên GitLab.
* GitHub dùng để lưu trữ và trình bày mã nguồn.
* Trọng tâm đề tài là GitLab CI (không sử dụng GitHub Actions).

# Pipeline gồm 3 giai đoạn

File cấu hình: `.gitlab-ci.yml`

```yaml
stages:
  - build
  - test
  - deploy
```
🔹 Build
* Clean project
* Đóng gói file `.jar`
🔹 Test
* Kiểm tra build thành công
* Không có kiểm thử tự động
🔹 Deploy
* Deploy demo thông báo thành công bằng GitLab Pages
https://healthyapp-2d8204.gitlab.io/
#  Kiểm thử hiệu năng (Performance Testing)
Công cụ sử dụng: Apache JMeter
Các kịch bản đã thực hiện:
    # Stress Test – Register (50 users)
    * 250 requests
    * 0% lỗi
    * Response ~ 80ms
    # Stress Test – Login (50 users)
    * 0% lỗi
    * Throughput ổn định
    
    # Stress Test – BMI (200 users)
    * 0% lỗi
    * Thời gian phản hồi ổn định

# Step Load Test
| Users | Trạng thái       |
| ----- | ---------------- |
| 100   | Ổn định          |
| 300   | Bắt đầu quá tải  |
| 500   | Lỗi cao (10–16%) |
Hệ thống ổn định ở mức tải vừa phải và bắt đầu vượt ngưỡng từ 300 users trở lên.

# Kết luận
* GitLab CI giúp tự động hóa build và deploy.
* Manual & API Testing đảm bảo hệ thống hoạt động đúng logic.
* Performance Testing xác định giới hạn chịu tải của hệ thống.
* Hệ thống ổn định ở mức 100 users đồng thời.
* Từ 300 users trở lên bắt đầu xuất hiện lỗi.
Dự án phù hợp với mô hình DevOps cơ bản và có thể mở rộng thêm trong tương lai.

# Hướng phát triển
Có thể mở rộng:
* Tích hợp kiểm thử tự động (JUnit/TestNG)
* Tích hợp SonarQube
* Deploy lên Cloud (AWS / Azure)
* Docker hóa ứng dụng
* Triển khai Kubernetes
* Kết hợp hệ thống giám sát (Prometheus + Grafana)
