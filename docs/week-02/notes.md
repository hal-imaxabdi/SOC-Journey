# Week 2 Notes — Cryptography, Encryption, and Hashing

These are my notes from week 2 of the bootcamp. This week was very hands-on — we used CyberChef and HashCalc to actually do the things we were learning about, not just read about them. That made a big difference in understanding.

---

## Cryptography

Cryptography is the method of communicating in the presence of an attacker or a third party. The goal is to have a conversation where only the intended recipient understands what is being said.

Think of it this way: if two people are speaking Yoruba in a room full of people who only understand English, they can have a full conversation and nobody else knows what they are saying. That is cryptography in action. The "language switch" is the encryption, and only the person who understands Yoruba can decrypt it.

Two terms you need to know:

**Plain text** — The original, readable message. Normal English that anyone can read.

**Cipher text** — The encrypted version. It looks like gibberish until you convert it back.

**Encryption** is the process of converting plain text into cipher text.
**Decryption** is the reverse — converting cipher text back into plain text.

---

## Symmetric Encryption

Symmetric encryption uses one key for both encryption and decryption. The same key that locks the message is the same key that unlocks it.

### Caesar Cipher

The simplest example of symmetric encryption is the Caesar cipher. The idea is straightforward: you shift each letter of the alphabet forward by a set number of steps, and that number is your key.

Example with a key of 2:
- A becomes C
- B becomes D
- T becomes V
- The word "the" becomes "vjg"

To decrypt, you just reverse it — count the same number of steps backward.

We practiced this on **CyberChef** (cyberchef.io). The steps were:
1. Type your plain text in the Input box
2. Search for ROT13 under Operations and drag it into the Recipe
3. Change the amount (the key) to whatever number you want
4. The output box shows your cipher text automatically

ROT13 means "rotate 13 times" — so the key is 13. But you can change it to any number.

One thing worth knowing: since there are only 26 letters in the alphabet, once you rotate 26 times you end up back at the start. So a key of 0 and a key of 26 give you the exact same result. Same with key 1 and key 27. It repeats every 26 steps.

### Why Symmetric Encryption is Weak

Two problems:

**Problem 1 — Key sharing.** If I encrypt a message and send it to you, I also have to tell you the key so you can decrypt it. But if someone intercepts the message telling you the key, they can decrypt it too.

**Problem 2 — Brute force.** Because there are only 25 possible keys (1 through 25), an attacker can just try all of them. We actually did this in class — you paste the cipher text into a Caesar cipher brute force tool and it tries every possible key at once. You just scroll through the results and find the one that makes sense. It takes seconds.

This is why symmetric encryption is not recommended for serious use.

---

## Asymmetric Encryption

Asymmetric encryption solves the problems of symmetric encryption by using two different keys instead of one.

- **Public key** — This one is shared with everyone. You can give it to anybody.
- **Private key** — This one stays with you. You never share it.

Here is how it works:

1. I generate both keys using CyberChef (Generate PGP Key Pair under Operations → Public Key)
2. I share my public key with everybody — post it in the group chat, put it on my website, whatever
3. If you want to send me a private message, you take my public key and use it to encrypt your message
4. You send me the encrypted message (cipher text)
5. I decrypt it using my private key

The important thing: **the public key can only encrypt. It cannot decrypt.** Only the private key can decrypt. So even though everyone has my public key, nobody can read messages that were encrypted with it — only I can, because only I have the private key.

We practiced this on CyberChef too:
- To encrypt: drag PGP Encrypt into the Recipe, paste the public key, type your plain text
- To decrypt: drag PGP Decrypt into the Recipe, paste the private key, paste the cipher text

When someone tries to decrypt using the public key instead of the private key, CyberChef just throws an error. It will not work. That is the whole point.

### Why Asymmetric Encryption is Stronger

Because there is no need to share a secret key. I share my public key openly and that is fine — it can only encrypt. The private key never leaves my hands, so there is nothing for an attacker to intercept. And unlike the Caesar cipher with only 25 possible keys, asymmetric encryption keys are mathematically complex enough that brute forcing them is not practical.

---

## Hashing

Hashing is different from encryption in one very important way: **it only goes in one direction.**

With encryption, you can go from plain text to cipher text, and back again. Hashing does not work like that. You take your data, run it through a hashing function, and get a fixed value called a hash. But you cannot take that hash and reverse it back into the original data.

### What a Hash Actually Is

A hash is a fixed alphanumeric value (letters and numbers) generated from the contents of a file or a piece of text. Every file has a unique hash based on what is inside it.

The jollof rice analogy from class explains it well: smoky jollof has a specific smell. That smell is the hash. If you add garlic to the pot, the smell changes — even though the jollof still looks the same from the outside. The hash is the same way. Change anything in the file, even a single space, and you get a completely different hash.

### Practicing with HashCalc

We used HashCalc to see this in action:

1. Open HashCalc, change Data Format to Text String
2. Make sure only MD5 is checked
3. Type TechCrush (capital T, capital C)
4. Click Calculate — you get a hash value starting with 95EA and ending in 0458
5. Now add a space after TechCrush and calculate again
6. Completely different hash — even though all you added was one invisible space

We also did it with files:
1. Change Data Format to File
2. Create a text file, type "Today's Thursday's class." (exact text, capital T, full stop at end)
3. Load the file into HashCalc, calculate the hash
4. Add another full stop to the file, save it, calculate again
5. Different hash

This is how you verify file integrity — if the hash matches, the file is unchanged. If it does not match, something was modified.

### Hashing Algorithms

There is more than one way to hash. Different algorithms produce hashes of different lengths:

| Algorithm | Full Name | Notes |
|---|---|---|
| MD5 | Message Digest 5 | Shortest hash, not recommended |
| SHA-1 | Secure Hash Algorithm 1 | Longer than MD5 |
| SHA-256 | Secure Hash Algorithm 256 | Recommended for most uses |
| SHA-384 | Secure Hash Algorithm 384 | Longer still |
| SHA-512 | Secure Hash Algorithm 512 | Longest of these five |

When you calculate the same file using all three algorithms at once in HashCalc, you can clearly see that the lengths are different. Same file, same content, but each algorithm produces a different length of hash.

### Hash Collisions

A hash collision is when two different files with different contents produce the same hash. This should not happen — different contents should always produce different hashes.

MD5 has a known problem with hash collisions. This means it is possible for two completely different files to end up with the same MD5 hash, which defeats the purpose of using hashing for integrity checking.

**This is why MD5 is not recommended.** Use SHA-256 instead. SHA-256 is far more resistant to collisions.

### How Websites Use Hashing for Passwords

This was one of the most practically useful things from this week.

When you create an account on a website and set a password, a secure website does not save your actual password. Instead, it calculates the hash of your password and saves that in its database.

Here is how the login process works:
1. You type your password to log in
2. The website calculates the hash of whatever you just typed
3. It compares that hash to the hash it has stored in its database
4. If they match, login successful. If not, incorrect password.

This means even if someone hacks the database and steals it, they do not get your actual passwords — they only get the hashes. And since hashing is one-directional, they cannot reverse the hashes back into the original passwords.

### Salting and Peppering

There is one more layer on top of this called salting.

Before hashing your password, the website adds a random string of characters to your password. So instead of hashing "mypassword", it hashes "mypassword" + some random characters specific to you. That combination gets hashed and stored.

This protects against attackers who might try to pre-calculate hashes for common passwords and then compare them against a stolen database.

**Salt** — Random characters added per user. Each user gets a different salt.
**Pepper** — Similar idea but the same value is applied to all users.

### Verifying File Downloads Using Hashes

A practical real-world use of hashing: when you download software from a legitimate website, most of them will publish the hash of the file alongside the download.

Wireshark does this — they have a signatures file on their website that lists the expected hash for each version of their software. After downloading the file, you calculate the hash yourself. If your hash matches what is on their website, you downloaded the correct file. If they do not match, the file was either modified or corrupted somewhere along the way.

---

## Key Differences: Encryption vs Hashing

| | Encryption | Hashing |
|---|---|---|
| Direction | Two-way (reversible) | One-way (not reversible) |
| Purpose | Hiding data in transit | Verifying data integrity |
| Key needed | Yes | No |
| Can be reversed | Yes, with the right key | No |

---

## What I Practiced This Week

- Used CyberChef to encrypt and decrypt using Caesar cipher (symmetric)
- Used CyberChef to generate PGP key pairs and practice asymmetric encryption
- Participated in the class exercise where everyone encrypted a message using the shared public key and sent it to the instructor to decrypt
- Used HashCalc to calculate MD5 hashes for text strings and files
- Observed how a single space changes the entire hash value
- Calculated SHA-1 and SHA-256 alongside MD5 and compared the output lengths

---

## What I Want to Revisit

- Salting in more depth — I understand the concept but want to see a practical example
- How asymmetric encryption is used in HTTPS — I know the connection exists, want to understand it properly
- The difference between PGP and other asymmetric encryption standards
