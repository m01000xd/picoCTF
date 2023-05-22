![image](https://github.com/m01000xd/picoCTF/assets/122852491/2c126ff5-b04a-4d11-ae9c-668e1864603d)

Mở file otp.py:

![image](https://github.com/m01000xd/picoCTF/assets/122852491/bb270313-dc01-49d1-98ae-5e591f3f5f19)

Ta thấy ở hàm startup, nó in ra flag bằng cách xor lần lượt giá trị các phần tử của flag với giá trị key tương ứng, mà key của chúng ta dài 50000 kí tự.

Chạy netcat trên server:

![image](https://github.com/m01000xd/picoCTF/assets/122852491/5821b5ca-76ee-4966-8baa-61ba80faa9ae)


Flag của chúng ta được trả về bằng cách chuyển sang dạng hexan rồi bỏ đi 2 kí tự 0x ở đầu, chỉ giữ lại 2 kí tự cuối. Vì kết quả trả về dài 64 kí tự,
nên flag của chúng ta có độ dài 32 kí tự. 

Ngay sau khi in ra màn hình kết quả flag cho chúng ta, chương trình sẽ tiếp tục chạy hàm encrypt(key_location), với tham số trả về là key_location có được
ở hàm startup().

![image](https://github.com/m01000xd/picoCTF/assets/122852491/e939f8b5-48c6-40d3-8016-1c9150ce1129)

Ta thấy, key của chúng ta sẽ trở về ban đầu nếu stop >= key_len = 50000. Vì khi in ra màn hình flag bị mã hóa, key_location của chúng ta đã là = 32,
vậy nên để key quay về ban đầu, thì ta sẽ nhập thêm 49968 kí tự nữa, rồi lại tiếp tục nhập 32 kí tự nữa để tìm được key ban đầu:

Netcat trên server:

![image](https://github.com/m01000xd/picoCTF/assets/122852491/011e7f6d-39f3-453b-a891-d3bac87c2690)

Ở đây mình nhập chuỗi s gồm 32 kí tự a, và nó trả về chuỗi s được mã hóa. Vì chuỗi s được mã hóa bằng cách xor lần lượt từng phần tử của s với
giá trị của key, nên key sẽ bằng s ^ s được mã hóa. Khi có key, chúng ta sẽ đem xor với flag được mã hóa sẽ trả về flag ban đầu. Ta sẽ dùng Python:

![image](https://github.com/m01000xd/picoCTF/assets/122852491/90b5532f-dea4-427f-810c-bd27ef9dac16)

**Flag:** **picoCTF{abf2f7d5edf082028076bfd7a4cfe9a9}**

