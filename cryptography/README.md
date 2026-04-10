# Cryptography

### Shannons Secret
Author: RFB
499 pts

Diberikan c1. C2 dan hint (ciphertext)
Hasil decode hint Base64:
Welcome to ShannCipher!!, please make it done!!...

Disini menggunakan OTP dengan rumus enkripsi C1 = (P1 ⊕ K)
Karena ada c1 dan c2 maka:
C1 = (P1 ⊕ K)
C2 = (P2 ⊕ K)
Yang berarti XOR keduanya:
c1 ⊕ c2 = p1 ⊕ p2
Karena sudah tahu sebagian plaintext dari P1 maka
K = C1 ⊕ P1

Dengan implementasi Python:
from binascii import unhexlify
import base64

c1 = unhexlify("4a81cfcd727349de4d0b79b40ccc326d567c40f6827980f0f8db9d45b00eee15bcfdce1311a5bc449e3e04d1d8487e87061c")
c2 = unhexlify("4d8bd1c16e5d78b842096d8910992c5c776700c1853cd68eed90b21c805ed447f7a4c1270db1ea6fd669038cc0")

hint = base64.b64decode("V2VsY29tZSB0byBTaGFubkNpcGhlciEhLCBwbGVhc2UgbWFrZSBpdCBkb25lISEuLi4=")

#untuk key
key = bytes([c1[i] ^ hint[i] for i in range(len(hint))])

#untuk decrypt c2
p2 = bytes([c2[i] ^ key[i] for i in range(len(c2))])

print(p2.decode())

<img width="1212" height="394" alt="image31" src="https://github.com/user-attachments/assets/bbc93111-0bc0-4cda-9a71-eee3dd9dcc99" />

### Flag
PorosCTF{m4nt4p_br0_b7w_9k_5U1I7k4n_y4?_h3h3}
