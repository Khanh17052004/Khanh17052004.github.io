---
title: "Nhật ký Tuần 3: Tự động hóa hạ tầng & Kiến trúc bảo mật đa tầng"
date: 2024-01-01
weight: 1
chapter: false
pre: " <b> 1.3. </b> "
---
## Mục tiêu Tuần 3:
* **Giám sát chuyên sâu:** Nghiên cứu dịch vụ AWS CloudWatch (Metrics, Logs, Alarms) để theo dõi sức khỏe hệ thống.
* **Tối ưu hóa hiệu năng:** Thực hiện Stress Test để kiểm chứng khả năng phản ứng của Auto Scaling Group.
* **Quản trị & Tuân thủ:** Triển khai AWS CloudTrail và AWS Config để kiểm soát hành vi người dùng và thay đổi cấu hình.
* **Bảo mật mạng đa lớp:** Phân tích và thực hành phân biệt giữa NACL (Stateless) và Security Group (Stateful).
* **Hoàn thiện Lab:** Tích hợp toàn diện Monitoring và Security cho mô hình Web 3-Tier.

## Nhật ký công việc chi tiết (Tuần 3):

| Thứ | Công việc | Ngày BĐ | Ngày KT | Nguồn tài liệu |
| :--- | :--- | :--- | :--- | :--- |
| **04/05** | CloudWatch Deep Dive: Tìm hiểu Metrics (CPU, Network), Log Groups và Alarm cơ bản | 04/05/2026 | 04/05/2026 | [AWS Monitoring](https://aws.amazon.com/cloudwatch/) |
| **05/05** | Stress Test & Auto Scaling: Dùng `stress-ng` đẩy CPU > 70% để kích hoạt Alarm và ASG | 05/05/2026 | 05/05/2026 | [AWS Auto Scaling](https://aws.amazon.com/autoscaling/) |
| **06/05** | Governance & Audit: Triển khai CloudTrail theo dõi User và AWS Config giám sát thay đổi | 06/05/2026 | 06/05/2026 | [AWS Governance](https://aws.amazon.com/cloudtrail/) |
| **07/05** | Infrastructure as Code với CloudFormation | 07/05/2026 | 07/05/2026 | [VPC Security](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_Security.html) |
| **08/05** | Final Lab: Tích hợp Full Stack Monitoring + Security cho hệ thống 3-Tier & Viết Blog | 08/05/2026 | 08/05/2026 | Tự học |

--- 

### Minh chứng thực tế:

### 1. CloudWatch & Monitoring System:

**Mục tiêu:**
Thiết lập khung quản trị an toàn, thực thi nguyên tắc "Đặc quyền tối thiểu" (Least Privilege) để cô lập rủi ro vận hành.

**Giải thích dịch vụ:**
 
**Amazon CloudWatch**

Chức năng: Đây là dịch vụ giám sát và quản trị hệ thống.

Metrics (Chỉ số): Thu thập các con số về hiệu suất như CPU, băng thông mạng (Network) của máy chủ.

Alarms (Cảnh báo): Đóng vai trò như một bộ cảm biến, tự động kích hoạt hành động khi các chỉ số vượt ngưỡng cho phép (ví dụ CPU > 10%).

Dashboards (Bảng điều khiển): Gom các biểu đồ vào một trang duy nhất để bạn có cái nhìn tổng quan về "sức khỏe" của toàn bộ hạ tầng mạng.

**Amazon SNS (Simple Notification Service)**

Chức năng: Dịch vụ thông báo đẩy.

Vai trò trong Lab: Đóng vai trò là "người đưa thư" để gửi thông tin từ CloudWatch Alarm đến Email cá nhân của bạn ngay khi có sự cố xảy ra.

**Amazon EC2 (Elastic Compute Cloud)**

Chức năng: Cung cấp các máy chủ ảo (Instance) chạy trên đám mây.

Vai trò trong Lab: Là đối tượng chính để bạn thực hành giám sát và tra tấn CPU (Stress Test) nhằm kiểm tra độ nhạy của hệ thống cảnh báo.

**Stress Tool (Công cụ Stress)**

Chức năng: Một phần mềm dùng để giả lập tình trạng quá tải tài nguyên.

Vai trò trong Lab: Dùng để ép CPU của máy chủ lên cao, giúp xác thực xem CloudWatch và SNS có thực sự hoạt động và gửi Email về hay không.

**Bước 1: Chuẩn bị "Vật mẫu" giám sát (EC2 Instance)**
Để có dữ liệu (Metrics) cho CloudWatch, tôi khởi tạo một máy chủ thực tế:
* **Khởi tạo:** Tại EC2 Dashboard, chọn **Launch instance**.
* **Cấu hình:** 
    * Name: `Monitoring-Lab-Server`.
    * AMI: Amazon Linux 2023 | Instance type: `t2.micro`.
    * Network: Chọn VPC/Subnet cá nhân, kích hoạt **Auto-assign public IP**.
* **Kết quả:** Máy chủ chuyển sang trạng thái **Running** sau 2 phút.

**Bước 2: Nghiên cứu Metrics (Chỉ số hệ thống)**
* **Truy cập:** CloudWatch Console -> Metrics -> All metrics -> EC2 -> Per-Instance Metrics.
* **Theo dõi:** Tôi tập trung vào các chỉ số: `CPUUtilization`, `NetworkIn`, `NetworkOut`.
* **Tối ưu:** Đổi khoảng thời gian (**Period**) sang **1 Minute** để quan sát biểu đồ dạng răng cưa chi tiết hơn thay vì mặc định 5 phút.

> **Giải thích kỹ thuật:** Metric giống như các chỉ số trên đồng hồ xe máy. Trong AWS, Metric là các con số phản ánh "sức khỏe" thực tế của tài nguyên (ví dụ: CPU đang tải bao nhiêu %).

**Bước 3: Thiết lập SNS (Simple Notification Service)**
Tôi cần một kênh trung gian để gửi cảnh báo:
* **Topic:** Tạo Topic tên `MinhKhanh-Alert-Topic` (loại Standard).
* **Subscription:** Đăng ký nhận tin qua **Email**.
* **Xác thực:** Tôi truy cập email cá nhân và nhấn **Confirm Subscription**. Đây là bước chống Spam bắt buộc của AWS để xác nhận tôi đồng ý nhận thông báo từ hệ thống.

**Bước 4: Cấu hình CloudWatch Alarm**
Thiết lập quy tắc tự động: *"Nếu CPU > 70%, gửi mail báo động"*.
* **Điều kiện:** Chọn Metric `CPUUtilization`, ngưỡng (Threshold) là **Static > 70**.
* **Hành động:** Khi trạng thái là **In alarm**, hệ thống sẽ gửi thông báo tới SNS Topic đã tạo ở Bước 3.
* **Định danh:** Đặt tên Alarm là `High-CPU-Warning-MinhKhanh`.

**Bước 5: Xây dựng Network Monitoring Dashboard**
Dành cho mục tiêu chuyên sâu về Network Engineering:
* **Tạo Dashboard:** Tên `Network-Monitoring-Khanh`.
* **Widget:** Thêm biểu đồ dạng **Line** cho hai thông số `NetworkIn` và `NetworkOut`.
* **Giá trị:** Dashboard này giúp tôi có cái nhìn tổng thể về lưu lượng băng thông ra/vào máy chủ trên một màn hình duy nhất mà không cần tìm kiếm thủ công trong hàng trăm Metrics.

### 1. Giám sát lưu lượng mạng (Network Monitoring)
Tôi đã thiết lập biểu đồ theo dõi lưu lượng thực tế trên Dashboard để đảm bảo kiểm soát được băng thông hệ thống.

![Network Monitoring](images/network.png)
*Theo dõi lưu lượng mạng (NetworkIn/NetworkOut) trên CloudWatch Dashboard. Biểu đồ cho thấy sự thay đổi băng thông khi thực hiện các tác vụ trên Instance, giúp kỹ sư mạng kiểm soát lưu lượng truy cập thời gian thực.*

---

### 2. Kiểm thử hiệu năng (Stress Testing)
Để kiểm tra tính hoạt động của hệ thống báo động, tôi sử dụng lệnh `stress` để giả lập quá tải tài nguyên.

![Stress Testing](images/testing.png)
*Sử dụng công cụ stress trên Amazon Linux 2023 để giả lập tình trạng tải CPU cao. Thao tác này nhằm mục đích kiểm tra độ nhạy của hệ thống giám sát và khả năng kích hoạt báo động khi tài nguyên chạm ngưỡng giới hạn.*

---

### 3. Trạng thái báo động (CloudWatch Alarm)
Khi tài nguyên đạt ngưỡng giới hạn đã cấu hình (10%), hệ thống lập tức ghi nhận trạng thái bất thường.

![CloudWatch Alarm](images/auto.png)
*Cấu hình CloudWatch Alarm "High-CPU-Khanh-Test". Khi CPUUtilization vượt ngưỡng 10%, hệ thống tự động chuyển từ trạng thái OK sang ALARM, kích hoạt quy trình thông báo qua giao thức SNS.*

---

### 4. Hệ thống thông báo tự động (Auto Notification)
Ngay khi báo động được kích hoạt, một thông báo được gửi trực tiếp đến quản trị viên để xử lý kịp thời.

![Auto Notification](images/email.png)
*Thông báo đẩy (Push Notification) gửi trực tiếp về email quản trị viên thông qua Amazon SNS ngay khi sự cố xảy ra. Điều này đảm bảo tính phản ứng nhanh (High Availability & Reliability) trong vận hành hệ thống Cloud.*


### 2. Xây dựng hệ thống giám sát tự động để truy vết hành động người dùng (Auditing) và kiểm soát tính an toàn của tài nguyên mạng (Governance) trên AWS:**
**Mô hình kiến trúc hệ thống**

![](images/mohinh.png)

Đây là luồng hoạt động từ lúc User thao tác cho đến khi log được phân tích và lưu trữ.


**Mục tiêu**
Xây dựng hệ thống giám sát tự động để truy vết hành động người dùng (**Auditing**) và kiểm soát tính an toàn của tài nguyên mạng (**Governance**) trên nền tảng AWS.

---

**Giải thích dịch vụ**

**AWS CloudTrail:** 

Ghi nhật ký mọi hành động (API Call) để biết Ai đã làm gì trên hệ thống.

**AWS Config:**

Theo dõi lịch sử thay đổi cấu hình tài nguyên và tự động cảnh báo khi có Sai phạm bảo mật.

**Amazon S3:** 

Làm "kho lưu trữ" tập trung, an toàn cho toàn bộ dữ liệu log của hệ thống.

---

## Các bước triển khai

**Bước 1: Khởi tạo "Kho lưu trữ" log trên Amazon S3**
Mọi dữ liệu kiểm toán cần một nơi lưu trữ an toàn và biệt lập.

1. Truy cập **S3 Console** > **Create bucket**.
2. **Bucket name**: Nhập tên duy nhất (Ví dụ: `audit-log-storage-minhkhanh`).
3. **Block Public Access**: Đảm bảo đã tích chọn **Block all public access** để bảo vệ tính riêng tư của log.
4. Nhấn **Create bucket**.

**Bước 2: Thiết lập "Camera giám sát" với AWS CloudTrail**
Ghi lại mọi hành động tác động vào tài nguyên hệ thống.

1. Truy cập **CloudTrail Console** > **Trails** > **Create trail**.
2. **Trail name**: `Main-Audit-Trail`.
3. **Storage location**: Chọn *Use existing S3 bucket* và trỏ về bucket đã tạo ở Bước 1.
4. **Log file validation**: Chọn **Enabled** (Đảm bảo log không bị sửa đổi trái phép).
5. Tại màn hình **Choose log events**, chọn **Management events** (Ghi lại các thao tác quản trị).
6. Nhấn **Next** > **Create trail**.

**Bước 3: Triển khai "Hệ thống giám sát cấu hình" AWS Config**
Theo dõi trạng thái tài nguyên mạng theo thời gian thực.

1. Truy cập **AWS Config Console** > **Settings** > **Get started**.
2. **Resource types to record**: Chọn *Record specific resource types*.
3. Tìm và chọn: `EC2:Instance` và `EC2:SecurityGroup`.
4. **Amazon S3 bucket**: Chọn bucket đã tạo ở Bước 1.
5. **IAM Role**: Chọn *Use an existing AWS Config service-linked role*.
6. Nhấn **Next**.

**Bước 4: Thiết lập Quy tắc tuân thủ (Compliance Rules)**
Định nghĩa các tiêu chuẩn an toàn cho hệ thống.

1. Tại màn hình **AWS Config** > **Rules** > **Add rule**.
2. Tìm kiếm Rule: `vpc-sg-open-only-to-authorized-ports`.
3. **Cấu hình**: Thiết lập cảnh báo nếu có bất kỳ Port nào ngoài `80` hoặc `443` bị mở công khai.
4. Nhấn **Save**. 


### 1. Dòng thời gian thay đổi tài nguyên (Resource Timeline)
![](images/Timeline.png)
Ảnh này cho thấy sự phối hợp tuyệt vời giữa Config và CloudTrail. Tại thời điểm 10:17:08, một sự kiện CloudTrail đã xảy ra dẫn đến việc thay đổi cấu hình ngay sau đó.

### 2. Bảng điều khiển tuân thủ (Compliance Dashboard)
![](images/Dashboard.png)
Hệ thống báo đỏ (Noncompliant) vì phát hiện Security Group đang mở port không nằm trong danh sách cho phép, giúp Admin nhận diện rủi ro ngay lập tức.

### 3.Infrastructure as Code với CloudFormation x
**Mục tiêu Lab**
Bài Lab này tập trung vào việc chuyển dịch từ quản trị hạ tầng thủ công (ClickOps) sang quản trị bằng mã nguồn (Infrastructure as Code - IaC):
* **Tự động hóa:** Khởi tạo hạ tầng VPC và EC2 chỉ với một tệp tin cấu hình duy nhất.
* **Quản trị rủi ro:** Tìm hiểu cơ chế **Update Stack** và **Rollback** tự động khi có lỗi xảy ra.
* **DevOps Mindset:** Xây dựng nền tảng tư duy tự động hóa cho các công cụ như Terraform và CDK sau này.

---

**Các dịch vụ AWS sử dụng**
Trong bài Lab này, chúng ta sẽ định nghĩa và kết nối các dịch vụ cốt lõi sau:
1. **AWS CloudFormation:** Dịch vụ chính dùng để đọc file template và triển khai tài nguyên theo đúng trình tự.
2. **Amazon VPC (Virtual Private Cloud):** Tạo mạng ảo cô lập để đặt các tài nguyên máy chủ.
3. **Amazon EC2 (Elastic Compute Cloud):** Khởi tạo máy chủ ảo chạy hệ điều hành Amazon Linux 2.
4. **Systems Manager (SSM) Parameter Store:** Tự động truy vấn AMI ID mới nhất của AWS để đảm bảo tính linh hoạt trên mọi Region.
5. **Security Groups:** Thiết lập tường lửa ảo để kiểm soát lưu lượng truy cập (Port 22 cho SSH).

---

**Các bước triển khai**

**Bước 1: Viết Template CloudFormation (YAML)**
Sử dụng mã nguồn dưới đây để định nghĩa hạ tầng. File này đã được tối ưu hóa để tự động tra cứu AMI ID:


    AWSTemplateFormatVersion: '2010-09-09'
    Description: 'Infrastructure as Code Final Lab - Success Version'

    Parameters:**
    LatestAmiId:
        Type: 'AWS::SSM::Parameter::Value<AWS::EC2::Image::Id>'
        Default: '/aws/service/ami-amazon-linux-latest/amzn2-ami-hvm-x86_64-gp2'

    Resources:
    # Khởi tạo mạng lưới
    MyLabVPC:
        Type: AWS::EC2::VPC
        Properties:
        CidrBlock: 10.0.0.0/16
        EnableDnsSupport: true
        EnableDnsHostnames: true
        Tags:
            - Key: Name
            Value: Lab-IaC-VPC

    # Khởi tạo Subnet
    MyLabSubnet:
        Type: AWS::EC2::Subnet
        Properties:
        VpcId: !Ref MyLabVPC
        CidrBlock: 10.0.1.0/24
        AvailabilityZone: !Select [ 0, !GetAZs '' ]
        Tags:
            - Key: Name
            Value: Lab-IaC-Subnet

    # Security Group cho phép SSH
    MySecurityGroup:
        Type: AWS::EC2::SecurityGroup
        Properties:
        GroupDescription: Allow SSH access from anywhere
        VpcId: !Ref MyLabVPC
        SecurityGroupIngress:
            - IpProtocol: tcp
            FromPort: 22
            ToPort: 22
            CidrIp: 0.0.0.0/0

    # Khởi tạo EC2 Instance
    MyEC2Instance:
        Type: AWS::EC2::Instance
        Properties:
        InstanceType: t2.micro
        ImageId: !Ref LatestAmiId
        SubnetId: !Ref MyLabSubnet
        SecurityGroupIds:
            - !Ref MySecurityGroup
        Tags:
            - Key: Name
            Value: Lab-IaC-EC2-Fixed

### 1. Trạng thái event
![CloudFormation Success](images/event.png)

### 2. Các tài nguyên đã được khởi tạo đúng theo cấu hình định nghĩa
![EC2 Running](/images/Reso.png)
---

### Kết quả đạt được tuần 3:
* **Bảo mật mạng:** Triển khai thành công Bastion Host để SSH an toàn vào vùng Private.
* **Tự động hóa:** Hệ thống tự động phản ứng với lưu lượng truy cập tăng đột biến qua Scaling Policies.
* **Kiến trúc 3-Tier:** Hoàn thiện mô hình hạ tầng chuẩn doanh nghiệp (Web - App - Database) với độ bảo mật cao nhất.


