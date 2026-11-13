## x) Read and summarize
# 1. Bruce Schneier: Applied Cryptography (Chapter 1 - Foundations):
   
Sender and Receiver: The communicating entities (traditionally referred to as Alice and Bob) trying to exchange information securely across an untrusted network.

Messages and Encryption:

Plaintext: The original, unencrypted, readable message.
Ciphertext: The scrambled, unreadable message produced after encryption.
Encryption: The mathematical process of converting plaintext into ciphertext.
Decryption: The reverse process of recovering plaintext from ciphertext.

Algorithms and Keys:

Cryptographic Algorithm (Cipher): The mathematical rule or formula governing encryption and decryption operations.
Key: A secret or public parameter supplied to the algorithm that determines the specific transformation of the data.
Cryptosystem: The complete system comprising the algorithm, all possible plaintexts, ciphertexts, and keys.

Symmetric Algorithms:

Also known as secret-key or conventional algorithms.
Use the same key for both encryption and decryption.
Require both communicating parties to securely share the key in advance.

Public-Key Algorithms:

Also known as asymmetric algorithms.
Use two different keys: a public key for encryption and a private key for decryption.
The public key can be published openly, allowing anyone to encrypt a message, but only the holder of the corresponding private key can decrypt it.

# 2. Bruce Schneier: Applied Cryptography (Chapter 2 - Protocol Building Blocks)
2.5 Communications Using Public-Key Cryptography
Purpose: Eliminates the traditional requirement for a prior secure channel or shared secret key exchange before secure communication can begin.
Basic Protocol Flow:
If Bob wants to send a secure message to Alice, he encrypts the message using Alice’s public key.
Bob transmits the resulting ciphertext across an untrusted channel.
Upon receiving the transmission, Alice decrypts the message using her private key.
Characteristics: While public-key cryptography solves key distribution problems, public-key algorithms are computationally expensive and much slower than symmetric algorithms. Consequently, they are frequently used in hybrid cryptosystems to securely exchange symmetric session keys rather than encrypting entire data streams directly.

# 3. Tero Karvinen: PGP - Send Encrypted and Signed Message - gpgTero Karvinen: PGP - Send Encrypted and Signed Message - gpg
Overview: A practical walkthrough demonstrating how to use GNU Privacy Guard (gpg) on the Linux command line to simulate secure, authenticated communication between two users (e.g., Alice and Tero).

Core Security Objectives:
Encryption: Keeps message contents confidential so eavesdroppers cannot read them.
Signing: Proves the identity of the sender and protects the message from tampering (integrity and authentication).
Key Commands & Workflow (gpg):
Key Generation & Management: Generating keypairs and exporting/importing public keys across untrusted networks to establish trust.

Command Flags:
--encrypt (or -e): Encrypts the message data.

--recipient: Specifies the target recipient via email address or key fingerprint.

--sign (or -s): Signs the message using the sender's private key.

--armor: Converts binary output into printable ASCII (Base64) text so it can be safely copied and pasted into email bodies or chat messages.

--output: Saves the resulting ciphertext to a designated file.

Verification: The recipient imports the sender's public key, verifies the digital signature, and successfully decrypts the message

## a) Pubkey today
