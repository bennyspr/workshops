# Cryptography: Encrypting & Decrypting Files

## What is Cryptography?

**Cryptography** is the science of protecting information by transforming it into a secure format. It ensures:

- **Confidentiality** – Only authorized people can read the information.
- **Integrity** – The message is not altered during transmission.
- **Authentication** – Verifying the sender’s identity.
- **Non-repudiation** – The sender cannot deny having sent the message.

---

## Types of Cryptography

### 1. Symmetric Encryption
- Uses the **same key** for both encryption and decryption.
- Fast and suitable for large files.
- Key must be shared securely.

**Examples**: AES, DES

### 2. Asymmetric Encryption
- Uses a **key pair**: a **public key** for encryption and a **private key** for decryption.
- Slower, but more secure for exchanging keys.

**Examples**: RSA, ECC

## Exercise 1: Symmetric Encryption Using OpenSSL (AES)

1. **Encrypt a file:**
```bash
openssl enc -aes-256-cbc -salt -in plain.txt -out encrypted.txt -pass pass:yourpassword
```

2. **Decrypt the file:**
```bash
openssl enc -aes-256-cbc -d -in encrypted.txt -out decrypted.txt -pass pass:yourpassword
```

Replace `yourpassword` with a strong password. You can also use `-pass file:./key.txt` to read the password from a file.

---

## Exercise 2: Asymmetric Encryption Using OpenSSL (RSA)

1. **Generate a private key:**
```bash
openssl genpkey -algorithm RSA -out private.pem -pkeyopt rsa_keygen_bits:2048
```

2. **Extract the public key:**
```bash
openssl rsa -pubout -in private.pem -out public.pem
```

3. **Encrypt a file using the public key:**
```bash
openssl rsautl -encrypt -inkey public.pem -pubin -in plain.txt -out encrypted.txt
```

4. **Decrypt the file using the private key:**
```bash
openssl rsautl -decrypt -inkey private.pem -in encrypted.txt -out decrypted.txt
```

RSA encryption is suitable only for small files. For larger data, encrypt the data with AES, then encrypt the AES key using RSA (hybrid approach).

---

## Exercise 3: Hybrid Encryption and Integrity Verification

1. **Generate a random symmetric key and save to file:**
```bash
openssl rand -base64 32 > sym.key
```

2. **Encrypt a file using the symmetric key:**
```bash
openssl enc -aes-256-cbc -salt -in plain.txt -out encrypted.txt -pass file:./sym.key
```

3. **Encrypt the symmetric key using the RSA public key:**
```bash
openssl rsautl -encrypt -inkey public.pem -pubin -in sym.key -out sym.key.enc
```

4. **Decrypt the symmetric key using the RSA private key:**
```bash
openssl rsautl -decrypt -inkey private.pem -in sym.key.enc -out sym.key.dec
```

5. **Decrypt the file using the decrypted symmetric key:**
```bash
openssl enc -aes-256-cbc -d -in encrypted.txt -out decrypted.txt -pass file:./sym.key.dec
```

6. **Generate a file hash for integrity checking:**
```bash
openssl dgst -sha256 plain.txt
```
