---
title: "Nhật ký Tuần 6: Tự động hóa triển khai vùng mạng cô lập (VPC) và máy chủ Web (EC2) trên AWS sử dụng Terraform Modules."
date: 2024-01-01
weight: 1
chapter: false
pre: " <b> 1.6. </b> "
---
## Mục tiêu tuần 6:
Tuần này tập trung vào việc hiện thực hóa mô hình **Hạ tầng dạng mã (Infrastructure as Code - IaC)**, chuyển dịch từ việc cấu hình thủ công trên AWS Console sang tự động hóa toàn bộ quy trình khởi tạo, quản lý và triển khai hạ tầng bằng **HashiCorp Terraform**:

1. **Làm Chủ Cốt Lõi IaC (Terraform Core Concepts):** Hiểu sâu cấu trúc ngữ pháp HCL (HashiCorp Configuration Language), cách thức hoạt động của AWS Provider để thiết lập kết nối an toàn và quản lý vòng đời tài nguyên đám mây.
2. **Module Hóa Hạ Tầng (Modular Infrastructure Design):** Xây dựng tư duy thiết kế hệ thống có khả năng tái sử dụng (reusability) bằng cách đóng gói các thành phần độc lập thành các Terraform Modules chuẩn hóa.
3. **Quản Lý Trạng Thái Hạ Tầng Đám Mây (State Management):** Thực thi các nguyên tắc quản lý `terraform.tfstate` an toàn, hiểu rõ cơ chế State Locking và tầm quan trọng của việc lưu trữ trạng thái tập trung (Remote Backend).
4. **Tự Động Hóa Triển Khai Thực Tế (Automated Resource Provisioning):** Thực hành viết code triển khai thực tế các thành phần core networking (VPC, Subnets, Route Tables) và compute (EC2, Security Groups) theo chuẩn bảo mật.
5. **Tối Ưu Hóa Tài Liệu & Đóng Gói (IaC Documentation):** Tổng hợp mã nguồn, làm sạch cấu trúc thư mục dự án, trực quan hóa luồng tài nguyên được quản lý bởi IaC và đóng gói bài báo cáo chuyên nghiệp trên Blog Technical.

---

## Nhật Ký Lộ Trình Tuần 6

| Thứ | Công việc | Ngày BĐ | Ngày KT | Nguồn tài liệu |
| :--- | :--- | :--- | :--- | :--- |
| **26/05** | Introduction to IaC & Terraform: Hiểu tư duy Hạ tầng dạng mã | 26/05/2026 | 26/05/2026 | [Terraform Intro Docs](https://developer.hashicorp.com/terraform/intro) |
| **27/05** | Terraform & AWS Provider: Cấu hình và kết nối tài khoản AWS | 27/05/2026 | 27/05/2026 | [AWS Provider Registry](https://registry.terraform.io/providers/hashicorp/aws/latest/docs) |
| **28/05** | Reusable Code with Terraform Modules: Thiết kế hạ tầng dạng module | 28/05/2026 | 28/05/2026 | [Terraform Modules Guide](https://developer.hashicorp.com/terraform/tutorials/modules/module) |
| **29/05** | Terraform State Management: Quản lý và bảo mật file trạng thái hệ thống | 29/05/2026 | 29/05/2026 | [Terraform State Docs](https://developer.hashicorp.com/terraform/language/state) |
| **30/05** | Provisioning Network Infrastructure: Tự động hóa deploy AWS VPC | 30/05/2026 | 30/05/2026 | [Terraform AWS VPC Tutorial](https://developer.hashicorp.com/terraform/tutorials/aws-get-started/aws-build) |
| **31/05** | Automating Compute & Security: Deploy EC2 và Security Group bằng code | 31/05/2026 | 31/05/2026 | [Terraform AWS EC2 Instance](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/instance) |
| **01/06** | Documentation, Code Clean-up & Blog Writing: Tổng hợp mã nguồn và viết báo cáo | 01/06/2026 | 01/06/2026 | Hugo Portfolio Blog |

---
### Minh chứng thực tế:
## 1. Tự động hóa triển khai vùng mạng cô lập (VPC) và máy chủ Web (EC2) trên AWS sử dụng Terraform Modules.

**Mô hình kiến trúc hệ thống**
![](/images/mohinh.png)
---

## 2. Mục tiêu của bài Lab
* **Làm chủ Infrastructure as Code (IaC):** Thay thế hoàn toàn thao tác cấu hình thủ công bằng việc định nghĩa hạ tầng bằng mã nguồn Declarative.
* **Tư duy Module hóa:** Biết cách phân rã hệ thống thành các khối độc lập (`vpc` và `ec2`), tăng khả năng tái sử dụng mã nguồn.
* **Quản lý Trạng thái (State Management):** Hiểu rõ cách `terraform.tfstate` ánh xạ mã nguồn với tài nguyên thực tế trên AWS Cloud.
* **Xử lý lỗi thực tế:** Trải nghiệm và khắc phục các lỗi liên quan đến đồng bộ Region, kẹt cache State và định dạng AMI ID.

---

## 3. Các thành phần và Mục đích sử dụng

Bài lab được tối ưu hóa theo mô hình tinh gọn để tập trung vào luồng xử lý dữ liệu giữa các Module:

### Kiến trúc thư mục cấu trúc Lab
    terraform-lab6/
        ├── main.tf                 # File cấu hình chính kết nối các Module
        ├── provider.tf             # Định nghĩa AWS Provider và khu vực làm việc (Singapore)
        ├── modules/                # Thư mục cô lập các tài nguyên riêng biệt
        │   ├── vpc/
        │   │   ├── main.tf         # Định nghĩa mạng: VPC, Subnet, Internet Gateway, Route Table
        │   │   └── outputs.tf      # Xuất các biến vpc_id, subnet_id ra ngoài
        │   └── ec2/
        │       ├── main.tf         # Định nghĩa máy chủ EC2 và Security Group bảo mật
        │       └── variables.tf    # Khai báo các biến nhận vào từ module VPC

### Bảng Thành Phần Tài Nguyên

Dưới đây là danh sách các tài nguyên AWS được khởi tạo và quản lý hoàn toàn tự động thông qua mã nguồn Terraform trong bài thực hành này:

| Tài nguyên | Mục đích sử dụng |
| :--- | :--- |
| **AWS Provider** | Kết nối Terraform CLI với AWS API tại khu vực `ap-southeast-1` (Singapore). |
| **VPC & Subnet** | Tạo lập vùng mạng ảo cô lập và phân đoạn mạng Public phục vụ cho máy chủ web. |
| **Internet Gateway & Route Table** | Cấp quyền và định tuyến lưu lượng giúp máy chủ kết nối được với Internet bên ngoài. |
| **Security Group** | Card mạng bảo mật đóng vai trò tường lửa, chỉ mở các port thiết yếu như `22` (SSH) và `80` (HTTP). |
| **EC2 Instance** | Máy chủ ảo chạy hệ điều hành Ubuntu Server để vận hành ứng dụng. |

---

## 4. Hướng Dẫn Các Bước Triển Khai Chi Tiết
### Bước 1: Khởi tạo thông tin xác thực (Credentials)
Trước khi viết code, cần cấu hình AWS CLI dưới máy cục bộ để cấp quyền truy cập bảo mật cho Terraform:

    aws configure

### Bước 2: Thiết lập file Provider (provider.tf)
Khai báo nhà cung cấp dịch vụ đám mây và phiên bản cấu hình yêu cầu.

    terraform {
      required_providers {
        aws = {
          source  = "hashicorp/aws"
          version = "~> 5.0"
        }
      }
    }

    provider "aws" {
      region = "ap-southeast-1"
    }

### Bước 3: Xây dựng Module VPC (modules/vpc/)
Khởi tạo phân vùng mạng ảo, chia subnet public và thiết lập các bảng định tuyến (Route Table) kết nối trực tiếp với Internet Gateway.

### Bước 4: Xây dựng Module EC2 linh hoạt (modules/ec2/)
Giải pháp tối ưu: Để tránh lỗi biến động ID AMI tĩnh từ AWS (ví dụ lỗi InvalidAMIID.Malformed), bài lab sử dụng khối data để tự động quét tìm bản cập nhật Ubuntu sạch và mới nhất từ chính chủ (Canonical).

    data "aws_ami" "ubuntu" {
      most_recent = true
      owners      = ["099720109477"] # Canonical

      filter {
        name   = "name"
        values = ["ubuntu/images/hvm-ssd/ubuntu-jammy-22.04-amd64-server-*"]
      }

      filter {
        name   = "virtualization-type"
        values = ["hvm"]
      }
    }

    resource "aws_instance" "web" {
      ami           = data.aws_ami.ubuntu.id
      instance_type = "t2.micro"
      # Các cấu hình network_interface và security group đi kèm...
    }

### Bước 5: Đấu nối hạ tầng tại file tổng (main.tf)
Gọi các module thành phần và truyền tham số đầu vào/đầu ra (Input/Output Variables) để liên kết mạng VPC với máy chủ EC2.

### Bước 6: Vận hành câu lệnh CLI
Thực hiện chu trình triển khai hạ tầng chuẩn quy trình GitOps/DevOps:

    # Khởi tạo môi trường làm việc và tải các provider/module
    terraform init

    # Kiểm tra và duyệt trước kế hoạch khởi tạo tài nguyên
    terraform plan

    # Áp dụng thay đổi để triển khai hạ tầng lên AWS Cloud
    terraform apply -auto-approve

## Kết quả đạt được tuần 6

Sau quá trình nghiên cứu và khắc phục triệt để các lỗi phát sinh về định dạng ảnh đĩa ảo (InvalidAMIID.Malformed) cũng như hiện tượng lệch vùng đồng bộ hệ thống, bài lab đã hoàn thành xuất sắc toàn bộ các mục tiêu đề ra:

Hạ tầng hiển thị chuẩn xác trên AWS Console: Khi chuyển đổi giao diện quản lý sang vùng Singapore (ap-southeast-1), toàn bộ cụm tài nguyên bao gồm VPC mạng nội bộ, cổng Internet Gateway, nhóm bảo mật Security Group và máy chủ Ubuntu Server đều xuất hiện ở trạng thái hoạt động bình thường (Running).

Làm chủ cơ chế tự động hóa động: Thay vì gán cứng các mã chuỗi tĩnh dễ gãy (hardcoded AMI ID), việc tích hợp thành công bộ lọc dữ liệu động data "aws_ami" giúp mã nguồn luôn tự động tìm kiếm phiên bản tối ưu nhất của nhà phát hành, tăng tính bền vững khi tái áp dụng hoặc mở rộng code sau này.

Thu gọn và dọn dẹp tài nguyên thông minh: Chỉ với một câu lệnh duy nhất, toàn bộ hạ tầng thử nghiệm được thu hồi sạch sẽ, giúp tối ưu chi phí và quản lý tài chính đám mây hiệu quả:

    Bash
    terraform destroy -auto-approve




