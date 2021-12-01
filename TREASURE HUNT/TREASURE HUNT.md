# TREASURE HUNT
> It looks like the Mortar Hat Pirates are on their way to find the cursed treasure but,it seems they are having a bit of a problem please help them and find the treasure together.

> The flag format has the form: ISITDTU{md5(KEYOFTREASURE_NAMEOFTREASURE_ITEMINSIDETREASURE)}

          
           + KEYOFTREASURE: Fruitevil
           
           + NAMEOFTREASURE: Dark Lmao

           + ITEMINSIDETREASURE: ISITDTU{XXX_XXX_XXX_XXX}
           
           + ISITDTU{md5(Fruitevil_DarkLmao_XXX_XXX_XXX_XXX)}
       

> Note: Because the treasure is cursed if you enter it wrongs 10 times you will no longer have the opportunity to open it so be careful

[Link Download](https://drive.google.com/file/d/1Vymh6zux_EHb3hVAcVM1vRVUhuCGCIXm/view?usp=sharing)

# Analysis

```
┌──(kali㉿kali)-[~/Desktop/ISITDTU/Treasure]
└─$ file MON3TR-OP-20211122-082508.raw 
MON3TR-OP-20211122-082508.raw: data
```

Đề cho ta file raw, tiến hành phân tích bằng `volatility`

Đầu tiên mình xác định profile

```
┌──(kali㉿kali)-[~/Desktop/ISITDTU/Treasure]
└─$ ../../Tools/volatility/Vol.py -f MON3TR-OP-20211122-082508.raw imageinfo
Volatility Foundation Volatility Framework 2.6.1
INFO    : volatility.debug    : Determining profile based on KDBG search...
          Suggested Profile(s) : Win7SP1x64, Win7SP0x64, Win2008R2SP0x64, Win2008R2SP1x64_24000, Win2008R2SP1x64_23418, Win2008R2SP1x64, Win7SP1x64_24000, Win7SP1x64_23418
                     AS Layer1 : WindowsAMD64PagedMemory (Kernel AS)
                     AS Layer2 : FileAddressSpace (/home/kali/Desktop/ISITDTU/Treasure/MON3TR-OP-20211122-082508.raw)
                      PAE type : No PAE
                           DTB : 0x187000L
                          KDBG : 0xf800029fc0a0L
          Number of Processors : 4
     Image Type (Service Pack) : 1
                KPCR for CPU 0 : 0xfffff800029fdd00L
                KPCR for CPU 1 : 0xfffff880009f1000L
                KPCR for CPU 2 : 0xfffff88002f6a000L
                KPCR for CPU 3 : 0xfffff88002fe1000L
             KUSER_SHARED_DATA : 0xfffff78000000000L
           Image date and time : 2021-11-22 08:25:09 UTC+0000
     Image local date and time : 2021-11-22 00:25:09 -0800
```

Xác định các tiến trình đang chạy 

```
┌──(kali㉿kali)-[~/Desktop/ISITDTU/Treasure]
└─$ ../../Tools/volatility/Vol.py -f MON3TR-OP-20211122-082508.raw --profile=Win7SP1x64 pstree
Volatility Foundation Volatility Framework 2.6.1
Name                                                  Pid   PPid   Thds   Hnds Time
-------------------------------------------------- ------ ------ ------ ------ ----
 0xfffffa8030fd07b0:csrss.exe                         388    376      9    349 2021-11-22 07:57:41 UTC+0000
. 0xfffffa80311bb630:conhost.exe                      780    388      2     51 2021-11-22 08:25:05 UTC+0000
. 0xfffffa80313c1b30:conhost.exe                     3148    388      2     51 2021-11-22 08:01:17 UTC+0000
 0xfffffa8032d36060:winlogon.exe                      424    376      5    118 2021-11-22 07:57:41 UTC+0000
 0xfffffa80321a5b30:csrss.exe                         328    312      9    354 2021-11-22 07:57:41 UTC+0000
 0xfffffa8032c83b30:wininit.exe                       368    312      3     81 2021-11-22 07:57:41 UTC+0000
. 0xfffffa8032cb5b30:lsass.exe                        484    368      7    571 2021-11-22 07:57:41 UTC+0000
. 0xfffffa8032cb4370:lsm.exe                          492    368     11    144 2021-11-22 07:57:41 UTC+0000
. 0xfffffa8032509b30:services.exe                     468    368      8    206 2021-11-22 07:57:41 UTC+0000
.. 0xfffffa8032ef7b30:spoolsv.exe                    1036    468     12    281 2021-11-22 07:57:42 UTC+0000
.. 0xfffffa8031077060:sppsvc.exe                     1300    468      4    149 2021-11-22 07:59:43 UTC+0000
.. 0xfffffa8032dffb30:svchost.exe                     664    468      7    252 2021-11-22 07:57:41 UTC+0000
.. 0xfffffa8032e57320:svchost.exe                     796    468     18    451 2021-11-22 07:57:41 UTC+0000
... 0xfffffa8033138470:dwm.exe                       1552    796      3     75 2021-11-22 07:57:42 UTC+0000
.. 0xfffffa8033358b30:svchost.exe                    2720    468      6    101 2021-11-22 07:58:57 UTC+0000
.. 0xfffffa8031099b30:svchost.exe                    1284    468     12    327 2021-11-22 07:59:44 UTC+0000
.. 0xfffffa8032f04b30:svchost.exe                     336    468     16    383 2021-11-22 07:57:42 UTC+0000
.. 0xfffffa8033110b30:taskhost.exe                   1460    468      7    186 2021-11-22 07:57:42 UTC+0000
.. 0xfffffa803323db30:SearchIndexer.                 1220    468     14    655 2021-11-22 07:57:49 UTC+0000
.. 0xfffffa80332eb1d0:wmpnetwk.exe                   1292    468      9    217 2021-11-22 07:57:49 UTC+0000
.. 0xfffffa8032e38b30:svchost.exe                     848    468     34    964 2021-11-22 07:57:41 UTC+0000
.. 0xfffffa8032ed72b0:svchost.exe                     980    468     10    278 2021-11-22 07:57:42 UTC+0000
.. 0xfffffa803301bb30:svchost.exe                    1192    468     16    288 2021-11-22 07:57:42 UTC+0000
.. 0xfffffa8032d84b30:svchost.exe                     592    468     11    360 2021-11-22 07:57:41 UTC+0000
.. 0xfffffa8032e26630:svchost.exe                     744    468     20    495 2021-11-22 07:57:41 UTC+0000
... 0xfffffa80312ff060:audiodg.exe                   3728    744      6    133 2021-11-22 08:25:02 UTC+0000
.. 0xfffffa8032f9db30:svchost.exe                    1064    468     19    337 2021-11-22 07:57:42 UTC+0000
 0xfffffa8030eb7890:System                              4      0     89    507 2021-11-22 07:57:40 UTC+0000
. 0xfffffa80323fa810:smss.exe                         248      4      2     32 2021-11-22 07:57:40 UTC+0000
 0xfffffa803302e4a0:GoogleCrashHan                   1956   1784      5     97 2021-11-22 07:57:43 UTC+0000
 0xfffffa8033168b30:GoogleCrashHan                   1948   1784      5    104 2021-11-22 07:57:43 UTC+0000
 0xfffffa803314ab30:explorer.exe                     1596   1508     30    931 2021-11-22 07:57:42 UTC+0000
. 0xfffffa8032eaa060:DumpIt.exe                      2948   1596      1     25 2021-11-22 08:25:05 UTC+0000
. 0xfffffa8032c4e520:mspaint.exe                     2692   1596      7    282 2021-11-22 07:58:57 UTC+0000
. 0xfffffa80313a8920:cmd.exe                         3140   1596      1     19 2021-11-22 08:01:17 UTC+0000
. 0xfffffa80310e7060:chrome.exe                      2420   1596     30    881 2021-11-22 07:59:47 UTC+0000
.. 0xfffffa80312473b0:chrome.exe                     3100   2420     14    170 2021-11-22 08:24:39 UTC+0000
.. 0xfffffa80310f4650:chrome.exe                     3000   2420     14    191 2021-11-22 07:59:53 UTC+0000
.. 0xfffffa803114cb30:chrome.exe                      564   2420     13    222 2021-11-22 07:59:49 UTC+0000
.. 0xfffffa8031111b30:chrome.exe                     1332   2420      6    132 2021-11-22 07:59:49 UTC+0000
.. 0xfffffa8031308ab0:chrome.exe                     2388   2420     15    405 2021-11-22 08:00:56 UTC+0000
.. 0xfffffa80314bb060:chrome.exe                     3576   2420     15    234 2021-11-22 08:24:24 UTC+0000
.. 0xfffffa8031098060:chrome.exe                     1512   2420      8     98 2021-11-22 07:59:47 UTC+0000
.. 0xfffffa8031369060:chrome.exe                     1916   2420      9    177 2021-11-22 08:01:02 UTC+0000
 0xfffffa80324ee530:notepad.exe                      2564   2520      1     61 2021-11-22 07:58:46 UTC+0000
```

Ở đây mình thấy được các tiến trình đáng nghi ở đây là: `cmd.exe`, `chrome.exe``notepad.exe`


```
┌──(kali㉿kali)-[~/Desktop/ISITDTU/Treasure]
└─$ ../../Tools/volatility/Vol.py -f MON3TR-OP-20211122-082508.raw --profile=Win7SP1x64 cmdline 
Volatility Foundation Volatility Framework 2.6.1

************************************************************************
notepad.exe pid:   2564
Command line : "C:\Windows\system32\NOTEPAD.EXE" C:\Users\Mon3tr\Desktop\P@55WORD.py
************************************************************************

```


Ta có 1 file `P@55WORD.py`, dùng filescan để tìm offset của file py
```
┌──(kali㉿kali)-[~/Desktop/ISITDTU/Treasure]
└─$ ../../Tools/volatility/Vol.py -f MON3TR-OP-20211122-082508.raw --profile=Win7SP1x64 filescan | grep py
Volatility Foundation Volatility Framework 2.6.1
0x000000007e1f3b70     16      0 R--rw- \Device\HarddiskVolume2\Users\Mon3tr\Desktop\P@55WORD.py
0x000000013ee7db70     16      0 R--rw- \Device\HarddiskVolume2\Users\Mon3tr\Desktop\P@55WORD.py

```
Dùng dumpfiles để dump file này ra

```
┌──(kali㉿kali)-[~/Desktop/ISITDTU/Treasure]
└─$ ../../Tools/volatility/Vol.py -f MON3TR-OP-20211122-082508.raw --profile=Win7SP1x64 dumpfiles  --dump-dir=file -Q 0x000000013ee7db70
Volatility Foundation Volatility Framework 2.6.1

DataSectionObject 0x13ee7db70   None   \Device\HarddiskVolume2\Users\Mon3tr\Desktop\P@55WORD.py
                                                                                                
```
```

┌──(kali㉿kali)-[~/Desktop/ISITDTU/Treasure/file]
└─$ cat file.None.0xfffffa8032b5a2b0.dat 
#Looks like the treasure key is cursed we need to know the SECRET to break this curse
#1e19724f1e0605071f1c151a79556e4c7759654971486441765e6b253f2c3a467a34411f0f0c6c4b137e374e735465283c282c3443167135235704

import hashlib
import sys
import secret

def xr(s1,s2):
    return ''.join(chr(ord(a) ^ ord(b)) for a,b in zip(s1,s2))

def repeat(s, l):
    return (s*(int(l/len(s))+1))[:l]
key = secret.key
plaintext = secret.password + key
plaintext += hashlib.md5(plaintext.encode()).hexdigest()
cipher = xr(plaintext, repeat(key, len(plaintext))).encode()

a = []
for i in cipher:
    a.append(i)
c = b''
for i in range(len(a)):
    if i > 0:
        a[i] ^= a[i-1]
    a[i] ^= a[i] >> 4;
    a[i] ^= a[i] >> 3;
    a[i] ^= a[i] >> 2;
    a[i] ^= a[i] >> 1;
    c += chr(a[i]).encode()
c = c.hex()
print("Enc: " + c)

#lalalalalalalalalalalalalalalalalalallalala
```
Như comment trên đoạn code `Looks like the treasure key is cursed we need to know the SECRET to break this curse` ta còn thiếu 1 `SECRET` để lấy được key của đoạn python này

Tiếp tục tìm tìm kiếm

```
┌──(kali㉿kali)-[~/Desktop/ISITDTU/Treasure]
└─$ ../../Tools/volatility/Vol.py -f MON3TR-OP-20211122-082508.raw --profile=Win7SP1x64 iehistory         
Volatility Foundation Volatility Framework 2.6.1

**************************************************
Process: 1596 explorer.exe
Cache type "URL " at 0x2965700
Record length: 0x100
Location: :2021111520211122: Mon3tr@file:///C:/Users/Mon3tr/Desktop/S3cr3t.txt
Last modified: 2021-11-21 07:27:00 UTC+0000
Last accessed: 2021-11-22 08:08:54 UTC+0000
File Offset: 0x100, Data Offset: 0x0, Data Length: 0x0
**************************************************
...........................................
**************************************************
Process: 1596 explorer.exe
Cache type "URL " at 0x2966700
Record length: 0x100
Location: :2021111520211122: Mon3tr@file:///C:/Users/Mon3tr/Desktop/1P.png
Last modified: 2021-11-21 23:59:09 UTC+0000
Last accessed: 2021-11-22 08:08:54 UTC+0000
File Offset: 0x100, Data Offset: 0x0, Data Length: 0x0
```
Check history mình thấy được 1 file `S3cr3t.txt` và `1P.png`. Tiếp tục dùng filescan để kiếm 2 file này

```
┌──(kali㉿kali)-[~/Desktop/ISITDTU/Treasure]
└─$ ../../Tools/volatility/Vol.py -f MON3TR-OP-20211122-082508.raw --profile=Win7SP1x64 filescan | grep 1P.png          148 ⨯ 2 ⚙
Volatility Foundation Volatility Framework 2.6.1
0x0000000051348f20     16      0 R--r-d \Device\HarddiskVolume2\Users\Mon3tr\Desktop\1P.png
0x000000013eab4f20     15      0 R--r-d \Device\HarddiskVolume2\Users\Mon3tr\Desktop\1P.png
```

Dùng filescan ta chỉ tìm thấy được file `1p.png`, dump file xem ta có gì

```
┌──(kali㉿kali)-[~/Desktop/ISITDTU/Treasure]
└─$ ../../Tools/volatility/Vol.py -f MON3TR-OP-20211122-082508.raw --profile=Win7SP1x64 dumpfiles  --dump-dir=file -Q 0x0000000051348f20
Volatility Foundation Volatility Framework 2.6.1
DataSectionObject 0x51348f20   None   \Device\HarddiskVolume2\Users\Mon3tr\Desktop\1P.png

┌──(kali㉿kali)-[~/Desktop/ISITDTU/Treasure]
└─$ cd file                                                                                                                   2 ⚙
                                                                                                                                
┌──(kali㉿kali)-[~/Desktop/ISITDTU/Treasure/file]
└─$ mv file.None.0xfffffa8030f864d0.dat 1P.png                                                                                2 ⚙
                                                                                                                                  
┌──(kali㉿kali)-[~/Desktop/ISITDTU/Treasure/file]
└─$ eog 1P.png
```

![image](https://user-images.githubusercontent.com/62060867/144242309-94b0fae0-8205-42be-92a9-f3f9bfa1aae5.png)

Ok ta có được file `S3cr3t.txt` được screenshoot lại. 

Bây giờ ta đã có key `gaialimetaimeo`, tiến hành decrypt file python


Ta thấy mỗi kí tự enc đều đc làn lượt encrypt

Mình ko quan tâm cách dịch ngược code lại thế nào

Chỉ cần bruteforce theo như đề để tìm lại các bytes ban đâu

```
from pwn import *

s=b''
for byte in range(255):
	tmp=byte^(byte>>4)
	tmp^=tmp>>3
	tmp^=tmp>>2
	tmp^=tmp>>1
	if tmp==0x1e:
		s+=bytes([byte])
		break

enc=bytes.fromhex('1e19724f1e0605071f1c151a79556e4c7759654971486441765e6b253f2c3a467a34411f0f0c6c4b137e374e735465283c282c3443167135235704')
print(enc)
for i in range(len(enc)-1):
	for byte in range(256):
		tmp=byte^(enc[i])
		tmp^=tmp>>4
		tmp^=tmp>>3
		tmp^=tmp>>2
		tmp^=tmp>>1
		if tmp==enc[i+1]:
			s+=bytes([byte])
			print(byte,tmp,enc[i+1],hex(s[i]))
			break
	print(s)
		
```
Run code:

```
┌──(stirring㉿Stirring)-[/mnt/c/Users/thanh/Desktop]
└─$ python3 solve.py 
b'\x13\tZ\x16\\\x1b\x01\x01\x15\x0f\x00\x00V\x08\x06\x08\x08\r\x05\x04\x08\x11\x15\x08\x04\x08\nZ\x01\x02\x0fR\x08QXS\x16\x04\\\x0eR[WR\x0c\x03\x08^\x0e\x07\x15\x07[]W[\x01QQ' 
```
Ta có cipher đem xor với key `gaialimetaimeo`

```
┌──(stirring㉿Stirring)-[/mnt/c/Users/thanh/Desktop]
└─$ python3
Python 3.9.7 (default, Sep 24 2021, 09:43:00)
[GCC 10.3.0] on linux
>>> cipher = '\x13\tZ\x16\\\x1b\x01\x01\x15\x0f\x00\x00V\x08\x06\x08\x08\r\x05\x04\x08\x11\x15\x08\x04\x08\nZ\x01\x02\x0fR\x08QXS\x16\x04\\\x0eR[WR\x0c\x03\x08^\x0e\x07\x15\x07[]W[\x01QQ'
>>> key = 'gaialimetaimeo'
>>> def str_xor(s1, s2):
...  return "".join([chr(ord(c1) ^ ord(c2)) for (c1,c2) in zip(s1,s2)])
>>> decode = str_xor(cipher, key)
>>> decode
'th3w0rldanim3g'
```

















