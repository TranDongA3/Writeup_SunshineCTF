# Lunar Shop

![image.png](attachment:a1a45ee7-23ee-4697-b412-f2c1af40293f:image.png)

Thử thách này được mô tả là có mặt hàng flag không được phát hành , đi vào giao diện trang web ta thấy như sau:

![image.png](attachment:1280fcb4-1f20-4086-905b-2de9e4dfec6b:image.png)

Có 3 product được phát hành xem từng cái ,

![image.png](attachment:743354aa-353a-495b-9a7c-a97953bf2eb4:image.png)

Ứng với mỗi id thì sẽ có một mặt hàng tương ứng với mô tả , ở đây ban đầu mình nghĩ là sẽ thử id từ 1 đến 100 để bắt flag nhưng mà thất bại , còn một cách khác đó là ta sẽ test thử SQL injection

payload : 1 order by 5  

cho ta thông báo 

![image.png](attachment:897c4cf2-542c-4f87-b075-c0d1cfab0989:image.png)

Vậy chỉ có 4 cột thôi thay vì fuzzing mình sẽ thử thẳng cột flag ở bảng flag luôn, dù chưa khoa học nhưng sẽ là chuẩn đoán ban đầu.

![image.png](attachment:2d7e4872-39e7-4608-a42d-fdf0d0e28f1c:image.png)

May mắn là có luôn nên ta không cần mắc nhiều thời gian để fuzz.