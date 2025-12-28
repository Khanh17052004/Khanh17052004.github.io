---
title: "Lập trình Multicast UDP & Java RMI - Lập trình mạng (Buổi 8)"
date: 2025-11-01T08:00:00+07:00
draft: false
author: "Trung Nguyen Quang"
tags: ["Java", "Network Programming", "UDP", "Multicast", "RMI", "Distributed Systems", "Hutech"]
categories: ["Lập trình mạng"]
description: "Hướng dẫn lập trình truyền thông nhóm (Multicast UDP) và gọi phương thức từ xa (Java RMI) với các bài tập thực hành Chat Room và Calculator."
---

## Nội dung chính

Bài viết này tóm tắt nội dung buổi 8 về **Multicast UDP** và **Java RMI (Remote Method Invocation)** trong khóa học Lập trình mạng máy tính.

1.  Lý thuyết Multicast UDP
2.  Lập trình MulticastSocket trong Java
3.  Tổng quan về Java RMI (Remote Method Invocation)
4.  Mô hình kiến trúc RMI
5.  Các bước xây dựng ứng dụng RMI
6.  Bài tập thực hành

---

## 1. Lập trình Multicast UDP

### 1.1. Khái niệm Multicast
Khác với Unicast (Gửi 1-1) hay Broadcast (Gửi cho tất cả), **Multicast** là kỹ thuật gửi dữ liệu từ một nguồn tới một nhóm các máy đích cụ thể (Group).
* **Địa chỉ Multicast:** Sử dụng địa chỉ IP lớp D (từ `224.0.0.0` đến `239.255.255.255`).
* **Ứng dụng:** Truyền hình trực tuyến, họp hội nghị, bảng giá chứng khoán thời gian thực, Chat nhóm.

### 1.2. Lớp MulticastSocket
Trong Java, lớp `java.net.MulticastSocket` được dùng để gửi và nhận các gói tin UDP dành cho một nhóm.

**Các phương thức quan trọng:**
* `joinGroup(InetAddress group)`: Gia nhập vào một nhóm multicast.
* `leaveGroup(InetAddress group)`: Rời khỏi nhóm.
* `send(DatagramPacket p)`: Gửi gói tin đi.
* `receive(DatagramPacket p)`: Nhận gói tin về.

**Ví dụ Code (Client tham gia Chat Room):**

```java
// Địa chỉ Multicast group và cổng
InetAddress group = InetAddress.getByName("224.0.0.1");
int port = 6789;

// Tạo socket và gia nhập nhóm
MulticastSocket socket = new MulticastSocket(port);
socket.joinGroup(group);

// Gửi tin nhắn
String msg = "Hello Group!";
DatagramPacket data = new DatagramPacket(msg.getBytes(), msg.length(), group, port);
socket.send(data);

// Nhận tin nhắn
byte[] buf = new byte[1024];
DatagramPacket recv = new DatagramPacket(buf, buf.length);
socket.receive(recv);
System.out.println("Received: " + new String(recv.getData()));

// Rời nhóm
socket.leaveGroup(group);
socket.close();