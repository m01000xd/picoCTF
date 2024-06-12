File elf

![image](https://github.com/m01000xd/picoCTF/assets/122852491/263b3dca-efb0-4724-834f-1138b7b0d425)
Ném vào IDA:

![image](https://github.com/m01000xd/picoCTF/assets/122852491/952b22d5-297f-4458-8953-cb3acae1b542)

Chương trình bắt nhập input đầu vào với độ dài 50 kí tự, sau đó tạo các biến hằng số ```secret1```, ```secret2```, ```secret3```, ```fix```.

![image](https://github.com/m01000xd/picoCTF/assets/122852491/1de82fdc-d43f-4ab6-aef8-2f2559c5e9d6)

Vòng lặp:

![image](https://github.com/m01000xd/picoCTF/assets/122852491/c7738121-7564-485d-b437-5c452f20816b)

Trông khá đơn giản, với mỗi giá trị của input được mã hóa bằng cách lấy giá trị input ban đầu cộng với hằng số và lấy phần dư với 0x1A: ```((random2 & secret3) + input[i_0] - fix + (secret3 & (random2 >> 4)))``` rồi cộng với ```fix```

Lặp lại ba lần như vây, sẽ đem input so với output là chuỗi ```mpknnphjngbhgzydttvkahppevhkmpwgdzxsykkokriepfnrdm```. Nếu đúng thì input chính là password đúng
và flag sẽ có dạng ```picoctf{password}```

Nhìn vào output thì có thể thấy đều là chữ cái thường, và phép mã hóa sau khi mình thử đặt breakpoint ở mỗi vòng thử với các chữ cái thường thì
output cũng trả về chữ cái thường, không lệch đi quá xa(sang chữ in hoa hoặc chữ số).

Nhìn qua ta đã có sơ qua hướng giải là brute force các kí tự input từ 0-255. Nhưng như phân tích ở trên, ta có thể nghĩ ưu tiên đến trường hợp input chỉ gồm
các chữ cái thường.

### Script
```python3
from pwn import *

secret1 = 0x55
secret2 = 0x33
secret3 = 0xf
fix = 0x61

output = "mpknnphjngbhgzydttvkahppevhkmpwgdzxsykkokriepfnrdm"
s = ''
for i in range(50):
    random1 = (secret1 & (i % 0xFF)) + (secret1 & ((i % 0xFF) >> 1))
    random2 = (random1 & secret2) + (secret2 & (random1 >> 2))
    
    for k in range(97,123):
        a = ((random2 & secret3) + k - fix + (secret3 & (random2 >> 4))) % 0x1a + fix
        b = ((random2 & secret3) + a - fix + (secret3 & (random2 >> 4))) % 0x1a + fix
        c = ((random2 & secret3) + b - fix + (secret3 & (random2 >> 4))) % 0x1a + fix
        if c == ord(output[i]):
            s += chr(k)
    

target = remote('titan.picoctf.net' ,52483)
target.send(s)
print(target.recvline())
```
![image](https://github.com/m01000xd/picoCTF/assets/122852491/de243e3b-2154-4a8b-a802-7e80d7c1a829)

### Flag
    
    picoCTF{s0lv3_angry_symb0ls_ddcc130f}
