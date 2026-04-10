# Misc

### [495 pts] PenjaruyV1

Di awal, saat mencoba beberapa command ternyata banyak yang keblock, seperti builtins, globals, import, cat, dst, jadi harus mencoba untuk bypass kata-kata yang diblock.

<img width="565" height="273" alt="image27" src="https://github.com/user-attachments/assets/dc02f35f-3a20-4a6b-8c64-c25acfa7d69c" />

Di awal, saya mencoba untuk lihat semua class yang ada di Python runtime

<img width="1208" height="703" alt="image5" src="https://github.com/user-attachments/assets/55b3d01c-c4ff-48f4-b12e-ba10f0745d03" />

Ini menggunakan ().__class__.__base__.__subclasses()
Lalu saya mencoba untuk cari class yang bisa diexploit, dan menemukan tapi hanya dalam bentuk angka, belum nama classnya, menggunakan command:
<img width="1206" height="83" alt="image21" src="https://github.com/user-attachments/assets/d1fc0f99-0299-496d-8b2c-2bd7bb72451f" />

Ini menunjukkan cara masuk ke builtins
Lalu, saya coba untuk masuk ke builtins tapi harus bypass block, berkali-kali gagal karena ke block yang seperti os, read, sys, open, dan import. Tapi akhirnya dapat juga pakai

<img width="1204" height="730" alt="image43" src="https://github.com/user-attachments/assets/79bd49ed-f854-48fe-9d26-e6b533807c91" />

getattr(().__class__.__base__.__subclasses__()[103].__init__, '__global'+'s__')[chr(95)+chr(95)+chr(98)+chr(117)+chr(105)+chr(108)+chr(116)+chr(105)+chr(110)+chr(115)+chr(95)+chr(95)]

Selama chall ini, saya melakukan string obfuscation untuk bypass kata kata yang diblock dengan bikin string dari angka

Karena di awal udah list semua class, saya mencari yang mana yang warnings dan subprocess.Popen karena ingin dipakai untuk exploit

<img width="474" height="649" alt="image25" src="https://github.com/user-attachments/assets/9e00454b-a0ca-4526-97fe-bf38b537bc68" />

Lalu, saya coba untuk baca isi file server.py tapi juga menyamarkan read karena diblock
(lambda b:getattr(b[chr(111)+chr(112)+chr(101)+chr(110)]('server.py'),''.join([chr(x)for x in[114,101,97,100]]))())(getattr([].__class__.__base__.__subclasses__()[233].__init__,chr(95)*2+''.join([chr(x)for x in[103,108,111,98,97,108,115]])+chr(95)*2)[chr(95)*2+''.join([chr(x)for x in[98,117,105,108,116,105,110,115]])+chr(95)*2])

<img width="1209" height="753" alt="image24" src="https://github.com/user-attachments/assets/900f0fef-d6f7-47ff-bde6-0ca34b1c68a6" />

Disini ketawan apa aja kata yang diblock

<img width="704" height="68" alt="image10" src="https://github.com/user-attachments/assets/2cc14cdf-7978-4c39-a5b0-3554077e4258" />

Lalu saya cek lagi isi directory dari server ini untuk cari semacam flag.txt dll
Saya coba untuk buka open(flag.txt) pake bypass chr tapi ternyata tidak ada, tapi pas dicoba lagi pake /flag.txt dan pake bypass lagi, flagnya ditemukan
<img width="1208" height="83" alt="image46" src="https://github.com/user-attachments/assets/1d0c8b7e-c6c3-4d8a-96ff-6d306404adf2" />


<img width="1202" height="87" alt="image26" src="https://github.com/user-attachments/assets/41700b27-a5da-4b86-8ddd-f967eff21770" />

### Flag
PorosCTF{jujur_biar_category_miscnya_kepake}
