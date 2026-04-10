### Reverse Engineering

# [488 pts] her

Author: 2byte

Cek dulu type file menggunakan file her
ELF 64-bit LSB executable, x86-64, dynamically linked, stripped

Cari lagi isi dari her menggunakan strings her untuk mencari strings yang terbaca
Disini menemukan: 
fgets stdin puts strlen strcspn printf zEXEYi~lQbOSuBE]uKXOuSE_
Disini terlihat bahwa program meminta input user, ada juga string panjang yang terencode (bisa jadi flag)

Run her menggunakan ./her dan akan muncul:

Tapi apapun yang diketik, hasilnya akan seperti itu

Lalu saya disassembly dengan objdump -d -M intel her | less
Insight dari disassembly:
Input harus tepat 935 karakter 
Setiap karakter dicek: input[i] == stored_byte[i] XOR 0x2A
Artinya flag = semua stored bytes di-XOR dengan 0x2A

Lalu cari storedbytes dengan readelf
Section read only ada di file offset 0x2000, data dimulai dari address 0x402020
Jadi file offset: 0x2000 x 0x402020 = 0x2020

Lalu buat decoder untuk unextract XOR:
with open('her', 'rb') as f:    
    f.seek(0x2020)               
    stored = f.read(935)         

flag = bytes(b ^ 0x2a for b in stored)  
print(flag.decode())

Dan run script pythonnya dan akan muncul flagnya


Flag
PorosCTF{Hey,_how_are_you?_You're_doing_okay,_right?_I_don't_have_many_words,_I'm_not_really_the_poetic_type,_but_I_know_what_I_want_to_say._First_of_all,_thank_you._Thank_you_for_making_me_feel_noticed,_for_making_me_feel_needed,_and_even_looked_up_to,_because_I_never_thought_someone_like_me_could_feel_that_way._That_feeling_pushed_me_to_grow,_to_become_a_better_person,_to_be_stronger_and_kinder._I_studied_so_I_could_answer_your_math_questions_back_then,_I_even_tried_going_to_a_barbershop_I_had_never_been_to_before_just_so_I_could_look_better,_and_I_worked_on_my_toxic_talks_so_I_would_not_accidentally_hurt_your_feelings,_so_thank_you_for_all_of_that._But_when_I_was_trying_to_improve_myself_and_we_went_back_to_school,_I_felt_like_you_pulled_away_from_me,_and_I_do_not_really_know_why._The_nickname_I_used_to_call_you,_you_do_not_respond_to_it_anymore,_and_what_hurts_the_most_is_realizing_that_you_have_become_closer_to_him.}
