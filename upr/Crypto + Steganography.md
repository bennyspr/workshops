#  Crypto + Steganography

## Hashing Algorithm: MD5

MD5 (message-digest algorithm) is a cryptographic hashing algorithm that generates a unique 128-bit "fingerprint" (hash) of a piece of data. It is fast but considered **insecure** for modern use.

1. **Create a text file**:
```bash
# Linux & macOS
mkdir crypto && cd crypto
echo "This is my original text file." > super.txt

# Windows (Command Prompt)
mkdir crypto && cd crypto
echo This is my original text file. > super.txt
```

2. **Generate the MD5 hash of the file**:
```bash
# Linux
md5sum super.txt

# macOS
md5 super.txt

# Windows (Command Prompt)
certutil -hashfile super.txt MD5
```

*Write down the hash value you see.*

3. **Modify the file slightly**:
```bash
# Linux & macOS
echo "Adding a new line." >> super.txt

# Windows (Command Prompt)
echo Adding a new line. >> super.txt
```

4. **Re-run the MD5 command**:
```bash
# Linux
md5sum super.txt

# macOS
md5 super.txt

# Windows (Command Prompt)
certutil -hashfile super.txt MD5
```

*Compare the new hash with the one from step 2.*

![SHA1](https://media.tenor.com/F4XDxBjrbsUAAAAM/chris-pratt-wow.gif)

*Practice downloading one of the the Python Release file and compare the given MD5 Sum.*
https://www.python.org/downloads/release/python-3922/

---

## Hashing Algorithm: SHA-1

SHA-1 (Secure Hash Algorithm 1) is a cryptographic hash function that takes an input and produces a 160-bit (20-byte) hash value, also known as a message digest, typically displayed as 40 hexadecimal digits. It is also **not recommended** due to known vulnerabilities.

1. **Create a Text File**:

```bash
# Linux & macOS
echo "cryptography is fun" > zero.txt

# Windows (Command Prompt)
echo cryptography is fun > zero.txt
```

2. **Generate SHA-1 Hash**:

```bash
# Linux
sha1sum zero.txt

# macOS
shasum zero.txt

# Windows (Command Prompt)
certutil -hashfile zero.txt SHA1
```

*Write down the hash value you see.*

3. **Modify the File and Hash Again**:

```bash
# Linux & macOS
echo "!" >> zero.txt

# If using zsh on macOS, ! might trigger history expansion — use: 
echo \! >> zero.txt

# Windows (Command Prompt)
echo ! >> zero.txt
```

4. **Generate the SHA-1 hash again**:

```bash
# Linux
sha1sum zero.txt

# macOS
shasum zero.txt

# Windows (Command Prompt)
certutil -hashfile zero.txt SHA1
```

*What do you notice about the hash? Why did it change? Let’s simulate a hash check.*

5. **Save the original hash value in a file**:

```bash
# Linux
sha1sum zero.txt > hash.txt

# macOS
shasum zero.txt > hash.txt

# Windows (Command Prompt)
certutil -hashfile zero.txt SHA1 > hash.txt
```

6. **Verify file integrity with**:

```bash
# Linux
sha1sum -c hash.txt

# macOS
shasum -c hash.txt

# Windows (Manual workaround in PowerShell)
$expected = Get-Content hash.txt
$current = (Get-FileHash zero.txt -Algorithm SHA1).Hash
if ($expected -like "*$current*") {
    Write-Output "zero.txt: OK"
} else {
    Write-Output "zero.txt: FAILED"
}
```

7. **Try modifying the file again**:

```bash
# Linux & macOS
echo "ZZ" >> zero.txt

# Windows (Command Prompt)
echo ZZ >> zero.txt
```

8. **And verify the file integrity one more time**:

```bash
# Linux
sha1sum -c hash.txt

# macOS
shasum -c hash.txt

# Windows (Manual workaround in PowerShell)
$expected = Get-Content hash.txt
$current = (Get-FileHash zero.txt -Algorithm SHA1).Hash
if ($expected -like "*$current*") {
    Write-Output "zero.txt: OK"
} else {
    Write-Output "zero.txt: FAILED"
}
```

*What happens?*

![SHA1](https://media.tenor.com/m6IumtMMB3EAAAAM/ninersunited-elmo.gif)

---

## Hashing Algorithm: SHA-2

SHA-2 is a family of cryptographic hash functions designed to provide stronger security than its predecessor, SHA-1. It is commonly used for digital signatures, data integrity verification, and other security-related applications. The SHA-2 family includes algorithms like SHA-224, SHA-256, SHA-384, and SHA-512, each with different output sizes.

1. **Create a Text File**:

```bash
# Linux & macOS
echo "This is a secret message." > shashasha.txt

# Windows (Command Prompt)
echo This is a secret message. > shashasha.txt
```

2. **Generate SHA-256 Hash Using sha256sum**:

```bash
# Linux
sha256sum shashasha.txt

# macOS
shasum -a 256 shashasha.txt

# Windows (Command Prompt)
certutil -hashfile shashasha.txt SHA256
```
*Copy the hash value. This represents the fingerprint of the file.*

3. **Let's add something to the file**:

```bash
# Linux & macOS
echo "Extra line." >> shashasha.txt

# Windows (Command Prompt)
echo Extra line. >> shashasha.txt
```

4. **Now run the hash again**:

```bash
# Linux
sha256sum shashasha.txt

# macOS
shasum -a 256 shashasha.txt

# Windows (Command Prompt)
certutil -hashfile shashasha.txt SHA256
```
*You’ll notice the hash is completely different.*

5. **Verify the File Integrity**

   Let's say you received a file with a known SHA-256 hash (e.g., from a website). Save the expected hash in a file called shashasha.txt.sha256:

```bash
# Linux
sha256sum shashasha.txt > shashasha.txt.sha256

# macOS
shasum -a 256 shashasha.txt > shashasha.txt.sha256

# Windows (Command Prompt)
certutil -hashfile shashasha.txt SHA256 > shashasha.txt.sha256
```

6. **Then to verify:**

```bash
# Linux
sha256sum -c shashasha.txt.sha256

# macOS
shasum -a 256 -c shashasha.txt.sha256

# Windows (Using PowerShell)
$expected = Get-Content shashasha.txt.sha256 | ForEach-Object { ($_ -split '\s+')[0] }
$actual = (Get-FileHash shashasha.txt -Algorithm SHA256).Hash
if ($expected -eq $actual) {
    Write-Host "shashasha.txt: OK"
} else {
    Write-Host "shashasha.txt: FAILED"
}
```

7. **Now modify the file**:

```bash
# Linux & macOS
echo "Get To The Choppa!" >> shashasha.txt

# Windows (Command Prompt)
echo Get To The Choppa! >> shashasha.txt
```

6. **Verify one more time**:

```bash
# Linux
sha256sum -c shashasha.txt.sha256

# macOS
shasum -a 256 -c shashasha.txt.sha256

# Windows (Using PowerShell)
$expected = Get-Content shashasha.txt.sha256 | ForEach-Object { ($_ -split '\s+')[0] }
$actual = (Get-FileHash shashasha.txt -Algorithm SHA256).Hash
if ($expected -eq $actual) {
    Write-Host "shashasha.txt: OK"
} else {
    Write-Host "shashasha.txt: FAILED"
}
```
![Chopper](https://media.tenor.com/A-ozELwp694AAAAM/thumbs-thumbs-up-kid.gif)

---

## Rainbow Tables

Rainbow tables are precomputed lists of hash values used to reverse hash functions (i.e., crack passwords).

1. **Create a Super Wordlist**

```bash
# Linux & macOS
echo -e "123456\npassword\nqwerty\nadmin\nletmein" > superwordlist.txt

# Windows (Command Prompt)
(
echo 123456
echo password
echo qwerty
echo admin
echo letmein
) > wordlist.txt
```

2. **Use sha1sum to generate hashes for each password.**

```bash
# Linux
while read p; do
  echo "$p $(echo -n "$p" | sha1sum | awk '{print $1}')"
done < superwordlist.txt > rainbow_table.txt

# macOS
while read p; do
  hash=$(echo -n "$p" | shasum | awk '{print $1}')
  echo "$p $hash"
done < superwordlist.txt > rainbow_table.txt

# Windows (PowerShell)
Get-Content superwordlist.txt | ForEach-Object {
  $word = $_.Trim()
  $bytes = [System.Text.Encoding]::UTF8.GetBytes($word)
  $sha1 = [System.Security.Cryptography.SHA1]::Create().ComputeHash($bytes)
  $hash = ($sha1 | ForEach-Object { $_.ToString("x2") }) -join ""
  "$word $hash"
} | Out-File -Encoding ASCII -FilePath rainbow_table.txt
```

3. **Simulate a Stolen Hash**

Let’s say an attacker gets the hash 5baa61e4c9b93f3f0682250b6cf8331b7ee68fd8
(Which is the SHA-1 of password)

Now try to find the original password by searching the rainbow table:

```bash
# Linux & macOS
grep "5baa61e4c9b93f3f0682250b6cf8331b7ee68fd8" rainbow_table.txt

# Windows (Command Prompt)
findstr 5baa61e4c9b93f3f0682250b6cf8331b7ee68fd8 rainbow_table.txt
```

If it matches a line like:

```bash
password 5baa61e4c9b93f3f0682250b6cf8331b7ee68fd8
```

Then the password has been cracked using the rainbow table!

![RainbowTable](https://gifdb.com/images/thumbnail/funny-hacker-face-hacking-l0wrqon9090nlxbh.gif)

---

## Password Salt

A **salt** is a random string added to a password before hashing.

1. **Create a Password**:

```bash
# Linux & macOS
echo -n "mypassword123" > password.txt

# Windows (Command Prompt)
<nul set /p= "mypassword123" > password.txt
```

This simulates a user choosing their password.

2. **Hash the Password (without salt)**:

```bash
# Linux
sha256sum password.txt

# macOS
shasum -a 256 password.txt

# Windows (Command Prompt)
certutil -hashfile password.txt SHA256
```
You'll get a SHA-256 hash of the plain password.

3. **Generate a Random Salt**:

```bash
# Linux & macOS & Windows (Command Prompt)
openssl rand -hex 8
```
Example output: 793222801a00d43a

Copy this value. This is your salt.

4. **Combine Password + Salt**:

```bash
# Linux & macOS
cat password.txt > salted.txt
echo -n "793222801a00d43a" >> salted.txt

# Windows (Command Prompt)
copy /b password.txt + nul salted.txt
<nul set /p=4a7d1ed414474e4033ac29ccb8653d9b >> salted.txt
```

This combines the password and the salt in one file.

5. **Hash the Salted Password**:

```bash
# Linux
sha256sum salted.txt

# macOS
shasum -a 256 salted.txt

# Windows (Command Prompt)
certutil -hashfile salted.txt SHA256
```

This final hash is what you would store in a database. Never save the plain password.

![Password](https://media2.giphy.com/media/IgLIVXrBcID9cExa6r/200w.gif?cid=6c09b952h45h0c5ie37sv7j5p4rgg45qz65cr5ofwak7uepd&ep=v1_gifs_search&rid=200w.gif&ct=g)

---

## Digital Signatures with OpenSSL

Digital signatures ensure **integrity** and **authenticity** of a file.

1. **Generate Keys**

```bash
# Linux & macOS & Windows (Command Prompt)
openssl genpkey -algorithm RSA -out private_key.pem
openssl rsa -pubout -in private_key.pem -out public_key.pem
```

2. **Create a simple text file**:

```bash
# Linux & macOS
echo "This is my confidential message." > bunny.txt

# Windows (Command Prompt)
echo This is my confidential message. > bunny.txt
```

3. **Sign a File**

```bash
# Linux & macOS & Windows (Command Prompt)
openssl dgst -sha256 -sign private_key.pem -out bunny.sig bunny.txt
```

4. **Verify Signature**

```bash
# Linux & macOS & Windows (Command Prompt)
openssl dgst -sha256 -verify public_key.pem -signature bunny.sig bunny.txt
```

5. **Let's simulate failure**:

```bash
# Linux & macOS
echo "This is NOT the same message." > bunny.txt

# Windows (Command Prompt)
echo This is NOT the same message. > bunny.txt
```

Now verify again:

```bash
# Linux & macOS & Windows (Command Prompt)
openssl dgst -sha256 -verify public_key.pem -signature bunny.sig bunny.txt
```

![Bunny](https://media.tenor.com/-pxBz6S9UusAAAAM/rabbit-meow.gif)

---

## X.509 Certificates

X.509 is a standard for **public key certificates** used in SSL/TLS (e.g., HTTPS).

1. **Generate a Private Key**:

```bash
# Linux, macOS, or Windows (Command Prompt)
openssl genrsa -out monkey.pem 2048
```
2. **Create a Certificate Signing Request (CSR)**:

```bash
# Linux & macOS & Windows (Command Prompt)
openssl req -new -key monkey.pem -out monkey.csr
```
3. **Generate a Self-Signed Certificate**:

```bash
# Linux & macOS & Windows (Command Prompt)
openssl x509 -req -days 365 -in monkey.csr -signkey monkey.pem -out monkey.crt
```
4. **Inspect Your Certificate**:

```bash
# Linux & macOS & Windows (Command Prompt)
openssl x509 -in monkey.crt -text -noout
```
5. **Use the Certificate for Local HTTPS Testing**:

Create the Server Script
```bash
# Linux & macOS
nano https_server.py

# Windows (Command Prompt)
notepad https_server.py
```

Paste the following code:
```python
import http.server, ssl

server_address = ('127.0.0.1', 4443)
httpd = http.server.HTTPServer(server_address, http.server.SimpleHTTPRequestHandler)

httpd.socket = ssl.wrap_socket(httpd.socket,
                               keyfile="monkey.pem",
                               certfile="monkey.crt",
                               server_side=True)

print("Serving HTTPS on https://127.0.0.1:4443")
httpd.serve_forever()
```

Start the HTTPS Server
```bash
# Linux & macOS & Windows (Command Prompt)
python3 https_server.py
```

Try access https://127.0.0.1:4443 on your browser.

![Monkey](https://media3.giphy.com/media/xhKRxnoozhaYE/200w.gif?cid=6c09b952l0yovb4djdb6znrs45j9xu7g7eq1uykbbl1njg9t&ep=v1_gifs_search&rid=200w.gif&ct=g)

### `monkey.pem` — Private Key
- Secret key used to sign and decrypt traffic.
- Used to **sign data**, create CSRs, and serve encrypted HTTPS traffic.
- Should be **kept secret** and never shared.
- Format: PEM (Base64-encoded, starts with -----BEGIN PRIVATE KEY-----)

### `monkey.csr` — Certificate Signing Request
- A request to generate a valid certificate.
- Contains your public key and identifying information (domain, organization, etc.).
- Typically sent to a Certificate Authority (CA).
- Format: PEM (Base64-encoded, starts with -----BEGIN CERTIFICATE REQUEST-----)

### `monkey.crt` — X.509 Certificate
- The public certificate for authentication (TLS).
- Can be: Self-signed, or Signed by a CA after submitting a CSR.
- Contains your public key, metadata (e.g., expiration date), and a digital signature.
- Used to establish secure TLS/SSL connections.

---

## Steganography + Crypto

Steganography is the art of hiding information within media files.
You can encrypt data **before** hiding it to add an extra layer of security.

1. **Encrypt a message**:

```bash
# Linux & macOS
echo "I'm Batman." | openssl enc -aes-256-cbc -salt -out batman.enc
```

2. **Download a demo image**:

```bash
# Linux & macOS
curl -o upr.jpg https://www.uprrp.edu/wp-content/uploads/sites/9/2019/09/cropped-logo-icon.jpg
```

3. **Install Steghide**:

Steghide is a steganography tool that hides data within image or audio files, making it difficult to detect the presence of secret information.

```bash
# Linux (Debian/Ubuntu/Kali)
sudo apt update && sudo apt install -y steghide

# macOS (via Homebrew)
brew install steghide
```

4. **Embed encrypted file (hides an encrypted file inside an image)**:

```bash
# Linux & macOS
steghide embed -cf upr.jpg -ef batman.enc
```

5. **Extract Encrypted Message from Image**:

```bash
# Linux & macOS
steghide extract -sf upr.jpg -xf batman2.enc
```

6. **Decrypt the Message**:

```bash
# Linux & macOS
openssl enc -d -aes-256-cbc -in batman2.enc -out decryptedbatman.txt
```

7. **View the decrypted message**:

```bash
# Linux & macOS
cat decrypted.txt
```

![Batman](https://media.tenor.com/e_Mp1ObO2GkAAAAM/batman-batman-intensifies.gif)


If you want to run a "Matrix"-style falling text animation in the terminal, first install:
```bash
# Linux
sudo apt install cmatrix -y

# macOS (via Homebrew)
brew install cmatrix
```
And then run:
```bash
# Linux & macOS
cmatrix
```
