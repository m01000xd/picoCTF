![image](https://github.com/m01000xd/picoCTF/assets/122852491/64ff3ebc-76bb-4a0c-a2f0-017bd64cc85f)

Check file trong Kali, đây là file elf

![image](https://github.com/m01000xd/picoCTF/assets/122852491/3868341f-3312-413a-9901-f41bdffb6723)


Ta ném file lên Ghidra để decompile:

![image](https://github.com/m01000xd/picoCTF/assets/122852491/39cd5e25-869e-4a94-8c74-82dda20823b7)

Trong các hàm ở mục Functions, ta thấy có 1 hàm là main.checkPassword:

![image](https://github.com/m01000xd/picoCTF/assets/122852491/3b1a515a-91a5-48e2-8b1f-891086016764)

Ở bên phần decompile:

![image](https://github.com/m01000xd/picoCTF/assets/122852491/43da1a65-b76c-4ac0-b4f7-f24e2ffc25bc)


Ta thấy, ban đầu, iVar3 = 0, ở trong vòng while có check (iVar3 == 0x20 = 32) thì sẽ return. Tức là password của chúng ta sẽ có 32 kí tự.

Xuống đến đây: 

![image](https://github.com/m01000xd/picoCTF/assets/122852491/813a88a3-0617-4864-a3f7-7276a65d5507)


Ta thấy password của chúng ta sẽ được tạo bằng cách xor các phần tử ở 2 mảng 

![image](https://github.com/m01000xd/picoCTF/assets/122852491/34b29e6d-756a-452e-b4c8-293945f41309)


Phần code assembly cho thấy password sẽ được tạo bằng cách xor lần lượt các giá trị ở 2 thanh ghi ebp và esi

Giờ ta sẽ dùng gdb đặt breakpoint để debug file này

![image](https://github.com/m01000xd/picoCTF/assets/122852491/6d3fbf6c-0fb7-4c55-9a53-4c5090a4af9e)


Đặt breakpoint

![image](https://github.com/m01000xd/picoCTF/assets/122852491/ed2c274a-0007-4e2a-b624-a97b446ed04c)

Ở đây bắt chúng ta nhập mật khẩu, mật khẩu có 32 kí tự, giờ ta sẽ thử nhập bừa 1 chuỗi từ 1 -> 0 + 2 sô 1 và 2 cho đủ 32 kí tư: 

![image](https://github.com/m01000xd/picoCTF/assets/122852491/c9533c8a-7324-4215-bb27-6d24b0bf6f40)


Debug thành công, giờ chúng ta sẽ lấy giá trị hex ở 2 mảng đem đi unhex rồi xor với nhau sẽ được password.

![image](https://github.com/m01000xd/picoCTF/assets/122852491/28782dbb-922e-45b8-a4ef-8f539a24474a)
 

Nháy vào local_40 và local_20, ta thấy giá trị hex được lưu tại 2 địa chỉ esp+0x4 và esp+0x24, vậy ta sẽ dùng hexdump để lấy các giá trị hex ra:

![image](https://github.com/m01000xd/picoCTF/assets/122852491/e72b1348-6e06-4910-9952-16219ab37fee)

Vậy ta được 2 giá trị hex: "3836313833366631336533643632376466613337356264623833383932313465" và "4a53475d414503545d025a0a5357450d05005d555410010e4155574b45504601"

Giờ ta sẽ dùng Python unhex và xor 2 giá trị này sẽ được password:

![image](https://github.com/m01000xd/picoCTF/assets/122852491/9139daef-ef7b-4b29-b33d-5abb99df9365)

Password của chúng ta là: **reverseengineericanbarelyforward**

Giờ ta sẽ chạy server mà chall đã đưa và nhập password

![image](https://github.com/m01000xd/picoCTF/assets/122852491/67aa50f9-3681-4cbc-bd45-164ad74f7e31)

Giờ chúng ta cần phải biết được key đã unhash thì mới ra được flag. Để ý thấy giá trị hex được unhash ở địa chỉ $esp+0x4 giống như 1 key bị hash, ta search trên google:

![image](https://github.com/m01000xd/picoCTF/assets/122852491/be2aaec2-07ec-4b25-bc5e-74fa2af9e0de)

Key được mã hóa bằng MD5

![image](https://github.com/m01000xd/picoCTF/assets/122852491/7e0fbb8f-d419-446f-b7d3-ab176b806cfc)

Vì vậy key ban đầu là **goldfish**

![image](https://github.com/m01000xd/picoCTF/assets/122852491/ee207ee3-5385-4648-b6c6-2d1a5749b389)


**Flag :** **picoCTF{p1kap1ka_p1c001b3038b}**

