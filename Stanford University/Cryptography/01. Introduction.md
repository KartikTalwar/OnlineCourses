# [Cryptography] (https://www.coursera.org/crypto/)

=================================

## Introduction

### Cryptography is everywhere

#### Secure Communications

**Web**

- HTTPS
- 802.11i WPA2

**Encrypting files on disk**

- EFS
- Truecrypt

**Content Protection**

- DVDs
- CSS (content scrambling system)

### Secure Communication

For web, the protocol is HTTPS (also called SSL/TLS)

#### Secure Sockets Layer / TLS

**Two main parts**

- Handshake Protocol:
    - Establish shared secrey key using public key cryptography
- Record Layer
    - Transmit data using shared secret key
    - Ensure confidentiality

#### Symmetric Encryption Systems

Alice and Bob share a secrey key `k` that the attacker does not know

```
        Alice                                  Bob
      _______                                _______
     |       |  E(k,m) = c   _____      c   |       |  D(k,c) = m
---->|   E   |------------->|  .  |-------->|   D   |------------->
     |_______|              |_____|         |_______|
         ^                                      ^
         |                                      |
         k                                      k
```

```
E, D : cipher
k    : Secret Key (128 bits or similar)
.    : Pipeline
m, c : Plaintext, Ciphertext
```

Encryption algorithm is publicly known

- Never use a proprietary cipher

### Use Cases

#### Single use key (one time key)

- Key is only used to encrypt one message
    - Email encryption

#### Multi use key

- Key used to encrypt multiple messages
    - Encrypted files
- Need more machinery than for one-time key

### Things to remember

#### Cryptography is

- A tremendous tools
- A basis for many security mechanisms

#### Cryptography is not

- Solution to all security problems
- Reliable unless implemented and used properly
- Something you should try to invent yourself

## What is Cryptography?

### Crypto Core

#### Depends on 

- Secret key Establishment
- Secure Communication

#### Digital Signatures

- Just like real life signatures where It's always the same
- In online world, its not always the same (not possible)

#### Anonymous Communication

- A Wants to talk to B anomynously
- Uses *mix net* uses proxies and makes the source untraceable 
    - It's bidirectional

#### Anonymous Digital Cash

- Can I spend a 'digital coin' without anyone knowing who I am?
- How to prevent double spending?
- You make sure that once A spends the coin once, nobody knows who she is but if the same coin is used again, you get exposed

### Protocols

#### Private Auctions

Winner is the highest bidder but the amount he pays is the 2nd highest bid

#### Secure Multi-Party Computation

- Goal : compute `$f(x1, x2, x3, x4)$`
    - How can they compute the output such that the value of the output is revealed but nothing else is revealed
- You do it through a trusted party
    - You reveal the data to a trusted source and they reveal it to the public

**Theorem**

Anything that can be done with a trusted authority, can also be done without the authority. As a solution, the inputs variables communicate with each other and compute the final answer

### Crypto Magic

#### Privately outsourcing computation

- Alice has a search query
- There are encryption schemes such that Alice can send her encrypted query to Google
- Google can compute the output of the value without knowing the the unencrypted plain text query was
- Google delivers encrypted results

#### Zero Knowledge (Proof of Knowledge)

```python
$N = p \times q$
```

- There is a number `N` which is a product of 2 large primes (~1000 digits)
- Finding the factors from the product is hard
- Alice knows `N`, `p` and `q`
- Bob only knows `N`
- Alice can prove to Bob that she knows the factorization of `N` and Bob can check that
- Bob learns nothing about `p` and `q`

### A Rigorous Science

#### The 3 steps in cryptography

1. Precisely specify the threat model
2. Propose a constructoin
3. Prove that breaking construction under threat mode will solve an underlying hard problem


## History of Cryptography

### Symmetric Ciphers


```
        Alice                                  Bob
      _______                                _______
     |       |  E(k,m) = c   _____     c    |       |  D(k,c) = m
---->|   E   |------------->|  .  |-------->|   D   |------------->
     |_______|              |_____|         |_______|

```
```
E, D : cipher
k    : Secret Key (128 bits or similar)
.    : Pipeline
m, c : Plaintext, Ciphertext
```

**These are symmetric ciphers because the encrypter and decrypter use the same keys**

### Substitution Ciphers

The key for this cipher tells you how to map letters

|     key     |     map      |
|:-----------:|:------------:|
|      a      |       c      |
|      b      |       w      |
|      c      |       n      |
|     ...     |      ...     |
|      z      |       a      |

```tex
c = E(k, "bcza") = "wnac"
D(k, c) = "bzca"
```

### Caesar Cipher

- It's a substitution cipher where you shift by `3`
- It's not a cipher since there is no key



|     key     |     map      |
|:-----------:|:------------:|
|      a      |       d      |
|      b      |       e      |
|      c      |       f      |
|     ...     |      ...     |
|      y      |       b      |
|      z      |       c      |

### Quiz

**What is the size of key space in the substitution cipher assuming 26 letters?**

- (a) |k| = 26
- (b) |k| = 26!
- (c) |k| = 2^26
- (d) |k| = 26^2

**Answer:** (b)

#### How to break a substitution cipher?

**What is the most common letter in English text?**

- (a) X
- (b) L
- (c) E
- (d) H

**Answer:** (c)

#### Breaking it

1. Step One
    - Using frequency of English letters
        - **E** appears 12.7%
    - You examine the cipher and look for the character that is the most repeated
        - That character is the mapping of **E**
    - You repeat this for the second most frequent letter
        - That's the mapping of the letter **T**
        - Third most used letter is **A**

2. Step 2
    - Use frequency of pairs of letters (diagrams)
        - Count how many times each pair of text appears
    - You know that in english most common pairs are 
        - **he**, **an**, **in**, **th**    


### Vigener Cipher

```
k = CRYPTO
m = WHATANICEDAYTODAY
          ^
          |
k = CRYPTOCRYPTOCRYPT
m = WHATANICEDAYTODAY
          ^
          |
        _     _
k = CR |Y| P |T| OCRYPTOCRYPT
m = WH |A| T |A| NICEDAYTODAY   (mod 26)
_______|_|____________________ +
c = ZZ |Z| J |U| CLUDTNUWGCQS
```

#### Encryption

- Key is a word
- You write the message under the key
- Replicate the key as many times you need it to cover the message
- Then you add the key letters to the message letters `mod 26`

#### Decryption

- You need to know the length of the key (6 in our case)
- Break the text to groups of 6 letters
- Look at the first letter in each group
- The most common letter in the set is likely to be the mapping of letter `E`
    - Suppose most common = `H`
    - First letter of key = `H` - `E` = `C`
- Subtract the key from the cipher text


### Data Encryption Standard

- DES
    - Keys : 2^56
    - Block Size: 64 bits
- AES
    - 128 Bit key
- Salsa20


## Discrete Probability

### Finite Set

#### U

```
$U = {0, 1}^n$
```

#### Definition

Probability distribution `P` over `U` is a function `$P:U -> [0, 1]$` such that `$sum{X}{U} P(x) = 1$`

#### Examples

**Uniform Distribution** 

```
for all X in U: P(x) = 1/|U|
```

**Point Distribution at `x_0`**

```
P(x_0) = 1
for all x != x_0 : P(x) = 0
```

Distribution Vector  `$dim[2^n]$`

```
P(000), P(001), P(010), ..., P(111 )
```

### Notation

For a set `A subset U :`

```
Pr[A] = sum_x-in-A P(x)   is element of [0,1]
```
The set `A` is called an **event**

#### Example

```
A = { all x in {0,1}^n such that lsb_2(x) = 11 }
        for the uniform distribution on {0, 1}^n: Pr[A] = 1/4
```

### Union Bound

For events `A_1` and `A_2`

```
Pr[A_1 union A_2] <= Pr[A_1] + Pr[A_2] 
```

```
A_1 = { all x in {0,1}^n such that lsb_2(x) = 11 };
A_2 = { all x in {0,1}^n such that msb_2(x) = 11 };

Pr[lsb_2(x) = 11 or msb_2(x) = 11] 
= Pr[A_1 union A_2]  <= 1/4  + 1/4
= 1/26
```

### Random Variables

#### Definition

A random variable `X` is a function `X : U -> V`

#### Example

```
$X : {0, 1}^n  \rightarrow {0, 1,..., n};$
X(y) = #1's (y)        [y element of {0, 1}^n]
```

Random variable `X` induces a distribution on `V`

```
$Pr[X=v] := Pr[X^-1(v)]$
```
#### Example

In the example for `v element of {0, 1, \dots n}`

```
$Pr[X=v] = \frac{n \choose v}{2^n}$
```


### Randomized Algorithms

#### Deterministic Algorithm

```
$y \leftarrow A(x)$
```

- `y` is the output of algorithm `A` on input `x`

### Randomized

``` 
           ---> random
          |
y <- A(x, r)

      OR

      r
y  <----   A(x)
```

where `r` is uniform in `{0, 1}^n`
 

### Independence

#### Definition

Events `A` and `B` are independent if:  

```
$Pr[A and B] = Pr[A] * Pr[B]$
```
### Independent variables

Random variables `$X, Y : U \rightarrow V$` are independent if 

```
for all a and for all b in V :
$Pr[X=a and Y=b] = Pr[X=a] * Pr[Y=b]$
```

#### Theorem

`A` a random variable over `{0, 1}^n`, `X` an independent uniform variable on `{0, 1}^n`, Then `Y := A xor X` is uniform variable on `{0, 1}^n`

- XOR is addition of bits mod 2

**Proof**

|  A  | Pr  |
|:---:|:---:|
|  0  | P_0 |
|  1  | P_1 |

|  X  | Pr  |
|:---:|:---:|
|  0  | 1/2 |
|  1  | 1/2 |

| A   |  X  | combined |
|:---:|:---:|:--------:|
|  0  |  0  |   P_0/2  |
|  0  |  1  |   P_0/2  |
|  1  |  0  |   P_1/2  |
|  1  |  1  |   P_1/2  |


Probability that XOR is zero is 

- Probability when `A` and `X` are `0` and `0`
- Probability when `A` and `X` are `1` and `1`

```
  $Pr[Y=0] = P_0/2 + P_1/2 $
$= 1/2 * (P_0 + P_1) $
$= 1/2 $
```
