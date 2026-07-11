---
title: "Nhật Ký Tuần 10: Xây Dựng Ứng Dụng Cloud Native & Thiết Lập Hệ Thống Observability"
date: 2024-01-01
weight: 2
chapter: false
pre: " <b> 1.10. </b> "
---
## Mục tiêu tuần 10:
Tuần này tập trung vào việc làm chủ tư duy thiết kế hệ thống thế hệ mới, xây dựng các ứng dụng có khả năng co giãn độc lập và thiết lập hệ thống giám sát chuyên sâu (Observability) để đảm bảo tính ổn định tối đa khi vận hành thực tế:

1. **Xây Dựng Ứng Dụng Cloud Native Hoàn Chỉnh:** Thiết kế kiến trúc ứng dụng tận dụng tối đa sức mạnh của điện toán đám mây, chia nhỏ hệ thống thành các dịch vụ độc lập, giúp tối ưu hiệu năng và dễ dàng bảo trì.
2. **Tăng Cường Khả Năng Giám Sát (Observability):** Vượt qua giới hạn của giám sát truyền thống bằng cách thu thập và liên kết chặt chẽ bộ ba dữ liệu cốt lõi (Metrics, Logs, Traces), giúp chủ động phát hiện và cô lập sự cố trong vài phút.
3. **Áp Dụng Các Nguyên Tắc Vận Hành Hệ Thống Production:** Triển khai các mô hình giao tiếp bất đồng bộ, xử lý lỗi tự động và tối ưu hóa luồng dữ liệu để hệ thống đạt trạng thái sẵn sàng cao (High Availability) trước các mức tải đột biến.

---

## Nhật Ký Lộ Trình Tuần 10

| Thứ | Công việc | Ngày BĐ | Ngày KT | Nguồn tài liệu |
| :--- | :--- | :--- | :--- | :--- |
| **23/06** | Cloud Native Architecture: Tiếp cận tư duy thiết kế ứng dụng tối ưu hóa cho môi trường đám mây | 23/06/2026 | 23/06/2026 | [CNCF Cloud Native Definition](https://github.com/cncf/toc/blob/main/DEFINITION.md) |
| **24/06** | Advanced Monitoring & Logging: Cấu hình thu thập và quản lý logs tập trung quy mô lớn | 24/06/2026 | 24/06/2026 | [Amazon CloudWatch User Guide](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/WhatIsCloudWatch.html) |
| **25/06** | Distributed Tracing: Theo dõi chi tiết đường đi của từng request xuyên suốt các microservices | 25/06/2026 | 25/06/2026 | [AWS X-Ray Developer Guide](https://docs.aws.amazon.com/xray/latest/devguide/aws-xray.html) |
| **26/06** | Event-Driven Architecture: Tìm hiểu kiến trúc hướng sự kiện giúp giảm sự phụ thuộc giữa các dịch vụ | 26/06/2026 | 26/06/2026 | [AWS Event-Driven Architecture](https://aws.amazon.com/event-driven-architecture/) |
| **27/06** | Message Queue & Notification: Triển khai giao tiếp bất đồng bộ an toàn qua hàng đợi và kênh thông báo | 27/06/2026 | 27/06/2026 | [Amazon SQS & SNS Developer Guide](https://docs.aws.amazon.com/AWSSimpleQueueService/latest/SQSDeveloperGuide/welcome.html) |
| **28/06** | Cloud Native Mini Project: Hiện thực hóa hệ thống microservices tự động co giãn và chịu lỗi | 28/06/2026 | 28/06/2026 | Tự triển khai / AWS Architecture Center |
| **29/06** | Documentation & Architecture Review: Kiểm tra sơ đồ kiến trúc hệ thống và hoàn thiện bài viết blog | 29/06/2026 | 29/06/2026 | Hugo Portfolio Blog |

### Minh chứng thực tế:

## 1. Triển khai Hệ thống Microservices Đặt Vé Phim Hướng Sự Kiện (Event-Driven Architecture) Trên Nền Tảng AWS ECS Fargate Tích Hợp Hệ Thống Giám Sát Tập Trung (Distributed Tracing & Observability)

**Mô hình kiến trúc hệ thống**
![](/images/mohinh.png)

---

## 2. Mục tiêu của bài Lab
* **Containerization:** Đóng gói thành công các dịch vụ Microservices (`order-service` và `notification-service`) độc lập bằng Docker.
* **Orchestration & Serverless Compute:** Triển khai và quản lý các container chạy ngầm ổn định trên Cloud sử dụng **AWS ECS Fargate** mà không cần quản lý máy chủ EC2 vật lý.
* **Asynchronous Communication:** Xây dựng luồng xử lý bất đồng bộ thông qua hàng đợi **Amazon MQ (RabbitMQ)** để giảm tải cho dịch vụ API chính, tăng tính chịu tải hệ thống.
* **Distributed Tracing (Observability):** Hiện thực hóa khả năng giám sát phân tán bằng cách truyền mã lưu vết tập trung (`Trace ID`) xuyên suốt từ Client -> API -> Queue -> Worker và kiểm soát luồng đổ log về **Amazon CloudWatch**.
* **Hạ tầng mạng & Troubleshooting:** Kiểm tra và phân tích hành vi định tuyến mạng, xử lý các xung đột bảo mật liên VPC khi tích hợp dịch vụ Public Cloud Access và mạng Lab độc lập.

---

## 3. Các thành phần và Mục đích sử dụng

| Thành phần | Mục đích sử dụng trong hệ thống |
| :--- | :--- |
| **Order Service (Flask API)** | Tiếp nhận yêu cầu đặt vé phim từ phía Client (Postman), sinh mã `Trace ID` và đẩy thông tin đơn hàng vào hàng đợi. |
| **Amazon MQ (RabbitMQ)** | Làm Broker hàng đợi trung gian (Message Queue), lưu trữ tạm thời các sự kiện đặt vé một cách an toàn, phân tách độc lập (Loose Coupling) giữa API và Worker xử lý phía sau. |
| **Notification Service (Worker)** | Chạy ngầm liên tục, chủ động "hút" dữ liệu đơn hàng từ Amazon MQ để xử lý logic (giả lập gửi email xác nhận) bất đồng bộ. |
| **AWS ECR (Elastic Container Registry)** | Kho lưu trữ bảo mật (Private Repository) để quản lý các gói Docker Image (`latest`) được build từ máy local. |
| **AWS ECS Fargate** | Hệ thống điều phối container tự động Serverless, chịu trách nhiệm duy trì trạng thái hoạt động (`ACTIVE`) của các Task dịch vụ. |
| **Amazon CloudWatch Logs** | Thu thập và tập trung hóa toàn bộ dữ liệu log của hệ thống phân tán, hỗ trợ truy vết lỗi và tracking hành trình request qua `Trace ID`. |

---

## 4. Hướng dẫn các bước triển khai chi tiết

### Bước 4.1: Đóng gói và đẩy Docker Image lên AWS ECR
1. Khởi tạo cấu trúc mã nguồn local, viết `Dockerfile` cho từng dịch vụ độc lập.
2. Khởi động Docker Desktop local và thực hiện xác thực với AWS CLI qua lệnh:

        aws ecr get-login-password --region ap-southeast-1 | docker login --username AWS --password-stdin <id_tai_khoan_aws>.dkr.ecr.ap-southeast-1.amazonaws.com

Thực hiện Build, Tag và Push các Image lên Repository tương ứng trên AWS ECR:

        # Đối với Order Service
        cd order-service && docker build -t order-service .
        docker tag order-service:latest <id_tai_khoan_aws>[.dkr.ecr.ap-southeast-1.amazonaws.com/order-service:latest](https://.dkr.ecr.ap-southeast-1.amazonaws.com/order-service:latest)
        docker push <id_tai_khoan_aws>[.dkr.ecr.ap-southeast-1.amazonaws.com/order-service:latest](https://.dkr.ecr.ap-southeast-1.amazonaws.com/order-service:latest)

        # Đối với Notification Service
        cd ../notification-service && docker build -t notification-service .
        docker tag notification-service:latest <id_tai_khoan_aws>[.dkr.ecr.ap-southeast-1.amazonaws.com/notification-service:latest](https://.dkr.ecr.ap-southeast-1.amazonaws.com/notification-service:latest)
        docker push <id_tai_khoan_aws>[.dkr.ecr.ap-southeast-1.amazonaws.com/notification-service:latest](https://.dkr.ecr.ap-southeast-1.amazonaws.com/notification-service:latest)

### Bước 4.2: Cấu hình Task Definitions và khởi chạy Cluster trên ECS Fargate
        Tạo cụm ECS Cluster mang tên cinema-cluster trên nền tảng cơ sở hạ tầng Serverless AWS Fargate.

        Thiết lập 2 bản thiết kế Task Definitions (order-task mở container port 5000 và notification-task không mở port). Khai báo chính xác đường dẫn Image URI từ ECR để hệ thống có quyền kéo bản đóng gói về Cloud.

        Tạo và kích hoạt 2 dịch vụ chạy ngầm (Services): order-api-service (gắn vào Public Subnets kèm Public IP để nhận request bên ngoài) và notification-worker-service.

![](/images/hinh1.png)

![](/images/hinh2.png)
## Kết quả đạt được tuần 10

Xung đột phân tách vùng mạng (VPC Isolation): Khi cấu hình Amazon MQ Broker ở chế độ Public access không chỉ định VPC tùy biến, AWS mặc định đưa tài nguyên này vào bảo mật của VPC mặc định (Default VPC) của tài khoản. Trong khi đó, các dịch vụ ECS Container được triển khai biệt lập bên trong VPC Lab riêng biệt (cloud-native-lab).

Chặn cổng tường lửa (Security Group Block): Hai vùng VPC này hoạt động hoàn toàn độc lập và không có đường định tuyến trực tiếp nối thông nội bộ. Thêm vào đó, nhóm bảo mật mặc định (default Security Group) của VPC mặc định đang chặn toàn bộ lưu lượng dòng vào (Inbound traffic) qua cổng 5671 (cổng mã hóa AMQPS bảo mật của RabbitMQ), khiến gói tin từ ECS gửi qua Internet công cộng bị Timeout hoàn toàn.

