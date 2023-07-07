#wordlists #passwords #hash

# Hashed Passwords

[Hashing](hashing) Theory.

## Recognize the Hash

Prefix/hin | Algorithm | Example
--- | --- | ---
\$1\$ | md5crypt, used in Cisco stuff and older Linux/Unix systems
\$2\$, \$2a\$, \$2b\$, \$2x\$, \$2y\$ | Bcrypt (Popular for web applications) | `$2a$06$7yoU3Ng8dHTXphAg913cyO6Bjs3K5lBnwq5FJyA6d01pMSrddr1ZG`
\$6\$ | sha512crypt (Default for most Linux/Unix systems) | `$6$GQXVvW4EuM$ehD6jWiMsfNorxy5SINsgdlxmAEl3.yif0/c3NqzGLa0P.S7KRDYjycw5bnYkF5ZtB8wQy8KnskuWQS3Yr1wQ0`
hexidecimal, 256 bits, 64 Chars | SHA2-256 | `9eb7ee7f551d2f0ac684981bd1f1e2fa4a37590199636753efe614d4db30e8e1`
hexidecimal, 128 bits, 32 Chars | MD4/MD5 | `b6b0d451bbf6fed658659a9e7e5598fe`

More examples: [https://hashcat.net/wiki/doku.php?id=example_hashes](hashcat)

### Automated hash Recognition

Exists but is unreliable.

e.g. [Python hashID](https://pypi.org/project/hashID/)
e.g. https://www.tunnelsup.com/hash-analyzer/

## Crack the Hash


You have to crack them by using a large number of different inputs (rockyou.txt). Possible with adding salt.

Use the GPU to crack. (better at some of the math involved than CPUs thus faster)
Virtual Machines normally dont have access to the graphics card, so preferrably use the host machine.

Normally used Approaches/Tools:
- [Bruteforce](Bruteforce.md)
    - [hashcat](hashcat)
    - [John the Ripper](John%20the%20Ripper.md)
-  Lookup tables -Hashes are pre-computed from a dictionary and then stored with their corresponding password into a lookup table structure.
-  Reverse lookup tables - This attack allows for a cyber attacker to apply a dictionary or brute-force attack to many hashes at the same time without having to pre-compute a lookup table.
-  Rainbow tables - Rainbow tables are a time-memory technique. They are similar to lookup tables, except that they sacrifice hash cracking speed to make the lookup tables smaller.
-  Hashing with salt - With this technique, the hashes are randomized by appending or prepending a random string, called a “salt.” This is applied to the password before hashing.