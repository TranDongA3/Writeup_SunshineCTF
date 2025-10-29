![image](static/1.png)

Như đề bài thì nó yêu cầu về việc xâm nhập vào admin panel để nhận được flag, nên mục tiêu cũng rõ ràng.

![image](static/2.png)

Ta sẽ tiến hành recon trang web này , đầu tiên là check file robots.txt thấy được endpoint bị disallow như sau:

![image](static/3.png)

Tiến hành truy cập vào , thì ra giao diện một trang đăng nhập như này:

![image](static/4.png)

ViewSoure thì ta thấy một đoạn mã Js xử lí như sau:

<script>
/*
  To reduce load on our servers from the recent space DDOS-ers,
  we have lowered login attempts by using Base64 encoded "encryption"
  on the client side. 💀
  
  TODO: implement proper encryption.
*/

const real_username = atob("YWxpbXVoYW1tYWRzZWN1cmVk");
const real_passwd   = atob("UzNjdXI0X1BAJCR3MFJEIQ==");

document.addEventListener("DOMContentLoaded", () => {
  const form = document.querySelector("form");

  function handleSubmit(evt) {
    evt.preventDefault();

    const username = form.elements["username"].value;
    const password = form.elements["password"].value;

    if (username === real_username && password === real_passwd) {
      // remove this handler and allow form submission
      form.removeEventListener("submit", handleSubmit);
      form.submit();
    } else {
      alert("[ Invalid credentials ]");
    }
  }

  form.addEventListener("submit", handleSubmit);
});
</script>


Trông rất đơn giản vì chỉ cần đăng nhập đúng:

const real_username = atob("YWxpbXVoYW1tYWRzZWN1cmVk");

---

const real_passwd   = atob("UzNjdXI0X1BAJCR3MFJEIQ==");

Hàm atob trong JS là để giải mã base64 , sau khi giải mã thì ta được :

username=’alimuhammadsecured’

password=’S3cur4_P@$$w0RD!’

Done , thử thách quá dễ.

![image](static/5.png)