---
title: "Nhật ký Tuần 5: Advanced Cloud Operations & Production Architecture"
date: 2024-01-01
weight: 1
chapter: false
pre: " <b> 1.5. </b> "
---


### Mục tiêu tuần 5:


Tuần này tập trung vào việc hiện thực hóa một hạ tầng đám mây chuẩn **Production (Production-Ready Architecture)**, chuyển dịch từ các mô hình thử nghiệm sang hệ thống có tính sẵn sàng cao, bảo mật nghiêm ngặt và khả năng chịu lỗi tối ưu trên AWS:
1. **Thiết kế Mạng Kín & Bảo Mật Cao:** Làm chủ kiến trúc Advanced VPC với mô hình Multi-AZ, cô lập hoàn toàn môi trường tính toán trong Private Subnet và kiểm soát truy cập nghiêm ngặt thông qua Bastion Host (Jump Box) an toàn.
2. **Quản Lý Kết Nối & Định Tuyến An Toàn:** Vận hành NAT Gateway hiệu quả cho luồng outbound traffic của hạ tầng nội bộ mà không làm lộ địa chỉ IP private ra ngoài Internet public.
3. **Đảm Bảo Tính Sẵn Sàng Cao & Chịu Lỗi (HA/DR):** Thiết kế và triển khai kiến trúc Multi-AZ tự động co giãn và khắc phục sự cố, kết hợp chiến lược Backup & Disaster Recovery toàn diện giúp bảo vệ dữ liệu tối đa trước mọi rủi ro gián đoạn.
4. **Gia Cố Bảo Mật Tầng Biên (Security Hardening):** Triển khai các lớp lá chắn bảo vệ lớp ứng dụng web chống lại các cuộc tấn công DDoS và lỗ hổng bảo mật thông dụng nhờ AWS WAF và AWS Shield.
5. **Hiện Thực Hóa Dự Án Thực Tế (Mini Production Project):** Đóng gói toàn bộ kiến thức thành một hệ thống thực tế hoàn chỉnh, tiến hành trực quan hóa kiến trúc (Architecture Diagram) và tài liệu hóa chuẩn chuyên nghiệp trên Blog Technical.

## Nhật Ký Lộ Trình Tuần 5

| Thứ | Công việc | Ngày BĐ | Ngày KT | Nguồn tài liệu |
| :---: | :--- | :---: | :---: | :--- |
| **19/05** | Advanced VPC & Bastion Architecture: Hiểu mô hình mạng production | 19/05/2026 | 19/05/2026 | [AWS VPC Docs](https://docs.aws.amazon.com/vpc/) |
| **20/05** | NAT Gateway, Private Subnet & Secure Access: Xây dựng hệ thống private | 20/05/2026 | 20/05/2026 | AWS Networking Lab |
| **21/05** | Backup & Disaster Recovery trên AWS: Đảm bảo an toàn dữ liệu | 21/05/2026 | 21/05/2026 | [AWS Backup Service](https://aws.amazon.com/backup/) |
| **22/05** | High Availability Multi-AZ Architecture: Thiết kế hệ thống chịu lỗi | 22/05/2026 | 22/05/2026 | [AWS Architecture Center](https://aws.amazon.com/architecture/) |
| **23/05** | WAF, Shield & Security Hardening: Bảo vệ hệ thống web | 23/05/2026 | 23/05/2026 | [AWS WAF & Shield](https://aws.amazon.com/waf/) |
| **24/05** | Mini Production Project Deployment: Xây dựng hệ thống hoàn chỉnh | 24/05/2026 | 24/05/2026 | Tự triển khai |
| **25/05** | Documentation, Diagram & Blog Technical Writing: Tổng hợp và viết báo cáo | 25/05/2026 | 25/05/2026 | Hugo Portfolio Blog |

---
### Minh chứng thực tế:


**Mô hình kiến trúc hệ thống**
![](/images/mohinh.png)

## 1. Mục tiêu của bài Lab

Bài lab tổng hợp này hiện thực hóa một hạ tầng đám mây chuẩn Production bền vững với 3 mục tiêu cốt lõi:
* **Tính sẵn sàng cao (High Availability):** Thiết kế kiến trúc Multi-AZ phân tán tài nguyên trên nhiều trung tâm dữ liệu độc lập, loại bỏ điểm lỗi đơn lẻ (Single Point of Failure).
* **Bảo mật đa lớp (Defense in Depth):** Cô lập máy chủ ứng dụng hoàn toàn trong vùng mạng Private Subnet, kiểm soát chặt chẽ luồng traffic quản trị qua Bastion Host và triển khai lá chắn **AWS WAF** ở tầng ứng dụng (Lớp 7) để lọc mã độc.
* **Đảm bảo an toàn dữ liệu (Disaster Recovery):** Tự động hóa quy trình sao lưu snapshot hệ thống phòng ngừa thảm họa toàn diện.

---

## 2. Các thành phần và Mục đích sử dụng

| Thành phần AWS | Vai trò kỹ thuật & Mục đích sử dụng |
| :--- | :--- |
| **Advanced VPC** | Phân chia hệ thống mạng thành 2 Public Subnets (`10.0.0.0/24`, `10.0.1.0/24`) tiếp xúc Internet và 2 Private Subnets (`10.0.128.0/24`, `10.0.129.0/24`) cách ly tuyệt đối. |
| **Bastion Host** | Trạm trung chuyển bảo mật duy nhất nằm ở vùng Public, dùng để quản trị (SSH nội bộ) các dòng máy chủ ứng dụng vùng Private. |
| **NAT Gateway** | Cho phép các máy chủ trong Private Subnet kết nối Outbound ra Internet để cập nhật bản vá hệ điều hành nhưng ngăn không cho Internet truy cập ngược lại vào. |
| **Application Load Balancer (ALB)** | Tiếp nhận và điều phối traffic người dùng phân phối đều sang hai AZ để đảm bảo hiệu năng chịu lỗi. |
| **AWS WAF (Web ACL)** | Tường lửa ứng dụng lớp 7 ngăn chặn các cuộc tấn công khai thác lỗ hổng bảo mật phổ biến như SQL Injection, Cross-Site Scripting (XSS). |
| **AWS Backup** | Quản lý tập trung các điểm khôi phục, tự động hóa quy trình Snapshot của cụm máy chủ theo lịch trình. |

---

## 3. Hướng dẫn các bước triển khai chi tiết

### Bước 1: Khởi tạo mạng lưới bảo mật (Advanced VPC Architecture)
1. Truy cập **VPC Dashboard** -> chọn **Create VPC** -> tích chọn ô **VPC and more**.
2. Thiết lập thông số hạ tầng:
   * **Name tag auto-generation**: `Production-VPC`
   * **IPv4 CIDR block**: `10.0.0.0/16`
   * **Number of Availability Zones (AZs)**: `2` (AZ A và AZ B).
   * **Number of Public subnets**: `2`
   * **Number of Private subnets**: `2`
   * **NAT Gateways**: Chọn **In 1 AZ** để tối ưu hóa chi phí cho môi trường Lab.
3. Nhấn **Create VPC** và đợi hệ thống ánh xạ tự động các Route Tables.

### Bước 2: Cấu hình Security Groups (Tường lửa lớp Instance)
Khởi tạo 3 nhóm quy tắc an toàn cốt lõi:
* **Bastion-SG:** Mở Inbound port `22` (SSH) duy nhất từ địa chỉ IP mạng cục bộ của Người quản trị (`My IP`).
* **ALB-SG:** Mở Inbound port `80` (HTTP) cho toàn bộ Internet công cộng (`0.0.0.0/0`).
* **WebServer-SG:** Siết chặt an toàn hạ tầng:
  * Mở port `80` (HTTP) nhận traffic có nguồn (`Source`) chỉ định từ **ALB-SG**.
  * Mở port `22` (SSH) nhận traffic quản trị có nguồn (`Source`) chỉ định từ **Bastion-SG**.

### Bước 3: Triển khai cụm máy chủ EC2
1. Khởi chạy 1 máy chủ mang tên `Bastion-Host` thuộc mạng `Public Subnet 1`, bật tùy chọn **Enable Auto-assign public IP**, gán group `Bastion-SG`.
2. Khởi chạy 2 máy chủ Web Server đặt hoàn toàn trong vùng tối mạng nội bộ, tắt tính năng Public IP, sử dụng chung mã khóa mật mã và áp dụng group `WebServer-SG`:
   * `Web-Server-A` đặt tại mạng `Private Subnet 1`.
   * `Web-Server-B` đặt tại mạng `Private Subnet 2`.
3. Nhúng đoạn script khởi tạo tự động (User data) cấu hình môi trường ứng dụng:
    ```bash
    #!/bin/bash
    sudo dnf update -y
    sudo dnf install -y httpd
    sudo systemctl start httpd
    sudo systemctl enable httpd
    echo "<h1>Hello from Web Server (Private AZ)</h1>" > /var/www/html/index.html

### Bước 4: Thiết lập cân bằng tải High Availability với ALB
1. Truy cập **Target Groups** -> tạo mới một nhóm Instance định danh `Web-TG` chạy port `80`, tiến hành gom cả 2 máy chủ `Web-Server-A` và `Web-Server-B` vào trạng thái sẵn sàng nhận tải.
2. Truy cập **Load Balancers** -> tạo mới một **Application Load Balancer** mang tên `Prod-ALB`, chọn chế độ hướng ngoại (**Internet-facing**).
3. Tại mục **Network mapping**, liên kết ALB vào 2 Public Subnets trên cả 2 AZ, gán nhóm bảo mật `ALB-SG`.
4. Tại mục **Listeners and routing**, cấu hình luật mặc định chuyển tiếp toàn bộ yêu cầu về nhóm mục tiêu `Web-TG`.

### Bước 5: Kích hoạt lá chắn bảo mật tầng ứng dụng với AWS WAF
1. Truy cập dịch vụ **WAF & Shield** -> chọn mục **Protection packs (web ACLs)** -> nhấn **Create protection pack (web ACL)**.
2. Tại phần cấu hình ứng dụng:
   * **App category**: Chọn `Other` (hoặc `General`).
   * **App focus**: Giữ mặc định `Both API and web`.
3. Tại mục **Select resources to protect**, chọn loại hình **Add regional resources** -> Tìm chọn **Application Load Balancer** -> liên kết trực tiếp vào hệ thống cân bằng tải `Prod-ALB`.
4. Tại mục **Choose initial protections**, tích chọn gói quy tắc mẫu **Recommended rules for you** để tự động kích hoạt bộ luật cốt lõi phòng chống SQL Injection và lọc danh tiếng mã nguồn IP độc hại.
5. Đặt tên gói bảo vệ là `Prod-Web-ACL` và tiến hành khởi tạo.

### Bước 6: Cấu hình hệ thống dự phòng thảm họa (Disaster Recovery)
1. Truy cập dịch vụ **AWS Backup** -> vào mục **Backup vaults** -> chọn **Create Backup vault** để tạo hòm lưu trữ mã hóa an toàn mang tên `Prod-Backup-Vault`.
2. Chuyển sang mục **Backup plans** -> chọn **Create backup plan** -> xây dựng kế hoạch mới từ đầu (**Build a new plan**) đặt tên `Prod-Daily-Backup`.
3. Thiết lập thông số vận hành (*Backup rule*): Tần suất chạy hằng ngày (**Daily**), lưu giữ dữ liệu an toàn trong vòng **30 ngày** (**Days** - lưu ý điều chỉnh đơn vị từ *Years* sang *Days* để tránh lãng phí chi phí) và chọn đích lưu về `Prod-Backup-Vault`.
4. Tại giao diện kế hoạch sau khi tạo, nhấn **Assign resources** -> chọn phương thức gán theo loại tài nguyên chuyên biệt (**Include specific resource types**) -> chọn phân hệ **EC2** và chỉ định chính xác ID của `Web-Server-A` và `Web-Server-B`.

### Bước 7. Kiểm tra, Nghiệm thu & Bài học thực tế

#### Kiểm tra 1: Khả năng chia tải và chịu lỗi Multi-AZ
1. Tiến hành truy cập liên kết URL DNS Name của hệ thống ALB trên một trình duyệt ẩn danh độc lập. Kết quả khi làm mới trang liên tục (**F5**), traffic được phân bổ luân phiên mượt mà giữa hai trung tâm dữ liệu:
   * **Lượt 1:** Hiển thị nội dung `Hello from Web Server A (Private AZ-A)`.
   * **Lượt 2:** Hiển thị nội dung `Hello from Web Server B (Private AZ-B)`.
2. **Thử nghiệm đánh sập (Failover):** Khi chủ động tắt máy chủ (`Stop`) `Web-Server-A`, hệ thống cân bằng tải tự động nhận biết trạng thái lỗi thông qua cơ chế Health Check, lập tức điều phối toàn bộ kết nối của người dùng về `Web-Server-B` mà không gây bất kỳ gián đoạn dịch vụ nào.

![](/images/WebA.png)
![](/images/WebB.png)

#### Kiểm tra 2: Thực nghiệm tấn công giả lập kiểm thử AWS WAF
1. Đóng vai trò là một kẻ tấn công cố tình chèn chuỗi truy vấn dữ liệu trái phép lớp 7 thông qua mã độc SQL Injection trực tiếp lên URL:
   ```text
   http://<DNS_NAME_ALB>/?id=1'+OR+1=1+UNION+SELECT+null,username,password+FROM+users--

![](/images/403WAF.png)