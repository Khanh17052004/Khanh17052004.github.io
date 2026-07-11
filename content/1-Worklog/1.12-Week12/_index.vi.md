---
title: "Nhật Ký Tuần 12: Tối Ưu Hóa Dự Án Cuối Khóa & Chuẩn Bị Hành Trình Sự Nghiệp Cloud Engineer"
date: 2024-01-01
weight: 2
chapter: false
pre: " <b> 1.12 </b> "
---
## Mục tiêu tuần 12:
Tuần cuối cùng tập trung vào việc tinh chỉnh chuyên sâu, kiểm thử toàn diện toàn bộ hệ thống dự án (Capstone Project) và chuẩn bị các nền tảng kỹ thuật, hồ sơ chuyên nghiệp để sẵn sàng bước vào thị trường lao động với vai trò Kỹ sư Điện toán đám mây:

1. **Đánh Giá & Tối Ưu Toàn Diện Kiến Trúc (Architecture & Cost Optimization):** Áp dụng triệt để khung chuẩn AWS Well-Architected Framework để rà soát toàn bộ hệ thống, loại bỏ tài nguyên dư thừa nhằm tối ưu hóa chi phí và nâng cao hiệu năng vận hành.
2. **Kiểm Trực An Ninh & Sẵn Sàng Vận Hành (Security & DR Validation):** Giả lập các tình huống sự cố để kiểm thử khả năng phục hồi sau thảm họa (Disaster Recovery), cấu hình sao lưu tự động và hoàn thiện danh mục kiểm tra sẵn sàng chạy thực tế (Production Readiness Checklist).
3. **Hoàn Thiện Tài Liệu Kỹ Thuật & Định Hướng Sự Nghiệp (Career & Portfolio Preparation):** Đóng gói toàn bộ mã nguồn, sơ đồ kiến trúc, viết tài liệu vận hành (Runbook) chi tiết lên Portfolio Blog cá nhân và chuẩn bị tâm thế cho các vòng phỏng vấn tuyển dụng.

---

## Nhật Ký Lộ Trình Tuần 12

| Thứ | Công việc | Ngày BĐ | Ngày KT | Nguồn tài liệu |
| :--- | :--- | :--- | :--- | :--- |
| **07/07** | End-to-End Architecture Review: Đánh giá tổng thể mối liên kết giữa các dịch vụ trong hệ thống | 07/07/2026 | 07/07/2026 | [AWS Well-Architected Framework](https://aws.amazon.com/architecture/well-architected/) |
| **08/07** | Performance & Cost Optimization: Rà soát kích thước tài nguyên (Right-sizing) để tối ưu chi phí | 08/07/2026 | 08/07/2026 | [AWS Cost Optimization Pillars](https://docs.aws.amazon.com/wellarchitected/latest/cost-optimization-pillar/welcome.html) |
| **09/07** | Security & DR Validation: Kiểm tra tường lửa, phân quyền và diễn tập kịch bản khôi phục hệ thống | 09/07/2026 | 09/07/2026 | [AWS Reliability Pillar: Disaster Recovery](https://docs.aws.amazon.com/wellarchitected/latest/reliability-pillar/disaster-recovery-dr.html) |
| **10/07** | Production Readiness Checklist: Rà soát các tiêu chuẩn an toàn trước khi bàn giao hệ thống | 10/07/2026 | 10/07/2026 | [AWS Operational Excellence Pillar](https://docs.aws.amazon.com/wellarchitected/latest/operational-excellence-pillar/welcome.html) |
| **11/07** | Technical Documentation & Runbook: Soạn thảo hướng dẫn vận hành và xử lý sự cố chuẩn kỹ sư | 11/07/2026 | 11/07/2026 | [AWS Systems Manager Runbooks](https://docs.aws.amazon.com/systems-manager/latest/userguide/systems-manager-automation.html) |
| **12/07** | Final Demo & Presentation: Hoàn thiện kịch bản chạy thử nghiệm hạ tầng và trực quan hóa sơ đồ | 12/07/2026 | 12/07/2026 | [AWS Architecture Icons & Diagrams](https://aws.amazon.com/architecture/icons/) |
| **13/07** | Portfolio & Career Preparation: Đồng bộ mã nguồn lên GitHub, làm sạch giao diện blog để kết nối tuyển dụng | 13/07/2026 | 13/07/2026 | Hugo Portfolio Blog |


### Minh chứng thực tế:

# 1. Production Hardening & Capstone Delivery

**Mô hình kiến trúc hệ thống**

![](/images/mohinh.png)


---

## 2. Mục Tiêu Của Bài Lab
*   **High Availability & Fault Tolerance:** Thiết kế hệ thống tự động co giãn và chịu lỗi đa vùng (Multi-AZ).
*   **Production Security Hardening:** Áp dụng nguyên tắc đặc quyền tối thiểu (Least Privilege), cô lập hoàn toàn Web Server vào Private Subnet, chỉ mở cổng thông qua Application Load Balancer.
*   **System Observability:** Cấu hình giám sát hiệu năng CPU thông qua CloudWatch Alarms và thiết lập hệ thống cảnh báo tự động qua Amazon SNS Email.
*   **Chaos Testing:** Thực hiện giả lập sự cố sập server (Terminate máy ảo) để kiểm thử thực tế khả năng tự chữa lành (Self-healing) của hạ tầng mà không gây gián đoạn dịch vụ (Zero Downtime).

---

## 3. Các Thành Phần Và Mục Đích Sử Dụng

| Thành phần AWS | Mục đích sử dụng trong hệ thống |
| :--- | :--- |
| **Amazon VPC** | Tạo môi trường mạng ảo cô lập độc lập (`10.0.0.0/16`) chia làm 2 vùng Public và 2 vùng Private Subnets. |
| **NAT Gateway** | Đặt tại Public Subnet nhằm cho phép các máy ảo trong Private Subnet đi ra ngoài Internet tải Nginx và mã nguồn mà không cho chiều ngược lại truy cập vào. |
| **Launch Template** | Bản thiết kế khuôn mẫu chứa cấu hình máy ảo (`t3.micro`), hệ điều hành (Amazon Linux 2023), Security Group và script cài đặt giao diện Portfolio động qua IMDSv2. |
| **Application Load Balancer (ALB)** | Cửa ngõ hứng traffic công khai (Internet-facing), định tuyến thông minh Port 80 và cân bằng tải dữ liệu đến cụm máy ảo phía sau. |
| **Auto Scaling Group (ASG)** | Quản lý vòng đời cụm EC2 (Min: 2, Desired: 2, Max: 4), tự động nở thêm máy ảo khi CPU > 70% hoặc tự đúc lại máy mới khi phát hiện máy cũ bị lỗi. |
| **CloudWatch + SNS** | Theo dõi sát sao chỉ số CPUUtilization và tự động bắn Email cảnh báo mỗi khi hệ thống vượt ngưỡng an toàn. |

---

## 4. Hướng Dẫn Các Bước Triển Khai Chi Tiết

### Bước 1: Khởi tạo mạng lưới nền tảng (VPC Networking)
Khởi tạo một VPC mạng mang tên `capstone-production-vpc` với dải mạng `10.0.0.0/16`. Cấu hình tự động tách biệt 2 vùng Availability Zones (`ap-southeast-1a` và `ap-southeast-1b`), tạo lập 2 Public Subnet hứng traffic và 2 Private Subnet cô lập an toàn cho Web Server. Đặt 1 NAT Gateway tại vùng Public để hỗ trợ tải package bên trong mạng nội bộ.

### Bước 2: Thiết lập Application Load Balancer (ALB)
Tạo một Target Group `tg-capstone-production` định tuyến dạng Instance ở Port 80. Tiến hành khởi tạo Application Load Balancer tên là `alb-capstone-prod` dạng Internet-facing. Cấu hình Network Mapping trỏ chính xác vào các **Public Subnet** ở cả hai AZ để đảm bảo đường truyền bên ngoài đi vào được cửa ngõ Load Balancer.

### Bước 3: Tạo Launch Template chứa mã nguồn Portfolio cao cấp
Biến đổi hoặc tạo mới một bản thiết kế Launch Template mang tên `capstone-web-template`, sử dụng cấu hình chip tối ưu `t3.micro`. Tại phần Advanced Details, tích hợp đoạn Script tự động tải Nginx và cấu hình giao diện Portfolio Glassmorphic sử dụng phương thức gọi dữ liệu bảo mật **IMDSv2 Token** để lấy thông tin máy ảo động:

        #!/bin/bash
        dnf update -y
        dnf install -y nginx
        systemctl start nginx
        systemctl enable nginx

        # Lấy Metadata bảo mật qua IMDSv2 Token
        TOKEN=$(curl -s -X PUT "[http://169.254.169.254/latest/api/token](http://169.254.169.254/latest/api/token)" -H "X-aws-ec2-metadata-token-ttl-seconds: 21600")
        INSTANCE_ID=$(curl -s -H "X-aws-ec2-metadata-token: $TOKEN" [http://169.254.169.254/latest/meta-data/instance-id](http://169.254.169.254/latest/meta-data/instance-id))
        AZ=$(curl -s -H "X-aws-ec2-metadata-token: $TOKEN" [http://169.254.169.254/latest/meta-data/placement/availability-zone](http://169.254.169.254/latest/meta-data/placement/availability-zone))

        # Ghi đè file html Portfolio giao diện Glassmorphism
        cat <<EOF> /usr/share/nginx/html/index.html
        ... (Đoạn mã code giao diện HTML/CSS) ...
        EOF
        systemctl restart nginx

### Bước 4: Triển khai cụm Auto Scaling Group (ASG)
Khởi tạo Auto Scaling Group mang tên asg-capstone-prod trỏ trực tiếp đến bản Launch Template mới nhất (Version Latest). Rải toàn bộ cụm máy ảo vào 2 Private Subnets nhằm giấu hoàn toàn các web server khỏi Internet. Cấu hình số lượng máy ảo mong muốn Desired: 2, Min: 2, và Max: 4. Kích hoạt tính năng Target tracking scaling policy dựa trên CPUUtilization ở ngưỡng 70%.

![](/images/Auto-Scaling.png)
Minh chứng: Cụm Auto Scaling Group kiểm soát ổn định trạng thái 2/2 máy ảo Healthy.

### Bước 5: Siết chặt bảo mật hạ tầng (Production Hardening)
Tiến hành rà soát an toàn thông tin (Security Auditing). Tại Security Group của Web Server (sg-web-server), ta tiến hành xóa bỏ quyền truy cập mở 0.0.0.0/0 ở cổng HTTP (Port 80). Thay vào đó, cấu hình Source trỏ thẳng định danh Security Group ID của ALB. Quy trình này đảm bảo các máy ảo trong Private Subnet từ nay chỉ nhận dữ liệu được kiểm duyệt đi qua Load Balancer, ngăn chặn tuyệt đối các cuộc tấn công trực diện vào máy chủ.


### Bước 6: Kiểm tra kết quả hiển thị dữ liệu động
Sau khi quá trình đồng bộ hạ tầng và Instance Refresh hoàn tất, truy cập vào đường dẫn DNS Name công khai của Load Balancer. Kết quả trả về giao diện Portfolio cá nhân sang trọng với hiệu ứng Glassmorphism sắc nét cùng khung avatar vuông bo góc theo đúng thiết kế tiêu chuẩn.

![](/images/web2.png)

![](/images/web1.png)

Minh chứng: Website vận hành thực tế qua link DNS Load Balancer, hiển thị chính xác mã Instance ID và phân vùng Deployed AZ từ hệ thống AWS.

### Bước 7: Chaos Testing — Thử nghiệm phá hủy và Phục hồi tự động
Tiến hành giả lập thảm họa mạng bằng cách truy cập vào EC2 Console và lựa chọn Terminate (Xóa hoàn toàn) một máy ảo đang chạy.

Kết quả cân bằng tải: Khi nhấn F5 liên tục trên trình duyệt, dịch vụ không hề bị ngắt quãng (Zero Downtime). Load Balancer ngay lập tức phát hiện máy ảo lỗi thông qua cơ chế Health Check và chuyển hướng người dùng sang máy ảo còn lại ở AZ đối lập. Dòng thông tin máy ảo tự động chuyển đổi mã định danh sang máy mới.

Kết quả tự chữa lành: Chờ 2 phút, Auto Scaling Group phát hiện số lượng máy hoạt động thấp hơn ngưỡng cấu hình yêu cầu, nó lập tức kích hoạt lệnh khởi tạo tự động một EC2 mới tinh để đưa hệ thống về lại trạng thái cân bằng.

![](/images/report.png)

![](/images/email.png)

## Kết quả đạt được tuần 12

Hiểu sâu sắc về luồng đi mạng trong AWS: Phân biệt rõ ràng vai trò của Security Group dành cho các thành phần (ALB cần mở mở cổng cho Internet 0.0.0.0/0, trong khi EC2 chỉ được phép tin tưởng traffic truyền đến từ chính ALB).

Tầm quan trọng của IMDSv2: Nhận thức rõ cơ chế bảo mật Token mới trên hệ điều hành Amazon Linux 2023 khi thực hiện các câu lệnh curl lấy Metadata hệ thống, tránh việc bị trống thông tin giao diện khi vận hành.

Tư duy thiết kế hệ thống lớn: Không chỉ dừng lại ở việc dựng hạ tầng chạy được, một Cloud Engineer thực thụ phải biết cách đặt máy chủ vào vùng an toàn (Private Subnets) và chuẩn bị các kịch bản tự động co giãn, cảnh báo tự động để tối ưu chi phí và đảm bảo hệ thống luôn luôn hoạt động ổn định 24/7 trước mọi sự cố.