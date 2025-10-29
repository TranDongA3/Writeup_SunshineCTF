---
title: "Lunar Auth"
author: "Dong"
last_updated: "2025-10-29"
canonical: "https://maze-toy-02a.notion.site/Lunar-Auth-27deac4f7306808f9e05d62111ca9790"
---

# Lunar Auth

Nguồn gốc: xem bản gốc trên Notion: [Lunar Auth (Notion)](https://maze-toy-02a.notion.site/Lunar-Auth-27deac4f7306808f9e05d62111ca9790)

## Tổng quan
- Thể loại: Web Exploitation / Auth
- Mục tiêu: Phân tích và khai thác cơ chế xác thực của bài "Lunar Auth" để đạt quyền truy cập mong muốn.
- Kết quả: … (điền cờ/điểm/điều kiện hoàn thành nếu có)

## Mô tả thử thách
- Tóm tắt ngắn gọn đề bài, endpoint chính, ràng buộc, dữ liệu khởi tạo.

> Gợi ý: Dán mô tả gốc và chỉnh sửa cho súc tích; nêu rõ luồng login/authorize nếu có.

## Môi trường và thiết lập
- Công nghệ và framework: …
- Cách chạy local (nếu bạn đã thử): …
- Công cụ hỗ trợ: Burp/ZAP/cURL/... (nếu dùng)

## Phân tích chức năng xác thực
- Luồng đăng nhập/đăng ký/khôi phục: …
- Kiến trúc phiên: cookie/jwt/header tùy chỉnh: …
- Kiểm soát truy cập (RBAC/ABAC/feature flags): …
- Điểm nghi vấn: … (ví dụ: kiểm tra chữ ký JWT, kiểm tra thời hạn, so sánh chuỗi, xử lý Unicode/normalization, case-insensitive, timing, ...)

## Bề mặt tấn công và giả thuyết
- Danh sách các vector có thể khai thác:
  - …
  - …
- Giả thuyết trọng tâm: …

## Khai thác chi tiết
1. Bước 1: … (mô tả thao tác cụ thể, request/response liên quan)
2. Bước 2: …
3. Bước 3: …

### Mẫu yêu cầu (PoC)
```http
# Ví dụ: Đăng nhập bất thường
POST /login HTTP/1.1
Host: target
Content-Type: application/json

{"username":"...","password":"..."}
```

```bash
# Ví dụ cURL
curl -i -s -k -X $'POST' \
  -H $'Content-Type: application/json' \
  --data-binary $'{"username":"...","password":"..."}' \
  $'https://target/login'
```

### Kết quả và xác minh
- Quan sát phản hồi: …
- Bằng chứng chiếm quyền/đạt mục tiêu: …

## Ảnh minh họa
Đặt ảnh vào cùng thư mục dự án (gợi ý: `images/lunar_auth/`) và tham chiếu như dưới đây. Bạn có thể đổi tên file theo ảnh xuất từ Notion.

![Sơ đồ luồng auth](images/lunar_auth/flow.png)

![Giao diện đăng nhập](images/lunar_auth/login.png)

![JWT bất thường](images/lunar_auth/jwt.png)

> Nếu bạn có nhiều ảnh, thêm chú thích ngắn bên dưới mỗi ảnh để người đọc nắm bắt nhanh.

## Nguyên nhân gốc rễ (Root Cause)
- Phân tích kỹ thuật: …
- Vì sao kiểm tra/validate thất bại: …

## Biện pháp khắc phục (Defense)
- Sửa lỗi cốt lõi: …
- Tăng cường phòng thủ: … (ví dụ: xác thực chữ ký đúng thuật toán, rotate secrets, harden cookie flags, strict origin checks...)

## Tham chiếu
- Bản gốc Notion: [Lunar Auth (Notion)](https://maze-toy-02a.notion.site/Lunar-Auth-27deac4f7306808f9e05d62111ca9790)
- Tài liệu/bài viết liên quan: …

