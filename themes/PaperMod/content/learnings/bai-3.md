---
title: "Lập Trình Mạng - Buổi 3: Quản Lý Luồng Nhập Xuất (I/O Streams)"
date: 2025-11-15T08:00:00+07:00
draft: false
tags: ["Network Programming", "HUTECH", "Java", "IO Stream", "Serialization"]
categories: ["Khoá học"]
author: "Trung Nguyen Quang"
description: "Tìm hiểu về cơ chế nhập xuất dữ liệu trong Java (I/O Streams), phân biệt luồng Byte và luồng Ký tự, cùng kỹ thuật Serialization."
---

## Nội Dung Chính

1. Tổng quan về Luồng (Streams)
2. Phân loại: Luồng Byte vs Luồng Ký tự
3. Các lớp I/O quan trọng
4. Serialization (Tuần tự hóa đối tượng)
5. Xử lý ngoại lệ & Try-with-resources

---

## 1. Tổng Quan Về Luồng Nhập Xuất (I/O Streams)

### Khái Niệm
* **Luồng (Stream):** Là dòng dữ liệu được di chuyển từ nguồn đến đích.
* **Nguồn/Đích:** Có thể là tập tin (file), thiết bị ngoại vi, socket mạng, hoặc bộ nhớ (mảng).
* **Cơ chế:** Dữ liệu được đọc/ghi tuần tự.

### Tại sao cần quản lý luồng?
Trong lập trình mạng, việc truyền nhận dữ liệu giữa Client và Server thực chất là thao tác trên các luồng nhập xuất (Input/Output Streams). Hiểu rõ I/O là nền tảng để xây dựng các ứng dụng mạng.

---

## 2. Phân Loại Luồng Trong Java

Java chia luồng thành 2 loại chính dựa trên đơn vị dữ liệu:

### A. Luồng Byte (Byte Streams)
* **Đơn vị xử lý:** 8-bit bytes.
* **Dùng cho:** Dữ liệu nhị phân (ảnh, video, file âm thanh, object serialization).
* **Lớp gốc (Abstract):**
    * `InputStream`: Dùng để đọc.
    * `OutputStream`: Dùng để ghi.

### B. Luồng Ký Tự (Character Streams)
* **Đơn vị xử lý:** 16-bit unicode characters.
* **Dùng cho:** Dữ liệu văn bản (text files).
* **Lớp gốc (Abstract):**
    * `Reader`: Dùng để đọc.
    * `Writer`: Dùng để ghi.

> **Quy tắc:**
> * Dữ liệu là văn bản -> Dùng **Luồng Ký tự**.
> * Dữ liệu nhị phân hoặc không rõ định dạng -> Dùng **Luồng Byte**.

---

## 3. Các Lớp I/O Hiện Thực Quan Trọng

### Luồng Byte (Byte Streams)
* **File:** `FileInputStream`, `FileOutputStream` (Đọc/ghi file).
* **Bộ nhớ:** `ByteArrayInputStream`, `ByteArrayOutputStream` (Đọc/ghi mảng byte).
* **Lọc dữ liệu (Filter):** `DataInputStream`, `DataOutputStream` (Đọc/ghi các kiểu dữ liệu nguyên thủy như `int`, `float`, `boolean`...).

### Luồng Ký Tự (Character Streams)
* **File:** `FileReader`, `FileWriter` (Đọc/ghi file văn bản).
* **Bộ nhớ:** `CharArrayReader`, `CharArrayWriter`, `StringReader`, `StringWriter`.
* **Bộ đệm (Buffer):** `BufferedReader`, `BufferedWriter` (Tăng hiệu năng đọc ghi nhờ vùng đệm).

### Chuyển Đổi (Bridge Streams)
* `InputStreamReader` và `OutputStreamWriter`: Cầu nối chuyển đổi từ luồng Byte sang luồng Ký tự và ngược lại.

---

## 4. Serialization (Tuần tự hoá đối tượng)

### Khái niệm
* **Serialization:** Quá trình chuyển đổi trạng thái của một đối tượng (Object) thành chuỗi byte để có thể lưu vào file hoặc truyền qua mạng.
* **Deserialization:** Quá trình ngược lại, khôi phục đối tượng từ chuỗi byte.

### Ứng dụng
* Lưu trạng thái chương trình (Save game, session).
* Truyền đối tượng giữa các máy tính trong mạng (RMI, Socket).

### Cách dùng
* Đối tượng cần Serialize phải implement interface `java.io.Serializable`.
* Sử dụng `ObjectOutputStream` để ghi và `ObjectInputStream` để đọc.

---

## 5. Kỹ Thuật Lập Trình & Best Practices

### Try-with-Resources (TWR)
Thay vì phải đóng luồng thủ công bằng `close()` trong khối `finally`, Java 7 giới thiệu TWR giúp code gọn gàng và an toàn hơn:

```java
// Ví dụ đọc file dùng TWR
try (FileInputStream fis = new FileInputStream("input.txt")) {
    int content;
    while ((content = fis.read()) != -1) {
        System.out.print((char) content);
    }
} catch (IOException e) {
    e.printStackTrace();
}
// Stream tự động được đóng sau khi kết thúc khối try