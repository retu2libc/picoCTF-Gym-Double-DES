# picoCTF-Gym-Double-DES
You know things are bad if I felt the need to create a writeup for a challenge

## What's Going On Here?
This is pretty much standard DES with super small keys except its encrypted twice. We're probably looking to do some sort of meet in the middle attack which s where we leverage the advantage that with keys A and B ```DEC_A(ENC_A(ENC_B(M))) = ENC_B(M)```

## What's The Catch?
Honestly the worst part of this challenge is the fact that the second encoding treats the strings as their literal byte values so instead of ```b'111111     '``` we're dealing with ```b'\x11\x11\x11     '```.
A close second might be the fact that YOU DON"T WRAP THE SOLUTION WITH picoCTF{} like literally every other challenge.
