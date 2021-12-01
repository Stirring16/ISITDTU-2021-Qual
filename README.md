# ISITDTU-2021-Qual

![image](https://user-images.githubusercontent.com/62060867/144184344-12803ba2-3cf7-4621-9e08-092218afe9cf.png)


# CHARACTERS
>The perfect change. Good luck :))

>Format flag: isitdtu{}

>[Link Download](https://drive.google.com/file/d/1CEJ-Ph10ja6wxf5oGGTbIvejtU-mEpEn/view)


Đề cho mình file `rar`

```
┌──(stirring㉿Stirring)-[Desktop/ISITDTU/MISC]
└─$ unrar e Characters.rar

UNRAR 6.02 freeware      Copyright (c) 1993-2021 Alexander Roshal
Extracting from Characters.rar
Extracting  Go find the flag.txt                                      OK
Extracting  Hex_Base.rar                                              OK
All OK

┌──(stirring㉿Stirring)-[Desktop/ISITDTU/MISC]
└─$ ls
 Characters.rar  'Go find the flag.txt'   Hex_Base.rar
```

Sau khi giải nén mình có 2 file `Go find the flag.txt` và `Hex_Base.rar`

> * File Go find the flag.txt

```
ak ak ak ak ak ak ak ak ak ak pika pipi ak pipi ak ak ak pipi ak ak ak ak ak ak ak pipi ak ak ak ak ak ak ak ak ak ak pichu pichu pichu pichu ka chu pipi pipi pipi ak ak ak ak ak ak ak ak ak ak ak ak ak ak pikachu pipi ak ak ak ak pikachu ka ka ka pikachu pichu pichu ak ak pikachu pipi pipi ak ak ak ak ak ak ak ak ak ak ak pikachu pichu ak ak ak ak ak ak ak ak ak ak ak ak ak pikachu pipi ak ak ak pikachu pikachu ak ak ak ak pikachu ka ka ka ka ka ka ka ka pikachu ak ak ak pikachu pichu ak ak ak pikachu pichu pikachu pipi ak ak ak ak ak pikachu pipi ak pikachu pichu pichu ak ak ak ak ak ak ak ak ak ak ak ak ak ak ak ak ak ak ak ak ak ak ak ak ak ak pikachu pichu ak ak ak ak ak ak ak ak ak ak ak ak ak ak ak ak ak ak ak ak ak ak pikachu pipi ka ka ka pikachu pikachu ka ka ka ka ka pikachu ka pikachu ka pikachu
```
Ta có thể thấy file này chỉ gồm các chữ: `ak`, `ka`, `chu`, `pika`, `pipi`, `pikachu`, `pichu`, `pikapi`

Đây là ngôn ngữ `Pikalang` (là một ngôn ngữ lập trình dựa trên ngôn ngữ BrainFuck nhưng có các toán tử được thay thế bằng tiếng kêu của Pokemon Pikachu)

| BrainFuck | Pikalang | 
| ----------- | ----------- | 
| + | pi  | 
| - | ka | 
| > | pipi | 
| < | pichu | 
| [ | pika | 
| ] | chu | 
| . | pikachu | 
| , |  pikapi |

Trong file ` Go to the flag.txt` có từ `ak` không hề có trong table và trong file không có `pi` nên mình đã replace all `ak` -> `pi`

```
pi pi pi pi pi pi pi pi pi pi pika pipi pi pipi pi pi pi pipi pi pi pi pi pi pi pi pipi pi pi pi pi pi pi pi pi pi pi pichu pichu pichu pichu ka chu pipi pipi pipi pi pi pi pi pi pi pi pi pi pi pi pi pi pi pikachu pipi pi pi pi pi pikachu ka ka ka pikachu pichu pichu pi pi pikachu pipi pipi pi pi pi pi pi pi pi pi pi pi pi pikachu pichu pi pi pi pi pi pi pi pi pi pi pi pi pi pikachu pipi pi pi pi pikachu pikachu pi pi pi pi pikachu ka ka ka ka ka ka ka ka pikachu pi pi pi pikachu pichu pi pi pi pikachu pichu pikachu pipi pi pi pi pi pi pikachu pipi pi pikachu pichu pichu pi pi pi pi pi pi pi pi pi pi pi pi pi pi pi pi pi pi pi pi pi pi pi pi pi pi pikachu pichu pi pi pi pi pi pi pi pi pi pi pi pi pi pi pi pi pi pi pi pi pi pi pikachu pipi ka ka ka pikachu pikachu ka ka ka ka ka pikachu ka pikachu ka pikachu
```
Mình dùng https://www.dcode.fr/pikalang-language để giải mã, decode ta được:

```
The password is: 77210
```

> * file Hex_rar.rar

Một file rar có password, tất nhiên password ta đã có được ở trên `77210`

```
┌──(stirring㉿Stirring)-[Desktop/ISITDTU/MISC]
└─$ unrar x Hex_Base.rar -p77210

UNRAR 6.02 freeware      Copyright (c) 1993-2021 Alexander Roshal

Extracting from Hex_Base.rar

466f726d61742074686520666c6167206279206368617261637465723a20377b375f325f31307d

Creating    Hex_Base                                                  OK
Extracting  Hex_Base/Player.txt                                       OK
All OK
```
Khi giải nén với password mình có được 1 file `Player.txt` và 1 đoạn archive comments `466f726d61742074686520666c6167206279206368617261637465723a20377b375f325f31307d`

Decode đoạn `ASCII` ta có: `Format the flag by character: 7{7_2_10}`

Và ta có file Player.txt:

```
cat Player.txt
IFdlbGNvbWUgdG8gSVNJVERUVSBDVEYgMjAyMSEKIEhlcmUgaXMgYSBsaXN0IG9mIHRoZSBwbGF5ZXJzIEkgZm91bmQKLS0tLS0tLS0tLSAtLS0tLS0tLS0tLQpELiBBbGxpCkYuIFRvcnJlcwpDLiBIdWRzb24tT2RvaQpNLiBNb3VudApNLiBSYXNoZm9yZApDLiBMZW5nbGV0ClIuIEx1a2FrdQpBLiBHcmllem1hbm4KQy4gUm9uYWxkbwpELiBQYXlldApKLiBLaW1taWNoCkwuIE1lc3NpCkcuIEhpZ3VhaW4KUC4gQXViYW1leWFuZwpYYXZpClMuIFJhbW9zCkUuIENhdmFuaQpQLiBMYWhtCksuIFRyYXBwCkouIFNhbmNobwpULiBLcm9vcwpLLiBOYXZhcwpNLiBJY2FyZGkKSi4gVGl2YWIKSy4gU2NobWVpY2hlbApHLkJhbGUKLS0tLS0tLS0tLSAtLS0tLS0tLS0tLQpUaGUgRW5kCiEgIQoK
```
Encode bằng base64, decode:

```
Welcome to ISITDTU CTF 2021!
 Here is a list of the players I found
---------- -----------
D. Alli
F. Torres
C. Hudson-Odoi
M. Mount
M. Rashford
C. Lenglet
R. Lukaku
A. Griezmann
C. Ronaldo
D. Payet
J. Kimmich
L. Messi
G. Higuain
P. Aubameyang
Xavi
S. Ramos
E. Cavani
P. Lahm
K. Trapp
J. Sancho
T. Kroos
K. Navas
M. Icardi
J. Tivab
K. Schmeichel
G.Bale
---------- -----------
The End
! !
```

Nhìn lại format flag mà ta có ở trên: `7{7_2_10}`, tổng các kí tự là 26 trùng với số lượng cầu thủ

Nhìn sơ ta có thể thấy ngay các kí tự cuối cùng của tên các cầu thủ khi join lại ta được flag: isitdtu{nothing_is_possible}


