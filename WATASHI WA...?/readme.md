# WATASHI WA...?

> Do you know which universe this historical figure belongs to?

 ![Anime](https://user-images.githubusercontent.com/62060867/144490280-28e847b8-ef8d-4d6a-8957-466cd8d5aabc.jpg)

# Phân tích ban đầu

Đầu tiên mình quăng ảnh lên https://aperisolve.fr/ để check Zsteg, Steghide, Outguess, ExifTool, ExifTool, Foremost, Strings. 

Khi strings mình ta nhận được fake flag ~~ISITDTU{Flag_N0t_h3re!!!}~~

![image](https://user-images.githubusercontent.com/62060867/144491769-8f44437f-a491-49a0-bd20-9574e434ccc3.png)

Binwalk ta thấy có 1 file zip 

![image](https://user-images.githubusercontent.com/62060867/144491684-36b0059c-22ed-4332-9ee3-2d5545ff9846.png)

Mình đã thử extract file và mở file zip nhưng không thể 

![image](https://user-images.githubusercontent.com/62060867/144491986-9b36e281-e5af-4765-8fc4-20c7054bb9e4.png)

Lí do là vì file ảnh này bị corrupt với lỗi `14016 extraneous bytes before marker 0xd9`

![image](https://user-images.githubusercontent.com/62060867/144492224-770a43b2-22b3-4824-858a-c0020c147702.png)

Mình kiểm tra hex của file ảnh và phát hiện ra rằng file ảnh này gồm 3 phần

> ##  JPG.1 + zip file + JPG.2 = Anime.jpg

# Solotion

## Step 1:

Mình tách hình JPG.1 ra từ offset `0 -> 119BB` vì kết thúc `JPEG` là `FF D9`

![image](https://user-images.githubusercontent.com/62060867/144493316-289ce782-d8da-4cc2-9f96-5915bdf2e1a0.png)



Coppy toàn bộ offset 0 -> 119BB tạo 1 file mới là `JPG1.jpg` không khác gì ảnh gốc

![JPG1](https://user-images.githubusercontent.com/62060867/144493648-38e22d81-40b6-4ac5-a6be-ec65601c5560.jpg)

Sau khi phân tích tiếp mình nhận ra có một kĩ thuật giấu tin trong ảnh JPG bằng cách tăng Height của bức ảnh

![image](https://user-images.githubusercontent.com/62060867/144494049-b2da0528-3b2f-4b74-ac2c-648f61ea3a30.png)

Bức ảnh này đang ở size 400x350 mình sẽ tăng lên 400x400 bằng cách thay đổi bytes ở vị trí height ảnh dưới 

![image](https://user-images.githubusercontent.com/62060867/144494596-4be6c140-ef13-414e-abbf-7c2ed730960f.png)

Search hex-value `FF C0` và thay đổi bytes height

![image](https://user-images.githubusercontent.com/62060867/144494888-431f36de-0c52-4e83-9a5e-290d912eb8a5.png)

Mình thay đổi `01 5E 01 90` thành `01 90 01 90`

![JPG1](https://user-images.githubusercontent.com/62060867/144495006-30b05b62-c351-4dc5-ac4d-13054f517523.jpg)

yeah có vẻ như ta có key `0K1TA-SAN` để làm gì đó

## Step 2

Tiếp tục phần còn lại của `zip file + JPG.2 = Anime.jpg`

Có thể thấy 4 null bytes đầu có thể là `50 4b 03 04` 

![image](https://user-images.githubusercontent.com/62060867/144498129-a1c1528b-8252-4740-8ead-62ea88f24976.png)

Vì vậy mình thay đổi `00 00 00 00` -> `50 4b 03 04`  tiếp tục tách file zip 

```
┌──(stirring㉿Stirring)-[Users/thanh/Desktop]
└─$ zip -FF Anime.jpg --out flag.zip
Fix archive (-FF) - salvage what can
 Found end record (EOCDR) - says expect single disk archive
Scanning for entries...
 copying: flag.txt  (54 bytes)
Central Directory found...
EOCDR found ( 1  72343)...
```
Ta đã có file zip nhưng ta cần password.

## Step 3

Còn phần JPG.2 cuối cùng mình xóa 242 bytes zip và phần fake flag cuối file và thay các byte đầu thành `FF D8 FF E0 00 10 4A 46 49 46` ta tiếp tục có bức ảnh

![JPG2](https://user-images.githubusercontent.com/62060867/144501133-f7142d22-33d4-4bc3-bc22-397ec3729772.jpg)


Chắc chắn bức ảnh này có giấu file bằng steghide, để chắc chắn mình dùng `stegseek` để kiểm tra

```
┌──(kali㉿kali)-[~/Desktop]
└─$ time stegseek --seed JPG2.jpg 
StegSeek version 0.5
Progress: 1.71% (73589583 seeds)           

[i] --> Found seed: "811b8ba2"
Plain size: 35.0 Byte(s) (compressed)
Encryption Algorithm: rijndael-128
Encryption Mode:      cbc

real    6.55s
user    25.54s
sys     0.13s
cpu     391%
```
Vậy chắc chắn rằng key `0K1TA-SAN` ta tìm được ở Step 1 chính là password

```
┌──(kali㉿kali)-[~/Desktop]
└─$ steghide --extract -sf JPG2.jpg 
Enter passphrase: 
wrote extracted data to "S3cr3t.txt".
                                                              
┌──(kali㉿kali)-[~/Desktop]
└─$ cat S3cr3t.txt                      
FG0!!! 
```
Ta có được file `S3cr3t.txt` và password cho file `flag.txt` là `FG0!!!`

```
┌──(stirring㉿Stirring)-[Users/thanh/Desktop]  
└─$ unzip -P "FG0!!!" flag.zip        
Archive:  flag.zip      
skipping: flag.txt 
┌──(stirring㉿Stirring)-[Users/thanh/Desktop]
└─$ cd flag
┌──(stirring㉿Stirring)-[Users/thanh/Desktop/flag]
└─$ cat flag.txt
ISITDTU{St3g0_15_Fun!!!}
```









