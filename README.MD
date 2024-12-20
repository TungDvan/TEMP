# TRÍCH DATA

```PYTHON
import idaapi
import idautils
import idc

start_addr = 0x00007FF6EE574080
end_addr = 0x7ff6ee57471c + 1
values = []

for addr in range(start_addr, end_addr):
    value = idc.get_wide_byte(addr)
    values.append(value)
print(values)

f = open('z_file.txt', 'w')
for i in range(len(values)):
    if i % 10 == 9: f.write(f"0x{values[i]:02X},\n")
    else: f.write(f"0x{values[i]:02X}, ")
f.close()
print("OK")
```

# PRINT

```PYTHON
for i in range(len(tungdvan)):
    if i % 10 == 9: print(f'0x{tungdvan[i]:02X}', end = ',\n')
    else: print(f'0x{tungdvan[i]:02X}', end = ', ')
```

```python
ans = [

]

for i in range(len(ans)):
    if i % 10 == 9: print(f'0x{ans[i]:02X}', end = ',\n')
    else: print(f'0x{ans[i]:02X}', end = ', ')
```

# BYTE TO NUM

```python
import os
n = int(input("Nhap so byte: "))
data = [

]
m = int(input("1 la little, 2 la big: "))

data = bytes(data)

f = open("tmp.bin", "wb")
f.write(data)
f.close()

f = open("tmp.bin", "rb")
byte = f.read(n)

ans = []

while byte:
    if m == 1: tmp = int.from_bytes(byte, 'big')
    if m == 2: tmp = int.from_bytes(byte, 'little')
    ans.append(tmp)
    byte = f.read(n)
f.close()
os.remove('tmp.bin')
for i in range(len(ans)):
    if i % 10 == 9: print(f'0x{ans[i]:0{2*n}X}', end = ',\n')
    else: print(f'0x{ans[i]:0{2*n}X}', end = ', ')
```

# PRINT_INPUT_HEX

```PYTHON
form_flag = 'KCSC'
s = "SeradipityLoveBHP"
n = int(input("Nhap chieu dai cua chuoi: "))
n = n - len(form_flag) - 2
ans = ''
for i in range(n):
    ans += s[i % len(s)]
s = form_flag + "{" + ans + "}"
print(s)
for i in range(len(s)):
    if i % 10 == 9: print(f'0x{ord(s[i]):02x}', end = ',\n')
    else: print(f'0x{ord(s[i]):02x}', end = ', ')
# all_char = [
#     0x7a, 0x78, 0x63, 0x76, 0x62, 0x6e, 0x6d, 0x2c, 0x2e, 0x2f,
#     0x5a, 0x58, 0x43, 0x56, 0x42, 0x4e, 0x4d, 0x3c, 0x3e, 0x3f,
#     0x61, 0x73, 0x64, 0x66, 0x67, 0x68, 0x6a, 0x6b, 0x6c, 0x3b,
#     0x27, 0x41, 0x53, 0x44, 0x46, 0x47, 0x48, 0x4a, 0x4b, 0x4c,
#     0x3a, 0x22, 0x71, 0x77, 0x65, 0x72, 0x74, 0x79, 0x75, 0x69,
#     0x6f, 0x70, 0x5b, 0x5d, 0x5c, 0x51, 0x57, 0x45, 0x52, 0x54,
#     0x59, 0x55, 0x49, 0x4f, 0x50, 0x7b, 0x7d, 0x7c, 0x60, 0x31,
#     0x32, 0x33, 0x34, 0x35, 0x36, 0x37, 0x38, 0x39, 0x30, 0x2d,
#     0x3d, 0x7e, 0x21, 0x40, 0x23, 0x24, 0x25, 0x5e, 0x26, 0x2a,
#     0x28, 0x29, 0x5f, 0x2b
# ]   # zxcvbnm,./ZXCVBNM<>?asdfghjkl;'ASDFGHJKL:"qwertyuiop[]\QWERTYUIOP{}|`1234567890-=~!@#$%^&*()_+
# # max: 126  min = 33
```

# Z3_EASY

```PYTHON
from z3 import *

heso = [
    [2, 3, 4, 5],
    [12, 4, 6, 2],
    [3, 5, 2, 5],
    [2, 0, 3, 23]
]

ans = [
    14, 24, 15, 28
]

def hpt(word, index):
    res = 0
    for i in range(len(ans)): res += word[i] * heso[index][i]
    return res & 0xff

flag = [BitVec(f"flag[{i}]", 64) for i in range(4)]
s = Solver()

for i in range(len(ans)):
    s.add(ans[i] == hpt(flag, i))
print(s)
if s.check() == sat:
    m = s.model()
    for c in flag:
        print(m[c].as_long())
```

# MÃ HÓA VÀ GIẢI MÃ AES.

```python
# mã hóa
from Crypto.Cipher import AES
from Crypto.Util.Padding import pad, unpad

def encrypt_aes(key, iv, plaintext_bytes):
    cipher = AES.new(key, AES.MODE_CBC, iv) 
    ciphertext = cipher.encrypt(pad(plaintext_bytes, AES.block_size)) 
    return ciphertext

def decrypt_aes(key, iv, ciphertext):
    cipher = AES.new(key, AES.MODE_CBC, iv)  
    plaintext_bytes = unpad(cipher.decrypt(ciphertext), AES.block_size)  
    return plaintext_bytes

if __name__ == "__main__":
    try:
        key_hex = input("Nhap khoa AES (64 ky tu): ").strip()
        key = bytes.fromhex(key_hex) 
        if len(key) != 32:  
            raise ValueError("Khoa AES phai co do dai 256-bit (64 ky tu dang hex).")
        iv_hex = input("Nhap IV (hex, 32 ky tu): ").strip()
        iv = bytes.fromhex(iv_hex)
        if len(iv) != 16:
            raise ValueError("IV phai co do dai 128-bit (32 ky tu hex).")
        plaintext_hex = input("Nhap chuoi can ma hoa (hex): ").strip()
        plaintext_bytes = bytes.fromhex(plaintext_hex) 
        ciphertext = encrypt_aes(key, iv, plaintext_bytes)
        print("Chuoi da ma hoa (hex):", ciphertext.hex())
    except ValueError as e:
        print(f"Loi: {e}")
```

```python
# giải mã
from Crypto.Cipher import AES
from Crypto.Util.Padding import pad, unpad

def encrypt_aes(key, iv, plaintext_bytes):
    cipher = AES.new(key, AES.MODE_CBC, iv) 
    ciphertext = cipher.encrypt(pad(plaintext_bytes, AES.block_size)) 
    return ciphertext

def decrypt_aes(key, iv, ciphertext):
    cipher = AES.new(key, AES.MODE_CBC, iv)  
    plaintext_bytes = unpad(cipher.decrypt(ciphertext), AES.block_size)  
    return plaintext_bytes

if __name__ == "__main__":
    try:
        key_hex = input("Nhap khoa AES (64 ky tu): ").strip()
        key = bytes.fromhex(key_hex) 
        if len(key) != 32:  
            raise ValueError("Khoa AES phai co do dai 256-bit (64 ky tu dang hex).")
        iv_hex = input("Nhap IV (hex, 32 ky tu): ").strip()
        iv = bytes.fromhex(iv_hex)
        if len(iv) != 16:
            raise ValueError("IV phai co do dai 128-bit (32 ky tu hex).")
        plaintext_hex = input("Nhap chuoi can ma hoa (hex): ").strip()
        plaintext_bytes = bytes.fromhex(plaintext_hex) 
        decrypted_bytes = decrypt_aes(key, iv, plaintext_bytes)
        decrypted_hex = decrypted_bytes.hex()
        print("Chuoi da giai ma (hex):", decrypted_bytes)
        print("Chuoi da giai ma (hex):", decrypted_hex)

    except ValueError as e:
        print(f"Lỗi: {e}")
```

# BYTE, HEX,...

```PYTHON
flag_1 = list(bytes.fromhex('7E54595F09434B0F4A5D59757B514A5B6D550D0F0C765B7D45'))
flag_2 = list(b'\x7fELF\x02\x01\x01\x00\x00\x00')
```

# RC4

```python
def RC4(key, cipher):
    map = [i for i in range(256)]
    ans = []
    tmp1 = 0
    for i in range(256):
        tmp1 = (tmp1 + map[i] + key[i % len(key)]) & 0xFF
        map[i], map[tmp1] = map[tmp1], map[i]
    tmp1, tmp2 = 0, 0
    for i in range(len(cipher)):
        tmp1 = (tmp1 + 1) & 0xFF
        tmp2 = (tmp2 + map[tmp1]) & 0xFF
        map[tmp1], map[tmp2] = map[tmp2], map[tmp1]
        ans.append(cipher[i] ^ map[(map[tmp1] + map[tmp2]) & 0xFF])
    return ans
```

# XTEA

```python
cipher = [
    # 0x56, 0x64, 0x6C, 0x4B, 0x65, 0x39, 0x75, 0x70, 0x66, 0x42, 
    # 0x46, 0x6B, 0x6B, 0x4F, 0x30, 0x4C
    0x4b6c6456, 0x70753965, 0x6b464266, 0x4c304f6b
]

flag_en = [
    # 0x19, 0x2C, 0x30, 0x2A, 0x79, 0xF9, 0x54, 0x02, 0xB3, 0xA9, 
    # 0x6C, 0xD6, 0x91, 0x80, 0x95, 0x04, 0x29, 0x59, 0xE8, 0xA3, 
    # 0x0F, 0x79, 0xBD, 0x86, 0xAF, 0x05, 0x13, 0x6C, 0xFE, 0x75, 
    # 0xDB, 0x2B, 0xAE, 0xE0, 0xF0, 0x5D, 0x88, 0x4B, 0x86, 0x89, 
    # 0x33, 0x66, 0xAC, 0x45, 0x9A, 0x6C, 0x78, 0xA6
    0x2a302c19, 0x0254f979, 0xd66ca9b3, 0x04958091, 0xa3e85929, 0x86bd790f, 0x6c1305af, 0x2bdb75fe, 0x5df0e0ae, 0x89864b88, 
    0x45ac6633, 0xa6786c9a
]

import sys
from ctypes import *

def decipher(v, k): 
    y = c_uint32(v[0])
    z = c_uint32(v[1])
    sum = c_uint32(0xc6ef3720)
    delta = 0x9e3779b9
    n = 32
    w = [0,0]

    while(n>0):
        z.value -= ( y.value << 4 ) + k[2] ^ y.value + sum.value ^ ( y.value >> 5 ) + k[3]
        y.value -= ( z.value << 4 ) + k[0] ^ z.value + sum.value ^ ( z.value >> 5 ) + k[1]
        sum.value -= delta
        n -= 1

    w[0] = y.value
    w[1] = z.value
    return w

if __name__ == "__main__":
    ans = []
    for i in range(0, len(flag_en), 2):
        v = flag_en[i:i+2:1]
        k = cipher
        tmp = decipher(v, k)
        ans += tmp

    for i in ans:
        while i: 
            print(end = chr(i & 0xff))
            i >>= 8
    # PTITCTF{bdc90e23aa0415e94d0ac46a938efcf3}
```

# MÃ HÓA BASE64

```PYTHON
# MÃ HÓA
import base64
data = [

]
data_bytes = bytes(data)
base64_encoded = base64.b64encode(data_bytes)
base64_string = base64_encoded.decode('utf-8')
print(base64_string)
```

```PYTHON
# GIẢI MÃ
import base64
base64_string = "SGVsbG8sIHdvcmxkIQ=="
decoded_bytes = base64.b64decode(base64_string)
print(decoded_bytes)
decoded_bytes = list(decoded_bytes)
for i in range(len(decoded_bytes)):
    if i % 10 == 9: print(f'0x{decoded_bytes[i]:02X}', end = ',\n')
    else: print(f'0x{decoded_bytes[i]:02X}', end = ', ')
```