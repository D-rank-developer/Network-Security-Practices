## Part 1: Network Security Context  
### Where Do Hash Functions Appear in Networks?

Hash functions are fundamental to network security and appear in:

1. **Network Protocol Security**
   - **TLS/SSL handshakes:** verifying certificate integrity and supporting secure session establishment
   - **IPsec:** ensuring packet integrity using **HMAC** (Hash-based Message Authentication Code)
   - **SSH:** host key fingerprints used for server verification

2. **Authentication Systems**
   - Password storage in authentication servers
   - Challenge-response protocols (e.g., CHAP / MS-CHAPv2)
   - Digital signatures in VPNs and secure communications

3. **Data Integrity**
   - File transfer verification (checksums in FTP/SFTP—note: checksums aren’t always cryptographic)
   - Firmware update verification for network devices (routers/switches)
   - Packet integrity checks (e.g., TCP checksum—integrity, not cryptographic security)

4. **Modern Applications**
   - Blockchain and distributed systems
   - Certificate pinning in mobile apps
   - Git version control (often used in network configuration management)

---

## Part 2: What is a Cryptographic Hash Function?

A cryptographic hash function is a mathematical algorithm that:

- Takes an input (message) of any length  
- Produces a fixed-size output (hash/digest)  
- Is deterministic (same input → same output)  
- Is one-way (computationally infeasible to reverse)

### Visual Example

**Input:** `RouterConfig_Version1` → **Hash Function** → **Output (digest)**  
97d8d2f1c45af2cbfe949881d6e85867c97466ccb3baa84687d7ceabf5f8992f


![Command Line SHA256](https://github.com/D-rank-developer/Network-Security-Practices/blob/2741c3a52b57a91c75510c2d3d66dc65bb523ea3/Applied%20Cryptography/Hash%20files/image12.png)

### Hashing vs Encryption (Key Difference)

- **Hashing:** one-way; used for integrity and verification  
- **Encryption:** two-way; used for confidentiality (decryptable with a key)

---

## Part 3: Essential Characteristics of Cryptographic Hash Functions

1. **Deterministic**  
   Same input always produces the same hash output.

2. **Fixed Output Size**  
   Output length stays constant regardless of input size.  
   - **MD5:** 128 bits (32 hex characters)  
   - **SHA-256:** 256 bits (64 hex characters)

3. **Avalanche Effect**  
   A small change in input creates a drastically different output.

4. **One-Way (Pre-image Resistance)**  
   Computationally infeasible to reverse a hash to recover the original input.

5. **Collision Resistance**  
   Extremely difficult to find two different inputs producing the same hash.

**Avalanche example (MD5):**
"password" → 5f4dcc3b5aa765d61d8327deb882cf99
"Password" → dc647eb65e6711e155375218212b3964


---

## Part 4: Network Security Scenario — SSH Host Key Verification

### Scenario
Connecting to a remote server for the first time via SSH can show:

The authenticity of host '192.168.1.100' can't be established.
ECDSA key fingerprint is SHA256:7b2d1f4ac9e8d3a6f5b8c1e9d2a7f4b3c6e8d1a9f5b2c7e4d8a3f6b9c2e5d1a8.
Are you sure you want to continue connecting (yes/no)?



### Why does SSH use hash functions?
- Public keys are long (2048+ bits); fingerprints are shorter and human-checkable
- A known-good fingerprint can be verified through a separate trusted channel
- Helps prevent **Man-in-the-Middle (MITM)** attacks

![SSH Key Fingerprint](https://github.com/D-rank-developer/Network-Security-Practices/blob/2741c3a52b57a91c75510c2d3d66dc65bb523ea3/Applied%20Cryptography/Hash%20files/image11.png)

---

## Part 5: Hands-On Exercise 1 — Testing Hash Functions

### Exercise 1A: Understanding the Avalanche Effect

I hashed the following network-related strings and recorded the results.

**Input 1:** `RouterPassword123`  
![RouterPassword123 Hash](https://github.com/D-rank-developer/Network-Security-Practices/blob/2741c3a52b57a91c75510c2d3d66dc65bb523ea3/Applied%20Cryptography/Hash%20files/image2.png)  
**Hash:** `e0edc8e9e4fd823857c3481bbecd8f49cab770ad3fa15361f9d462000c5e9ea`

**Input 2:** `routerPassword123` (lowercase `r`)  
![routerPassword123 Hash](https://github.com/D-rank-developer/Network-Security-Practices/blob/2741c3a52b57a91c75510c2d3d66dc65bb523ea3/Applied%20Cryptography/Hash%20files/image1.png)  
**Hash:** `fe99c7a14610afb739055f02e5be9d5538218aaf7badfd291784aa51f924986b`

**Input 3:** `RouterPassword124` (changed last digit)  
![RouterPassword124 Hash](https://github.com/D-rank-developer/Network-Security-Practices/blob/2741c3a52b57a91c75510c2d3d66dc65bb523ea3/Applied%20Cryptography/Hash%20files/image3.png)  
**Hash:** `5766936d62e971f4a43738e4bc76ab9beb1117b2b1e41526dfa32452a28e1547`

#### Observation Questions (Answers)

1) **How different are the hashes when only one character changes?**  
Even a 1-character change produces a completely different digest. The outputs are visually unrelated.

2) **Why is this important for network security?**  
It ensures integrity verification is strong: attackers cannot modify data slightly without changing the hash drastically.

3) **How would this affect an attacker trying to guess passwords?**  
Attackers cannot “incrementally” guess a password from the hash; they must brute force or use dictionaries/rainbow tables.

---

### Exercise 1B: Hash Function Comparison

**Input:** `10.0.0.1:admin:networksec2024`

**MD5 Hash (128-bit):**  
![MD5 Hash](https://github.com/D-rank-developer/Network-Security-Practices/blob/2741c3a52b57a91c75510c2d3d66dc65bb523ea3/Applied%20Cryptography/Hash%20files/image4.png)  
**Hash:** `3dd553e52a158d572f50bea38011352e`

**SHA-1 Hash (160-bit):**  
> The following screenshot was added because it was omitted during the study.

![SHA-1 Hash (Online Tool)](Hash%20files/image5.png)  
**Hash:** `8e877f0b66f608b4215ab9b8116166b2b2e4bca`

**SHA-256 Hash (256-bit):**  
![SHA-256 Hash](https://github.com/D-rank-developer/Network-Security-Practices/blob/2741c3a52b57a91c75510c2d3d66dc65bb523ea3/Applied%20Cryptography/Hash%20files/image6.png)  
**Hash:** `b45e21c1facafee817dec1f5e601672075fd754f9929f1e28eeabbf4111fa98d`

#### Analysis Questions (Answers)

1) **Which algorithm produces the longest hash?**  
SHA-256 produces the longest digest here (256-bit).

2) **Why might longer hashes be more secure?**  
Longer hashes increase the search space and make collision/brute-force attempts more expensive.

3) **Which algorithms are considered insecure today?**  
**MD5** and **SHA-1** are considered cryptographically broken for many security uses (collision attacks), and should not be used for new secure designs.

---

## Part 6: Network Authentication Scenario (RADIUS Example)

### Scenario
A network administrator manages authentication for multiple network devices. Credentials are stored in a RADIUS database.

![Authentication Database](https://github.com/D-rank-developer/Network-Security-Practices/blob/2741c3a52b57a91c75510c2d3d66dc65bb523ea3/Applied%20Cryptography/Hash%20files/image7.png)

| Username | Stored Hash | Hash Algorithm |
|---|---|---|
| admin@router.local | 5f4dcc3b5aa765d61… | MD5 |
| netadmin@switch.local | 8d969eef6ecad3c… | SHA-256 |
| firewall@corp.local | fc46e6bb74e235a5a… | MD5 |

**Security Issue:** MD5 hashing without salt is used for password storage.  
**Attack Vector:** An attacker gains read-only access to the authentication database during a breach.

---

## Part 7: Hands-On Exercise 2 — Rainbow Table Attack

### Understanding Rainbow Tables
Rainbow tables are precomputed lookup tables of hashes for common passwords:
- Attackers hash millions of common passwords in advance
- When they obtain a password hash, they search the table
- If found, the plaintext password is revealed quickly
- This is why **unsalted** hashes are vulnerable

---

### Exercise 2A: Cracking a Network Password (MD5)

**MD5 hash:** `44a34d475e43395047ae67c20a1024f2`  
![MD5 Hash Lookup](https://github.com/D-rank-developer/Network-Security-Practices/blob/2741c3a52b57a91c75510c2d3d66dc65bb523ea3/Applied%20Cryptography/Hash%20files/image8.png)

**Result:** `Student1`

#### Questions (Answers)

1) **How quickly was the password recovered?**  
Almost instantly (seconds). This demonstrates how dangerous unsalted weak-password hashes are.

2) **What does this tell you about password strength?**  
Common passwords are highly vulnerable, even if stored as hashes.

3) **Why did the attack work?**  
Because the hash was unsalted and the password is common—already present in rainbow tables.

---

### Exercise 2B: Testing a Stronger Algorithm (SHA-1)

**SHA-1 hash:** `896bcd1ab6d937bdb63472d3dee064b7830f34d5`  
![SHA-1 Hash Lookup](https://github.com/D-rank-developer/Network-Security-Practices/blob/2741c3a52b57a91c75510c2d3d66dc65bb523ea3/Applied%20Cryptography/Hash%20files/image9.png)

**Result:** `Student1`

#### Questions (Answers)

1) **Were you able to crack this hash?**  
Yes—because the password is weak and widely known.

2) **Did using SHA-1 instead of MD5 protect the password?**  
Not really. Weak passwords can be cracked across many algorithms using dictionaries/rainbow tables.

3) **What does this tell you about relying only on the hash algorithm?**  
Algorithm choice matters, but password strength and salting/adaptive hashing are essential. A strong algorithm alone is not enough if passwords are weak.

---

### Mitigation: Salted Hash

**Salted MD5 hash of** `Student1` + salt `x9K2mP7q`:  
`f9615b3171297b809d34a5f3aa2ca7f6`  

![Salted Hash Lookup](https://github.com/D-rank-developer/Network-Security-Practices/blob/2741c3a52b57a91c75510c2d3d66dc65bb523ea3/Applied%20Cryptography/Hash%20files/image10.png)

**Result:** Not found / could not be reversed in the lookup database.

**Why it didn’t crack:**
- The real input was `Student1x9K2mP7q`
- That salted combination is not in rainbow tables
- Salt makes each password hash unique, even for identical passwords

---

## Part 8: Defence Mechanisms in Modern Networks

### 1) Salting
A random value (salt) is added before hashing:
- User 1: `hash("password" + "x8K3mP9q")` → unique hash  
- User 2: `hash("password" + "a5Qq2Zr8")` → different unique hash  

**Result:** Rainbow tables become ineffective because each user’s hash is unique.

### 2) Modern Password Hashing Algorithms
Instead of MD5/SHA-1/SHA-256 for password storage, modern systems prefer:
- **bcrypt** (adaptive + built-in salt)
- **scrypt** (memory-hard; resists GPU attacks)
- **Argon2** (PHC winner, 2015)
- **PBKDF2** (widely used; e.g., WPA2)

These are **slow by design**, configurable, and include salting.

### 3) Network Implementation Examples

**Cisco IOS Password Types**
- Type 5: MD5 hash with salt (better than plain MD5, still not ideal)
- Type 8: PBKDF2-SHA256 (recommended)
- Type 9: scrypt (strong)



---
## Summary: 

Hash functions are core to network security (TLS, IPsec, SSH, authentication).

Key properties: deterministic, fixed output size, avalanche effect, one-way, collision resistance.

Rainbow tables exploit weak passwords + unsalted hashes.

MD5 and SHA-1 are not recommended for modern secure systems.

Strong defences include salting, adaptive password hashing (bcrypt/scrypt/Argon2), and strong password policy.

Hashing supports integrity, authentication, and trust in secure communications.

