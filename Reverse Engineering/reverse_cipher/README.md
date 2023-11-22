![image](https://github.com/m01000xd/picoCTF/assets/122852491/9c2add60-3c86-4b86-93c1-d05325501f86)

Bài này khá đơn giản. Đề cho 1 đoạn flag bị mã hóa bởi 2 vòng for xáo vị trí các kí tự:

![image](https://github.com/m01000xd/picoCTF/assets/122852491/643900bf-ba4e-4c08-9dec-4cce51627007)

Giờ ta chỉ cần viết 1 đoạn code check ngược lại điều kiện để ra được flag ban đầu.

```python
flag_encode = 'picoCTF{w1{1wq84fb<1>49}'
flag = 'picoCTF{'
for j in range(8, 23):
    v11 = ord(flag_encode[j])
    if ((j & 1) != 0):
        v11 += 2
        flag += chr(v11)
    else:
        v11 -= 5
        flag += chr(v11)
flag += '}'
print(flag)
```
**Flag:**
```python
picoCTF{r3v3rs36ad73964}
```
