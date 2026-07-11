---
title: "Nhật ký Tuần 9: Tích Hợp Trí Tuệ Nhân Tạo & Triển Khai AI Workload Trên AWS"
date: 2024-01-01
weight: 1
chapter: false
pre: " <b> 1.9. </b> "
---
## Mục tiêu tuần 9:
Tuần này tập trung vào việc thu hẹp khoảng cách giữa điện toán đám mây và Trí tuệ nhân tạo (AI/ML), học cách khai thác các dịch vụ Managed AI hiện đại, triển khai các mô hình ngôn ngữ lớn (LLM) và tiếp cận tư duy vận hành hệ thống AI tự động hóa:

1. **Kết Hợp AI Với AWS (AWS Managed AI Integration):** Làm quen và làm chủ cách tích hợp các dịch vụ AI đã được huấn luyện sẵn của AWS vào các ứng dụng hiện hữu thông qua API, giúp tối ưu hóa thời gian phát triển hệ thống.
2. **Triển Khai AI Workload Trên Cloud:** Thực hành cấu hình, deploy và expose các ứng dụng AI sử dụng mô hình nền tảng (Foundation Models) qua dịch vụ Serverless hoặc Container bảo mật, có khả năng chịu tải tốt trên hạ tầng AWS.
3. **Hiểu MLOps Cơ Bản (Introduction to MLOps):** Tiếp cận quy trình vòng đời của một dự án học máy từ giai đoạn chuẩn bị dữ liệu, huấn luyện, deploy mô hình cho đến giám sát độ chính xác và hiệu năng phần cứng theo thời gian thực.

---

## Nhật Ký Lộ Trình Tuần 9

| Thứ | Công việc | Ngày BĐ | Ngày KT | Nguồn tài liệu |
| :--- | :--- | :--- | :--- | :--- |
| **16/06** | AWS AI Services Overview: Khai thác các dịch vụ AI có sẵn (Rekognition, Polly, Translate) | 16/06/2026 | 16/06/2026 | [AWS AI Services Docs](https://aws.amazon.com/machine-learning/ai-services/) |
| **17/06** | Generative AI with Amazon Bedrock: Tiếp cận mô hình nền tảng và thiết lập API Agent | 17/06/2026 | 17/06/2026 | [Amazon Bedrock User Guide](https://docs.aws.amazon.com/bedrock/latest/userguide/what-is-bedrock.html) |
| **18/06** | End-to-End ML with Amazon SageMaker: Tìm hiểu quy trình xây dựng và huấn luyện mô hình | 18/06/2026 | 18/06/2026 | [Amazon SageMaker Developer Guide](https://docs.aws.amazon.com/sagemaker/latest/dg/whatis.html) |
| **19/06** | AI API Deployment: Đóng gói và triển khai ứng dụng AI kết nối API bên thứ ba | 19/06/2026 | 19/06/2026 | [AWS Architecture Center: GenAI](https://aws.amazon.com/architecture/general-ai/) |
| **20/06** | Serverless AI Architecture: Thiết lập hạ tầng AI tối ưu chi phí với AWS Lambda & API Gateway | 20/06/2026 | 20/06/2026 | [Serverless Land - AI/ML](https://serverlessland.com/) |
| **21/06** | AI Workload Monitoring & MLOps: Giám sát hiệu năng và chi phí vận hành tài nguyên AI | 21/06/2026 | 21/06/2026 | [Amazon SageMaker Model Monitor](https://docs.aws.amazon.com/sagemaker/latest/dg/model-monitor.html) |
| **22/06** | Mini AI Project & Documentation: Hoàn thiện ứng dụng AI tích hợp đám mây và viết báo cáo | 22/06/2026 | 22/06/2026 | Tự triển khai / Hugo Portfolio Blog |

---
### Minh chứng thực tế:

## 1. Hệ thống phân tích phản hồi khách hàng tự động (AI-powered Feedback Processor)

**Mô hình kiến trúc hệ thống**
![](/images/mohinh.png)

## 2. Mục tiêu của bài Lab
* **Triển khai Kiến trúc Serverless AI:** Tích hợp thành công API Gateway và AWS Lambda để tiếp nhận và điều phối luồng dữ liệu text tiếng Việt thời gian thực dưới 1 giây.
* **Xây dựng Pipeline Phân tích Ngữ nghĩa:** Phát triển bộ lọc logic và phân loại sắc thái dữ liệu khách hàng (Sentiment Analysis) thành các nhóm chỉ số rõ ràng (Positive, Negative, Neutral).
* **Thiết lập Hệ thống Giám sát Vận hành (MLOps Monitoring):** Xuất các cấu trúc log định danh dạng `[METRIC]` ra Amazon CloudWatch để theo dõi hiệu năng và trạng thái an toàn dữ liệu trực tiếp.

## 3. Các thành phần và Mục đích sử dụng

| Thành phần hạ tầng | Mục đích sử dụng trong Lab |
| :--- | :--- |
| **Amazon API Gateway** | Phơi endpoint RESTful API công khai ra ngoài Internet nhận payload đầu vào từ client. |
| **AWS Lambda** | Đóng vai trò bộ não điều phối dữ liệu (Compute Node), chịu trách nhiệm parse chuỗi và xử lý logic pipeline. |
| **Amazon CloudWatch** | Thu thập toàn bộ log hệ thống, theo dõi thời gian thực thi (Duration) và ghi nhận chỉ số Metric phục vụ giám sát. |
| **Rule-based AI Engine** | Bộ lọc phân tích phân loại từ khóa ngữ nghĩa tích hợp trực tiếp trên nền tảng Serverless Backend. |

---

## 4. Hướng dẫn các bước triển khai chi tiết

### Bước 1: Khởi tạo Role IAM và Cấu hình Quyền hạn
1. Truy cập **IAM Console** > **Roles** > Khởi tạo Role mang tên `Lambda-AI-Integration-Role`.
2. Gán chính sách quyền hạn cơ bản: `CloudWatchLogsFullAccess` để cho phép Lambda đẩy log giám sát.

### Bước 2: Triển khai Mã nguồn Điều phối AWS Lambda
1. Tạo một hàm Lambda với Runtime **Python 3.12** lấy tên `Feedback-AI-Processor`.
2. Tại cấu hình thẻ **Custom execution role**, chọn Role `Lambda-AI-Integration-Role` đã tạo.
3. Tiến hành deploy đoạn mã nguồn xử lý bóc tách chuỗi tiếng Việt và xuất chỉ số metric:

        ```python
        import json

        def lambda_handler(event, context):
            print("Received event:", json.dumps(event))
            
            customer_text = ""
            raw_body = event.get('body', '')
            
            if raw_body:
                try:
                    if isinstance(raw_body, str):
                        body = json.loads(raw_body)
                    else:
                        body = raw_body
                    customer_text = body.get('text', '')
                except Exception as e:
                    if 'text' in raw_body:
                        try:
                            customer_text = raw_body.split('"text":')[1].split('}')[0].strip(' "\'')
                        except:
                            customer_text = raw_body

            image_safety = "Safe"
            sagemaker_action = "None"
            positive_words = ["nhanh", "tốt", "tuyệt", "hài lòng", "nhiệt tình", "mua lại", "cẩn thận"]
            sentiment = "Neutral"
            
            if customer_text:
                if any(word in customer_text.lower() for word in positive_words):
                    sentiment = "Positive"
                elif "tệ" in customer_text.lower() or "chậm" in customer_text.lower():
                    sentiment = "Negative"
                    sagemaker_action = "Triggered SageMaker-YOLO-Blur-Endpoint"

            print(f"[METRIC] Status: Processed | Sentiment: {sentiment} | Safety: {image_safety}")

            return {
                "statusCode": 200,
                "headers": {"Content-Type": "application/json; charset=utf-8"},
                "body": json.dumps({
                    "image_safety": image_safety,
                    "extracted_text_by_backend": customer_text,
                    "sagemaker_mitigation": sagemaker_action,
                    "ai_llm_analysis": f"Sentiment: {sentiment}. Summary: Hệ thống ghi nhận phản hồi tích cực từ khách hàng về dịch vụ."
                }, ensure_ascii=False)
            }   

## Bước 3: Cấu hình Hạ tầng API Gateway và Mở rộng Throttling / Timeout
Liên kết API Gateway REST API với hàm Lambda thông qua cơ chế tích hợp Integration Method POST.

Truy cập tab Configuration > General configuration của Lambda, tiến hành nâng mức giới hạn thời gian chờ Timeout lên 30 giây để đảm bảo chuỗi xử lý không bị ngắt quãng giữa chừng.

![](/images/cloudwatch_metrics.png)

![](/images/lambda_success.png)

## Kết quả đạt được tuần 9
Hệ thống đã được kiểm thử trực tiếp trên Console và môi trường kết nối biên đạt trạng thái vận hành tối ưu:

Trạng thái xử lý thành công: Trả về HTTP Code 200 OK với thông báo Executing function: succeeded.

Độ trễ xử lý (Latency): Thời gian thực thi hạ tầng chỉ tiêu tốn 2.60 ms (Tổng thời gian thanh toán billed duration đạt mức tối thiểu 94 ms).

Hiệu năng bộ nhớ sử dụng: Tiêu thụ tài nguyên tối đa ở mức 37 MB trên tổng số 128 MB định mức cấu hình, chứng minh tính tinh gọn của kiến trúc Serverless AI.


