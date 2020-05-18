# ASIS_CTF_Quals_2019 
## delicious soup - crypto
let me see the encode code:
```python
#!/usr/bin/env python
#-*- coding:utf-8 -*-

import random
from flag import flag

def encrypt(msg, perm):
    W = len(perm)
    while len(msg) % (2*W):
        msg += "."
    msg = msg[1:] + msg[:1] # move first letter to the end of string
    msg = msg[0::2] + msg[1::2] # abcdef -> acebdf
    msg = msg[1:] + msg[:1] # move first letter to the end of string
    res = ""
    for j in xrange(0, len(msg), W):
        for k in xrange(W):
            res += msg[j:j+W][perm[k]] # read W chars at one time and change the postion of every char base on perm list
    msg = res
    return msg

def encord(msg, perm, l):
    for _ in xrange(l): # encrypt the flag l times
        msg = encrypt(msg, perm)
    return msg

W, l = 7, random.randint(0, 1337) # w =7, l in (0, 1337)
perm = range(W) # (0,1,2,3,4,5,6)
random.shuffle(perm) # shuffle perm

enc = encord(flag, perm, l)
f = open('flag.enc', 'w')
f.write(enc)
f.close()
```


[decrypt_reference](https://abcdsh.blogspot.com/2019/04/writeup-asis-2019-quals-delicious-soup.html)
decrypt script:
```python
import random
import itertools
 
 
def decrypt(enc,perm):
        W = len(perm)
        msg = "-"*len(enc)
        for j in xrange(0,len(enc),W):
                for k in xrange(W):
                        try:
                                m = enc[k+j] # pick every char in current loop one by one
                        except: #unknown bug 
                                pass
                        msg = msg[:j+perm[k]] + m + msg[j+perm[k]+1:] # put char in original position base on perm list
        msg = msg[-1] + msg[:-1]
        m = ""
        w = len(msg)/2
        for i in range(len(msg)/2):
                m += msg[i]+msg[w+i]
        msg = m
        msg = msg[-1] + msg[:-1]
        return msg.strip(".")
 
f = open("flag.enc","r")
enc_c = f.read()
print("Decrypting",enc_c)
         
W = 7
l = 1337
for perm in itertools.permutations(range(W)):
        enc = str(enc_c)
        for _ in xrange(l):
                enc = decrypt(enc,perm)
                if enc.startswith("ASIS"):
                        print(enc)
```


> msg = msg[:j+perm[k]] + m + msg[j+perm[k]+1:] # put char in original position base on perm list

* msg = "-------"
* enc -> dflcjh. plaintext-> fdhjl.c
* m = d
* msg[:j+perm[k]] -> j=0, k=0, perm[k] = 1 -> msg[1] = '-'
* msg[j+perm[k]+1:] -> '-----'
* msg = '-d-----'

## Halloween Party - crypto
Elliptic Curve: $y^2 \xi x^3 + ax + b mod p$
```python
#!/usr/bin/env python

from fastecdsa.curve import Curve
from fastecdsa.point import Point
from Crypto.Util.number import *
from secret import EC_params, flag, Q


p, a, b, q, Px, Py = EC_params
C = Curve('halloween', p, a, b, q, Px, Py)
P = Point(Px, Py, curve = C)
P1 = Point(p + 1, 467996041489418065436268622304855825266338280723, curve = C)
P2 = Point(p - 1, 373126988100715326072483107245781156204485119489, curve = C)
P3 = Point(p + 3, 245091091146774561796627894715885724307214901148, curve = C)

assert (2*P).x == Q.x
assert bytes_to_long(flag) == P.x
assert ((-1) * Q).y == 621803439821606291947646422656643138592770518069
```
1. find a b p
using P1, P2, P3 to calculate (a,b,p):
a = 48029713913392144447486256568923103286673283937
b = 433481663214462017150295835098295925800218140157
p = 883097976585278660619269873521314064958923370261

2. find x-coordinate of Q
the P, Q, P+P on the curve will be:
![](https://blog.bi0s.in/2019/04/23/Crypto/Elliptic-Curves/asis-quals19-halloween-party/1.png)
To find x-coordinate of Q by $x^3 + a∗x + (b − y^2) ≡ 0 mod p$: (708927573459527177103235542148826237228344428002)
Solve[x^3 + 48029713913392144447486256568923103286673283937 x + 837637963235166117552443765282645351326329278968 == 0, x, Modulus ->883097976585278660619269873521314064958923370261]
* backgound knowledge: [https://andrea.corbellini.name/2015/05/23/elliptic-curve-cryptography-finite-fields-and-discrete-logarithms](https://andrea.corbellini.name/2015/05/23/elliptic-curve-cryptography-finite-fields-and-discrete-logarithms/)
    * (5 + (-5)) mod 23 = (5 + 18) mod 23 = 0 -> a + b mod p = a + c mod p = 0 -> b-c | p -> b-c整除p
    * $b - y^2$ = r =-386639517773981957349206006095857646440196270939341612516183086632162532949706312209990437348604 -> too large
    * find a number - r mod p = 0 -> the number is 837637963235166117552443765282645351326329278968 = r % p

3. find x-coordinate of P(flag):

