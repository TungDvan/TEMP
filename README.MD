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
