# Web Force

![image.png](attachment:50df170a-16a9-4b56-9464-052f75b52c56:image.png)

Thử thách này cho phép fuzzing và là cũng là một bài blackbox .

![image.png](attachment:3d1eca79-dc25-439e-8551-ea63b2ae26d3:image.png)

File robots.txt cho ta các uri sau và hint có một header được set giá trị là true thì sẽ dùng được một SSRF tool và điều đó được áp dụng với trang này:

![image.png](attachment:c5d383ad-3068-4e28-b62c-2997ac6679ff:image.png)

Mình sẽ fuzzing trường header chỗ này và fix cứng giá trị true cho nó , list mình sử dụng là:

https://github.com/danielmiessler/SecLists/blob/master/Discovery/Web-Content/BurpSuite-ParamMiner/lowercase-headers

Và quan sát status trả về để xác định thôi.

![image.png](attachment:e4bd688a-4aa4-4a64-a964-2e2dc60f5a5f:image.png)

Có một trường Allow: true cho status 200.

![image.png](attachment:fef99462-4ad1-4d83-ba71-220e672e7a60:image.png)

Nó có một URL để ta nhập test SSRF mình sẽ thử một payload kinh điển: http://127.0.0.1/admin

Tại sao mình lại thử trang admin vì mình đã thử endpoint /admin ban đầu và được trả về 403 forbiden

![image.png](attachment:a2216eb6-91be-4ba1-b120-fd5db7f44ba6:image.png)

Có một thông báo lỗi là thiếu tham số template ở admin nên mình sẽ thêm: http://127.0.0.1/admin?template=Jinja

![image.png](attachment:a5872b01-348b-4604-ae15-868e1be09e78:image.png)

Kết nối bị từ chối , có thể là do cổng không đúng , ở đây mình sẽ intruder các cổng hợp lệ và phát hiện cổng đúng sẽ là 8000.

![image.png](attachment:889d94b3-040b-4b35-9901-a80c408de14b:image.png)

À ha , nó hiện ra chuỗi jinja là giá trị của template luôn, có sự reflect ở đây nên mình sẽ test luôn SSTI.

![image.png](attachment:8eeff75d-db06-4c8a-a0b8-33e6f7884da4:image.png)

Oke , có thể là JInja PYTHON và chắc chắn là lỗi SSTI. Mình sẽ dùng payload này để thực hiện fuzz: [PayloadsAllTheThings/Server Side Template Injection/Intruder/ssti.fuzz at master · swisskyrepo/PayloadsAllTheThings](https://github.com/swisskyrepo/PayloadsAllTheThings/blob/master/Server%20Side%20Template%20Injection/Intruder/ssti.fuzz)

![image.png](attachment:deb1dcb0-ccb9-4a93-be31-39b619ad36d5:image.png)

[http://127.0.0.1:8000/admin?template={{request|attr('application')|attr('\x5f\x5fglobals\x5f\x5f')|attr('\x5f\x5fgetitem\x5f\x5f')('\x5f\x5fbuiltins\x5f\x5f')|attr('\x5f\x5fgetitem\x5f\x5f')('\x5f\x5fimport\x5f\x5f')('os')|attr('popen')('id')|attr('read')()}}](http://127.0.0.1:8000/admin?template=%7b%7brequest%7cattr(%27application%27)%7cattr(%27%5cx5f%5cx5fglobals%5cx5f%5cx5f%27)%7cattr(%27%5cx5f%5cx5fgetitem%5cx5f%5cx5f%27)(%27%5cx5f%5cx5fbuiltins%5cx5f%5cx5f%27)%7cattr(%27%5cx5f%5cx5fgetitem%5cx5f%5cx5f%27)(%27%5cx5f%5cx5fimport%5cx5f%5cx5f%27)(%27os%27)%7cattr(%27popen%27)(%27id%27)%7cattr(%27read%27)()%7d%7d)

Có một payload trả về giá trị id luôn quá là tuyệt vời, phát triển tiếp thôi.

![image.png](attachment:669d1ef6-c0a0-48e2-abe6-5508e3033099:image.png)

‘ls’ thì hiện ra list file trong đó có file flag.txt chính là mục tiêu của ta bây giờ đọc nó thôi.

![image.png](attachment:97ca4790-8b60-4a63-8975-389131855c7a:image.png)

Ayda nhưng mà nó hình như bị filter rồi , nhưng mà không biết nó bị filter ở chữ cat hay là flag.txt sau khi test thử chỉ ghi là cat thì nó không hiện chữ ‘Nope ’ nên mình nghĩ chắc là thằng này đang filter flag.txt chẳng hạn . Nên mình quyết định thử là : cat flag*

![image.png](attachment:efa31b64-b7e5-435e-ac85-c55fb9924d6c:image.png)

Và chính nó rồi.