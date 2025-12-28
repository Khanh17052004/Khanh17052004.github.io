---
title: "Lập Trình Mạng - Buổi 2: Kiến Thức Nền Tảng"
date: 2025-11-08T08:00:00+07:00
draft: false
tags: ["Network Programming", "HUTECH", "Java", "TCP/IP", "Internet"]
categories: ["Khoá học"]
author: "Trung Nguyen Quang"
description: "Tổng hợp kiến thức nền tảng về Mạng máy tính, các giao thức mạng phổ biến và lý do lựa chọn Java cho lập trình mạng."
---

## Nội Dung Chính

1. Mạng Máy Tính & Internet
2. Các Ứng Dụng Mạng
3. Ngôn Ngữ Java cho Lập Trình Mạng

---

## 1. Mạng Máy Tính & Internet

### Quy Mô Internet
* **End-Devices:** Khoảng 15 tỷ thiết bị (số liệu 2022).
* **Core Network:** Tốc độ đường truyền cực cao, ví dụ kỷ lục 44.8 Tbps (2020) giữa Tokyo – Osaka.
* **Access Network:** Đa dạng công nghệ với độ trễ (latency) khác nhau, ảnh hưởng đến các ứng dụng thời gian thực như xe tự lái, game online, phẫu thuật từ xa.

### Chuyển Mạch Gói (Packet Switching)
Đây là công nghệ nền tảng của Internet (từ năm 1961), thay thế cho chuyển mạch kênh truyền thống.
* **Ưu điểm:**
    * **Hiệu quả:** Chia sẻ đường truyền tốt hơn.
    * **Mở rộng:** Dễ dàng thêm thiết bị mới.
    * **Linh hoạt:** Dữ liệu đi theo nhiều đường, chịu lỗi tốt.
* **Thách thức:** Mất gói tin (packet loss), độ trễ biến đổi (jitter), khó đảm bảo chất lượng dịch vụ (QoS).

### Bộ Giao Thức TCP/IP & Mô Hình OSI
Mô hình tham chiếu cho việc truyền thông mạng:
1.  **Application (Ứng dụng):** Nơi các ứng dụng mạng hoạt động (HTTP, SMTP...).
2.  **Transport (Giao vận):** Quản lý truyền dữ liệu giữa các tiến trình (TCP, UDP).
3.  **Internet (Liên mạng):** Định tuyến và đánh địa chỉ (IP).
4.  **Network Interface (Giao diện mạng):** Truyền dữ liệu vật lý (Ethernet, Wifi).

### Giao Thức Giao Vận: TCP vs UDP
* **TCP (Transmission Control Protocol):** Hướng kết nối, đảm bảo tin cậy, có cơ chế bắt tay 3 bước (3-way handshake), kiểm soát luồng và tắc nghẽn. Dùng cho Web, Email, File transfer.
* **UDP (User Datagram Protocol):** Không kết nối, nhanh nhưng không đảm bảo tin cậy. Dùng cho Streaming, DNS, VoIP.

---

## 2. Các Ứng Dụng Mạng Phổ Biến

### Web & HTTP
* **HTTP (HyperText Transfer Protocol):** Giao thức trao đổi siêu văn bản.
* Các phiên bản phát triển từ HTTP/1.0, 1.1 (Persistent connection) đến HTTP/2, HTTP/3.
* Các khái niệm liên quan: Cookies, Proxy Server (Web caching), CDN.

### Hệ Thống Tên Miền (DNS)
* Dịch vụ phân giải tên miền (ví dụ: www.google.com) sang địa chỉ IP để máy tính hiểu được.
* Hoạt động theo cấu trúc cây (DNS Tree).

### Email
Gồm 3 thành phần chính:
* **User Agents:** Trình đọc mail (Outlook, Webmail).
* **Mail Servers:** Máy chủ lưu trữ và chuyển tiếp thư.
* **Protocols:** SMTP (gửi thư), POP3/IMAP (nhận thư).

### FTP (File Transfer Protocol)
* Giao thức truyền tập tin tin cậy.
* Sử dụng 2 kết nối song song: Một cho điều khiển (port 21) và một cho dữ liệu (port 20).

---

## 3. Ngôn Ngữ Java

### Tại Sao Chọn Java Cho Lập Trình Mạng?
* **Độc lập nền tảng (Platform Independence):** "Write Once, Run Anywhere" nhờ JVM.
* **Thư viện phong phú:** Hỗ trợ mạnh mẽ cho Networking, I/O, Multithreading.
* **Bảo mật:** Tích hợp sẵn SSL/TLS, xác thực.
* **Đa luồng (Multithreading):** Tối ưu hóa hiệu năng cho các ứng dụng mạng phục vụ nhiều kết nối đồng thời.

### Các Nguyên Lý OOP Trong Java
* **Tính đóng gói (Encapsulation):** Bảo vệ dữ liệu, tổ chức code gọn gàng.
* **Tính kế thừa (Inheritance):** Tái sử dụng mã nguồn.
* **Tính đa hình (Polymorphism):** Linh hoạt trong xử lý đối tượng.

---

> **Tóm tắt:** Buổi học cung cấp bức tranh toàn cảnh về hạ tầng mạng, cách các ứng dụng giao tiếp với nhau và công cụ (Java) chúng ta sẽ sử dụng để xây dựng chúng.