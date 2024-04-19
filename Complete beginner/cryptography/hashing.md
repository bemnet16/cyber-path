## Hashing - Crypto 101
**Plaintext** - Data before encryption or hashing, often text but not always as it could be a photograph or other file instead.
**Encoding** - This is NOT a form of encryption, just a form of data representation like base64 or hexadecimal. Immediately reversible.
**Hash** - A hash is the output of a hash function. Hashing can also be used as a verb, "to hash", meaning to produce the hash value of some data.
**Brute force** - Attacking cryptography by trying every different password or every different key
**Cryptanalysis** - Attacking cryptography by finding a weakness in the underlying maths.

##### What's a hash function?
A hash function takes some input data of any size, and creates a summary or "digest" of that data. The output is a fixed size. It’s hard to predict what the output will be for any input and vice versa. Good hashing algorithms will be (relatively) fast to compute, and slow to reverse (Go from output and determine input). Any small change in the input data (even a single bit) should cause a large change in the output.
##### What's a hash collision?
A hash collision is when 2 different inputs give the same output. Hash functions are designed to avoid this as best as they can, especially being able to engineer (create intentionally) a collision.
##### What can we do with hashing?
Hashing is used for 2 main purposes in Cyber Security. To verify integrity of data (More on that later), or for verifying passwords.

Here's a quick table of the most Unix style password prefixes.

|                                       |                                                            |
| ------------------------------------- | ---------------------------------------------------------- |
| Prefix                                | Algorithm                                                  |
| `$1$`                                 | md5crypt, used in Cisco stuff and older Linux/Unix systems |
| `$2$`, `$2a$`, `$2b$`, `$2x$`, `$2y$` | Bcrypt (Popular for web applications)                      |
| `$6$`                                 | sha512crypt (Default for most Linux/Unix systems)          |

