# Lunar File Invasion

![image.png](attachment:2ec62fe6-9bcf-43a0-b8a3-ab79237516dd:image.png)

Recon thôi.

![image.png](attachment:38dbd14f-70c5-4972-8ec6-a450274cb97a:image.png)

Ở đây ta thấy một đường dẫn là ./gitignore file hay dùng để bỏ qua những file không muốn gửi lên github, tránh lộ thông tin nhạy cảm. Ở đây thì có thể xem file đó làm gì.

![image.png](attachment:e5f90fbc-428d-4466-b159-1741623f5788:image.png)

Có một file /index/static/login.html~ kiểm tra luôn.

![image.png](attachment:28e777b9-47fb-4e1e-a663-1c00325f3cbe:image.png)

Rồi luôn, có cả tài khoản và mật khẩu:

<div>
<img src="" alt="Image of Alien">
<form action="{{url_for('index.login')}}" method="POST">
<!-- TODO: use proper clean CSS stylesheets bruh -->
<p style="color: red;"> {{ err_msg }} </p>
<input type="hidden" name="csrf_token" value="{{ csrf_token() }}" />
<label for="Email">Email</label>
<input value="admin@lunarfiles.muhammadali" type="text" name="email">

```
        <label for="Password">Password</label>
        <!-- just to save time while developing, make sure to remove this in prod !  -->
        <input value="jEJ&(32)DMC<!*###" type="text" name="password">
        <button type="submit">Login</button>
    </form>
</div>

```

Và hãy nhớ là có một endpoint bị lộ ở robots đó chính là /login lấy tài khoản này vào.

![image.png](attachment:c1bd5554-1ad8-4330-9f69-b2d990ed8b3f:image.png)

Khi vào thì nó dẫn ta đến trang này , ban đầu mình ghĩ đến ý tưởngt bruforce nhma mã pin 10 số thì cũng hơi rén , vì độ phức tạo cao quá.

Có một endpoint cũng đáng nhắm đến đó là ./admin/dashboard

Đường dẫn trên sẽ dẫn chúng ta đến giao diện này.

![image.png](attachment:2b4b7c37-0d0b-4fca-b5be-b5e7131efe2d:image.png)

Đi thẳng vào chức năng manage files.

![image.png](attachment:77ac872c-f7bf-4770-a96f-80860e663f0b:image.png)

Xem từng file thì tương ứng nó gọi một API như này:

![image.png](attachment:a8f220dc-a334-4bfe-b80d-dcef76bea857:image.png)

Tức là nó sẽ down từ backend xuống fontend để hiển thị và như đề bài là Lunar File Invasion mình nghĩ nó là lỗi LFI , đây có thể là hint, hơn nữa trong file secret2.txt có đề cập đến việc những hacker dùng kỹ thuật trên hatrick để đánh cắp thông tin .

https://book.hacktricks.wiki/en/pentesting-web/file-inclusion/index.html

Và sau khi nghiên cứu và thử các payload , để filter bypass thì kết quả nó dẫn đến là :

![image.png](attachment:34bdc267-d9f6-4f13-ab89-16f43937b9da:image.png)

Nó chính là kiểu double encode nhưng mà trước đó mình cũng đã thử, kiểu này và thất bại, nhận về thông báo là file không tồn tại, nhưng mình không thể ngờ được là do mình lùi chưa đủ sâu để khám phá được nội dung file này.

Từ đây mình sẽ tiến hành đi tìm đường dẫn flag.

Mình sẽ thử một đường dẫn hệ thông như sau: /proc/self/cmdline

![image.png](attachment:3820be24-dba1-425c-b330-d1b65aa71f12:image.png)

LÀm lộ đường dẫn [app.py](http://app.py), nhưng mà để vào được app.py phải truy cập từ đường dẫn mặc định hiện tại làm việc ở đây có một đường dẫn như vậy đại diện cho đường dẫn hiện tại làm việc đó là /proc/self/cwd(đường dẫn này để hiểu thêm thì bạn có thể xem ở trang hatrick mình để ở trên).

Ta thử như này đi: /proc/self/cwd/app.py

![image.png](attachment:d11efe91-a625-4881-8325-772c1cc65cd2:image.png)

Payload:

GET /admin/download/..%252F.%252F..%252F.%252F..%252F.%252F..%252F.%252F..%252F.%252F..%252F.%252F..%252F.%252F..%252F.%252F..%252F.%252F..%252Fproc%252fself%252fcwd/app.py

Và flag của chúng ta nằm ở ./FLAG/flag.txt

![image.png](attachment:50655e0e-15ad-4756-a118-675d0945a448:image.png)