---
title: "Lập trình Đa tuyến & Xử lý đồng thời - Lập trình mạng (Buổi 7)"
date: 2025-11-01T08:00:00+07:00
draft: false
author: "Trung Nguyen Quang"
tags: ["Java", "Network Programming", "Multithreading", "Concurrency", "Thread", "Deadlock", "Hutech"]
categories: ["Lập trình mạng"]
description: "Tổng hợp kiến thức về lập trình đa tuyến (Multithreading), vòng đời của Thread, các vấn đề đồng bộ hóa (Concurrency) và bài tập ứng dụng Chat đa người dùng."
---

## Nội dung chính

Bài viết này tóm tắt nội dung buổi 7 về **Lập trình Đa tuyến (Multithreading) & Xử lý đồng thời (Concurrency)** trong khóa học Lập trình mạng máy tính.

1.  Bài tập ví dụ (Chat, Downloader)
2.  Lập trình đa tuyến (Thread)
3.  Xử lý đồng thời (Concurrency)
4.  Thuyết trình & Hỏi đáp

---

## 1. Tổng quan về Đa tuyến (Multithreading)

### 1.1. Thread (Tuyến) vs Process (Tiến trình)

* **Tiến trình (Process):** Là một chương trình hoạt động độc lập, có không gian bộ nhớ và tài nguyên riêng. Việc chuyển đổi giữa các tiến trình tốn nhiều thời gian và tài nguyên.
* **Tuyến (Thread):** Là đơn vị nhỏ nhất bên trong một tiến trình. Các thread chia sẻ chung vùng nhớ và tài nguyên của tiến trình cha. Việc tạo và chuyển đổi giữa các thread nhanh hơn rất nhiều.

**Tại sao dùng đa tuyến?**
* **Tăng hiệu suất:** Tận dụng tối đa tài nguyên CPU (ví dụ: vừa tải file vừa xử lý dữ liệu).
* **Khả năng đáp ứng:** Giúp ứng dụng không bị "đơ" (block) khi thực hiện các tác vụ nặng (ví dụ: giao diện người dùng vẫn mượt mà khi đang xử lý background).
* **Phân chia tác vụ:** Chia nhỏ công việc để xử lý đồng thời.

### 1.2. Vòng đời của một Thread

Một Thread sẽ trải qua các trạng thái sau:
1.  **NEW:** Thread được khởi tạo nhưng chưa chạy (`new Thread()`).
2.  **RUNNABLE:** Thread đã sẵn sàng (`start()`) và đang chờ CPU cấp phát tài nguyên.
3.  **RUNNING:** Thread đang thực thi phương thức `run()`.
4.  **NOT-RUNNABLE (Blocked/Waiting):** Thread tạm dừng do `sleep()`, `wait()`, hoặc chờ nhập/xuất (I/O).
5.  **DEAD:** Thread hoàn thành công việc hoặc bị dừng.

### 1.3. Tạo và quản lý Thread trong Java

**Cách 1: Implement Runnable interface**
```java
public class MyTask implements Runnable {
    public void run() {
        System.out.println("Thread is running...");
    }
}
// Chạy thread
Thread t = new Thread(new MyTask());
t.start();