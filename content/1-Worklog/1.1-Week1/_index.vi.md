---
title: "Nhật ký Tuần 1: Khởi động và Thiết lập môi trường"
date: 2026-04-23
weight: 1
chapter: false
pre: " <b> 1.1. </b> "
---

### Mục tiêu Tuần 1:
* Thiết lập môi trường thực tập chuyên nghiệp cho chương trình First Cloud Journey.
* Hiểu sâu sắc về hạ tầng toàn cầu của AWS.
* Nắm vững kiến thức nền tảng về mạng Cloud (VPC) và các dịch vụ tính toán (EC2).

### Nhật ký công việc chi tiết (Tuần 1):
| Thứ | Công việc | Ngày BĐ | Ngày KT | Nguồn tài liệu |
| :--- | :--- | :--- | :--- | :--- |
| 17/04 | Định hướng, thiết lập tài khoản & IAM User | 17/04/2026 | 17/04/2026 | [AWS Getting Started](https://cloudjourney.awsstudygroup.com/) |
| 18/04 | Nghiên cứu hạ tầng toàn cầu & phân nhóm dịch vụ | 18/04/2026 | 18/04/2026 | [Explore AWS Services](https://cloudjourney.awsstudygroup.com/) |
| 19/04 | Tìm hiểu Networking: VPC, Subnet, IGW, Route Tables | 19/04/2026 | 19/04/2026 | [Networking Essentials](https://cloudjourney.awsstudygroup.com/) |
| 20/04 | Cài đặt AWS CLI v2 và cấu hình xác thực | 20/04/2026 | 20/04/2026 | [AWS CLI Operations](https://cloudjourney.awsstudygroup.com/) |
| 21/04 | Nghiên cứu kiến trúc EC2, AMI, và vòng đời EBS | 21/04/2026 | 21/04/2026 | [Compute Essentials](https://cloudjourney.awsstudygroup.com/) |
| 22/04 | Thực hành: Triển khai EC2, cấu hình Security Group | 22/04/2026 | 22/04/2026 | [EC2 Workshop](https://cloudjourney.awsstudygroup.com/) |
| 23/04 | Tổng kết công việc tuần và cập nhật lên Hugo | 23/04/2026 | 23/04/2026 | Tự học |

---

### Minh chứng thực tế:

### 1. Bảo mật & Quản trị (IAM)

**Mục tiêu:**
Thiết lập khung quản trị an toàn, thực thi nguyên tắc "Đặc quyền tối thiểu" (Least Privilege) để cô lập rủi ro vận hành.

**Giải thích dịch vụ:**

AWS IAM (Identity and Access Management): Dịch vụ quản lý định danh và quyền hạn. Nó cho phép bạn kiểm soát ai (User) có thể truy cập tài nguyên nào (Service) và thực hiện hành động gì (Action).

IAM Policy: Là các file định dạng JSON chứa các quy tắc (Allow/Deny) áp dụng lên tài nguyên. Việc dùng Customer Managed Policy thay vì AdministratorAccess giúp đảm bảo tài khoản chỉ có quyền thực thi đúng những gì cần thiết cho công việc.

**Các bước thực hiện:**
1. **Khởi tạo User:** Tạo IAM User (`MinhKhanh-DevOps`) thay vì sử dụng tài khoản Root.
2. **Thiết kế Policy:** Thay vì sử dụng quyền `AdministratorAccess` (full quyền), tôi đã chủ động tạo một **Customer Managed Policy** tên là `MinhKhanh-EC2-Limited-Access`.
3. **Cấu hình Quyền (JSON):** Tôi sử dụng cú pháp JSON để cấp quyền chi tiết (`Describe`, `Start`, `Stop`, `CreateKeyPair`) trên EC2.
4. **Kiểm chứng:** Khi thử tạo mới một Key Pair, hệ thống đã chặn lại với thông báo lỗi `Access Denied` (do chưa cấp quyền `CreateKeyPair`). Sau đó, tôi đã chỉnh sửa Policy để cấp quyền và thực hiện thành công.

![IAM-User-Created](images/IAM-User-Created.png) 
*Giải thích: Tổng quan User `MinhKhanh-DevOps` đã được khởi tạo thành công với quyền hạn được giới hạn qua Policy.*

![Custom-Policy-Defined](images/Custom-Policy-Defined.png)
*Giải thích: Chi tiết Policy `MinhKhanh-EC2-Limited-Access` được định nghĩa, chỉ cho phép các hành động liệt kê và quản trị EC2.*

![Access-Denied-Error](images/Access-Denied-Error.png)
*Giải thích: "Huy chương" bảo mật - Thông báo lỗi từ AWS khi User chưa được cấp quyền `ec2:CreateKeyPair`, chứng minh nguyên tắc bảo mật đang hoạt động hiệu quả.*

### 2. Kiểm soát ngân sách (Billing):**

**Mục tiêu:**

 Kiểm soát tài chính dự án, thiết lập hệ thống cảnh báo và giám sát chi phí thực tế so với hạn mức tín dụng được cấp.

**Giải thích dịch vụ:**

AWS Billing & Cost Management: Công cụ quản lý chi phí, cho phép theo dõi mức độ tiêu thụ tài nguyên theo thời gian thực, giúp quản trị viên nắm bắt được chi phí phát sinh trước khi vượt quá ngân sách.

* **Các bước:** Truy cập Billing Dashboard -> Xác nhận hạn mức tín dụng ($200.00).

![Dashboard](images/console-dashboard.png) ![Billing](images/billing-credits.png)

### 3. Hạ tầng mạng (VPC - Virtual Private Cloud):**

**Mục tiêu:**

 Xây dựng "môi trường mạng riêng ảo" (Isolated Network), phân đoạn hạ tầng để bảo mật kết nối và kiểm soát luồng dữ liệu.

**Giải thích dịch vụ:**

Amazon VPC (Virtual Private Cloud): Cung cấp một không gian mạng biệt lập trong hạ tầng AWS. VPC cho phép bạn tự định nghĩa dải địa chỉ IP (CIDR block), tạo các mạng con (Subnets) và kiểm soát định tuyến.

Internet Gateway: Là cổng kết nối cho phép các tài nguyên trong VPC có thể giao tiếp với Internet bên ngoài.

* **Các bước:** Sử dụng Resource Map để phân tích VPC, Subnets, Internet Gateway.
![VPC Map](images/vpc-resource-map.jpg)

### 4. Định tuyến & Phân đoạn mạng (Routing & CIDR):**

**Mục tiêu:**

 Thiết lập các bảng điều phối gói tin (Route Tables) để định hướng lưu lượng giữa các mạng con, đảm bảo cấu trúc mạng hoạt động logic và có tính bảo mật cao.

**Giải thích dịch vụ:**

Route Table: Chứa các quy tắc (Routes) xác định hướng đi của lưu lượng mạng từ Subnet đến các Gateway hoặc các địa chỉ IP đích khác.

CIDR (Classless Inter-Domain Routing): Phương pháp phân chia không gian địa chỉ IP, dùng để xác định quy mô và phạm vi của từng Subnet trong hệ thống mạng của bạn.

* **Các bước:** Inspect Route Table -> Verify Subnet CIDR.
![Route Table](images/route-table-details.jpg) ![Subnet](images/subnet-details.jpg)

### 5. Tự động hóa với AWS CLI & Terraform (IaC):**

**Mục tiêu:**

 Chuyển đổi mô hình quản trị từ thao tác thủ công (Click-ops) sang tự động hóa (IaC - Infrastructure as Code) để tăng tính nhất quán và khả năng quản lý phiên bản.

**Giải thích dịch vụ & Công cụ:**

AWS CLI (Command Line Interface): Bộ công cụ dòng lệnh cho phép tương tác trực tiếp với API của AWS, giúp thực hiện các lệnh quản trị nhanh chóng và có thể nhúng vào các kịch bản (script) tự động.

Terraform (IaC): Một công cụ mã nguồn mở cho phép bạn định nghĩa hạ tầng dưới dạng mã (code). Thay vì cấu hình thủ công, bạn viết file cấu hình (main.tf), Terraform sẽ dựa vào đó để tạo, cập nhật hoặc xóa tài nguyên một cách chính xác.

AWS Provider: Plugin giúp Terraform hiểu cách thức giao tiếp và điều khiển các tài nguyên cụ thể của AWS thông qua API.

**Các bước thực hiện:**
1. **Cài đặt công cụ:**
   * **AWS CLI v2:** [Tải tại đây](https://aws.amazon.com/cli/).
   * **Terraform:** [Tải bản AMD64 tại đây](https://www.terraform.io/downloads.html).
2. **Cấu hình xác thực:** Chạy lệnh `aws configure` để thiết lập Access Key và Secret Key. Xác thực qua `aws sts get-caller-identity`.
3. **Khởi tạo hạ tầng (IaC):** * Tạo thư mục dự án và file `main.tf` định nghĩa Provider (AWS).
   * Chạy lệnh `terraform init` để tải bộ công cụ (plugin) cần thiết.

**Minh chứng thực tế:**
![CLI-Auth-Success](images/cli-success.png)
*Giải thích: Xác thực thành công tài khoản `MinhKhanh-User` thông qua AWS CLI.*

![Terraform-Init-Success](images/terraform-init-success.png)
*Giải thích: Terraform đã được khởi tạo thành công (`successfully initialized`), sẵn sàng để xây dựng hạ tầng.*

### 6. Máy chủ ảo EC2 & Lưu trữ (EBS):

**Mục tiêu:**
Bài lab này hướng dẫn quy trình xây dựng một cơ sở hạ tầng mạng cơ bản trên AWS bao gồm VPC, Subnet, Internet Gateway và Route Table. Sau đó, triển khai một máy chủ ảo (EC2) chạy dịch vụ Nginx để phục vụ nội dung web công cộng.

**Các bước thực hiện:**

**Bước 1: Thiết lập "Mảnh đất" (VPC)**

VPC (Virtual Private Cloud) là không gian mạng riêng biệt của bạn trên AWS.
* **Thao tác:** Truy cập **VPC Dashboard** -> **Your VPCs** -> **Create VPC**.
* **Cấu hình:**
    * Chọn "VPC only".
    * Tên (Name tag): `MinhKhanh-VPC`.
    * IPv4 CIDR block: `10.0.0.0/16`.
* **Giải thích:** CIDR `10.0.0.0/16` cung cấp dải địa chỉ IP nội bộ rộng lớn (hơn 65,000 địa chỉ) cho các tài nguyên trong VPC của bạn.

**Bước 2: Tạo "Cổng kết nối" (Internet Gateway & Subnet)**
* **Internet Gateway (IGW):** Chọn **Internet Gateways** -> **Create internet gateway** -> Tên: `MinhKhanh-IGW`. Sau khi tạo, nhấn **Actions** -> **Attach to VPC** -> chọn `MinhKhanh-VPC`.
    * *Giải thích:* IGW đóng vai trò như một chiếc cầu nối, cho phép tài nguyên trong VPC giao tiếp với Internet bên ngoài.
* **Subnet:** Chọn **Subnets** -> **Create subnet** -> Chọn `MinhKhanh-VPC`.
    * Tên: `MinhKhanh-Subnet`.
    * IPv4 CIDR block: `10.0.1.0/24`.
    * *Giải thích:* Subnet chia nhỏ VPC thành các phân đoạn mạng. `/24` cung cấp 256 địa chỉ IP, đủ cho các cụm máy chủ trong môi trường lab.
![Mô hình mạng VPC](images/VPCsubnet.png)
*(Hình 1: Cấu trúc VPC và Subnet đã thiết lập)*
**Bước 3: Mở đường ra Internet (Route Table)**
Nếu không có bước này, server của bạn sẽ là một "đảo hoang" không thể kết nối ra Internet.
* **Tạo bảng định tuyến:** Chọn **Route tables** -> **Create route table** -> Tên: `MinhKhanh-RT` -> Chọn `MinhKhanh-VPC`.
* **Cấu hình Route:** Chọn tab **Routes** -> **Edit routes** -> **Add route**:
    * Destination: `0.0.0.0/0`.
    * Target: **Internet Gateway** -> chọn `MinhKhanh-IGW`.
* **Liên kết Subnet:** Chọn tab **Subnet Associations** -> **Edit subnet associations** -> chọn `MinhKhanh-Subnet` -> **Save**.

**Bước 4: Tạo "Cổng bảo vệ" (Security Group)**
Security Group hoạt động như một bức tường lửa ảo kiểm soát lưu lượng ra/vào.
* **Thao tác:** Chọn **Security Groups** -> **Create security group**.
    * Tên: `minhkhanh-web-sg`.
    * VPC: `MinhKhanh-VPC`.
* **Inbound rules:** Thêm rule cho **HTTP** (Port 80) với Source là `0.0.0.0/0` (cho phép mọi người truy cập vào trang web).
* **Outbound rules:** Để mặc định `0.0.0.0/0` để server có thể phản hồi lại các yêu cầu hoặc tải cập nhật.
![Cấu hình Security Group](images/scInbound.png)
*(Hình 2: Cấu hình Inbound rules cho Web Server)*
**Bước 5: Khởi tạo Web Server (EC2)**
* **Launch Instance:** Vào EC2 Dashboard -> **Launch instance**.
    * Tên: `MinhKhanh-Web-Server`.
    * AMI: **Amazon Linux 2023**.
* **Network settings:** Nhấn Edit:
    * Chọn `MinhKhanh-VPC`.
    * Chọn `MinhKhanh-Subnet`.
    * Auto-assign public IP: **Enable** (Để server có IP công cộng mà người dùng có thể truy cập).
    * Chọn Security group: `minhkhanh-web-sg`.
* **User Data:** Script cài đặt tự động vào mục **Advanced details**:

        yum update -y

        yum install nginx -y

        systemctl start nginx

        systemctl enable nginx

![Kết quả Web Server](images/web.png)
*(Hình 3: Kết quả trang web chạy trên Nginx)*


### Kết quả đạt được trong tuần 1:
* **Tư duy quản trị:** Làm chủ nguyên tắc Đặc quyền tối thiểu (IAM) và Giám sát tài nguyên (Billing).
* **Kỹ năng chuyên môn:** Triển khai thành công hạ tầng mạng (VPC), tính toán (EC2) và lưu trữ (EBS).
* **Khả năng vận hành:** Sử dụng thành thạo CLI để tối ưu hóa công việc thay vì thao tác tay.