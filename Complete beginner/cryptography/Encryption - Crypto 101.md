Cryptography is used to protect confidentiality, ensure integrity, ensure authenticity.
- When you connect to SSH, your client and the server establish an encrypted tunnel so that no one can snoop on your session.
- When you connect to your bank, there’s a certificate that uses cryptography to prove that it is actually your bank rather than a hacker.
- When you download a file, how do you check if it downloaded right? You can use cryptography here to verify a checksum of the data.
- You rarely have to interact directly with cryptography, but it silently protects almost everything you do digitally.

#### Types of Encryption
The two main categories of Encryption are symmetric and asymmetric.
**Symmetric encryption** uses the same key to encrypt and decrypt the data. Examples of Symmetric encryption are DES (Broken) and AES. These algorithms tend to be faster than asymmetric cryptography, and use smaller keys (128 or 256 bit keys are common for AES, DES keys are 56 bits long).
**Asymmetric encryption** uses a pair of keys, one to encrypt and the other in the pair to decrypt. Examples are RSA and Elliptic Curve Cryptography. Normally these keys are referred to as a public key and a private key. Data encrypted with the private key can be decrypted with the public key, and vice versa. Your private key needs to be kept private, hence the name. Asymmetric encryption tends to be slower and uses larger keys, for example RSA typically uses 2048 to 4096 bit keys.

