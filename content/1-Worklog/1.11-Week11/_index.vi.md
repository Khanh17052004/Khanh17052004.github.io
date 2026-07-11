---
title: "Nhật Ký Tuần 11: Hoàn Thiện Dự Án Tốt Nghiệp & Đánh Giá Kiến Trúc Theo Chuẩn AWS Well-Architected"
date: 2024-01-01
weight: 2
chapter: false
pre: " <b> 1.11. </b> "
---
## Mục tiêu tuần 11:
Tuần này tập trung vào việc tổng hợp toàn bộ tri thức đã tích lũy để hoàn thiện dự án cuối khóa (Capstone Project), đồng thời áp dụng khung chuẩn quy dịch của AWS để rà soát, tối ưu hóa và chuẩn bị hồ sơ kỹ thuật chuyên nghiệp phục vụ việc bảo vệ dự án:

1. **Hoàn Thiện Capstone Project:** Hiện thực hóa một hệ thống hạ tầng đám mây hoàn chỉnh từ lớp mạng, tính toán, container hóa cho đến lưu trữ, đảm bảo mọi thành phần kết nối đồng bộ và tự động thông qua mã nguồn IaC.
2. **Đánh Giá Hệ Thống Theo AWS Well-Architected Framework:** Thực hiện rà soát kiến trúc dựa trên 6 trụ cột cốt lõi của AWS (Bảo mật, Hiệu năng, Độ tin cậy, Tối ưu chi phí, Vận hành xuất sắc và Bền vững) để chủ động phát hiện rủi ro.
3. **Chuẩn Bị Báo Cáo Và Trình Bày:** Thực thi các kịch bản kiểm thử tải nghiêm ngặt, hoàn thiện sơ đồ kiến trúc chuẩn hóa (Architecture Diagram) và đóng gói tài liệu kỹ thuật chuyên nghiệp trên Blog Technical.

---

## Nhật Ký Lộ Trình Tuần 11

| Thứ | Công việc | Ngày BĐ | Ngày KT | Nguồn tài liệu |
| :--- | :--- | :--- | :--- | :--- |
| **30/06** | Capstone Project Design: Lập kế hoạch chi tiết, phân rã kiến trúc và phân công tài nguyên triển khai | 30/06/2026 | 30/06/2026 | [AWS Architecture Center](https://aws.amazon.com/architecture/) |
| **01/07** | Automated Infrastructure Provisioning: Viết mã nguồn Terraform để tự động hóa toàn bộ hạ tầng cốt lõi | 01/07/2026 | 01/07/2026 | [Terraform Best Practices](https://developer.hashicorp.com/terraform/cloud-docs/recommended-practices) |
| **02/07** | Application Deployment & Pipeline: Cấu hình CI/CD đẩy mã nguồn ứng dụng lên môi trường vận hành đám mây | 02/07/2026 | 02/07/2026 | [AWS Whitepapers & Guides](https://aws.amazon.com/whitepapers/) |
| **03/07** | Performance Testing & Stress Test: Sử dụng công cụ đo tải để kiểm tra ngưỡng chịu lỗi và tự động co giãn | 03/07/2026 | 03/07/2026 | [AWS Fault Injection Service](https://docs.aws.amazon.com/fis/latest/userguide/what-is-fis.html) |
| **04/07** | AWS Well-Architected Review: Rà soát toàn bộ hệ thống bằng công cụ Tool chuyên dụng của AWS | 04/07/2026 | 04/07/2026 | [AWS Well-Architected Tool Docs](https://docs.aws.amazon.com/wellarchitected/latest/userguide/intro.html) |
| **05/07** | Cost & Security Optimization: Tinh chỉnh kích thước tài nguyên và thắt chặt các chính sách an ninh sau đánh giá | 05/07/2026 | 05/07/2026 | [AWS Cost Optimization Guide](https://aws.amazon.com/aws-cost-management/cost-optimization/) |
| **06/07** | Project Demo & Final Documentation: Hoàn thiện video demo, sơ đồ kiến trúc và xuất bản bài báo cáo tổng kết | 06/07/2026 | 06/07/2026 | Hugo Portfolio Blog |

### Minh chứng thực tế:

## Triển khai Hạ tầng Khả năng Sẵn sàng cao (High Availability) và Tự động Co giãn (Auto Scaling) trên AWS bằng Terraform


**Mô hình kiến trúc hệ thống**

![](/images/mohinh.png)

---

## 1. Mục tiêu của bài Lab

* **Hạ tầng dưới dạng mã (IaC):** Khởi tạo và quản lý toàn bộ tài nguyên hệ thống Cloud thông qua công cụ Terraform tự động, nhất quán.
* **Tính sẵn sàng cao (High Availability):** Thiết kế hệ thống mạng đa vùng (Multi-AZ) phân tán qua vùng `ap-southeast-1a` và `ap-southeast-1b` tại Singapore.
* **Cân bằng tải & Tự động phục hồi (Load Balancing & Self-Healing):** Sử dụng Application Load Balancer (ALB) để điều phối traffic và thiết lập Auto Scaling Group duy trì tính ổn định, tự động thay thế máy chủ lỗi.
* **Tối ưu hóa chi phí và hiệu năng (Auto Scaling Policy):** Cấu hình chính sách Target Tracking Scaling dựa trên hiệu suất CPU trung bình để hệ thống tự động co giãn linh hoạt theo lưu lượng thực tế.

---

## 2. Các thành phần và Mục đích sử dụng

| Thành phần tài nguyên | Loại dịch vụ AWS | Mục đích sử dụng cụ thể trong hệ thống |
| :--- | :--- | :--- |
| **VPC & Internet Gateway** | `aws_vpc` / `aws_internet_gateway` | Khởi tạo môi trường mạng cô lập định danh lớp mạng `10.0.0.0/16`, cấp quyền định tuyến kết nối Internet cho hạ tầng công khai. |
| **Public Subnets** | `aws_subnet` (1 & 2) | Chia hai phân đoạn mạng độc lập tại hai Availability Zone phân tách địa lý (`1a` & `1b`) để đặt ALB và EC2 Instances gộp chung tăng tốc độ định tuyến công khai. |
| **Security Groups** | `aws_security_group` | Thiết lập tường lửa 2 lớp: Lớp 1 (`alb_sg`) mở cổng 80 công khai cho người dùng đầu cuối. Lớp 2 (`ec2_sg`) mở cổng 80 bảo mật (chỉ nhận dữ liệu chuyển tiếp từ ALB) và cổng 22 (SSH) phục vụ giám sát kỹ thuật. |
| **Application Load Balancer** | `aws_lb` / `aws_lb_target_group` | Tiếp nhận và phân phối lưu lượng HTTP (Port 80) của người dùng, thực hiện kiểm tra sức khỏe vật lý (`health_check`) định kỳ để duy trì kết nối sạch. |
| **Launch Template** | `aws_launch_template` | Định nghĩa khuôn mẫu máy chủ: Sử dụng OS Amazon Linux 2023 (`ami-086f3cc9291780242`), cấu hình tài nguyên `t2.micro`, tích hợp script User Data tự động khởi chạy Web Server Apache (`httpd`) và công cụ stress-test khi khởi tạo. |
| **Auto Scaling Group** | `aws_autoscaling_group` | Ràng buộc số lượng vận hành máy chủ (`min=2`, `max=4`, `desired=2`). Theo dõi sát sao trạng thái thực tế để thực thi tăng/giảm số lượng node máy chủ tự động. |
| **Target Tracking Scaling Policy** | `aws_autoscaling_policy` | Cấu hình ngưỡng kích hoạt báo động. Nếu mức sử dụng CPU trung bình (`ASGAverageCPUUtilization`) vượt quá **50.0%**, hệ thống ngay lập tức kích hoạt lệnh Scale-out bổ sung máy chủ. |

---

## 3. Hướng dẫn các bước triển khai chi tiết

### BƯỚC 1: Chuẩn bị tệp cấu hình mã nguồn sạch (main.tf)

Bạn mở ứng dụng CMD trên máy tính lên. Di chuyển vào thư mục bài lab trên Desktop:

        cd C:\Users\matha\Desktop\aws-capstone-lab

Mở tệp cấu hình bằng Notepad để kiểm tra hoặc cập nhật:

        notepad main.tf

Nhấn Ctrl + A để xóa sạch nội dung cũ bên trong và dán toàn bộ đoạn mã chuẩn hóa dưới đây vào:

        terraform {
        required_providers {
            aws = {
            source  = "hashicorp/aws"
            version = "~> 5.0"
            }
        }
        }

        provider "aws" {
        region = "ap-southeast-1" # Khu vực Singapore
        }

        # 1. Khởi tạo mạng VPC & Internet Gateway công khai
        resource "aws_vpc" "capstone_vpc" {
        cidr_block           = "10.0.0.0/16"
        enable_dns_hostnames = true
        tags                 = { Name = "capstone-vpc" }
        }

        resource "aws_internet_gateway" "igw" {
        vpc_id = aws_vpc.capstone_vpc.id
        tags   = { Name = "capstone-igw" }
        }

        # 2. Hai Subnets phục vụ High Availability (Vùng 1a và 1b)
        resource "aws_subnet" "public_1" {
        vpc_id                  = aws_vpc.capstone_vpc.id
        cidr_block              = "10.0.1.0/24"
        availability_zone       = "ap-southeast-1a"
        map_public_ip_on_launch = true
        tags                    = { Name = "public-subnet-1" }
        }

        resource "aws_subnet" "public_2" {
        vpc_id                  = aws_vpc.capstone_vpc.id
        cidr_block              = "10.0.2.0/24"
        availability_zone       = "ap-southeast-1b"
        map_public_ip_on_launch = true
        tags                    = { Name = "public-subnet-2" }
        }

        # 3. Cấu hình bảng định tuyến cho phép đi Internet công khai
        resource "aws_route_table" "public_rt" {
        vpc_id = aws_vpc.capstone_vpc.id
        route {
            cidr_block = "0.0.0.0/0"
            gateway_id = aws_internet_gateway.igw.id
        }
        tags = { Name = "public-route-table" }
        }

        resource "aws_route_table_association" "pub1" {
        subnet_id      = aws_subnet.public_1.id
        route_table_id = aws_route_table.public_rt.id
        }

        resource "aws_route_table_association" "pub2" {
        subnet_id      = aws_subnet.public_2.id
        route_table_id = aws_route_table.public_rt.id
        }

        # 4. Security Group mở cổng 80 công khai cho Application Load Balancer (ALB)
        resource "aws_security_group" "alb_sg" {
        name        = "alb-security-group-v2"
        vpc_id      = aws_vpc.capstone_vpc.id
        ingress {
            from_port   = 80
            to_port     = 80
            protocol    = "tcp"
            cidr_blocks = ["0.0.0.0/0"]
        }
        egress {
            from_port   = 0
            to_port     = 0
            protocol    = "-1"
            cidr_blocks = ["0.0.0.0/0"]
        }
        }

        # 5. Security Group bảo mật cho EC2 (Nhận cổng 80 từ ALB và mở cổng 22 cho SSH)
        resource "aws_security_group" "ec2_sg" {
        name        = "ec2-security-group-v2"
        vpc_id      = aws_vpc.capstone_vpc.id

        ingress {
            from_port       = 80
            to_port         = 80
            protocol        = "tcp"
            security_groups = [aws_security_group.alb_sg.id]
        }

        ingress {
            from_port   = 22
            to_port     = 22
            protocol    = "tcp"
            cidr_blocks = ["0.0.0.0/0"] # Cấp quyền cho EC2 Instance Connect truy cập
        }

        egress {
            from_port   = 0
            to_port     = 0
            protocol    = "-1"
            cidr_blocks = ["0.0.0.0/0"]
        }
        }

        # 6. Thiết lập bộ Cân bằng tải Application Load Balancer
        resource "aws_lb" "alb" {
        name               = "capstone-alb"
        internal           = false
        load_balancer_type = "application"
        security_groups    = [aws_security_group.alb_sg.id]
        subnets            = [aws_subnet.public_1.id, aws_subnet.public_2.id]
        }

        resource "aws_lb_target_group" "tg" {
        name     = "capstone-tg"
        port     = 80
        protocol = "HTTP"
        vpc_id   = aws_vpc.capstone_vpc.id
        health_check {
            path                = "/"
            port                = "80"
            healthy_threshold   = 2
            unhealthy_threshold = 2
            timeout             = 3
            interval            = 10
        }
        }

        resource "aws_lb_listener" "http" {
        load_balancer_arn = aws_lb.alb.arn
        port              = "80"
        protocol          = "HTTP"
        default_action {
            type             = "forward"
            target_group_arn = aws_lb_target_group.tg.arn
        }
        }

        # 7. Launch Template định nghĩa máy chủ Amazon Linux 2023 & tự cài Web Server
        resource "aws_launch_template" "app_lt" {
        name_prefix   = "capstone-app-"
        image_id      = "ami-086f3cc9291780242" # Đúng chuẩn mã AMI gốc của tài khoản
        instance_type = "t2.micro"

        network_interfaces {
            associate_public_ip_address = true 
            security_groups             = [aws_security_group.ec2_sg.id]
        }

        user_data = base64encode(<<-EOF
                    #!/bin/bash
                    dnf update -y
                    dnf install -y httpd stress
                    systemctl start httpd
                    systemctl enable httpd
                    TOKEN=$(curl -X PUT "[http://169.254.169.254/latest/api/token](http://169.254.169.254/latest/api/token)" -H "X-aws-ec2-metadata-token-ttl-seconds: 21600")
                    EC2_AVAIL_ZONE=$(curl -H "X-aws-ec2-metadata-token: $TOKEN" -s [http://169.254.169.254/latest/meta-data/placement/availability-zone](http://169.254.169.254/latest/meta-data/placement/availability-zone))
                    echo "<h1>Hello World tu AZ $EC2_AVAIL_ZONE</h1>" > /var/www/html/index.html
                    EOF
        )
        }

        # 8. Cấu hình Auto Scaling Group quản lý co giãn
        resource "aws_autoscaling_group" "asg" {
        vpc_zone_identifier = [aws_subnet.public_1.id, aws_subnet.public_2.id]
        target_group_arns   = [aws_lb_target_group.tg.arn]
        desired_capacity    = 2
        max_size            = 4
        min_size            = 2

        launch_template {
            id      = aws_launch_template.app_lt.id
            version = "$Latest"
        }

        tag {
            key                 = "Name"
            value               = "capstone-app-instance"
            propagate_at_launch = true
        }
        }

        # 9. Chính sách Auto Scaling điều chỉnh tự động theo tải CPU trung bình
        resource "aws_autoscaling_policy" "scale_out" {
        name                   = "cpu-scale-out"
        autoscaling_group_name = aws_autoscaling_group.asg.name
        policy_type            = "TargetTrackingScaling"
        target_tracking_configuration {
            predefined_metric_specification {
            predefined_metric_type = "ASGAverageCPUUtilization"
            }
            target_value = 50.0
        }
        }

        output "alb_dns_name" {
        value       = aws_lb.alb.dns_name
        description = "URL kiem tra"
        }

Nhấn Ctrl + S để lưu lại và tắt Notepad.

### BƯỚC 2: Thực thi lệnh Terraform khởi chạy bài lab
Quay lại màn hình dòng lệnh CMD đang đứng tại đúng thư mục làm việc, chạy tuần tự các lệnh sau:

Khởi tạo lại môi trường:

        terraform init

Kiểm tra sơ đồ triển khai tài nguyên:

        terraform plan

Triển khai tự động hóa hạ tầng lên AWS:

        terraform apply -auto-approve
Lưu ý: Bạn hãy đợi khoảng 2 đến 3 phút để hệ thống tạo mới toàn bộ tài nguyên sạch. Khi màn hình xuất hiện dòng chữ màu xanh Apply complete! là thành công.

### BƯỚC 3: Kiểm tra hoạt động Cân bằng tải (Load Balancing)
1. Nhìn xuống cuối màn hình CMD, tìm dòng alb_dns_name = "..." và copy chuỗi đường link nằm trong dấu ngoặc kép.

2. Mở trình duyệt Web (hoặc một tab ẩn danh), dán link đó vào thanh địa chỉ để truy cập.

Kết quả đạt được: Trang web sẽ hiển thị thông báo thành công: Hello World tu AZ ap-southeast-1a (hoặc 1b). Bạn nhấn phím F5 (Tải lại) vài lần để thấy lưu lượng tự động chia đều sang cả hai khu vực.

![](/images/lab-alb-dns-success-1.png)

![](/images/lab-alb-dns-success-2.png)

### BƯỚC 4: Thực hiện Kiểm thử tải (Stress Test) chứng minh Auto Scaling
Đăng nhập vào AWS Web Console -> Chọn dịch vụ EC2 -> Vào danh sách Instances (Running).

Chọn 1 trong 2 con máy ảo mang tên capstone-app-instance -> Nhấn nút Connect ở góc trên.

Tại giao diện EC2 Instance Connect, nhấn tiếp nút Connect màu cam để truy cập Terminal Linux trực tiếp (Lúc này cổng 22 đã mở nên kết nối sẽ vào được ngay lập tức).

Để tạo áp lực tải, bạn copy và dán lệnh ép CPU chạy 100% công suất:

        stress --cpu 4 --timeout 400

![](/images/lab-ssh-connect.png)

Chứng minh kết quả: Giữ nguyên tab chạy lệnh đó. Bạn mở một tab AWS mới, truy cập vào mục EC2 > Instances. Chờ từ 2 đến 3 phút cho hệ thống CloudWatch đồng bộ dữ liệu tải, bạn sẽ thấy Auto Scaling Group tự động kích hoạt đẻ thêm từ 1 đến 2 máy ảo mới (Pending / Initializing) để san sẻ tải cho hệ thống đúng như thiết kế kiến trúc lý thuyết!

![](/images/lab-auto-scaling-instances.png)

## Kết quả đạt được tuần 11
4.1. Kết quả học được (Lessons Learned)
Kỹ năng làm chủ IaC (Infrastructure as Code): Hiểu rõ cách thức quản trị và phân rã toàn bộ một hệ thống hạ tầng mạng, máy chủ vật lý, tường lửa, và cân bằng tải phức tạp từ các nút bấm thủ công trên Web Console chuyển sang dạng mã nguồn cấu trúc tập trung (main.tf).

Tư duy thiết kế Khả năng sẵn sàng cao (High Availability): Nắm vững nguyên lý hoạt động của kiến trúc Multi-AZ. Hiểu cách Application Load Balancer tự động điều phối, phân tách các phiên truy cập của người dùng sang các vùng địa lý khác nhau (ap-southeast-1a và ap-southeast-1b) để triệt tiêu điểm lỗi đơn độc (Single Point of Failure).

Cơ chế vận hành của Hệ thống tự động co giãn (Auto Scaling): Trực tiếp cấu hình và hiểu sâu về mối liên kết giữa CloudWatch Alarms, Scaling Policy, và Auto Scaling Group. Biết cách thiết lập kịch bản giám sát chỉ số CPU trung bình để hệ thống tự động đưa ra quyết định tăng quy mô (Scale-out) khi quá tải và tự động thu hồi (Scale-in) khi hạ tải để tối ưu chi phí.

Cơ chế tự phục hồi (Self-Healing): Chứng minh được khả năng tự động phát hiện máy chủ lỗi và bù đắp tài nguyên của ASG, giúp ứng dụng duy trì tính liên tục 24/7 mà không cần sự can thiệp thủ công của kỹ sư hệ thống.

4.2. Kinh nghiệm thực tế rút ra (Kinh nghiệm xương máu khi fix lỗi)
Quản lý chặt chẽ Trạng thái Terraform (State Management): Rút ra kinh nghiệm quan trọng rằng khi thay đổi lớn về cấu trúc file mạng (như xóa bớt subnet, đổi tên tài nguyên), việc chạy thẳng lệnh terraform destroy sẽ rất dễ gây ra lỗi kẹt hạ tầng mạng (DependencyViolation). Luôn phải dọn dẹp các tài nguyên phụ thuộc (như RDS Database, ENI, ALB) trước khi hủy phần khung VPC.

Sự phụ thuộc vào Hệ điều hành và AMI ID: ID của hệ điều hành (AMI ID) phụ thuộc chặt chẽ vào từng vùng (Region) và tài khoản AWS cụ thể. Việc cấu hình sai AMI hoặc sử dụng câu lệnh cài đặt cũ (yum) trên hệ điều hành mới (Amazon Linux 2023 yêu cầu dnf) sẽ khiến đoạn Script khởi tạo (User Data) thất bại ngầm, dẫn đến lỗi 502 Bad Gateway.

Phối hợp nhất quán giữa Security Group và Dịch vụ Quản trị: Khi triển khai các máy chủ nằm trong mạng cô lập, nếu chỉ chú trọng mở cổng dịch vụ ứng dụng (Port 80) mà quên mở cổng quản trị hệ thống (Port 22), các công cụ như EC2 Instance Connect sẽ bị chặn hoàn toàn (Connection Timed Out), gây khó khăn cho việc giám sát và kiểm thử tải. Tường lửa luôn phải được thiết kế tối thiểu 2 lớp rõ ràng.

Độ trễ tính toán tải (Cooldown & Evaluation Periods): Hệ thống giám sát AWS CloudWatch cần một khoảng thời gian chu kỳ (thường từ 1-3 phút) để tích lũy dữ liệu và tính toán mức CPU trung bình của toàn bộ nhóm máy chủ chứ không tính riêng lẻ từng máy. Do đó, khi làm stress test, hệ thống sẽ có độ trễ nhất định trước khi máy ảo mới xuất hiện, đòi hỏi kỹ sư phải hiểu rõ để cấu hình thông số cooldown hợp lý trong thực tế.

