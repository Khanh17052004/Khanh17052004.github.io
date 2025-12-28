---
title: "Quản lý địa chỉ kết nối mạng - Lập trình mạng (Buổi 4)"
date: 2025-11-01T08:00:00+07:00
draft: false
author: "Trung Nguyen Quang"
tags: ["Java", "Network Programming", "URL", "InetAddress", "Web Crawler", "Hutech"]
categories: ["Lập trình mạng"]
description: "Tổng hợp kiến thức về quản lý địa chỉ mạng, lập trình với lớp URL, InetAddress và bài tập thực hành Web Crawler."
---

## Nội dung chính

Bài viết này tóm tắt nội dung buổi 4 về **Quản lý địa chỉ kết nối mạng** trong khóa học Lập trình mạng máy tính.

1. Khởi động đầu giờ
2. Bài tập mẫu: Web Crawler
3. Quản lý địa chỉ kết nối mạng (InetAddress, URL, URLConnection)
4. Bài tập tại lớp: Search Engine
5. Trắc nghiệm ôn tập

---

## 1. Bài tập mẫu: Web Crawler

**Mục tiêu:** Viết chương trình lấy danh sách URL từ một trang web ban đầu.

**Yêu cầu chức năng:**
1.  **Lập danh sách URL cần crawl:**
    * Người dùng nhập địa chỉ trang web ban đầu và độ sâu tìm kiếm.
    * Chương trình lập danh sách các đường link cần crawl và lưu vào file.
2.  **Hiển thị kết quả:** Menu cho phép xem danh sách URL đã lấy được.
3.  **Kết thúc chương trình.**

---

## 2. Các kiến thức nền tảng (Java)

Để thực hiện bài toán trên, chúng ta cần nắm vững các lớp hỗ trợ sau:

### 2.1. Lớp URL (`java.net.URL`)
Dùng để đại diện và thao tác với địa chỉ tài nguyên trên mạng.

* **Khởi tạo:** `URL url = new URL("https://www.example.com");`
* **Thuộc tính:** `protocol`, `host`, `port`, `path`, `query`.
* **Đọc dữ liệu từ URL:**
    ```java
    InputStream inputStream = url.openStream();
    BufferedReader reader = new BufferedReader(new InputStreamReader(inputStream));
    // Đọc từng dòng dữ liệu...
    ```

### 2.2. Giao diện Iterator
Duyệt qua một tập hợp phần tử mà không cần quan tâm cấu trúc bên trong.

* **Phương thức:** `hasNext()`, `next()`, `remove()`.
* **Ví dụ:** Duyệt danh sách `List<String>`.

### 2.3. Lớp Matcher & Pattern (Regular Expression)
Dùng để xử lý chuỗi, tìm kiếm và trích xuất thông tin (ví dụ: tìm link trong mã HTML).

* **Pattern:** `Pattern.compile("regex")`
* **Matcher:** `pattern.matcher("input")`
* **Regex cơ bản:** `.` (bất kỳ ký tự), `*` (0 hoặc nhiều), `+` (1 hoặc nhiều), `?` (0 hoặc 1).

### 2.4. Cấu trúc dữ liệu `HashSet` và `HashMap`
* **HashSet:** Lưu trữ tập hợp phần tử **duy nhất** (không trùng lặp), tìm kiếm nhanh. Dùng để lưu danh sách URL đã crawl để tránh trùng.
* **HashMap:** Lưu trữ cặp **Key-Value**. Dùng để lưu trữ dữ liệu có cấu trúc tra cứu.

---

## 3. Quản lý địa chỉ kết nối mạng

### 3.1. Địa chỉ IP và DNS
* **IPv4:** Cấu trúc 32-bit, chia thành các lớp A, B, C, D (Multicast), E.
* **DNS (Domain Name System):** Hệ thống phân giải tên miền thành địa chỉ IP (Ví dụ: `www.google.com` -> `142.250.x.x`).

### 3.2. Lớp `InetAddress`
Đại diện cho một địa chỉ IP (IPv4 hoặc IPv6).

* **Tạo đối tượng:**
    * `InetAddress.getLocalHost()`: Lấy IP máy hiện tại.
    * `InetAddress.getByName(String host)`: Lấy IP từ tên miền.
    * `InetAddress.getAllByName(String host)`: Lấy danh sách IP của tên miền.
* **Phương thức:** `getHostName()`, `getHostAddress()`, `isReachable(timeout)`.

---

## 4. Làm việc với URL và URLConnection

### 4.1. Lớp URLConnection
Tạo kênh giao tiếp hai chiều với Server (HTTP, FTP...).

* **Quy trình:** Tạo `URL` -> `openConnection()` -> Cấu hình -> Đọc Header/Data.
* **Lấy thông tin Header:** `getContentLength()`, `getDate()`, `getContentType()`, `getLastModified()`.
* **Luồng dữ liệu:** `getInputStream()` (đọc), `getOutputStream()` (ghi).

### 4.2. Mã hóa và Giải mã URL
* **Lý do:** URL chỉ chấp nhận các ký tự ASCII chuẩn. Các ký tự đặc biệt hoặc khoảng trắng cần được mã hóa (ví dụ: khoảng trắng -> `%20` hoặc `+`).
* **Lớp hỗ trợ:**
    * `URLEncoder.encode(String s, String enc)`
    * `URLDecoder.decode(String s, String enc)`

---

## 5. Bài tập thực hành

### Bài tập về nhà
1.  Nhập 2 tên miền, kiểm tra xem có cùng địa chỉ IP không.
2.  Kiểm tra mã trạng thái HTTP (Response Code) của một URL.
3.  Download source code HTML của một website về máy.
4.  Download toàn bộ hình ảnh trên trang chủ của một website về thư mục.

### Bài tập tại lớp: Search Engine (Nâng cao)
Phát triển từ bài Web Crawler:
1.  Nhập website và từ khóa tìm kiếm.
2.  Crawl danh sách link theo độ sâu.
3.  Tìm kiếm từ khóa trong nội dung các link đó.
4.  Thống kê số lần xuất hiện và sắp xếp link theo thứ tự giảm dần của từ khóa (Gợi ý: Dùng `TreeMap` và `Comparator`).

---
> **Tài liệu tham khảo:**
> * Slide bài giảng: 4_LTM_QuanLyDiaChiKetNoiMang.pdf - TRUNG NGUYEN QUANG (Hutech)