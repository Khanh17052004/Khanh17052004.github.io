---
title: "Nhật ký Tuần 4: DevOps, Container & Serverless on AWS"
date: 2024-01-01
weight: 1
chapter: false
pre: " <b> 1.4. </b> "
---
## Mục Tiêu Tuần 4

Tuần này đánh dấu bước chuyển mình quan trọng từ tư duy hạ tầng truyền thống (VPC, EC2) sang tư duy **Hiện đại hóa ứng dụng (Application Modernization)** với các mục tiêu cốt lõi:
1. **Làm chủ Công nghệ Container:** Hiểu sâu về cách đóng gói ứng dụng bằng Docker, quản lý image trên Amazon ECR và vận hành chúng một cách linh hoạt trên Amazon ECS.
2. **Tự động hóa chuẩn DevOps:** Xây dựng hoàn chỉnh luồng CI/CD tự động (Continuous Integration/Continuous Delivery) bằng bộ công cụ AWS Code-suite để giảm thiểu thao tác thủ công, tăng tốc độ bàn giao sản phẩm.
3. **Tiếp cận Tư duy Serverless:** Phá bỏ giới hạn quản lý server truyền thống bằng cách dịch chuyển sang kiến trúc Serverless (AWS Lambda, API Gateway, DynamoDB) giúp hệ thống tự động co giãn mạnh mẽ theo lượng truy cập.
4. **Chuẩn hóa Kiến trúc:** Áp dụng các nguyên tắc của *AWS Well-Architected Framework* nhằm tối ưu hóa chi phí và đảm bảo tính bảo mật cho hệ thống Cloud Native.

## Nhật Ký Lộ Trình Tuần 4

| Thứ | Công việc | Ngày BĐ | Ngày KT | Nguồn tài liệu |
| :---: | :--- | :---: | :---: | :--- |
| **12/05** | Docker Fundamentals & Containerization: Hiểu container và đóng gói ứng dụng | 12/05/2026 | 12/05/2026 | [Docker Docs](https://docs.docker.com/) |
| **13/05** | Amazon ECS & ECR Workshop: Deploy container lên AWS | 13/05/2026 | 13/05/2026 | [AWS ECS Workshop](https://aws.amazon.com/ecs/) |
| **14/05** | CI/CD Pipeline với CodePipeline & CodeBuild: Tự động build và deploy | 14/05/2026 | 14/05/2026 | AWS CodeSuite |
| **15/05** | AWS Lambda & API Gateway: Làm quen Serverless Architecture | 15/05/2026 | 15/05/2026 | Serverless Lab |
| **16/05** | DynamoDB & S3 Integration: Xây dựng backend serverless cơ bản | 16/05/2026 | 16/05/2026 | AWS NoSQL |
| **17/05** | Cost Optimization & Well-Architected Review: Tối ưu chi phí và đánh giá kiến trúc | 17/05/2026 | 17/05/2026 | [AWS Well-Architected](https://aws.amazon.com/architecture/well-architected/) |
| **18/05** | Final Project Integration & Technical Blog: Tổng hợp và document toàn bộ hệ thống | 18/05/2026 | 18/05/2026 | Tự học |

---
### Minh chứng thực tế:
### 1.Docker Fundamentals & Containerization

**Mô hình kiến trúc hệ thống**
![](/images/mohinh.png)
**1. Mục tiêu của bài Lab**

Bài thực hành này giúp thiết lập tư duy cốt lõi về **Containerization** (Đóng gói ứng dụng), xóa bỏ sự phụ thuộc vào môi trường máy vật lý hoặc ảo hóa truyền thống nhằm đạt được các mục tiêu lớn sau:
- **Hiểu rõ bản chất ảo hóa:** Phân biệt rõ sự khác biệt kiến trúc giữa Virtual Machine (VM) và Container (Cách tối ưu tài nguyên, chia sẻ Kernel).
- **Làm chủ quy trình đóng gói:** Thành thạo vòng đời của một ứng dụng Container hóa: *Viết cấu hình (Dockerfile) -> Đóng gói (Image) -> Lưu trữ (Registry) -> Khởi chạy thực thi (Container)*.
- **Làm chủ cơ chế mạng cơ bản:** Hiểu sâu về cách thức ánh xạ cổng (**Port Mapping**) để kết nối lưu lượng từ bên ngoài vào ứng dụng cô lập bên trong Container.
- **Xây dựng bước đệm Cloud vững chắc:** Sẵn sàng về mặt tư duy để chuẩn bị dịch chuyển hệ thống lên các dịch vụ quản lý Container nâng cao trên AWS như **Amazon ECS** và **Amazon EKS (Kubernetes)**.

---

**2. Các thành phần và Mục đích sử dụng**

Trong bài Lab này, các công cụ và thành phần đóng vai trò như các viên gạch nền tảng, có mối liên hệ trực tiếp với hạ tầng Cloud sau này:

| Thành phần | Mục đích sử dụng trong Lab | Thành phần tương đương trên AWS |
| :--- | :--- | :--- |
| **Dockerfile** | File text chứa tập hợp các chỉ thị để tự động hóa việc dựng môi trường cho ứng dụng. | **Task Definition** (Trong ECS) |
| **Docker Image** | Khuôn mẫu đóng gói dạng chỉ đọc (Read-only), chứa toàn bộ mã nguồn, thư viện runtime để ứng dụng có thể chạy ở bất cứ đâu. | **ECR Image Layer** |
| **Local Registry** | Kho lưu trữ cục bộ (Private) tại máy Host để quản lý và phân phối các Docker Image tự build. | **Amazon ECR** (Elastic Container Registry) |
| **Docker Container** | Thực thể chạy thực tế (Runtime instance) được tách biệt hoàn toàn để ứng dụng thực thi. | **ECS Task** (Chạy trên hạ tầng Serverless **Fargate**) |
| **Port Mapping (`-p`)** | Cơ chế NAT/Forward cổng từ máy Host (`8080`) vào cổng nội bộ Container (`3000`) để có thể truy cập Web từ trình duyệt. | **Application Load Balancer (ALB)** kết hợp với **Target Group** |

---

**3. Hướng dẫn các bước triển khai chi tiết**

**Bước 1: Khởi động môi trường Docker Desktop**
1. Hãy chắc chắn rằng ứng dụng **Docker Desktop** đã được cài đặt và đang chạy trên máy tính của bạn (Biểu tượng cá voi có màu xanh lá cây hiển thị trạng thái `Running`).
2. Mở **PowerShell** (Windows) hoặc **Terminal** (Mac/Linux) và chạy lệnh sau để xác nhận Docker hoạt động:
   ```bash
   docker --version
**Bước 2: Tạo thư mục chứa bài Lab:**
Trong cửa sổ PowerShell hoặc Terminal đang mở, bạn copy và chạy từng lệnh sau:
    

    # Tạo một thư mục tên là docker-lab ở ổ đĩa hiện tại
    mkdir docker-lab

    # Di chuyển hẳn vào bên trong thư mục vừa tạo
    cd docker-lab

**Bước 3: Tạo mã nguồn Ứng dụng Web (server.js):**
Chúng ta cần tạo một file chứa code chạy web.

Cách làm nhanh nhất bằng lệnh:

Dùng Windows (PowerShell), chạy lệnh này:

    Set-Content -Path server.js -Value 'const http = require("http"); http.createServer((req, res) => { res.statusCode = 200; res.setHeader("Content-Type", "text/html; charset=utf-8"); res.end("<h1>🐳 Chao Khanh! Container hoat dong tot roi nhe!</h1>\n"); }).listen(3000, "0.0.0.0", () => { console.log("Server running on port 3000"); });'
**Bước 4: Tạo Bản Thiết Kế Đóng Gói (Dockerfile):**
Tương tự, tạo một file tên là Dockerfile (viết hoa chữ D, không có đuôi file) nằm chung thư mục này để chỉ dẫn Docker cách đóng gói:

Dùng Windows (PowerShell):

    Set-Content -Path Dockerfile -Value "FROM node:18-alpine`nWORKDIR /app`nCOPY server.js .`nEXPOSE 3000`nCMD [`"node`", `"server.js`"]"
**Bước 5: Build Docker Image**
Tại thư mục docker-lab, gõ lệnh sau để Docker đọc Dockerfile và tải môi trường về đóng gói thành một Image có tên là my-web:

    docker build -t my-web:v1 .

Bạn sẽ thấy một dòng có tên my-web kèm tag v1.
**Bước 6: Chạy Container và Map Port**
Bây giờ, ta sẽ biến Image "đóng băng" đó thành một Container chạy thực tế, đồng thời nối cổng 8080 của máy tính bạn vào cổng 3000 của container:

    docker run -d -p 8080:3000 --name web-container my-web:v1

Thành quả: Mở trình duyệt Web (Chrome/Edge/Firefox) trên máy tính của bạn lên, nhập vào thanh địa chỉ:
👉 http://localhost:8080

![](/images/container.png)

Nếu màn hình hiện lên dòng chữ: "🐳 Chao Khanh! Container hoat dong tot roi nhe!" là bạn đã containerize thành công ứng dụng đầu tiên!
**Bước 7:Đẩy Ứng Dụng Lên Kho Lưu Trữ Cục Bộ (Local Registry)**
Dựng một Registry thu nhỏ:
Chạy một container đóng vai trò làm kho lưu trữ, hoạt động tại cổng 5001 của máy bạn:

    docker run -d -p 5001:5000 --name my-registry registry:2

**Bước 8:Dán nhãn (Tag) lại Image để trỏ về kho chứa này**

    docker tag my-web:v1 localhost:5001/my-web:v1

**Bước 9:Đẩy (Push) Image vào kho**

    docker push localhost:5001/my-web:v1

**Bước 10:Kiểm tra kho lưu trữ:**

    curl http://localhost:5001/v2/_catalog

 ![](/images/catalog.png)


### 2.Amazon ECS & ECR Infrastructure

**Mô hình kiến trúc hệ thống**
![](/images/mohinh1.png)

**1. Mục tiêu của bài Lab (Workshop Objectives)**

Bài thực hành nâng cao này giúp chuyển đổi toàn bộ hạ tầng ứng dụng từ môi trường Local Lab lên nền tảng **Cloud Enterprise** của AWS theo kiến trúc **Production-style**, đạt được các mục tiêu cốt lõi sau:
- **Làm chủ Container Orchestration:** Hiểu cách AWS tự động hóa việc quản lý, giám sát và vận hành hàng loạt Container cùng lúc thông qua Amazon ECS thay vì chạy thủ công từng lệnh ở local.
- **Tiếp cận tư duy Serverless Container:** Sử dụng công nghệ **AWS Fargate** để chạy ứng dụng trực tiếp mà không cần tốn thời gian quản trị, cập nhật hệ điều hành cho các máy chủ ảo EC2 nền tảng bên dưới.
- **Thiết kế hệ thống High Availability (Sẵn sàng cao):** Cấu hình phân phối đều lượng truy cập và tự động nhân bản ứng dụng chạy song song trên nhiều vùng độc lập (**Availability Zones**).
- **Bảo mật hạ tầng mạng tuyệt đối:** Thực hành tách biệt luồng mạng cô lập hệ thống Core bên trong vùng mạng kín, chỉ mở một lối vào duy nhất được kiểm duyệt nghiêm ngặt qua cổng Load Balancer.

---

**2. Bản đồ dịch vụ & Mục đích sử dụng (AWS Services Architecture)**

Trong bài Lab này, mỗi dịch vụ của AWS đóng vai trò như một mắt xích không thể tách rời, kết nối chặt chẽ theo mô hình điều phối:

| Dịch vụ AWS | Mục đích sử dụng trong Lab | Bản chất công nghệ |
| :--- | :--- | :--- |
| **Amazon ECR** *(Elastic Container Registry)* | Kho lưu trữ Docker Image bảo mật trên Cloud. Thay thế cho Local Registry máy Host để quản lý phiên bản Image (`latest`). | **Private Docker Registry** |
| **Task Definition** | Bản thiết kế kỹ thuật (Blueprint) khai báo cho AWS biết Container cần dùng bao nhiêu CPU, RAM, lấy Image từ đâu trong ECR và mở cổng Port nào. | **Infrastructure as Code (Configuration)** |
| **Amazon ECS Cluster** *(Elastic Container Service)* | "Ngôi nhà" logic tập hợp toàn bộ hạ tầng phần cứng ảo hóa để làm nền tảng khởi chạy các Service và Container. | **Container Orchestrator Engine** |
| **AWS Fargate** | Chế độ chạy Serverless. Đứng ra lo liệu toàn bộ việc cấp phát phần cứng ngầm bên dưới giúp Kỹ sư chỉ tập trung vào Container ứng dụng. | **Serverless Compute Engine for Containers** |
| **Application Load Balancer (ALB)** | Bộ cân bằng tải công cộng nằm ở vùng Public Subnet. Nhận traffic từ Internet (Port 80), tự động phân phối đều lượng truy cập và map port vào các Container phía sau. | **Layer 7 Routing Gateway** |
| **Target Group** | Nhóm định danh mục tiêu. Làm cầu nối giúp ALB biết chính xác cần phải đẩy traffic tiếp nhận được vào cụm Container cụ thể nào qua Port mạng tương ứng (`3000`). | **Traffic Router Logical Group** |

---

**3. Nhật ký các bước triển khai chi tiết (Step-by-Step Implementation)**

**Bước 1: Khởi tạo Kho lưu trữ Image trên Amazon ECR:**
1. Truy cập vào **AWS Management Console**, tìm kiếm và chọn dịch vụ **Elastic Container Registry (ECR)**.
2. Tại menu bên trái, chọn **Repositories** nằm trong mục **Private registry** ➔ Bấm nút **Create repository**.
3. Cấu hình thông số:
   - **Visibility settings:** Mặc định là `Private` (Do đứng sẵn trong Private registry).
   - **Repository name:** Điền duy nhất chữ **`my-aws-web-app`** *(Lưu ý: Không dán cả đường dẫn URL tài khoản vào ô này để tránh lỗi ký tự đặc biệt)*.
4. Giữ nguyên tất cả các cài đặt nâng cao khác, cuộn xuống dưới cùng và bấm nút màu cam **Create repository**.

**Bước 2: Login AWS CLI và Đẩy Image từ Local lên AWS Cloud:**
**1. Khởi tạo danh tính AWS Credentials trên máy cá nhân:**
Để lấy Key, truy cập vào dịch vụ **IAM** ➔ Vào mục **Users** ➔ Chọn tên User đang dùng ➔ Chọn tab **Security credentials** ➔ Tìm đến mục **Access keys** ➔ Chọn **Create access key** ➔ Chọn **Command Line Interface (CLI)** và bấm tạo. Hãy tải file `.csv` chứa `Access key ID` và `Secret access key` về máy.
Mở **PowerShell** hoặc **Terminal** tại thư mục dự án chứa file `server.js` và `Dockerfile`, chạy lệnh:

    aws configure

Tiến hành dán các thông tin tương ứng:

- **AWS Access Key ID:** (Nhập chuỗi Access Key của bạn)

- **AWS Secret Access Key:** (Nhập chuỗi Secret Key của bạn)

- **Default region name:** Nhập us-east-1 (Vùng N. Virginia nơi tạo ECR).

- **Default output format:** Ấn Enter để bỏ qua.

**2. Đăng nhập Docker Client vào AWS ECR Registry:**
Truy cập vào giao diện Repo my-aws-web-app vừa tạo trên Console, bấm nút View push commands ở góc trên bên phải. Copy câu lệnh số 1 (Lệnh xác thực token rất dài) dán vào PowerShell chạy:

    aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin 210347900763.dkr.ecr.us-east-1.amazonaws.com
**3. Đóng gói, Gắn Tag và Push Image lên mây:**
Chạy tuần tự 3 câu lệnh còn lại trong bảng mẫu (thay mã ID tài khoản bằng mã thực tế của bạn):
    ```bash
    # Build Docker Image cục bộ
    docker build -t my-aws-web-app .

    # Gắn tag định danh trỏ thẳng về URL kho chứa AWS ECR
    docker tag my-aws-web-app:latest [210347900763.dkr.ecr.us-east-1.amazonaws.com/my-aws-web-app:latest](https://210347900763.dkr.ecr.us-east-1.amazonaws.com/my-aws-web-app:latest)

    # Đẩy dữ liệu Image lên Amazon ECR Cloud
    docker push [210347900763.dkr.ecr.us-east-1.amazonaws.com/my-aws-web-app:latest](https://210347900763.dkr.ecr.us-east-1.amazonaws.com/my-aws-web-app:latest)

Xác minh: Tắt bảng câu lệnh, nhấn F5 lại trang ECR Repo, thấy xuất hiện Image có tag latest kèm dung lượng cụ thể là thành công.

**Bước 3: Cấu hình Task Definition (Thiết kế Blueprint hệ thống)**

**1: Trên thanh tìm kiếm AWS Console, gõ ECS ➔ Chọn Elastic Container Service.**

**2: Tại menu bên trái, chọn Task Definitions ➔ Bấm nút màu cam Create new task definition (Chọn dòng đầu tiên dùng giao diện UI trực quan).**

**3: Thiết lập thông số:**

  - Task definition family: Đặt tên là web-app-task.

  - Launch type: Chọn ô AWS Fargate (Chế độ Serverless container).

  - Task size: Chọn mức cấu hình nhỏ nhất để tối ưu chi phí tài khoản: CPU: 0.25 vCPU, Memory: 0.5 GB.

**4: Cấu hình Container chính (Container details):**

  - Name: Gõ web-container.

  - Image URI: Dán chính xác đường dẫn URI của Image trong ECR đã tạo ở Bước 2 (210347900763.dkr.ecr.us-east-1.amazonaws.com/my-aws-web-app:latest).

  - Container port: Điền cổng 3000 (Vì đây là cổng mạng nội bộ mà mã nguồn file server.js Node.js đang lắng nghe).

  - Protocol: Giữ nguyên mặc định là HTTP.

**5: Kéo xuống cuối cùng trang và bấm nút màu cam Create.**

**Bước 4: Tạo ECS Cluster và Triển khai Service kèm bộ cân bằng tải Application Load Balancer (ALB)**
**1: Khởi tạo ECS Cluster**
  - Tại giao diện ECS, chọn mục Clusters ở menu trái ➔ Bấm nút Create cluster.

  - Cluster name: Đặt tên mới để tránh xung đột hệ thống mạng cũ, ví dụ: my-ecs-cluster-v2.

  - Infrastructure: Chỉ tích chọn duy nhất ô AWS Fargate (serverless).

  - Bấm Create. Hệ thống sẽ tự động tạo một Cluster rỗng ở trạng thái Active.

**2: Cấu hình Triển khai Dịch vụ (Create Service)**
  - Bấm trực tiếp vào tên Cluster mới tạo my-ecs-cluster-v2 ➔ Tại tab Services, bấm nút Create.

  - Cấu hình thông số theo các phân mục lớn:

    - Deployment configuration:

        Compute options: Tích chọn Launch type ➔ Chọn dòng FARGATE.

        Application type: Chọn Service.

        Task Definition: Chọn Family là web-app-task (Bản thiết kế tạo ở Bước 3) với Revision lớn nhất là 1 (LATEST).

        Service name: Điền tên dịch vụ quản lý là web-app-service.

        Desired tasks: Điền số 2 (AWS sẽ nhân bản chạy song song đồng thời 2 Container để gánh tải hệ thống mạng).

    - Networking (Mạng):

        VPC: Chọn VPC mặc định (Default VPC).

        Subnets: Bấm chọn tích vào 3 Subnet thuộc các Availability Zone khác nhau (us-east-1a, us-east-1b, us-east-1d). Tuyệt đối né các subnet có tên tiền tố 'RDS-Pvt-subnet' vì đó là vùng mạng kín chuyên dụng cho database, không có đường ra internet công cộng.

        Security group: Bấm Create a new security group ➔ Đặt tên là web-container-sg ➔ Cấu hình Inbound rules: Chọn Type: Custom TCP, Port range: 3000, Source: Anywhere-IPv4 (0.0.0.0/0) để cho phép Load Balancer định tuyến dữ liệu vào cổng container.

    - Load balancing (Mở lối truy cập công cộng):

        Load balancer type: Tích chọn ô Application Load Balancer (ALB).

        Load balancer: Chọn Create a new load balancer ➔ Điền tên vào ô trống phía dưới là my-web-alb.

        Container to load balance: Chọn đúng dòng web-container : 3000. Bạn click chuột trái trực tiếp vào dòng chữ xanh này để hệ thống bung toàn bộ các ô nhập liệu ẩn phía dưới ra.

        Listener: Chọn mục Create new listener ➔ Ô Port giữ nguyên mặc định là 80 (Giao thức HTTP tiêu chuẩn cho người dùng gõ link web từ Internet), Protocol giữ nguyên HTTP.

        Target group: Chọn mục Create new target group ➔ Ô Target group name điền tên: web-target-group ➔ Ô Port của Target group (đang hiện số 80) tiến hành sửa lại thành số 3000 (Để ALB chuyển tiếp traffic nhận từ port 80 truyền thẳng vào cổng 3000 của container). Mục Health check path giữ nguyên là /.

**3: Kéo xuống cuối cùng trang và bấm nút màu cam Create** Đợi 2-3 phút để hệ thống tự động thiết lập và kéo trạng thái Deployment về mức ổn định (Running).

**Bước 5: Kiểm tra Truy cập Công cộng (Test Public Access qua DNS)**
- Tìm kiếm và truy cập vào dịch vụ EC2 trên AWS Console ➔ Kéo menu bên trái xuống cuối cùng chọn mục Load Balancers nằm trong phân hệ Load Balancing.

- Nhấp chuột vào tên con Load Balancer vừa được sinh ra: my-web-alb.

- Tại tab Details phía dưới màn hình, tìm đến dòng thông tin DNS name (Đường dẫn URL công cộng dạng chuỗi dài có đuôi kết thúc bằng .amazonaws.com). Bấm biểu tượng sao chép bên cạnh.

- Mở một tab mới hoàn toàn trên trình duyệt, dán (Paste) đường link DNS này vào và ấn Enter (Không thêm bất kỳ đuôi cổng mạng nào phía sau).

- Thành quả thu được: Màn hình trình duyệt hiển thị rõ ràng nội dung chào mừng chứng minh toàn bộ hạ tầng đã thông suốt!

 ![](/images/hinhanh.png)

## 4. Xây dựng REST API không máy chủ (Serverless) bằng AWS Lambda và API Gateway


**Mô hình kiến trúc hệ thống**

![](/images/mohinhbaimoi.png)
---

**2. Mục tiêu bài lab**
- **Hiểu mô hình Event-Driven (Hướng sự kiện):** Cách hệ thống tự động kích hoạt xử lý dựa trên các sự kiện (HTTP Requests) từ phía người dùng mà không cần tài nguyên chạy liên tục.
- **Làm quen với Backend Serverless:** Trải nghiệm quy trình triển khai ứng dụng, logic tính toán mà không cần quan tâm đến việc quản lý, cài đặt hệ điều hành hay bảo trì máy chủ (EC2).
- **Cơ chế Tự động Giãn nở (Auto-scaling):** Hiểu cách AWS tự động quản lý tài nguyên, mở rộng số lượng thực thi song song (concurrency) để đáp ứng từ 1 đến hàng ngàn request đồng thời.
- **Giám sát hệ thống:** Sử dụng hệ thống log tập trung để debug và phân tích hành vi của mã nguồn xử lý.

---

**3. Các thành phần và Mục đích sử dụng**

| Thành phần | Mục đích sử dụng trong bài Lab |
| :--- | :--- |
| **AWS Lambda** | Đóng vai trò là bộ não xử lý (Backend). Chứa mã nguồn Python để tiếp nhận dữ liệu từ API Gateway, xử lý logic cấu trúc dữ liệu JSON và phản hồi kết quả về. |
| **AWS API Gateway** | Đóng vai trò là cổng kết nối công khai (API Endpoint). Tiếp nhận các HTTP Request (GET) từ Client ngoài Internet, chuyển hướng sự kiện vào Lambda và chuyển dữ liệu phản hồi ngược lại cho Client. |
| **AWS CloudWatch Logs** | Đóng vai trò giám sát và lưu trữ nhật ký hệ thống. Tự động ghi lại toàn bộ hoạt động thực thi, các lệnh `print()` debug từ Lambda để phục vụ quản trị và sửa lỗi. |

---

**4. Hướng dẫn các bước triển khai chi tiết**

**Bước 1: Tạo AWS Lambda Function**
1. Truy cập vào AWS Console, tìm và chọn dịch vụ **Lambda**.
2. Nhấn nút **Create function** với cấu hình:
   - Chọn **Author from scratch** (Tạo từ đầu).
   - **Function name:** `my-serverless-api-function`
   - **Runtime:** `Python 3.12`
   - **Architecture:** `x86_64`
   - **Permissions:** Giữ nguyên mặc định (*Create a new role with basic Lambda permissions*).
3. Nhấn **Create function**.
4. Tại tab **Code**, thay thế mã nguồn trong file `lambda_function.py` bằng đoạn code chuẩn hóa dưới đây:

        import json
        import datetime

        def lambda_handler(event, context):
            # Log lại event nhận được từ API Gateway để kiểm tra trong CloudWatch
            print("Received event: " + json.dumps(event, indent=2))
            
            # Tạo dữ liệu JSON để trả về cho Client
            response_body = {
                "status": "Success",
                "message": "Xin chào! Bạn đã gọi AWS Lambda thành công qua API Gateway.",
                "timestamp": datetime.datetime.utcnow().isoformat() + "Z",
                "author": "Silent Guardian of The Network"
            }
            
            # Cấu hình HTTP Response đúng chuẩn REST API
            response = {
                "statusCode": 200,
                "headers": {
                    "Content-Type": "application/json",
                    "Access-Control-Allow-Origin": "*" # Cho phép CORS nếu cần gọi từ Frontend khác
                },
                "body": json.dumps(response_body, ensure_ascii=False)
            }
            
            return response
5. Bấm nút Deploy để áp dụng mã nguồn mới.
**Bước 2: Khởi tạo và Cấu hình REST API trên API Gateway**
1. Truy cập dịch vụ **API Gateway** từ AWS Console.
2. Tại mục **REST API** (lưu ý: không chọn loại *Private*), nhấn nút **Build**.
3. Cấu hình các thông tin ban đầu bao gồm:
   - **Protocol:** `REST`
   - **Create new API:** `New API`
   - **API name:** `MyServerlessAPI`
   - **Endpoint Type:** `Regional`
4. Nhấn **Create API**.

**Tạo Resource mới:**
1. Tại danh mục tài nguyên bên trái, chọn thư mục gốc `/`, bấm **Create resource**.
2. **Cực kỳ quan trọng:** Tắt công tắc **Proxy resource** *(Lưu ý: Nút gạt phải ở trạng thái màu xám tắt, nếu vô tình bật xanh sẽ kích hoạt cấu hình greedy path và gây lỗi hệ thống)*.
3. Điền thông tin định tuyến:
   - **Resource name:** `hello`
4. Nhấn **Create resource**.

**Tạo Method mới:**
1. Chọn vào tài nguyên `/hello` vừa tạo thành công, nhấn **Create method**.
2. Thiết lập cấu hình method tích hợp:
   - **Method type:** Chọn `GET`.
   - **Integration type:** Chọn `Lambda Function`.
   - Bật tính năng **Lambda proxy integration** *(Bắt buộc bật để chuyển tiếp toàn bộ cấu trúc Request Event gốc vào hàm xử lý)*.
   - **Lambda function:** Điền chính xác tên hàm backend của bạn: `my-serverless-api-function`.
3. Nhấn **Create method** và xác nhận pop-up phân quyền (Grant Permission) cho phép API Gateway kích hoạt hàm Lambda.

---

**Bước 3: Deploy API ra Internet**
1. Tại giao diện quản trị của `MyServerlessAPI`, nhìn lên góc trên cùng bên phải và nhấn nút **Deploy API**.
2. Cấu hình môi trường phát hành tại bảng hiện lên:
   - **Stage:** Chọn `New stage`.
   - **Stage name:** Điền `dev` (hoặc `prod` tùy nhu cầu phân tách môi trường).
3. Nhấn **Deploy**. Hệ thống sẽ ngay lập tức cấp cho bạn một đường dẫn phân phối **Invoke URL** dạng: 
   `https://<your-api-id>.execute-api.<region>.amazonaws.com/dev`

> 💡 **Đường dẫn kiểm thử (Endpoint URL) hoàn chỉnh của bạn sẽ là:**

> `https://<your-api-id>.execute-api.<region>.amazonaws.com/dev/hello`

![](/images/ketquabaimoi.png)

**Bước 4: Kiểm tra Logging với CloudWatch**

Để thực sự hiểu sâu cách hệ thống Serverless vận hành ngầm và quản lý tài nguyên theo cơ chế Event-Driven, chúng ta sẽ truy cập vào CloudWatch để phân tích dữ liệu nhật ký (Logs).

1. Truy cập vào dịch vụ **CloudWatch** từ thanh tìm kiếm của AWS Console.
2. Tại danh mục menu bên trái, tìm đến mục **Logs** và chọn **Log groups**.
3. Tiến hành tìm kiếm và chọn đúng nhóm log được AWS tự động khởi tạo cho hàm: 
   `/aws/lambda/my-serverless-api-function`
4. Tại giao diện Log group, bạn sẽ thấy danh sách các **Log streams** *(mỗi stream này đại diện cho một vòng đời hoạt động hay còn gọi là một instance container của Lambda)*. Hãy chọn vào **Log stream mới nhất** ở trên cùng.

**Các thành phần Log quan trọng cần phân tích:**

* **Vạch kích hoạt hệ thống (`START`):**
  ```text
  START RequestId: c1b2e3f4-5678-abcd-ef01-23456789abcd Version: $LATEST

Dòng này đánh dấu thời điểm chính xác container Lambda được đánh thức (Trigger) để xử lý sự kiện từ API Gateway chuyển sang.
* **Dữ liệu Debug thực tế (`Received event`):**
    ```text
    Received event: { ... }

Đây chính là kết quả đầu ra của câu lệnh print() trong mã nguồn Python. Nó hiển thị toàn bộ cấu trúc định dạng JSON mà API Gateway bóc tách từ HTTP Request của Client (bao gồm: Request Headers, Query String Parameters, User-Agent, và Client IP).
* **Vạch kết thúc và Báo cáo tài nguyên (`END & REPORT`):**
    ```text
    END RequestId: c1b2e3f4-5678-abcd-ef01-23456789abcd
    REPORT RequestId: c1b2e3f4-5678-abcd-ef01-23456789abcd  Duration: 15.42 ms  Billed Duration: 16 ms  Memory Size: 128 MB  Max Memory Used: 64 MB

## 5. Xây dựng Ứng dụng Serverless: Tải ảnh lên S3 và Quản lý Metadata với DynamoDB

**Mô hình kiến trúc hệ thống**
![](/images/mohinhS3.png)

---

**2. Mục tiêu của bài Lab**
* **Hiểu kiến trúc Serverless hoàn chỉnh:** Nắm vững cách các dịch vụ Managed Services của AWS phối hợp hoạt động không cần quản trị server.
* **Làm quen với NoSQL Database:** Biết cách khởi tạo, cấu hình cấu trúc bảng và thực hiện các thao tác CRUD cơ bản trên Amazon DynamoDB.
* **Tích hợp Lưu trữ & Tính toán:** Thành thạo việc sử dụng AWS Lambda để xử lý logic, giải mã dữ liệu Base64, upload file lên Amazon S3 và đồng bộ dữ liệu metadata sang DynamoDB.
* **Phơi API bảo mật:** Biết cách cấu hình Amazon API Gateway (REST API) tích hợp Lambda Proxy để tiếp nhận các request từ phía Client.

---

**3. Các thành phần và Mục đích sử dụng**
| Thành phần (Dịch vụ) | Mục đích sử dụng trong bài Lab |
| :--- | :--- |
| **Amazon S3** | Lưu trữ đối tượng (Object Storage) - Dùng để lưu file hình ảnh nhị phân sau khi giải mã một cách an toàn và tối ưu chi phí. |
| **Amazon DynamoDB** | Cơ sở dữ liệu NoSQL - Dùng để lưu trữ thông tin có cấu trúc (Metadata) của ảnh như ID duy nhất, tên file, người upload, đường dẫn S3 URL và thời gian. |
| **AWS Lambda** | Tính toán Serverless (FaaS) - Chứa mã nguồn xử lý logic cốt lõi: nhận dữ liệu từ API Gateway, giải mã Base64, kết nối và ghi dữ liệu đồng thời vào cả S3 và DynamoDB. |
| **Amazon API Gateway** | Quản lý API - Đóng vai trò là cửa ngõ (Endpoint HTTP POST) tiếp nhận payload dữ liệu JSON từ Client và trigger Lambda thực thi. |
| **AWS IAM (Role)** | Ủy quyền và Bảo mật - Cấp quyền thực thi (`Execution Role`) cho phép Lambda có đủ đặc quyền ghi file vào S3 và cập nhật item vào DynamoDB. |

---

**4. Hướng dẫn các bước triển khai chi tiết**

**Bước 1: Tạo Amazon S3 Bucket**
1. Đăng nhập vào AWS Management Console, tìm kiếm và chọn dịch vụ **S3**.
2. Click chọn **Create bucket**.
3. Cấu hình các thông tin:
   * **Bucket name:** Nhập tên duy nhất trên toàn cầu (Ví dụ: `my-serverless-images-bucket-khanh`).
   * **AWS Region:** Chọn vùng gần bạn nhất (Ví dụ: `ap-southeast-1` Singapore hoặc `us-east-1` N. Virginia).
4. Các cấu hình khác (Block Public Access, Encryption) giữ mặc định.
5. Click **Create bucket** ở cuối trang.

**Bước 2: Tạo Amazon DynamoDB Table**
1. Tìm kiếm và chọn dịch vụ **DynamoDB** trên AWS Console.
2. Click chọn **Create table**.
3. Cấu hình các thông số:
   * **Table name:** Điền chính xác `ImageMetadata`.
   * **Partition key:** Điền `image_id` và chọn kiểu dữ liệu là **String**.
4. Tại mục **Table settings**, giữ nguyên tùy chọn **Default settings**.
5. Click **Create table**.

**Bước 3: Tạo IAM Execution Role cho AWS Lambda**
1. Tìm kiếm và mở dịch vụ **IAM (Identity and Access Management)**.
2. Chọn mục **Roles** ở menu bên trái, sau đó click **Create role**.
3. **Trusted entity type:** Chọn **AWS service**.
4. **Service or use case:** Chọn **Lambda** từ danh sách dropdown $\\rightarrow$ Nhấn **Next**.
5. Tại trang **Add permissions**, tìm kiếm và tích chọn các Policy sẵn có sau để cấp quyền nhanh:
   * `AWSLambdaBasicExecutionRole` (Quyền ghi log hệ thống ra CloudWatch).
   * `AmazonS3FullAccess` (Quyền thao tác dữ liệu với S3 Bucket).
   * `AmazonDynamoDBFullAccess` (Quyền đọc/ghi dữ liệu vào DynamoDB Table).
6. Nhấn **Next**.
7. **Role name:** Đặt tên gợi nhớ như `Lambda-S3-DynamoDB-Role`.
8. Kiểm tra lại thông tin và nhấn **Create role**.

**Bước 4: Tạo và cấu hình AWS Lambda Function**
1. Tìm kiếm và chọn dịch vụ **Lambda**.
2. Click **Create function** và cấu hình:
   * Chọn mục **Author from scratch**.
   * **Function name:** Điền `ImageUploadHandler`.
   * **Runtime:** Chọn **Python 3.12** (hoặc phiên bản Python mới nhất).
   * **Permissions:** Mở rộng phần *Change default execution role*, tích chọn **Use an existing role**, sau đó tìm chọn đúng role `Lambda-S3-DynamoDB-Role` đã tạo ở Bước 3.
3. Kéo xuống góc dưới cùng, mở rộng mục **> Additional settings** (Đảm bảo *không* bật Proxy resource tại đây) $\\rightarrow$ Nhấn **Create function**.
4. Tại tab **Code source**, thay thế toàn bộ mã nguồn mặc định bằng đoạn code Python dưới đây:
        ```python
        import json
        import boto3
        import base64
        import uuid
        import os
        from datetime import datetime

        # Khởi tạo các AWS SDK clients
        s3 = boto3.client('s3')
        dynamodb = boto3.resource('dynamodb')

        # Lấy cấu hình tên S3 và DynamoDB từ Environment Variables
        BUCKET_NAME = os.environ.get('BUCKET_NAME')
        TABLE_NAME = os.environ.get('TABLE_NAME')
        table = dynamodb.Table(TABLE_NAME)

        def lambda_handler(event, context):
            try:
                # 1. Tiếp nhận và Parse dữ liệu từ API Gateway Proxy Integration
                body = json.loads(event.get('body', '{}')) if isinstance(event.get('body'), str) else event.get('body', {})
                
                image_base64 = body.get('image_data')
                file_name = body.get('file_name', 'unnamed.jpg')
                uploader = body.get('uploader', 'anonymous')
                
                if not image_base64:
                    return {
                        'statusCode': 400,
                        'body': json.dumps({'message': 'Missing image_data in request body'})
                    }
                    
                # 2. Giải mã chuỗi Base64 sang định dạng Binary nhị phân
                image_binary = base64.b64decode(image_base64)
                
                # 3. Tạo định danh UUID duy nhất cho ảnh và cấu hình Object Key trên S3
                image_id = str(uuid.uuid4())
                s3_key = f"uploads/{image_id}_{file_name}"
                
                # 4. Thực hiện Put Object ghi dữ liệu lên S3 Bucket
                s3.put_object(
                    Bucket=BUCKET_NAME,
                    Key=s3_key,
                    Body=image_binary,
                    ContentType='image/jpeg'
                )
                
                # Tạo đường dẫn S3 URL truy cập ảnh công khai (hoặc phục vụ map dữ liệu)
                image_url = f"https://{BUCKET_NAME}[.s3.amazonaws.com/](https://.s3.amazonaws.com/){s3_key}"
                
                # 5. Thực hiện Put Item thêm bản ghi Metadata vào DynamoDB Table (CRUD - Create)
                timestamp = datetime.utcnow().isoformat()
                metadata_item = {
                    'image_id': image_id,
                    'file_name': file_name,
                    'uploader': uploader,
                    's3_url': image_url,
                    'uploaded_at': timestamp
                }
                table.put_item(Item=metadata_item)
                
                # 6. Trả lời phản hồi (Response) chuẩn HTTP về cho API Gateway Client
                return {
                    'statusCode': 201,
                    'headers': {
                        'Content-Type': 'application/json',
                        'Access-Control-Allow-Origin': '*'
                    },
                    'body': json.dumps({
                        'message': 'Upload thành công!',
                        'image_id': image_id,
                        's3_url': image_url
                    }, ensure_ascii=False)
                }
                
            except Exception as e:
                print(f"Error details: {str(e)}")
                return {
                    'statusCode': 500,
                    'body': json.dumps({'message': 'Internal Server Error', 'error': str(e)})
                }


5. Nhấn nút **Deploy** (thanh màu xám nằm ngay cạnh nút *Test*) để cập nhật cấu hình và áp dụng code mới cho hàm Lambda.
6. **Cấu hình Biến môi trường (Environment variables):**
   * Chuyển sang tab **Configuration** từ thanh menu phía trên của Function.
   * Chọn mục **Environment variables** ở menu danh sách bên trái.
   * Nhấn nút **Edit** và tiến hành thêm 2 biến môi trường sau:
     * **`BUCKET_NAME`**: Điền chính xác tên S3 Bucket bạn đã tạo ở Bước 1 (Ví dụ: `my-serverless-images-bucket-khanh`).
     * **`TABLE_NAME`**: Điền chính xác tên bảng DynamoDB là `ImageMetadata`.
   * Nhấn **Save** để hoàn tất.

---

**Bước 5: Cấu hình Amazon API Gateway làm Endpoint tiếp nhận Request**

1. Tìm kiếm và mở dịch vụ **API Gateway** trên thanh tìm kiếm AWS Console.
2. Tại mục **REST API** (Lưu ý: *Không chọn bản REST API Private*), nhấp chọn nút **Build**.
3. Chọn tùy chọn **New API** và điền thông tin:
   * **API name:** `ServerlessImageAPI`
   * **Endpoint Type:** Chọn **Regional**
4. Nhấn **Create API**.

**Tạo Resource `/upload`**
1. Tại giao diện quản lý API vừa tạo, nhấn nút **Create resource**.
2. > ⚠️ **Lưu ý quan trọng:** Hãy **TẮT** nút gạt **Proxy resource** (đảm bảo nút chuyển sang màu xám) để tránh bị lỗi định tuyến tham số greed-path.
3. Cấu hình thông tin Resource:
   * **Resource name:** Nhập `upload`
   * **Resource path:** Hệ thống sẽ tự động điền và map thành `/upload`
4. Tích chọn mục **CORS (Cross-Origin Resource Sharing)** để tránh lỗi chặn domain khi gọi từ client bên ngoài.
5. Nhấn **Create resource**.

**Tạo Method `POST` và tích hợp Lambda**
1. Click chọn vào dòng resource `/upload` vừa tạo, nhấn nút **Create method**.
2. Cấu hình thông tin Method:
   * **Method type:** Chọn `POST`
   * **Integration type:** Chọn `Lambda Function`
   * 🟢 **BẬT** nút gạt **Lambda proxy integration** lên (Chuyển sang màu xanh). *Mục này là bắt buộc để API Gateway truyền đầy đủ dữ liệu HTTP Event (Header, Body) vào cho hàm Lambda xử lý.*
   * **Lambda function:** Chọn đúng vùng (Region) và tìm chọn hàm `ImageUploadHandler` mà bạn đã tạo ở bước trước.
3. Nhấn **Create method**.

**Triển khai (Deploy) API ra môi trường**
1. Nhấn nút **Deploy API** ở góc trên bên phải giao diện quản lý.
2. Cấu hình thông tin Deployment:
   * **Stage:** Chọn `*New stage*`
   * **Stage name:** Nhập `prod`
3. Nhấn nút **Deploy**.
4. Sau khi deploy thành công, hệ thống sẽ hiển thị mục **Invoke URL** có định dạng cấu trúc như sau:
   ```text
   https://[api-id].execute-api.[region][.amazonaws.com/prod](https://.amazonaws.com/prod)


![](/images/minhchungS3.png)

## Kết quả đạt được tuần 4

Kết thúc Tuần 4, hệ thống kỹ năng và sản phẩm thực tế đã được nâng cấp toàn diện:
* **Đóng gói & Triển khai Container thành công:** Thành thạo viết `Dockerfile`, quản lý repository trên **Amazon ECR**, và cấu hình Task Definitions/Services để chạy ứng dụng ổn định trên **Amazon ECS**.
* **Vận hành Pipeline Tự động hóa:** Thiết lập thành công luồng **AWS CodePipeline** liên kết với GitHub. Mỗi khi commit code mới, **CodeBuild** sẽ tự động kích hoạt để build Docker image và deploy trực tiếp lên ECS mà không cần can thiệp thủ công.
* **Xây dựng Hệ thống Serverless hoàn chỉnh:** Triển khai thành công một API Backend hoạt động theo mô hình hướng sự kiện (Event-driven): Tiếp nhận request qua **API Gateway**, xử lý logic thông qua **AWS Lambda**, lưu trữ dữ liệu phi cấu trúc vào **Amazon DynamoDB** và quản lý static files trên **Amazon S3**.
* **Tối ưu chi phí & Quản trị hệ thống:** Biết cách phân tích các chỉ số chi phí, áp dụng chiến lược tối ưu hóa tài nguyên cho Container/Serverless và hoàn thành bài viết kỹ thuật (Technical Blog) chất lượng cao để chia sẻ lại kiến thức.


