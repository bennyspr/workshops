# Cryptography: Practice Exercises

## Caesar Cipher

For the following encrypted texts, find the plaintext message using the Caesar Cipher:

1. Xlmw mw xli jswx   
2. Wklv lv d whvw phvvdjh  
3. Lzw espql lyo zqopb  

---

## Vigenère Cipher

For the following encrypted texts, find the plaintext using the **Vigenère Cipher** and the given keys:

1. **Key**: `ccom`  
   > gn gmrq rq pchgtczqu

2. **Key**: `pirate`  
   > ml, zaaum shf bgu lgdcd ztatx ts dqgcy  

3. **Key**: `shadow`  
   > yb vbcw fn azs iwxjy jjjpizmu cw yyvn xbq gqjtjws, zvkg dp jnxz fr!

4. **Key**: `cipher`  
   > xjva hcbaqle q vxg'x hnrs vqmr, hdkec'n gtat te iwlpitvaxq  

---

## AES (256-bit)

Follow these steps to practice file encryption and decryption using AES-256 and OpenSSL:

1. Create a text file with a short message:
   ```bash
   echo "This is a secret message." > message.txt
   ```

2. Encrypt the file using AES-256:
   ```bash
   openssl enc -aes-256-cbc -salt -in message.txt -out message.enc -k yourpassword
   ```

3. Decrypt the file:
   ```bash
   openssl enc -aes-256-cbc -d -in message.enc -out decrypted.txt -k yourpassword
   ```

> Replace `yourpassword` with any password you choose. You can open `decrypted.txt` to confirm that it matches the original message.


---

## RSA

Use the provided RSA private and public keys to decrypt the hex-encoded message. You may use any online RSA decryption tool or a custom script.

### Set 1

**Keys**:  
Private (d, n):  
`(1e048059,8310ea48c887443b8ffd409399a969a7)`  
Public (e, n):  
`(6349ca9d80aed3e14503a3f1505a47e9,8310ea48c887443b8ffd409399a969a7)`

**Encrypted Message (Hex)**:
```
39a240c7569b59dcbf9596e06d1a8e34
```

### Set 2

**Keys**:  
Private (d, n):  
`(31f5f433,65709d14f94447cc41bcdd542e555483)`  
Public (e, n):  
`(3e4dfabb9e1beb8fb9c8a9b9347b877b,65709d14f94447cc41bcdd542e555483)`

**Encrypted Message (Hex)**:
```
2bfd6d093341e004fcd23a276792a5aa
```
