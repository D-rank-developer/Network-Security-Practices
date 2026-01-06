# Applied Cryptography - Hash Files Analysis

## Overview
This document analyzes various cryptographic operations captured in terminal sessions, including SSH key generation, hash computations, and password security practices.

## Image Analyses

### Image 11: SSH Key Fingerprint
![Image 11](https://github.com/D-rank-developer/Network-Security-Practices/blob/2741c3a52b57a91c75510c2d3d66dc65bb523ea3/Applied%2520Cryptography/Hash%2520files/image11.png)

**Command:** `ssh-keygen -lf /etc/ssh/ssh_host_ed25519_key.pub`

**Description:** This command displays the fingerprint of the ED25519 SSH host key using SHA256 hashing. The `-l` flag lists the fingerprint, and `-f` specifies the file.

**Key Information:**
- Key type: ED25519
- Fingerprint: `SHA256:otwAFN/2C+1 jNpD8BRZqqu@gpPGBm7gglhqF idu2bW@rw`
- Owner: `root@ububtuserver`
- Key size: 256 bits

**Security Context:** Checking SSH key fingerprints helps verify host authenticity and prevent man-in-the-middle attacks.

### Image 12: SHA256 Hash of RouterConfig_Version1
![Image 12](https://github.com/D-rank-developer/Network-Security-Practices/blob/2741c3a52b57a91c75510c2d3d66dc65bb523ea3/Applied%2520Cryptography/Hash%2520files/image12.png)

**Command:** `echo -n RouterConfig_Versionl | sha256sum`

**Description:** Generates SHA256 hash of the string "RouterConfig_Versionl" (note the typo: "Versionl" instead of "Version1"). The `-n` flag prevents adding a newline character.

**Output:** `97d8d2flc45af2cbfe949881d6e85867c97466ccb3baa84687d7ceabf5f8992f`

**Function:** Hash functions like SHA256 are used for data integrity verification, digital signatures, and password storage.

### Image 13: SHA256 Hash of RouterConfig_Version2
![Image 13](https://github.com/D-rank-developer/Network-Security-Practices/blob/2741c3a52b57a91c75510c2d3d66dc65bb523ea3/Applied%2520Cryptography/Hash%2520files/image13.png)

**Command:** `echo -n RouterConfig_Version2 | sha256sum`

**Description:** Similar to Image 12 but hashes "RouterConfig_Version2".

**Output:** `52a7a88814e64c186e76ce258f22abfflb126e4b41b281eb6bfe6a18f8725b448`

**Comparison:** The completely different hash output demonstrates the avalanche effect - small input changes produce vastly different hashes.

### Image 14: MD5 Hashes of Password Variations
![Image 14](https://github.com/D-rank-developer/Network-Security-Practices/blob/2741c3a52b57a91c75510c2d3d66dc65bb523ea3/Applied%2520Cryptography/Hash%2520files/image14.png)

**Commands:**
```bash
echo -n "password" | md5sum
echo -n "Password" | md5sum
```

**Outputs:**
- "password": `5f4dcc3b5aa765d61d8327deb882cf99`
- "Password": `dc647eb65e6711e155375218212b3964`

**Security Note:** MD5 is cryptographically broken and should not be used for security purposes. Case sensitivity significantly changes the hash.

### Image 15: SHA256 Hashes of Router Passwords
![Image 15](https://github.com/D-rank-developer/Network-Security-Practices/blob/2741c3a52b57a91c75510c2d3d66dc65bb523ea3/Applied%2520Cryptography/Hash%2520files/image15.png)

**Commands:**
```bash
echo -n "RouterPassword123" | sha256sum
echo -n "routerPassword123" | sha256sum
echo -n "RouterPassword124" | sha256sum
```

**Outputs:**
- `fe99c7a1461&afb739855f82e5be3d5538218aaf7badfd291784aa51f824986b`
- `e8edc8e9e4fd823857c3481bbecd84f49cab77&ad3fa15361f9d4628&ac5e9ea`
- `5766936d62e971f4a43738e4bc76ab8beb1117b2b1e41526dfa32452a28e1547`

**Analysis:** Demonstrates how minor variations (case, number increment) create completely different SHA256 hashes.

### Image 16: SSH Host Key Verification
![Image 16](https://github.com/D-rank-developer/Network-Security-Practices/blob/2741c3a52b57a91c75510c2d3d66dc65bb523ea3/Applied%2520Cryptography/Hash%2520files/image16.png)

**Command:** `ssh steve@stapp02.stratos.xfusioncorp.com`

**Description:** SSH connection attempt showing the host's ED25519 key fingerprint verification prompt.

**Fingerprint:** `SHA256:JuCogEA30/yGUngD92kqPo2tXxZpgMZ3QkFhQhzJvg`

**Security Practice:** Users should verify this fingerprint matches the server's known fingerprint before connecting.

### Image 17: Multiple Hash Algorithms Comparison
![Image 17](https://github.com/D-rank-developer/Network-Security-Practices/blob/2741c3a52b57a91c75510c2d3d66dc65bb523ea3/Applied%2520Cryptography/Hash%2520files/image17.png)

**Commands:**
```bash
echo -n "10.0.0.1:admin:networksec2024" | md5sum
echo -n "10.0.0.1:admin:networksec2024" | sha1sum
echo -n "10.0.0.1:admin:networksec2024" | sha256sum
```

**Outputs:**
- MD5: `3dd553e52a158d572f5bbad36011352e`
- SHA1: `9e97f78b6bf68bb4215ab9b8118166bf2b2e4bca`
- SHA256: `b45e21c1facafec817dec1f5e0016f2075fd75af0929f1e28eeabbf4111fa9bd`

**Comparison:** Shows different hash lengths and outputs for the same input string across algorithms.

### Image 18: MD5 Hash of "password"
![Image 18](https://github.com/D-rank-developer/Network-Security-Practices/blob/2741c3a52b57a91c75510c2d3d66dc65bb523ea3/Applied%2520Cryptography/Hash%2520files/image18.png)

**Command:** `echo -n "password" | md5sum`

**Output:** `5f4dcc3b5aa765d61d8327deb882cf99`

**Note:** This matches the hash from Image 14, confirming consistency.

### Image 19: John the Ripper Installation
![Image 19](https://github.com/D-rank-developer/Network-Security-Practices/blob/2741c3a52b57a91c75510c2d3d66dc65bb523ea3/Applied%2520Cryptography/Hash%2520files/image19.png)

**Description:** Installation process of John the Ripper password cracking tool and its dependencies.

**Packages Installed:**
- `john-data` (1.9.8-2build1)
- `libgomp1:amd64` (14.2.8-4ubuntu2~24.84)
- `john` (1.9.8-2build1)

**Security Context:** John the Ripper is used for password strength testing and recovery.

### Image 20: Creating Hash File
![Image 20](https://github.com/D-rank-developer/Network-Security-Practices/blob/2741c3a52b57a91c75510c2d3d66dc65bb523ea3/Applied%2520Cryptography/Hash%2520files/image20.png)

**Command:** `echo 5f4dcc3b>aafbodb1ld83< deb88c<cf33 >hash.txt`

**Analysis:** This appears to be a malformed command attempting to write a hash to `hash.txt`. The correct syntax would be: `echo "5f4dcc3b5aa765d61d8327deb882cf99" > hash.txt`

### Image 21: SHA256 Hash of Complex Password
![Image 21](https://github.com/D-rank-developer/Network-Security-Practices/blob/2741c3a52b57a91c75510c2d3d66dc65bb523ea3/Applied%2520Cryptography/Hash%2520files/image21.png)

**Command:** `echo -n "Student1x9k2mP7q" | sha256sum`

**Output:** `d55a9f84851be3 f8c2a7be92 f9d329aGe66323c89 fd5e56097 f4b¥lbe7d34ed2`

**Note:** The output contains spaces and special characters, suggesting potential display or corruption issues.

### Image 22: Whois Installation
![Image 22](https://github.com/D-rank-developer/Network-Security-Practices/blob/2741c3a52b57a91c75510c2d3d66dc65bb523ea3/Applied%2520Cryptography/Hash%2520files/image22.png)

**Description:** Installation of the `whois` package (version 5.5.22) which is used for querying WHOIS databases.

**Security Context:** Whois helps in gathering domain registration information during reconnaissance phases.

### Image 23: Bcrypt Password Generation
![Image 23](https://github.com/D-rank-developer/Network-Security-Practices/blob/2741c3a52b57a91c75510c2d3d66dc65bb523ea3/Applied%2520Cryptography/Hash%2520files/image23.png)

**Command:** `mkpasswd -m bcrypt`

**Description:** Generates a bcrypt password hash. The `-m` flag specifies the method.

**Output Format:** `$2b$[cost]$[salt][hash]`

**Example:** `$2b$@5$T28F JpTD jhKskRmoJn2vd, »WxzkD3NoKG8qLRM6HWOH . zKglu jpeym`

**Security Advantage:** Bcrypt is designed to be computationally expensive, making brute-force attacks slower.

## Security Best Practices Demonstrated

1. **Key Fingerprint Verification** (Images 11, 16)
   - Always verify SSH host key fingerprints
   - Prevent man-in-the-middle attacks

2. **Hash Function Selection** (Images 12, 13, 15, 17)
   - Prefer SHA256 over MD5 and SHA1
   - MD5 and SHA1 are cryptographically broken

3. **Password Storage** (Image 23)
   - Use adaptive hashing algorithms like bcrypt
   - Include salt to prevent rainbow table attacks

4. **Case Sensitivity** (Images 14, 15)
   - Case changes create completely different hashes
   - Enhances password complexity

## Common Vulnerabilities

1. **Using Weak Hashes** (Images 14, 17, 18)
   - MD5 should not be used for security purposes
   - SHA1 is also deprecated for many security applications

2. **Simple Passwords** (Image 14)
   - Common passwords like "password" are easily cracked

3. **Command Typos** (Images 12, 13, 20)
   - Typos in commands can lead to unexpected results
   - "sha25bsunm" instead of "sha256sum"
   - Malformed echo command in Image 20

## Recommendations

1. Use SHA256 or SHA3 for data integrity checks
2. Use bcrypt or Argon2 for password storage
3. Always verify SSH fingerprints on first connection
4. Use password managers to generate and store complex passwords
5. Regularly audit and update cryptographic implementations
