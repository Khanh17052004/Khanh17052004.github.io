---
title: "Nhật ký Tuần 8: Triển Khai Hệ Thống DevSecOps & Cloud Security"
date: 2024-01-01
weight: 1
chapter: false
pre: " <b> 1.8. </b> "
---
## Mục tiêu tuần 8:
Tuần này tập trung vào việc áp dụng tư duy bảo mật toàn diện vào chu trình phát triển phần mềm và hạ tầng đám mây, chuyển dịch từ mô hình bảo mật truyền thống sang kiểm tra an ninh tự động hóa (Shift-Left Security):

1. **Hiểu Bảo Mật Trên AWS Theo Thực Tế Doanh Nghiệp:** Nắm vững mô hình trách nhiệm chung (Shared Responsibility Model), các tầng phòng thủ chuyên sâu (Defense in Depth) và thực thi nghiêm ngặt nguyên tắc phân quyền tối thiểu (Least Privilege) trên AWS IAM.
2. **Tự Động Hóa Kiểm Tra Bảo Mật:** Tích hợp các công cụ quét lỗ hổng mã nguồn (SAST), quét cấu hình sai hạ tầng (IaC Scanning) và cấu hình dịch vụ tự động rà quét mã độc, lỗ hổng hệ thống trên môi trường AWS AWS.
3. **Xây Dựng Hệ Thống DevSecOps Cơ Bản:** Đóng gói quy trình kiểm tra an ninh vào pipeline CI/CD tự động, đảm bảo mọi thay đổi về code và hạ tầng đều được kiểm duyệt lỗ hổng tự động trước khi triển khai lên production.

---

## Nhật Ký Lộ Trình Tuần 8

| Thứ | Công việc | Ngày BĐ | Ngày KT | Nguồn tài liệu |
| :--- | :--- | :--- | :--- | :--- |
| **09/06** | AWS Security Fundamentals: Tiếp cận các nguyên tắc bảo mật cốt lõi trên đám mây | 09/06/2026 | 09/06/2026 | [AWS Cloud Security Docs](https://aws.amazon.com/security/) |
| **10/06** | IAM Advanced & Least Privilege: Tối ưu hóa chính sách phân quyền chi tiết | 10/06/2026 | 10/06/2026 | [AWS IAM Security Practices](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html) |
| **11/06** | Secrets Management: Quản lý và bảo mật thông tin nhạy cảm tập trung | 11/06/2026 | 11/06/2026 | [AWS Secrets Manager Guide](https://docs.aws.amazon.com/secretsmanager/latest/userguide/intro.html) |
| **12/06** | Vulnerability Scanning: Sử dụng công cụ quét lỗ hổng mã nguồn và container | 12/06/2026 | 12/06/2026 | [Trivy Vulnerability Scanner](https://aquasecurity.github.io/trivy/latest/) |
| **13/06** | AWS Inspector & Security Hub: Quản lý rủi ro và tập trung hóa cảnh báo an ninh | 13/06/2026 | 13/06/2026 | [AWS Security Hub User Guide](https://docs.aws.amazon.com/securityhub/latest/userguide/what-is-securityhub.html) |
| **14/06** | DevSecOps Pipeline Integration: Tích hợp các bước kiểm tra bảo mật tự động vào CI/CD | 14/06/2026 | 14/06/2026 | [AWS DevSecOps Architecture](https://aws.amazon.com/blogs/devops/building-a-secure-devsecops-pipeline-on-aws/) |

---
### Minh chứng thực tế:

## 1. Xây Dựng Pipeline DevSecOps Tự Động Hóa Quét Lỗ Hổng Và Quản Lý Thông Tin Mật Trên AWS

**Mô hình kiến trúc hệ thống**
![](/images/mohinh.png)

---

## 2. Mục tiêu của bài Lab
* **Tự động hóa an ninh (Security Automation):** Tích hợp công cụ Trivy vào GitHub Actions để tự động quét lỗ hổng mã nguồn (Filesystem Scan) trước giai đoạn triển khai.
* **Xác thực phi tập trung (Keyless Authentication):** Loại bỏ hoàn toàn AWS Access Keys cố định, thay thế bằng cơ chế định danh OpenID Connect (OIDC) giữa GitHub và AWS.
* **Bảo mật dữ liệu nhạy cảm (Secrets Management):** Triển khai kiến trúc Zero-Hardcode bằng cách quản lý tập trung thông tin mật tại AWS Secrets Manager.
* **Giám sát an ninh liên tục (Continuous Monitoring):** Thiết lập hệ thống quét lỗ hổng thực thi (Runtime) trên EC2 bằng AWS Inspector và tập trung hóa dữ liệu cảnh báo tại AWS Security Hub.

---

## 3. Các thành phần và Mục đích sử dụng

| Thành phần | Phân loại | Mục đích sử dụng trong Lab |
| :--- | :--- | :--- |
| **GitHub Actions** | CI/CD Pipeline | Điều phối toàn bộ quy trình từ quét mã nguồn đến kích hoạt deploy. |
| **Trivy Scanner** | SCA Tool | Quét tĩnh toàn bộ mã nguồn (Kiểu `fs`) để phát hiện CVE của package phụ thuộc. |
| **AWS IAM OIDC** | Identity Governance | Cấp quyền ngắn hạn (AssumeRoleWithWebIdentity) cho GitHub Actions. |
| **AWS Secrets Manager** | Data Security | Lưu trữ và mã hóa chuỗi kết nối Database (`prod/app/db_creds`). |
| **Amazon EC2** | Compute Node | Máy chủ chạy ứng dụng Web (Sử dụng Amazon Linux 2023). |
| **AWS Inspector** | Vulnerability Management | Quét liên tục các lỗ hổng mạng (Network Reachability) trên EC2. |
| **AWS Security Hub** | SIEM / Dashboard | Tập trung toàn bộ phát hiện bảo mật từ AWS Inspector về một giao diện quản trị. |

---

## 4. Hướng dẫn các bước triển khai chi tiết

### Bước 4.1: Khởi tạo Kho mật an toàn (AWS Secrets Manager)
1. Truy cập **Secrets Manager Console** -> Chọn **Store a new secret**.
2. Chọn loại secret: `Other type of secret`.
3. Điền cấu hình Key/Value:
   * **Key:** `db_password`
   * **Value:** `SuperSecretPassword2026!`
4. Đặt tên Secret: `prod/app/db_creds` và tiến hành lưu lại chuỗi **Secret ARN**.

### Bước 4.2: Cấu hình Phân quyền nghiêm ngặt (AWS IAM & OIDC)
1. Tại **IAM Console** -> **Identity Providers**, thêm mới một Provider:
   * **Provider URL:** `https://token.actions.githubusercontent.com`
   * **Audience:** `sts.amazonaws.com`
2. Tạo IAM Role đặt tên là `GitHubActionsWorkflowRole`, cấu hình **Trust Relationship** giới hạn quyền cho chính xác kho lưu trữ:
   ```json
        {
            "Version": "2012-10-17",
            "Statement": [
            {
                "Effect": "Allow",
                "Principal": {
                "Federated": "arn:aws:iam::210347900763:oidc-provider/token.actions.githubusercontent.com"
                },
                "Action": "sts:AssumeRoleWithWebIdentity",
                "Condition": {
                "StringEquals": { "token.actions.githubusercontent.com:aud": "sts.amazonaws.com" },
                "StringLike": { "token.actions.githubusercontent.com:sub": "repo:Khanh17052004/devsecops-lab:*" }
                }
            }
            ]
        }

## 1. Gán Inline Policy GitHubDeployEC2Policy giới hạn quyền thao tác hạ tầng cho Role trên:

JSON
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": ["ec2:DescribeInstances", "ec2:StartInstances", "ec2:StopInstances"],
            "Resource": "*"
        }
    ]
}
## 2. Tạo IAM Instance Profile đặt tên là EC2AppRole, đính kèm policy AmazonSSMManagedInstanceCore và quyền đọc secret:

JSON
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": "secretsmanager:GetSecretValue",
            "Resource": "arn:aws:secretsmanager:us-east-1:210347900763:secret:prod/app/db_creds-xxxxxx"
        }
    ]
}
## Bước 4.3: Triển khai Hạ tầng Máy chủ (Amazon EC2)
Khởi tạo một Instance với thông số:

AMI: Amazon Linux 2023 (Free Tier).

Instance Type: t2.micro.

Key Pair: Proceed without a key pair (Quản trị bảo mật hoàn toàn qua Systems Manager).

Security Group: Chỉ mở duy nhất cổng 80 (HTTP) từ nguồn 0.0.0.0/0.

IAM Instance Profile: Gán Role EC2AppRole.

## Bước 4.4: Xây dựng File cấu hình DevSecOps Pipeline
Tạo file .github/workflows/devsecops-pipeline.yml trong mã nguồn:

        YAML
        name: DevSecOps CI/CD Pipeline

        on:
        push:
            branches: [ "main" ]

        permissions:
        id-token: write
        contents: read

        jobs:
        Security-Scan:
            runs-on: ubuntu-latest
            steps:
            - name: Checkout Code
                uses: actions/checkout@v3

            - name: Run Trivy vulnerability scanner (Repo Scan)
                uses: aquasecurity/trivy-action@master
                with:
                scan-type: 'fs'
                scan-ref: '.'
                format: 'table'
                exit-code: '0'
                severity: 'CRITICAL,HIGH'

        Deploy-to-AWS:
            needs: Security-Scan
            runs-on: ubuntu-latest
            steps:
            - name: Checkout Code
                uses: actions/checkout@v3

            - name: Configure AWS Credentials
                uses: aws-actions/configure-aws-credentials@v2
                with:
                role-to-assume: ${{ secrets.AWS_ROLE_ARN }}
                aws-region: us-east-1

            - name: Deploy Application
                run: |
                echo "Pipeline đã quét bảo mật thành công và đang thực hiện Deploy an toàn lên EC2..."

![](/images/Dashboard.png)

Hơn 20+ phát hiện (Findings) từ nhiều dịch vụ khác nhau.

![](/images/port22.png)

Lỗi 1: Port 22 is reachable from an Internet Gateway - TCP (Severity: Medium)

Nguyên nhân: Cổng 22 (Dùng để SSH quản trị máy chủ) hiện đang mở công khai ra toàn bộ Internet. Điều này khiến hacker có thể dò quét và tấn công brute-force mật khẩu.

![](/images/port80.png)

Lỗi 2: Port 80 is reachable from an Internet Gateway - TCP (Severity: Low)

Nguyên nhân: Cổng 80 (Giao thức HTTP thông thường) đang mở ra Internet. Lỗi này ở mức thấp (Low) vì ứng dụng Web bắt buộc phải mở cổng này để người dùng truy cập, tuy nhiên trong thực tế doanh nghiệp sẽ cần nâng cấp lên HTTPS (Port 443).

## Kết quả đạt được tuần 8

Hệ thống đã vận hành chính xác và xuất ra các chỉ số an ninh hạ tầng thực tế sau:

### 1. Chỉ số quét mã nguồn tĩnh (Trivy Scan Metric)
* Trạng thái tiến trình: Hoàn thành thành công (Success) trong thời gian 12 giây.
* Kết quả kiểm tra: Không phát hiện lỗi bảo mật trong mã nguồn tĩnh và mã nguồn sạch hoàn toàn, không chứa thông tin mật bị lộ (Zero Hardcoded Secrets). Toàn bộ dữ liệu nhạy cảm được phân tách sang AWS Secrets Manager.

### 2. Chỉ số rủi ro mạng hạ tầng (AWS Inspector Metrics)
Hệ thống giám sát runtime phát hiện hai lỗ hổng cấu hình mạng đang hoạt động trên máy chủ i-0b7a205925e2cd0b7 bao gồm:
* Lỗi cấu hình cổng 22 (Mức độ Medium): Cổng 22 đang mở công khai ra Internet Gateway. Khuyến nghị đóng hoàn toàn cổng này và chuyển sang quản trị qua AWS Systems Manager.
* Lỗi cấu hình cổng 80 (Mức độ Low): Cổng 80 mở công khai ra Internet Gateway. Đây là rủi ro chấp nhận được đối với ứng dụng web public, khuyến nghị nâng cấp lên HTTPS thông qua ALB trong tương lai.

### 3. Chỉ số quản trị tập trung (AWS Security Hub Insights)
* Hệ thống ghi nhận tổng cộng hơn 20 phát hiện bảo mật tự động đổ về Dashboard tập trung.
* Thống kê lỗi cấu hình sai (Misconfigurations) bao gồm: 8 lỗi Critical, 9 lỗi High, 14 lỗi Medium và 4 lỗi Low. Các lỗi trọng điểm liên quan đến việc chưa chặn quyền truy cập công khai của EBS Snapshot và việc sử dụng tài khoản Root vượt ngưỡng quy định an toàn doanh nghiệp.
