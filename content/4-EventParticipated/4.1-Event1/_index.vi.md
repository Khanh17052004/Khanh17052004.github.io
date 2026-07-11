---
title: "Event 1"
date: 2024-01-01
weight: 1
chapter: false
pre: " <b> 4.1. </b> "
---

# FCAJ Community Day 2026

## Tên sự kiện
* **FCAJ Community Day** (Diễn ra vào thứ Bảy, ngày 23/05/2026).

## Mục Đích Của Sự Kiện
* Tạo không gian kết nối, giao lưu và truyền cảm hứng giữa các thành viên trong cộng đồng công nghệ.
* Chia sẻ các kiến thức thực tế, xu hướng mới và kinh nghiệm "thực chiến" từ các chuyên gia trong lĩnh vực Điện toán đám mây (Cloud), Trí tuệ nhân tạo (AI), DevOps và Platform Engineering.
* Định hướng tư duy, kỹ năng cần thiết cho nhân sự ngành IT (đặc biệt là sinh viên, thực tập sinh) trước những biến động nhanh chóng của thị trường lao động toàn cầu và Việt Nam.

## Danh Sách Diễn Giả
1. **Anh Nguyễn Gia Hưng**: Solution Architect tại AWS Việt Nam, đồng thời là Người sáng lập FCAJ.
2. **Anh Tịnh Trương**: Platform Engineer tại Gotamic (Gotam X).
3. **Anh Hải Anh**: Diễn giả trẻ (23 tuổi), làm việc tại Pacific Việt Nam.
4. **Anh Nguyễn Hấn Thịnh**: DevOps Engineer.
5. **Chị Uyển & Chị Thảo**: Các kỹ sư công nghệ/đồng nghiệp, đại diện nhóm đạt giải Winner tại cuộc thi Hackathon *Los H* (Dự án UTMMorpho).
6. **Chuyên gia từ VPBank**: Chuyên gia có 2 năm kinh nghiệm thực chiến trong mảng Platform và AI tại VPBank.

## Nội Dung Nổi Bật
Sự kiện bao gồm 6 phiên chia sẻ xoay quanh các chủ đề cốt lõi:

* **Bức tranh thị trường & Xu hướng AI (Anh Nguyễn Gia Hưng):** Khi AI giúp phát triển phần mềm rẻ hơn, nhu cầu xây dựng phần mềm sẽ bùng nổ. Xuất hiện các luồng công việc mới như sửa lỗi code do AI tạo ra (AI code fixing) và Platform Engineering để vận hành hệ thống lớn.
* **Tầm quan trọng của Ngữ cảnh trong AI (Anh Tịnh Trương):** Làm rõ khái niệm "Context trong AI" và hiện tượng *Internet Buller* (lạm dụng kéo tài nguyên bừa bãi trên mạng làm nhiễu AI). Muốn AI chạy chính xác, cần cung cấp đúng và đủ ngữ cảnh riêng của dự án thay vì nhồi nhét prompt quá dài hay cung cấp kiến thức chung chung.
* **Amazon Q Apps & Agent (Anh Hải Anh):** Demo cách biến dữ liệu thô (như Excel) thành bảng phân tích BI tự động và xây dựng các con Agent thông minh có thể thực hiện hành động (như tóm tắt cuộc họp và tự gửi email).
* **Cơ chế Flat Rate Pricing của AWS CloudFront (Anh Nguyễn Hấn Thịnh):** Giải pháp giúp doanh nghiệp tránh hiện tượng "Bill Spike" (chi phí tăng vọt sau một đêm do bị DDoS hoặc tăng traffic đột biến). CloudFront giúp nén dữ liệu đến 82%, giảm tải CPU cho EC2 và tăng cường bảo mật (với mTLS, VPC Origin).
* **Hành trình 36 giờ giành giải Hackathon (Chị Uyển & Chị Thảo):** Chia sẻ cách xây dựng ứng dụng *UTMMorpho* (AI sinh giao diện HTML/CSS từ hình ảnh và cho phép kéo thả, chỉnh sửa trực tiếp) trong tình trạng kiệt sức, cạn kiệt token.
* **Hệ thống Multi-Agent chuẩn Doanh nghiệp (Diễn giả VPBank):** Phân tích nghiệp vụ đánh giá tín dụng cho Startup. Cách thiết kế hệ thống nhiều Agent chuyên biệt (Tài chính, Thị trường, Đội ngũ, Rủi ro) phối hợp dưới sự điều phối của một Orchestrator để giải quyết bài toán phức tạp mà một Agent đơn lẻ không làm được.

## Những Gì Học Được

### Kỹ năng chuyên môn (Hard Skills)
* **Tối ưu hóa Context Window:** Hiểu rằng AI (LLM) có giới hạn về cửa sổ ngữ cảnh. Cần cô đọng dữ liệu đầu vào và phân rã các tác vụ phức tạp thành nhiều nhánh thay vì nhồi nhét tất cả yêu cầu vào một câu lệnh duy nhất.
* **Làm chủ cơ chế vận hành của mô hình ngôn ngữ lớn:** Nhận diện AI bản chất là mô hình xác suất. Việc thiết lập `temperature = 0` không đảm bảo kết quả trùng khớp hoàn toàn qua các lượt chạy do các kỹ thuật tối ưu hóa hạ tầng (Inference Optimization) của nhà cung cấp. Do đó, hệ thống phía sau (downstream services) phải luôn được thiết kế để xử lý linh hoạt và kiểm thử liên tục (*testing, testing, testing*).
* **Quản trị chi phí đám mây và hạ tầng:** Tiếp cận giải pháp tính phí cố định (Flat Rate) như của CloudFront giúp doanh nghiệp lập kế hoạch tài chính chính xác, loại bỏ rủi ro nợ cước do tấn công DDoS hoặc tăng đột biến traffic lưu lượng.
* **Ứng dụng mô hình Multi-Agent trong thực tế:** Biết cách tổ chức hệ thống gồm nhiều Agent đóng vai trò các chuyên gia chuyên biệt (như phân tích tài chính, rủi ro) dưới sự kiểm soát của một điều phối viên (Orchestrator) để xử lý các luồng nghiệp vụ lớn của doanh nghiệp.

### Tư duy nghiệp vụ và Phát triển sự nghiệp (Mindset & Soft Skills)
* **Dịch chuyển từ "Demo" sang "Sản phẩm thực tế":** Thị trường hiện tại yêu cầu kỹ sư phải có khả năng xây dựng các sản phẩm thực tế giải quyết được bài toán cụ thể của ngành (Domain use cases), không còn ưu tiên các bài tập demo lý thuyết đơn thuần.
* **Giá trị của sự tuân thủ (Security & Compliance):** Khi đưa AI vào môi trường doanh nghiệp lớn như tài chính, ngân hàng, rủi ro về bảo mật thông tin và rò rỉ dữ liệu là tối quan trọng. Mọi giải pháp công nghệ (như cắm MCP, sử dụng AI tool) đều phải nằm trong ranh giới an toàn cho phép và tuân thủ các quy trình kiểm toán (Audit Trail).
* **Tập trung giải quyết nỗi đau cốt lõi (Core Pain Point):** Khi phát triển dự án hoặc tham gia Hackathon dưới áp lực thời gian, việc cố gắng ôm đồm quá nhiều tính năng dễ dẫn đến kiệt quệ tài nguyên. Cách tiếp cận đúng là tập trung giải quyết triệt để một bài toán nhỏ thực tế mà chính mình hoặc người dùng gặp phải hàng ngày.
* **Thấu hiểu KPI của cộng sự:** Kỹ năng giao tiếp và làm việc với các bên liên quan trong doanh nghiệp hiệu quả nhất dựa trên việc thấu hiểu áp lực công việc và mục tiêu (KPI) của họ (như đội Bảo mật hoặc Đội Vận hành), từ đó đề xuất giải pháp dễ được chấp thuận hơn.
* **Hành động ngay và không trì hoãn:** Trước tốc độ phát triển chóng mặt của công nghệ (sức mạnh AI tăng gấp đôi sau mỗi 4 tháng), việc trì hoãn nâng cấp bản thân sẽ làm tăng độ khó cạnh tranh trên thị trường lao động lên gấp nhiều lần.

> **Thông điệp cốt lõi của sự kiện:** *"Xây dựng một hệ thống không phải chỉ để nó chạy được. Nó phải chạy một cách an toàn, đáng tin cậy và thực sự phục vụ người dùng."*

#### Một số hình ảnh khi tham gia sự kiện

![](/images/1.png)

![](/images/2.png)

![](/images/3.png)

![](/images/4.png)

![](/images/5.png)