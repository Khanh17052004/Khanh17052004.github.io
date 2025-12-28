---
title: "Lập trình Sockets TCP - Lập trình mạng (Buổi 5)"
date: 2025-11-01T08:00:00+07:00
draft: false
author: "Trung Nguyen Quang"
tags: ["Java", "Network Programming", "TCP", "Sockets", "Hutech"]
categories: ["Lập trình mạng"]
description: "Tổng hợp kiến thức về lập trình TCP Sockets trong Java, mô hình Client-Server và các bài tập thực hành."
---

## Nội dung chính

Bài viết này tóm tắt nội dung buổi 5 về **Lập trình Sockets TCP** trong khóa học Lập trình mạng máy tính.

1. Khởi động đầu giờ
2. Bài tập ví dụ
3. Lập trình TCP Sockets
4. Truyền cảm hứng
5. Bài tập tại lớp
6. Trắc nghiệm ôn tập
7. Thuyết trình
8. Hỏi & Đáp

---

## 1. Mô hình Client-Server

Các ứng dụng mạng thường hoạt động theo mô hình Client/Server (Ví dụ: Web server/client, Mail server/client).

* **Server (Máy chủ):** Cung cấp dịch vụ, luôn chạy và lắng nghe yêu cầu từ client, xử lý và trả kết quả.
* **Client (Máy khách):** Gửi yêu cầu dịch vụ đến server và đợi phản hồi.

**Quy trình trao đổi dữ liệu:**
1.  Client gửi Request (yêu cầu) tới Server.
2.  Server xử lý yêu cầu.
3.  Server gửi Response (đáp ứng) lại cho Client.

---

## 2. Giao thức TCP (Transmission Control Protocol)

### Tại sao dùng TCP?
* **Đáng tin cậy:** Đảm bảo dữ liệu truyền đúng và đủ (tự động gửi lại nếu mất gói tin).
* **Luồng dữ liệu tin cậy:** Dữ liệu không bị thay đổi hoặc lẫn lộn.
* **Kiểm soát luồng (Flow Control):** Tránh làm người nhận quá tải.
* **Kiểm soát lỗi (Error Control):** Đảm bảo dữ liệu đến đích chính xác.
* **Giao tiếp hai chiều (Full-duplex):** Cho phép truyền/nhận đồng thời.

### Cơ chế hoạt động
1.  **Điều khiển luồng**
2.  **Phân đoạn gói tin TCP**
3.  **Quy trình Bắt tay 3 bước (3-Way Handshake):**
    * Thiết lập kết nối
    * Truyền dữ liệu
    * Giải phóng kết nối

---

## 3. Lập trình Socket trong Java

Trong Java, `java.net.Socket` và `java.net.ServerSocket` là hai lớp chính để lập trình TCP.

### 3.1. Lớp Socket (Dùng cho Client & Server)
Dùng để thiết lập và quản lý kết nối, truyền nhận dữ liệu.

**Khởi tạo:**
```java
// Kết nối đến host và port cụ thể
Socket socket = new Socket("localhost", 8080);