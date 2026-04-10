# OSINT

### [454 pts] Admin Lapar

Author: RFB

Dari image yang diberikan, ada 1 menu yang memudahkan pencarian lokasi: Nasi Cumi Setan Pedas
Masukkan ke Google Search dan akan muncul lokasi Nasi Cumi Hitam Madura Pak Kris

Untuk konfirmasi apakah benar atau tidak ini lokasinya, cek kategori Menu di Google Maps Nasi Cumi Hitam Madura Pak Kris dan menemukan gambar exact:

<img width="1077" height="705" alt="image28" src="https://github.com/user-attachments/assets/89ded79d-2ecf-45ee-ab2b-0e0a7116c6f2" />

Lalu, masuk ke URL https://bit.ly/word1_word2_word3 dan search nama lokasinya:
  
<img width="1006" height="506" alt="image1" src="https://github.com/user-attachments/assets/2c2dc353-8410-42e8-abbb-a6d89ad5410d" />

Ditemukan URL yang harus dikunjungi adalah: bit.ly/fingertip.seemingly.nicknames dan diarahkan ke link pastebin yang sudah expired
Untuk menemukan apa yang dihapus, gunakan Wayback Machine
Copy link pastebin dan paste di wayback, klik di 13 Maret 2026 (last saved time) dan ada snapshot yang mengarahkan ke https://web.archive.org/web/20260313044708/https://pastebin.com/myX3nsF5 yang berisi flag

### Flag
PorosCTF{MAm4h_4kU_j490_05iN7_K4RN4_m4Kan_cuM1_h174M}

### [488 pts] Membiru
Author: zenc

Dari clue yang diberi:
Ain't like we want
It's better than we want
I thought we're lover in a morning
And we don't need the sun

Dan clue gambar yaitu orang bermain drum, bisa diasumsikan bahwa ini adalah lirik lagu, dan setelah dicari-cari, ditemukan bahwa ini adalah lagu Broken Hearted for Stranger Is Such a Feeling that I Adore oleh Eleanor Whisper. Ini ditemukan dan dikonfirmasi dengan melihat lirik Spotify yang menunjukkan lirik ini.

<img width="738" height="1600" alt="image29" src="https://github.com/user-attachments/assets/4ed12c13-14ef-4abc-b26a-0b37eeee8a92" />

Dari sini langsung menuju ke Instagram Eleanor Whisper karena di clue gambar menunjukkan 5 dot dibawah yang menunjukkan slideshow post Instagram

<img width="143" height="32" alt="image22" src="https://github.com/user-attachments/assets/19e743be-3f7b-4662-ae42-0af256b8490e" />

Ini ada di slide ke 2 dan ada total 5 gambar di slideshow, jadi langsung mencari post slideshow dengan 5 gambar. Setelah scrolling panjang, inilah exact match dari clue gambar:

<img width="1367" height="852" alt="image18" src="https://github.com/user-attachments/assets/eded6a75-b009-4369-9688-37a78069fe3f" />

Di caption menunjukkan “UI ArtX 2021 at Rossi Musik”, menunjukkan bahwa Rossi Musik merupakan sebuah lokasi dan langsung dicari di Google Maps. Setelah melihat Rossi Musik di Maps, langsung ke bagian review dan recent dan menemukan ini:

<img width="436" height="336" alt="image11" src="https://github.com/user-attachments/assets/d92ed9d5-7639-4d92-9905-546281d478a3" />

Ini juga menandakan bahwa saya on the right track, dan langkah berikutnya adalah mencari “kalcer shi coffeeshop”. Ini sangat luas karena tidak diberi clue ada dimana coffeeshopnya. Di awal saya mencari review-review coffee shop di daerah Malang seperti; Critasena, Terrace, Culture Club, Disclosure, ooostereo, Berkat Terang Jaya, Fajar Baru, Nakoa, semua Kopjay, dan lainnya dan masih tidak ada. Karena gagal, saya coba untuk flashback ke challnya dan melihat “Membiru” dan mencari coffeeshop yang bernama Membiru di seluruh Indonesia (lol), tapi tidak ditemukan juga.

Jadi saya mempersempit skala pencariannya ke coffeeshop di dekat Rossi Musik.
Ada banyak coffee shop disini jadi saya mulai lagi pencarian di review. Saya mencari di coffeeshop Bakop, UIN, La Trobe, dan lainnya tapi tidak ketemu. Tetapi ternyata ada coffee shop yang sangat dekat bernama Frontier at Rossi.

<img width="531" height="422" alt="image45" src="https://github.com/user-attachments/assets/8e88d0f6-41f4-47c9-9d86-70b679ede2b4" />

Part 2 sudah ditemukan namun belum selesai. Dari sini tidak diberi clue, jadi instinct pertama saya langsung untuk cek profile dari pemberi review ini dan langsung ditemukan last part flagnya

<img width="439" height="523" alt="image12" src="https://github.com/user-attachments/assets/009de73a-210d-47b0-a9a0-d10faa39ae07" />

Dan jika menggabung ketiga bagian flag tersebut, flag ditemukan

### Flag
PorosCTF{7HI5_i5_7h3_m0M3N7_7H47_I_C4n'7_35c4P3}

### [484 pts] Netops

Buka webnya dan buka netcat di Ubuntu
Question 1/9
What's the public IP of the website?
Format: x.x.x.x
Answer: 129.226.4.167
Cara: Gunakan ping untuk mendapatkan IP nya di Ubuntu

<img width="595" height="42" alt="image40" src="https://github.com/user-attachments/assets/21778ea0-a7e3-4d65-89ea-d1c2d7502b6d" />

Question 2/9
What's the AS number of the website?
Format: ASxxxxxxxx
Answer: AS132203
Cara: Gunakan IP yang sudah ditemukan dan sambungkan dengan whois di Ubuntu (whois 129.226.4.167) dan akan muncul banyak informasi dan disini terlihat AS number website ini

<img width="317" height="48" alt="image36" src="https://github.com/user-attachments/assets/a3b8a04a-a4f9-455b-94df-c2d3eb9f05fd" />

Question 3/9
What's the AS name of the website?
Format: Gedung Fakultas Ilmu Komputer
Answer: Tencent Building Kejizhongyi Avenue

Cara: Ini merupakan pertanyaan yang sebenarnya tidak sulit namun saya memerlukan waktu yang sangat lama karena tidak mengerti formatnya. Saya menggunakan bgp.he.net., whois, DNS Checker, PeeringDB, IP2location, IDnic, Shodan, dan masih banyak web lainnya.
Dari semua ini, saya list semua jawaban salah saya dan bisa kurang lebih 50:

<img width="388" height="732" alt="image9" src="https://github.com/user-attachments/assets/9c71bab1-123a-4f8f-8c78-b72c7fa8d87f" />

Di list ini sebetulnya jawaban benar sudah ada namun salah format. Yang dikira formatnya adalah “Gedung” atau Bahasa Indonesia, ternyata formatnya adalah yang penting tidak menggunakan tanda baca. Jadi saya mencoba Tencent Building Kejizhongyi Avenue dan langsung benar.

Question 4/9
What's the fingerprint key SSH (ECDSA) of the website?
Format: 1a2b3c4d5e6f7890abcdef1234567890abcdef1234567890abcdef1234567890
Answer: 603f888e8eeb7048a5d32d0e61e1cd1d27b5990ab8377a58b9349bb0536dab79

Question 5/9
What's the others subdomain of the website?
Format: subdomain.example.com
Answer: go.zenc.cc
Cara: Disini saya menggunakan crt.sh untuk menemukan dengan search %.zenc.cc dan menemukan ada 2 yaitu netops.zenc.cc dan go.zenc.cc

Question 6/9
Who is the real name of website's creator?
Format: John Doe
Answer: Abi Abdillah
Cara: Sebelumnya sudah ditemukan subdomain lain yaitu go.zenc.cc dan jika dibuka website itu, akan muncul semua informasi pembuat website dari subdomain zenc.cc

<img width="953" height="613" alt="image7" src="https://github.com/user-attachments/assets/8229c1b2-6950-427f-a23f-4586d81157cf" />

Question 7/9
What's the creator Twitter/X username?
Format: @username
Answer: @zenCipher_
Cara: Di go.zenc.cc, tulisannya adalah @zenCipher namun jawabannya salah, dan jika diklik bagian X nya, usernamenya adalah @zenCipher_ 

<img width="588" height="398" alt="image3" src="https://github.com/user-attachments/assets/ccda11a8-ebd8-4596-9b68-ce166cfa7ecb" />

Question 8/9
Which city is the creator from?
Format: Epstein Island
>>> Answer: South Tangerang
Cara: Tidak langsung terlihat, tapi di go.zenc.cc ada opsi melihat CV dan di CVnya terpampang jelas bahwa ia dari South Tangerang 

<img width="468" height="58" alt="image14" src="https://github.com/user-attachments/assets/ceeb42c1-831e-4614-b990-7d9d3dd30564" />

Question 9/9
What's the creator's current GPA?
Format: x.xx
Answer: 3.89
Cara: Sama seperti cara sebelumnya, melihat GPA ada di CVnya

<img width="233" height="116" alt="image50" src="https://github.com/user-attachments/assets/0c4bc529-bcf3-4ac0-967e-e7a19cf6b9ef" />

### Flag
PorosCTF{doksing_dikit_ketua_kortim_poros_2026}


