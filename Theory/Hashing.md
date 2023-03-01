#hash 

**Usage**

1. [verify Integrity of data](hashing.md#data%20integrity%20verification)
2. [verify Passwords](hashing.md#password%20verification)
3. [identify the type of the hash](hashing.md#hash%20identification)

# Key Terms

- Plaintext - Data before encryption or hashing, often text but not always as it could be a photograph or other file instead.
- Encoding - This is NOT a form of encryption, just a form of data representation like base64 or hexadecimal. Immediately reversible.
- Hash - A hash is the output of a hash function. Hashing can also be used as a verb, "to hash", meaning to produce the hash value of some data.
- Brute force - Attacking cryptography by trying every different password or every different key
- Cryptanalysis - Attacking cryptography by finding a weakness in the underlying maths
- Hash Collission - 2 different Inputs give the same hashed output
- Pigeonhole Effect - set number of differen output values for any size input (more inputs than outputs - e.g. 128 pigeons for 96 pigeonholes, some pigeons have to share)
    - e.g. [MD5](https://www.mscs.dal.ca/~selinger/md5collision/)
    - e.g. [SHA1](https://shattered.io/)


## Hash Function

No Key and it should be impossible to revert.
Takes input Data from any size and creates a "summary" of that data with a fixed size.
A good Hashing Alogrithmus will be fast to compute and slow to reverse.
The Output is normally raw bytes, which are encoded ([base64](Decode#Base64), hexadecimal).

# Password Verification

Storing passwords in plain text would be really bad.
But passwords cant be encrypted as a key has to be stored somewhere. And with the key the passwort can be decrypted.

This leads to hashing Passwords. Instead of the password the hash of the password is stored in the database. If leaked the attacker has to crack each password. =>Problem: two users have the same password = same hash (rainbow table)
To protect against rainbow tables: add salt to the passwords (salt is random generated and unique to each user at the start or end of the password before the hash)
=> bcrypt and sha512crypt handle this automatically

[hashcat](hashcat) can be used to crack such hashed Passwords.

## Windows Passwords

Stored in SAM can be retrieved wit tools like mimikatz.
Hashed with NTLM (variant of MD4/MD5)

## Unix Passwords

Stored in /etc/passwd oder /etc/shadow

Standard Format: `$format$rounds$salt$hash`


# Data Integrity Verification

Check if files havent been changed/modified/downloaded correctly => if you put the same data in you always get the same output.

One Method is HMACs. A HMAC can be used to ensure that the person who created the HMAC is who they say they are (authenticity), and that the message hasn’t been modified or corrupted (integrity). They use a secret key, and a hashing algorithm in order to produce a hash.

# Hashes List

- MD5
- SHA1
- bcrypt
- SHA512crypt
<<<<<<< HEAD
- NTLM (Windows Passwords) (variant of MD4)

# Cracking Hashes
obsidian://open?vault=THM&file=Tools%2FJohn%20the%20Ripper
- [John the Ripper](John%20the%20Ripper.md)
=======
- NTHash/NTLM (Windows Passwords) (variant of MD4)


# Hash Identification

1) per Online Tool: https://hashes.com/en/tools/hash_identifier
2) per [Python Hash Identifier](https://gitlab.com/kalilinux/packages/hash-identifier/-/tree/kali/master)
	1) `wget https://gitlab.com/kalilinux/packages/hash-identifier/-/raw/kali/master/hash-id.py`
	2) `python3 hash-id.py`
>>>>>>> 877ecb89fc418bec54c43a1c7980b3e6f25b2acd
