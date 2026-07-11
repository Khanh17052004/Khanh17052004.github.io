---
title: "Nhật ký Tuần 7: Làm Chủ Hệ Sinh Thái Container Với Kubernetes & Amazon EKS"
date: 2024-01-01
weight: 1
chapter: false
pre: " <b> 1.7. </b> "
---
## Mục tiêu tuần 7:
Tuần này tập trung vào việc chuyển dịch hạ tầng ứng dụng sang kỷ nguyên Container Orchestration, làm quen với các khái niệm cốt lõi của **Kubernetes (K8s)** và thực hành triển khai, quản lý ứng dụng trên dịch vụ managed-cluster **Amazon EKS (Elastic Kubernetes Service)**:

1. **Nắm Vững Nền Tảng K8s (Kubernetes Architecture & Core Objects):** Hiểu rõ kiến trúc Master/Worker Node, cách thức hoạt động của Control Plane và làm chủ các đối tượng cơ bản cấu thành nên một ứng dụng như Pods, Deployments, Services.
2. **Xây Dựng Hạ Tầng EKS Chuẩn Doanh Nghiệp (Production-Ready EKS Cluster):** Tiếp cận tư duy thiết kế Cluster trên AWS, cấu hình IAM Roles for Service Accounts (IRSA), thiết lập VPC tối ưu cho EKS và khởi tạo Cluster thông qua công cụ chuyên dụng `eksctl` hoặc Terraform.
3. **Quản Lý Biến Thể & Triển Khai Ứng Dụng (Application Deployment & Networking):** Thực thi việc đóng gói, cấu hình lưu trữ, quản lý cấu hình (ConfigMap/Secret) và thiết lập các cơ chế định tuyến, phân phối tải (Load Balancing) cho ứng dụng chạy trong Cluster.
4. **Giám Sát & Vận Hành Hệ Thống (Kubernetes Observability):** Triển khai các giải pháp thu thập Metrics, Logs và giám sát trạng thái sức khỏe của Cluster cũng như của từng ứng dụng đang vận hành thực tế.
5. **Hiện Thực Hóa Dự Án Đóng Gói (Kubernetes Capstone Project):** Triển khai một dự án Microservices hoàn chỉnh lên Amazon EKS, áp dụng đầy đủ các kỹ thuật tự động co giãn, bảo mật lớp mạng và lưu trữ tài liệu kỹ thuật trên Blog Technical.

---

## Nhật Ký Lộ Trình Tuần 7

| Thứ | Công việc | Ngày BĐ | Ngày KT | Nguồn tài liệu |
| :--- | :--- | :--- | :--- | :--- |
| **02/06** | K8s Architecture & Fundamentals: Tìm hiểu kiến trúc tổng quan Kubernetes | 02/06/2026 | 02/06/2026 | [Kubernetes Concepts](https://kubernetes.io/docs/concepts/) |
| **03/06** | Core Objects (Pods, Deployments, Services): Khởi tạo và quản lý vòng đời tài nguyên | 03/06/2026 | 03/06/2026 | [K8s Workloads Guide](https://kubernetes.io/docs/concepts/workloads/) |
| **04/06** | Amazon EKS Overview & Architecture: Khám phá dịch vụ quản lý K8s của AWS | 04/06/2026 | 04/06/2026 | [Amazon EKS User Guide](https://docs.aws.amazon.com/eks/latest/userguide/what-is-eks.html) |
| **05/06** | Provisioning EKS Cluster: Thực hành triển khai Cluster EKS lên AWS | 05/06/2026 | 05/06/2026 | [EKS Cluster Creation](https://docs.aws.amazon.com/eks/latest/userguide/create-cluster.html) |
| **06/06** | Deploying Application on EKS: Đẩy microservices và expose traffic ra Internet | 06/06/2026 | 06/06/2026 | [AWS EKS App Deployment](https://docs.aws.amazon.com/eks/latest/userguide/sample-deployment.html) |
| **07/06** | EKS Monitoring & Logging: Cấu hình giám sát tài nguyên Cluster | 07/06/2026 | 07/06/2026 | [EKS Observability](https://docs.aws.amazon.com/eks/latest/userguide/eks-observe-logging.html) |
| **08/06** | Kubernetes Capstone Project & Documentation: Đóng gói dự án thực tế và viết blog | 08/06/2026 | 08/06/2026 | Tự triển khai / Hugo Portfolio Blog |

---
### Minh chứng thực tế:

## 1. Tên bài LAB
**Xây dựng Hệ thống Web chịu lỗi và Tự động Cân bằng tải trên nền tảng Amazon EKS (Elastic Kubernetes Service)**

**Mô hình kiến trúc hệ thống**
![](/images/mohinh.png)

---

## 2. Mục tiêu của bài Lab
* Hiểu và vận hành các thành phần cốt lõi của Kubernetes bao gồm: Pods, Deployments, và Services.
* Thực hành triển khai và quản trị một Cluster Kubernetes thực tế trên môi trường đám mây AWS thông qua công cụ `eksctl`.
* Cấu hình cơ chế tự động cân bằng tải (Load Balancer) để phân phối lưu lượng mạng ra ngoài Internet.
* Kích hoạt và sử dụng hệ thống giám sát hiệu năng (`metrics-server`) để theo dõi tài nguyên phần cứng (CPU/RAM) của hệ thống theo thời gian thực.

---

## 3. Các thành phần và Mục đích sử dụng

| Thành phần | Công nghệ / Dịch vụ | Mục đích sử dụng |
| :--- | :--- | :--- |
| **Control Plane** | Amazon EKS Cluster | Đóng vai trò bộ não trung tâm quản lý, điều phối và duy trì trạng thái mong muốn của toàn bộ Cluster. |
| **Worker Nodes** | AWS EC2 (t3.medium) | Các máy chủ ảo chịu trách nhiệm cung cấp tài nguyên phần cứng để chạy các ứng dụng. |
| **Workload** | Kubernetes Deployment | Quản lý vòng đời, trạng thái và duy trì số lượng bản sao (Replicas) ổn định cho các Pod Nginx. |
| **Application** | Pods (Nginx Container) | Nơi chứa mã nguồn và trực tiếp xử lý các yêu cầu HTTP từ người dùng. |
| **Networking** | Kubernetes Service (`LoadBalancer`) | Gọi dịch vụ mạng từ AWS để tạo bộ cân bằng tải Public, định tuyến traffic từ Internet tới các Pods nội bộ. |
| **Monitoring** | Kubernetes `metrics-server` | Thu thập thông số hiệu năng phần cứng giúp quản trị viên theo dõi sức khỏe của hệ thống. |

---

## 4. Hướng dẫn các bước triển khai chi tiết

### Bước 1: Khởi tạo Cluster Amazon EKS
Sử dụng công cụ `eksctl` trên Windows CMD để khởi tạo nhanh một hạ tầng hoàn chỉnh bao gồm VPC, Subnets, IAM Roles và 2 Worker Nodes:

        eksctl create cluster --name my-eks-cluster --region us-east-1 --nodegroup-name my-nodes --node-type t3.medium --nodes 2 --managed

Lưu ý: Quá trình khởi tạo hạ tầng tự động trên AWS sẽ diễn ra trong khoảng 15-20 phút.

Kiểm tra trạng thái sẵn sàng của các Node:

        kubectl get nodes

## Bước 2: Định nghĩa và Triển khai ứng dụng (Deployment)

Tạo file cấu hình app-deployment.yaml nhằm thiết lập cấu trúc chạy ứng dụng Web gồm 2 bản sao (Replicas) chạy song song:

        apiVersion: apps/v1
        kind: Deployment
        metadata:
        name: nginx-web-app
        labels:
            app: nginx
        spec:
        replicas: 2
        selector:
            matchLabels:
            app: nginx
        template:
            metadata:
            labels:
                app: nginx
            spec:
            containers:
            - name: nginx
                image: nginx:latest
                ports:
                - containerPort: 80
                resources:
                requests:
                    cpu: "100m"
                    memory: "128Mi"
                limits:
                    cpu: "200m"
                    memory: "256Mi"
Áp dụng cấu hình lên EKS Cluster và kiểm tra trạng thái của các Pods:

        
        kubectl apply -f app-deployment.yaml
        kubectl get pods -o wide
## Bước 3: Mở rộng dịch vụ ra Internet (Service LoadBalancer)

Khởi tạo file cấu hình mạng app-service.yaml để liên kết trực tiếp hạ tầng EKS với dịch vụ Load Balancing ngoại vi của AWS:

        apiVersion: v1
        kind: Service
        metadata:
        name: nginx-service
        spec:
        type: LoadBalancer
        selector:
            app: nginx
        ports:
            - protocol: TCP
            port: 80
            targetPort: 80

Triển khai cấu hình mạng và lấy địa chỉ DNS Public:

        kubectl apply -f app-service.yaml
        kubectl get svc nginx-service

Sao chép chuỗi ký tự tại cột EXTERNAL-IP và truy cập bằng trình duyệt web để kiểm tra kết quả.

![](/images/web.png)

## Bước 4: Thiết lập hệ thống Giám sát hiệu năng (Metrics)
Cài đặt cấu hình thu thập dữ liệu giám sát mặc định từ Kubernetes:


        kubectl apply -f [https://github.com/kubernetes-sigs/metrics-server/releases/latest/download/components.yaml](https://github.com/kubernetes-sigs/metrics-server/releases/latest/download/components.yaml)

 Do đặc thù xác thực chứng chỉ TLS trên môi trường Lab, tiến hành cập nhật (Patch) trực tiếp thuộc tính cấu hình cho metrics-server trên Windows CMD:


        kubectl patch deployment metrics-server -n kube-system --type="json" -p="[{\"op\": \"add\", \"path\": \"/spec/template/spec/containers/0/args/-\", \"value\": \"--kubelet-insecure-tls\"}]"

Đợi hệ thống ổn định và truy xuất thông số tài nguyên:

Xem lượng tài nguyên tiêu thụ của các máy chủ (Nodes):

        kubectl top nodes

![](/images/topnodes.png)

Xem lượng tài nguyên tiêu thụ của từng Pod ứng dụng:

        kubectl top pods
        
![](/images/toppods.png)

## Bước 5: DỌN DẸP TÀI NGUYÊN (XÓA LAB - TRÁNH MẤT TIỀN)
Vì Amazon EKS tính tiền theo giờ cho phần Control Plane (~$0.10/giờ) cộng thêm chi phí của 2 con EC2 t3.medium và Load Balancer, nên sau khi vọc vạch và hiểu bản chất, bạn bắt buộc phải xóa sạch Lab để tránh hóa đơn bất ngờ cuối tháng.

Chạy lệnh dọn dẹp tối thượng sau:

    eksctl delete cluster --name my-eks-cluster

## Kết quả đạt được tuần 7
Làm chủ công cụ CLI: Thành thạo các câu lệnh điều khiển hạ tầng qua eksctl và quản trị tài nguyên K8s nội bộ qua kubectl ngay trên môi trường Windows.

Xây dựng hạ tầng chịu lỗi cao (High Availability): Triển khai thành công ứng dụng Web chạy phân tán trên nhiều Worker Node, đảm bảo hệ thống vẫn hoạt động liên tục ngay cả khi có lỗi xảy ra ở một Pod riêng lẻ.

Tự động hóa kết nối đám mây: Hiểu rõ cơ chế tự động hóa liên kết khi Kubernetes Service tương tác trực tiếp với tài nguyên Network Load Balancer của AWS.

Khả năng quan sát (Observability): Khắc phục thành công các xung đột dữ liệu dòng lệnh trên hệ điều hành Windows, thiết lập cấu hình bỏ qua TLS thành công để đo lường chính xác các chỉ số tải CPU/RAM trong Cluster.


