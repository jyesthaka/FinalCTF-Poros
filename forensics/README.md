# Forensics

### [493 pts] Drag Path

Author: 2byte

Terdapat drag_path.img yang tidak bisa dibuka secara langsung. Maka harus dibuka dengan Autopsy, dan akan ada 2 file image

<img width="1188" height="143" alt="image13" src="https://github.com/user-attachments/assets/1630549e-7a82-4129-bed9-04197395b432" />

Saat membuka file _ur.png, hasilnya masih akan seperti ini:

<img width="1197" height="266" alt="image20" src="https://github.com/user-attachments/assets/90f81c00-8026-4cf9-b79d-a73f8e69b9c8" />

Ini adalah gambar yang sebenarnya utuh, jadi goal disini adalah untuk recover gambar ini seperti semula, 

Di awal saya juga mendapatkan file _hredder dan itu saya ekstrak dan menghasilkan beberapa file pyc termasuk shredder.pyc. File shredder.pyc ini adalah python bytecode sehingga perlu dilakukan decompile  

<img width="1708" height="212" alt="image44" src="https://github.com/user-attachments/assets/c61dd9b9-dd2b-4e8b-a56f-29f024aed2fa" />

Dilakukan decompile dengan PyLingual yang menghasilkan sc python. Lalu akan ditemukan bagian yang berisi base64 encoded data yang akan didecode
Berikut adalah hasil scriptnya: 
from PIL import Image
import sys, os, struct

STRIP_H = 6
SEED = 0xDEAD
MAGIC = b'\x13\x37\xCA\xFE'

target = sys.argv[1] if len(sys.argv) > 1 else 'our.png'

img = Image.open(target)
w, h = img.size
pixels = img.load()

def lcg(state):
    return ((state * 0x41C64E6D) + 0x3039) & 0xFFFFFFFF

def gen_key(s, x, y):
    v = (s * 0x1337) ^ (x * 0xBEEF) ^ (y * 0xCAFE)
    v = ((v >> 16) ^ v) * 0x45d9f3b
    v = ((v >> 16) ^ v) * 0x45d9f3b
    v = (v >> 16) ^ v
    return v & 0xFF

hidden = bytearray()
state = SEED
offsets = []

for s in range(h // STRIP_H + 1):
    state = lcg(state)
    y0 = s * STRIP_H
    y1 = min(y0 + STRIP_H, h)
    if state % 7 != 0:
        strip_buf = bytearray()
        for y in range(y0, y1):
            for x in range(w):
                r, g, b = pixels[x, y][:3]
                k = gen_key(s, x, y)
                strip_buf.extend([r ^ k, g ^ k, b ^ k])
                pixels[x, y] = (0, 0, 0)
        strip_buf = strip_buf[::-1]
        for i in range(0, len(strip_buf) - 1, 2):
            strip_buf[i], strip_buf[i + 1] = strip_buf[i + 1], strip_buf[i]
        offsets.append((s, len(strip_buf)))
        hidden.extend(strip_buf)

img.save(target)

trailer = bytearray()
trailer.extend(MAGIC)
trailer.extend(struct.pack('<H', len(offsets)))
for s, sz in offsets:
    trailer.extend(struct.pack('<HI', s, sz))
trailer.extend(hidden)

with open(target, 'ab') as f:
    f.write(trailer)

os.remove(target)

Dari script, diketahui bahwa gambar dibagi menjadi strip horizontal (STRIP_H = 6), pixel di xor dengan key gen_key, pixel asli dihapus menjadi hitam, dan data disimpan ke buffer

Lalu untuk membetulkan script gambar menggunakan: 

<img width="471" height="910" alt="image37" src="https://github.com/user-attachments/assets/3f83980e-a0b7-47b1-b201-3d1e8af887d5" />

### Flag

PorosCTF{0ur_m3m0ry_4r3_57i11_in_my_h34d}

### [490 pts] Iseng

Author: broKen

Diberikan file PCAP yang dibuka di Wireshark dan muncul banyak paket
Buka netcat dan akan muncul pertanyaan-pertanyaan

IP Victim?
Jawaban: 10.0.0.42
Ini adalah IP korban (digunakan oleh attacker untuk attack dengan IP Spoofing dan menyamar)

Jenis serangan apakah ini?
Jawaban: Smurf Attack
Diketahui karena terlihat bahwa di packet, ciri-ciri Smurf Attack sama dengan serangan ini, seperti banyak ICMP echo reply tanpa permintaan, lonjakan traffic ICMP dalam waktu singkat, spoofing IP attacker dan menyamar menjadi victim.

Apa berapa puluh jumlah IP yang membalas broadcast?
Jawaban: Empat Puluh

<img width="183" height="675" alt="image23" src="https://github.com/user-attachments/assets/1c1d1927-f68a-44ea-a017-46343afc4fb7" />

Karena tidak ada 10.0.0.34 (ini IP asli attacker) dan 10.0.0.42 adalah IP victim yang digunakan oleh attacker untuk menyerang

Pada layer TCP/IP berapa serangan ini bekerja?
Jawaban: Tiga
Karena ICMP bekerja pada Layer 3 (Network Layer). Smurf Attack ini menggunakan ICMP echo request reply pada Layer 3.

Attacker mengirimkan apa ketika menyamar menjadi IP Victim?
Jawaban: Broadcast
Menggunakan MAC penyerang jika diblock:

<img width="395" height="40" alt="image19" src="https://github.com/user-attachments/assets/c9aab0b1-7ce8-4584-ab22-eb654620b9af" />

<img width="245" height="25" alt="image42" src="https://github.com/user-attachments/assets/2d894f7b-8bcf-4320-a064-38f5af74c7ad" />

IP Penyerang?
Jawaban: 10.0.0.34
Yang sebelumnya saya ketahui tentang attacker dari info di bawah adalah bahwa; attacker memiliki MAC 26:6e:95:7b:cd:90 dan menggunakan IP korban untuk melakukan attack, saya belum tahu IPnya berapa dan tidak bisa dicek di Ubuntu karena tidak ada di LAN yang sama. 

<img width="653" height="21" alt="image4" src="https://github.com/user-attachments/assets/c6414b3c-fe88-401d-9cb3-0922f97370be" />

Dugaan pertama: Ada jumlah IP yang berurutan angkanya kecuali angka 10.0.0.34
Dugaan kedua: Dengan MAC 26:6e:95:7b:cd:90, jika di block di Wireshark menggunakan eth.src == 26:6e:95:7b:cd:90, akan muncul serangan-serangan attacker yang sourcenya 10.0.0.42 (IP korban yang digunakan attacker), namun ada 2 packet ARP

<img width="805" height="33" alt="image33" src="https://github.com/user-attachments/assets/49452733-a7da-4b85-a5aa-3a3df4fb1c52" />

Ini juga menunjukkan bahwa 10.0.0.34 merupakan IP sang attacker.

### Flag

PorosCTF{S3puH_BlU3_T34m_d4n_N3tw0rk_4n4lys1s_Harus_Join_ICN_Sih_Ini_ditunggu_yah_tahun_ini}

### [499 pts] Insider

Author: unlikeneptune

Diberikan file pcap yang dibuka di Wireshark, isinya adalah serangan brute force
Masuk ke netcat dan jawab 7 pertanyaan
What is the IP address of the attacker?
Jawaban: 192.168.92.128
What is the IP address of the victim?
Jawaban: 192.168.92.130
Dari pengamatan di awal, terlihat bahwa ada banyak request koneksi ke TCP SYN menuju port 22 (SSH), dan disini terlihat yang mana IP attacker dan IP victim dari ip source dan destination
What time did the brute force attack begin?
Jawaban: 2026-03-11 18:21:24
Serangan awal dimulai di waktu ini dengan melihat:

<img width="1289" height="38" alt="image16" src="https://github.com/user-attachments/assets/85c8d4ea-3ef5-49ef-bf59-d8da716edd61" />

Ini adalah timestamp pertama dimana TCP dengan ip src attacker dan ip dst victim ada, dan dimulainya request koneksi ke TCP SYN ke port 22
How long did the brute force attack last, in seconds?
Jawaban: 48
Dari semua packet, terlihat bahwa serangan brute force berhenti pada waktu 18:22:12 

<img width="1121" height="350" alt="image30" src="https://github.com/user-attachments/assets/12481ec7-9278-41e3-8a06-ddd6dc6c5770" />

Jika dihitung dari 18:21:24 hingga 18:22:12 maka totalnya adalah 48 detik
After gaining access, the attacker deployed malware that communicated with a C2 server. What port did the malware use to beacon out?
Jawaban: 4444
Setelah mereka mendapatkan akses, attacker “eksekusi” malware untuk mempertahankan akses. 

<img width="720" height="406" alt="image35" src="https://github.com/user-attachments/assets/ff62a7ca-c0a7-4f0f-b874-c41b44a71cd3" />

Malware mengirim sinyal ke penyerang menggunakan port 4444 (terlihat di dst port 4444)
What time was the first beacon sent?
Jawaban: 2026-03-11 19:08:56
Waktu pertama beacon terkirim melalui port 4444 adalah 19:08:56, sama sumbernya bagaimana mendapatkan port 4444 di bagian packet
Analyze the beacon payload. What are the compromised credentials found in the beacon traffic?
Jawaban: ElNumeroUno:verysecurepassword2026
Karena port yang digunakan untuk beacon out sudah ada (4444), block di Wireshark tcp.port == 4444, klik salah satu packet, dan follow lalu tcp stream. Disana akan ada informasi yang berisi user dan pass: 

<img width="380" height="87" alt="image8" src="https://github.com/user-attachments/assets/9a1f6612-ef2e-471c-81ec-487ac8950632" />

### Flag
PorosCTF{1NGiN_m3Nj4Di_bLUE_tE4M_N4mUn_en9g4n_M3mbACa_tRaFfIC}

### [Unsolved] ey DIH es

Buka suricata.zip ada log, dan buka netcat
=========================================================
          WELCOME TO THE POROS FORENSICS CHALLENGE!
=========================================================
---------------------------------------------------------
  Question 1/10
  What's the victim's IP address?
  Format: xx.xx.xx.xx
---------------------------------------------------------
Answer: 10.10.10.50
[+] Correct!
---------------------------------------------------------
  Question 2/10
  What's the attacker IP address?
  Format: xx.xx.xx.xx
---------------------------------------------------------
Answer: 10.10.10.100
[+] Correct!

---------------------------------------------------------
  Question 3/10
  There are 3 tools that used by attacker, what are they? (order it alphabetically)
  Format: abc,def,ghi
---------------------------------------------------------
Answer: hping3,nmap,sqlmap
[+] Correct!

---------------------------------------------------------
  Question 4/10
  There is a vulnerable plugin exploited by the attacker, what's the name of the plugin?
  Format: lowercase-letters
---------------------------------------------------------
Answer: easy-quotes
[+] Correct!

---------------------------------------------------------
  Question 5/10
  What's the CVE ID of the vulnerable plugin?
  Format: CVE-XXXX-XXXX
---------------------------------------------------------
Answer:  CVE-2025-26943
[+] Correct!

---------------------------------------------------------
  Question 6/10
  When exploitation of vulnerable plugin started?
  Format: DD/MM/YYYY HH:MM:SS
---------------------------------------------------------
Answer: 12/03/2026 16:52:39
[+] Correct!

---------------------------------------------------------
  Question 7/10
  What's the username exfiltrated by the attacker?
  Format: -
---------------------------------------------------------
Answer: 2byteFansClub
[+] Correct!

---------------------------------------------------------
  Question 8/10
  What's the password hash exfiltrated by the attacker?
  Format: $algorithm$hash
---------------------------------------------------------
Answer: $P$BDVrT8ijki8iH97aVCY25ZXJOSMtlL/
[+] Correct!

---------------------------------------------------------
  Question 9/10
  What's the password cracked by the attacker?
  Format: -
---------------------------------------------------------
>>> Answer:

