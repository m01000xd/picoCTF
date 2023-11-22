![image](https://github.com/m01000xd/picoCTF/assets/122852491/5ff669b2-0a92-4d9d-9f08-3dc09dd12b27)

Đề cho chúng ta 1 file elf và 1 file txt.
Ném file elf lên ida:

![image](https://github.com/m01000xd/picoCTF/assets/122852491/e07f729f-3dee-4dba-b216-7bf02ec36c47)

![image](https://github.com/m01000xd/picoCTF/assets/122852491/c2085804-d495-45f5-9845-553dee98b3ba)

Đọc 1 lượt đoạn code, đề bắt chúng ta nhập vào 100 kí tự, và sau 1 loạt các thuật toán, sẽ trả về 1 chuỗi, và kiểm tra xem chuỗi này có 
phải chuỗi mà đề đã cho không, nếu đúng thì chuỗi ban đầu mình nhập chính là key sẽ đem xor với 100 kí tự trong file txt sẽ trả về flag.

Thuật toán gồm 2 hàm chính:

* valid_char(): kiểm tra kí tự nhập vào có thuộc 'abcdef01234356789' hay không
![image](https://github.com/m01000xd/picoCTF/assets/122852491/cd6a7301-87f6-4bfe-9252-e01652bb0c85)


* jumble(): mã hóa chuỗi ban đầu thành 1 chuỗi khác:


![image](https://github.com/m01000xd/picoCTF/assets/122852491/3a3ba5fb-20b6-42dd-a18d-af3d6b93b4c9)


Thực chất 100 kí tự viết liền thực ra là 50 kí tự được chuyển sang hệ 16 rồi join lại. Giờ ta sẽ bắt tay code lại để ra được flag:

```
valid_char = "abcdef0123456789"
dest = [0]*100
s1 = [0]*100

def jumble(ch):
    a1 = ord(ch)
    if (a1 > 96):
        a1 += 9
    v3 = 2 *(a1 % 16)
    if (v3 > 15):
        v3 += 1
    return v3


s = "adpjkoadapekldmpbjhjhbaghlfldbhjdalgnbeedheenfoeddabpmdnliokcahomdphbcleipfgibjdcgmjcmadaomiakpdjcni"
hex_flag = "790ce176acf7c2b277040687b23e185b2bb0d0fcc1939bf782db10c1210218dc4b2b3c931a5c2f04ad5aa711d04175920aa0"

i = 0
while i <= 99:
    for character in valid_char:
        if i == 0:
            s1[0] = chr(jumble(character) % 16)
            if ord(s1[0]) + 97 == ord(s[0]):
                dest[0] = character
                i = i + 1
        else:
            v4 = jumble(character)
            v5 = ord(s1[i-1]) + v4
            v6 = ((ord(s1[i-1]) + v4) >> 31) >> 28
            s1[i] = chr(((v6 + v5) & 0xf) - v6)
            if ord(s1[i]) + 97 == ord(s[i]):
                dest[i] = character
                i = i + 1

flag = ''
for i in range(0,len(s), 2):
    flag += chr(int(s[i:i+2], 16) ^ int(hex_flag[i:i+2], 16))
print(flag)
```

**Flag:**

```
picoCTF{cust0m_jumbl3s_4r3nt_4_g0Od_1d3A_db877006}
```



  
