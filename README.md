# picoCTF-Gym-Double-DES
You know things are bad if I felt the need to create a writeup for a challenge

## What's Going On Here?
This is pretty much standard DES with super small keys except its encrypted twice. We're probably looking to do some sort of meet in the middle attack which is where we leverage the advantage that with keys A and B ```DEC_A(ENC_A(ENC_B(M))) = ENC_B(M)```

## What's The Catch?
Honestly the worst part of this challenge is the fact that the second encoding treats the strings as their literal byte values so instead of ```b'111111     '``` we're dealing with ```b'\x11\x11\x11     '```.
   
   
A close second might be the fact that ```YOU DON'T WRAP THE SOLUTION``` with picoCTF{}. Unlike literally every other challenge.
## Code Breakdown
I guess since its a write up I'll also break down the code
#### Talking To The Server
```
conn = pwn.remote('mercury.picoctf.net', 5958)
conn.recvuntil("Here is the flag:\n")
flag = conn.recvline().decode('utf-8').strip()
conn.recvuntil("What data would you like to encrypt? ")
conn.sendline('111111')
target = conn.recvline().decode('utf-8').strip()
conn.close()
```
#### Calculating The ENC_B(M)
```
for combo in itertools.product(string.digits, repeat=6):
    key = pad(''.join(combo))
    lookup[single_encrypt('111111', key)] = key
```
#### Calculating DEC_A(ENC_A(ENC_B(M)))
```
for combo in itertools.product(string.digits, repeat=6):
    key = pad(''.join(combo))
    candidate_pt = binascii.hexlify(single_decrypt(target, key)).decode()
    if candidate_pt in lookup:
        potential_keys.append({lookup[candidate_pt], key})
```
#### Trying The Different Keys Till Something Decodes
```
for (key1, key2) in potential_keys:
    try:
        print(double_decrypt(flag, key1, key2).decode())
        break
    except:
        continue
```
