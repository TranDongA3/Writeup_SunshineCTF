# SunshineCTF Writeup Collection

Bộ sưu tập writeup cho các thử thách Web trong cuộc thi SunshineCTF. Repository này chứa các phân tích chi tiết về cách khai thác các lỗ hổng bảo mật web phổ biến.

## 📋 Mục lục

- [Tổng quan](#tổng-quan)
- [Các thử thách](#các-thử-thách)
- [Cấu trúc thư mục](#cấu-trúc-thư-mục)
- [Kỹ thuật sử dụng](#kỹ-thuật-sử-dụng)

## 📖 Tổng quan

Repository này bao gồm các writeup chi tiết cho các thử thách Web trong SunshineCTF, tập trung vào các kỹ thuật tấn công và khai thác lỗ hổng bảo mật web như SSRF, SQL Injection, LFI, SSTI, và authentication bypass.

Mỗi writeup bao gồm:
- Phân tích chức năng của ứng dụng
- Quá trình recon và discovery
- Các kỹ thuật khai thác được sử dụng
- Payload và giải pháp
- Screenshots minh họa chi tiết

## 🎯 Các thử thách

### 1. [Intergalactic Webhook Service](./Writeup/Intergalactic_Webhook_Service/Intergalactic_Webhook_Service.md)

**Loại lỗ hổng:** SSRF (Server-Side Request Forgery) - DNS Rebinding

**Mô tả:** 
Thử thách về một dịch vụ webhook cho phép đăng ký và kích hoạt webhook đến các URL. Ứng dụng có kiểm tra chặn truy cập vào các IP private/localhost. Flag được lưu trên localhost port 5001.

**Kỹ thuật khai thác:**
- DNS Rebinding attack
- Sử dụng dịch vụ `1u.ms` để bypass IP filtering
- Khai thác race condition trong DNS resolution

---

### 2. [Lunar Auth](./Writeup/Lunar_Auth/Lunar_Auth.md)

**Loại lỗ hổng:** Authentication Bypass

**Mô tả:**
Trang web yêu cầu đăng nhập vào admin panel để lấy flag. Credentials được mã hóa Base64 trong JavaScript phía client.

**Kỹ thuật khai thác:**
- Phát hiện endpoint ẩn qua robots.txt
- Decode Base64 để lấy credentials
- Authentication bypass

---

### 3. [Lunar File Invasion](./Writeup/Lunar_File_Invasion/Lunar_File_Invasion.md)

**Loại lỗ hổng:** LFI (Local File Inclusion) - Path Traversal

**Mô tả:**
Ứng dụng có chức năng quản lý và tải file. Người dùng có thể khai thác LFI để đọc các file nhạy cảm trên hệ thống.

**Kỹ thuật khai thác:**
- Path traversal với double encoding (`%252F`)
- Khai thác `/proc/self/cwd` để lấy đường dẫn working directory
- Đọc file flag từ hệ thống

---

### 4. [Lunar Shop](./Writeup/Lunar_Shop/Lunar_Shop.md)

**Loại lỗ hổng:** SQL Injection

**Mô tả:**
Trang web bán hàng với chức năng xem sản phẩm theo ID. Có một mặt hàng flag chưa được phát hành trong database.

**Kỹ thuật khai thác:**
- SQL Injection thông qua tham số ID
- Fuzzing để xác định số cột
- Truy vấn trực tiếp vào bảng flag

---

### 5. [Web Force](./Writeup/Web_Force/Web_Force.md)

**Loại lỗ hổng:** SSRF + SSTI (Server-Side Template Injection)

**Mô tả:**
Blackbox challenge cho phép fuzzing. Ứng dụng có SSRF tool và endpoint admin với template injection vulnerability.

**Kỹ thuật khai thác:**
- Header fuzzing để phát hiện header `Allow: true`
- SSRF để truy cập localhost endpoint `/admin`
- SSTI (Jinja2) để thực thi lệnh hệ thống
- Đọc file flag.txt bằng RCE

## 📁 Cấu trúc thư mục

```
Writeup_SunshineCTF/
├── README.md
├── Writeup/
│   ├── Intergalactic_Webhook_Service/
│   │   ├── Intergalactic_Webhook_Service.md
│   │   └── static/
│   │       ├── 1.png
│   │       ├── 2.png
│   │       ├── 3.png
│   │       └── 4.png
│   ├── Lunar_Auth/
│   │   ├── Lunar_Auth.md
│   │   └── static/
│   │       ├── 1.png
│   │       └── ...
│   ├── Lunar_File_Invasion/
│   │   ├── Lunar_File_Invasion.md
│   │   └── static/
│   │       └── ...
│   ├── Lunar_Shop/
│   │   ├── Lunar_Shop.md
│   │   └── static/
│   │       └── ...
│   └── Web_Force/
│       ├── Web_Force.md
│       └── static/
│           └── ...
└── src/
    └── (Source code của các thử thách)
```

## 🔧 Kỹ thuật sử dụng

### DNS Rebinding
Sử dụng dịch vụ như `1u.ms` để tạo domain rebind từ public IP sang localhost, bypass IP filtering trong SSRF.

### Path Traversal với Double Encoding
Sử dụng `%252F` thay vì `/` để bypass basic filter trong LFI attacks.

### SSTI Payloads
Sử dụng Jinja2 template injection để thực thi code Python và đọc file hệ thống:
```python
{{request|attr('application')|attr('\x5f\x5fglobals\x5f\x5f')|...}}
```

### SQL Injection
Kỹ thuật order by để xác định số cột, sau đó truy vấn trực tiếp vào các bảng nhạy cảm.

## 📚 Tài liệu tham khảo

- [HackTricks - File Inclusion](https://book.hacktricks.xyz/pentesting-web/file-inclusion)
- [PayloadsAllTheThings - SSTI](https://github.com/swisskyrepo/PayloadsAllTheThings/blob/master/Server%20Side%20Template%20Injection/)
- [SecLists - Headers](https://github.com/danielmiessler/SecLists/blob/master/Discovery/Web-Content/BurpSuite-ParamMiner/lowercase-headers)

## ⚠️ Disclaimer

Các writeup này chỉ nhằm mục đích giáo dục và nghiên cứu bảo mật. Việc khai thác các lỗ hổng trên các hệ thống không được phép là bất hợp pháp. Hãy chỉ sử dụng kiến thức này trong môi trường được phép và có sự đồng ý.

## 👤 Tác giả

[TranDongA3](https://github.com/TranDongA3)

## 📄 License

This repository is for educational purposes only.

