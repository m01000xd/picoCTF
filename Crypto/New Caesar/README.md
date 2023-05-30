![image](https://github.com/m01000xd/picoCTF/assets/122852491/6474e721-a01e-41d7-9d0b-7fe0d64adeab)

Mở file new_caesar.py:

![image](https://github.com/m01000xd/picoCTF/assets/122852491/4421e17d-0d14-4e95-b60f-42395eda1c5e)


Flag ban đầu của chúng ta, sẽ được encrypt qua hàm **b16_encode()**, trả về cipher, sau đó, cipher lại qua hàm **shift()**, lại được encrypt
thêm lần nữa rồi trả về biến **enc** chính là một dãy kí tự mà chal đã cho:

![image](https://github.com/m01000xd/picoCTF/assets/122852491/9357b0d5-e014-43c3-88a2-d10b4dae630d)


Giờ ta cần phải hiểu bản chất của 2 hàm **b16_encode()** và **shift()**


* **b16_encode(plain)** : Hàm **b16_encode()**, truyền vào argument **plain**, đầu tiên sẽ duyệt qua lần lượt các phần tử của **plain**, rồi 
lấy thự tự của các chữ cái đó ở trong bảng Unicode, rồi chuyển sang dạng 8 bit nhị phân. Sau đó sẽ tách 4 bit đầu và 4 bit sau, chuyển 4 bit
đó sang dạng thập phân, rồi lấy đó làm index và trả về chữ cái tại index đó (**ALPHABET[i]**), với **ALPHABET** là chuỗi gồm 16 chữ cái đầu 
của bảng chữ cái:


![image](https://github.com/m01000xd/picoCTF/assets/122852491/8f387268-a51b-4ffd-8420-e155ac4399e0)


Sau đó tạo 1 biến **enc** là một chuỗi rỗng, và thêm lần lượt các chữ cái được trả về ở trên vào **enc**. Hàm cuối cùng sẽ trả về **enc**.

* **shift(c,k)**: Hàm **shift(c,k)**, truyền vào 2 arguments c, k. Hàm trả về **ALPHABET[(t1 + t2) % len(ALPHABET)]**, với **t1 = ord(c) - ord('a')**,
**t2 = ord(k) - ord('a')**

Cuối cùng, để ra được một chuỗi kí tự mà chal đã cho, ta lại qua thêm một vòng for nữa:

![image](https://github.com/m01000xd/picoCTF/assets/122852491/b2cbc393-06b5-424a-a117-01bda74c37b4)

Đầu tiên, tạo một biến **b16 = b16_encode(flag)**, với arguments truyền vào là **flag**, Sau đó duyệt b16 qua 1 vòng for, dùng hàm **shift(c,k)**,
với **c** là các phần tử của **b16**, k là **key**. Để ý thấy:

![image](https://github.com/m01000xd/picoCTF/assets/122852491/ea41fe2c-7aec-4e8c-a31b-1a65c26d1378)

Key của chúng ta chỉ nằm trong 16 chữ cái của **ALPHABET**, và key chỉ gồm 1 chữ cái. Sau khi qua hàm **shift()**, tạo 1 biến **enc**, get hết chữ cái
mà hàm **shift()** trả về, rồi in ra màn hình chính là chuỗi kí tự mà chal đã cho.

Giờ chúng ta có **enc**, giờ để lần ngược ra **flag** thì ta phải tìm được **b16**

Để ý rằng, nếu ta có **b16**, với key = x, qua hàm **shift()**, ta được **enc**. Nhưng ngược lại, ta có **enc**, vẫn key = x, qua hàm **shift()**, ta vẫn thu lại được **b16**:

![image](https://github.com/m01000xd/picoCTF/assets/122852491/0e58f3be-0653-42c7-bdce-a7bf5fc138dd)

![image](https://github.com/m01000xd/picoCTF/assets/122852491/de953941-4d85-48a3-a28d-b39ce14d86ab)

Vì vậy, với ràng buộc **key** chỉ gồm 1 chữ cái và 1 trong 16 chữ cái của **ALPHABET**, ta sẽ brute force để lần ra flag:

![image](https://github.com/m01000xd/picoCTF/assets/122852491/9d8de116-ecf2-4aaa-960e-9b68f583a7ad)

![image](https://github.com/m01000xd/picoCTF/assets/122852491/70494c78-c6e3-49f7-92c8-24dd083035f0)

Flag của chúng ta nằm trong 16 trường hợp trên. Thử lần lượt từng cái 1, và flag của chúng ta:

* **Flag**: **picoCTF{et_tu?_0797f143e2da9dd3e7555d7372ee1bbe}**



