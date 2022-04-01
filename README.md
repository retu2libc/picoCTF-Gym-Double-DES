# picoCTF-Gym-Double-DES
You know things are bad if I felt the need to create a writeup for a challenge

## What's Going On Here?
This is pretty much standard DES with super small keys except its encrypted twice. We're probably looking to do some sort of meet in the middle attack which s where we leverage the advantage that with keys A and B ```DEC_A(ENC_A(ENC_B(M))) = ENC_B(M)```
