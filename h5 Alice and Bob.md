# x) Read and summarize
## 1. Bruce Schneier: Applied Cryptography (Chapter 1 - Foundations):
   
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

## 2. Bruce Schneier: Applied Cryptography (Chapter 2 - Protocol Building Blocks)
2.5 Communications Using Public-Key Cryptography
Purpose: Eliminates the traditional requirement for a prior secure channel or shared secret key exchange before secure communication can begin.
Basic Protocol Flow:
If Bob wants to send a secure message to Alice, he encrypts the message using Alice’s public key.
Bob transmits the resulting ciphertext across an untrusted channel.
Upon receiving the transmission, Alice decrypts the message using her private key.
Characteristics: While public-key cryptography solves key distribution problems, public-key algorithms are computationally expensive and much slower than symmetric algorithms. Consequently, they are frequently used in hybrid cryptosystems to securely exchange symmetric session keys rather than encrypting entire data streams directly.

## 3. Tero Karvinen: PGP - Send Encrypted and Signed Message - gpgTero Karvinen: PGP - Send Encrypted and Signed Message - gpg
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

# a) Pubkey today
1) WhatsApp with end-to-end-encryption: A private key is stored on the smartphone, while the public key is stored on the messenger's servers. When someone sends a message, their app retrieves the public key to negotiate a shared session key. Only the private key on the device can decrypt the message.

2) Online Banking & Mobile Payments: The smartphone generates a key pair in the security chip. The public key is stored with the respective bank. When a transfer is authorized or the phone is held up to the terminal, the phone signs the transaction with the private key. The bank verifies this digital signature using the public key and thereby knows that the authorization truly originated from the corresponding device.

3) Passkeys (Anmelden per Fingerabdruck oder FaceID statt Passwort): During setup, the device generates a key pair specifically for that particular website. The public key is stored on the website. The next time you log in, you use your fingerprint to unlock the private key locally, which then sends a cryptographic signature to the website. Even if the website is hacked, it only contains public keys, which attackers cannot use to their advantage.

# b) Encrypt a message
For this, I used Tero's instructions from the PGP thread. I had some problems when it comes to attaching my vm back to the Internet, but this was solved in 5 minutes due to other commands in my terminal. In the meantime, I've shut down the VM completely again.
The steps can be seen underneath:

commands: 

sudo poweroff (The VM window closes or indicates that the machine is turned off)

ping -c 3 8.8.8.8 (To check in the console whether there is an Internet connection)

ip a (Checking whether the network interface has an IP address)

sudo dhclient (Manually Request an IP Address (DHCP))

It then looked like in the following:

<img width="473" height="269" alt="image" src="https://github.com/user-attachments/assets/965b7f90-80d4-4b73-88c2-686ae039e367" />

The image confirms that the network interface (enp0s1) already has a valid IP address (192.168.64.2). Sometimes the host system (macOS) or UTM blocks plain ICMP pings to the outside, which is why the ping failed. Instead, a check was performed to see if the router was reachable and if data was flowing: 

commands:

ip route (checking the standard-Gateway (Router))

ping -c 3 192.168.64.1 (Ping the router)

curl -I https://www.debian.org (Test Internet Connection via HTTP)

<img width="449" height="82" alt="image" src="https://github.com/user-attachments/assets/c01b8084-2622-4254-a5db-cffc55e27d6a" />

The ping to the router (192.168.64.1) was successful (0% packet loss)

commands:

su - (Switch to the root user)

ping -c 3 8.8.8.8 (Test Direct Internet Access to the Outside)

apt update && apt install -y curl gpg (Update package sources and install curl and gpg)

curl -I https://www.debian.org (Verify Installation and Internet Access)

<img width="474" height="365" alt="image" src="https://github.com/user-attachments/assets/ebb94a78-2cb5-4715-b6e2-42db68b1e45b" />

The HTTP/2 200 status code confirms that your virtual machine is now fully connected to the Internet.

Step 1: Create a Test File

command:
 echo "This is a secret message for my homework." > secret.txt

-----hier noch die restlichen schritte einfügen----

Passkey for the installation of the key pair:

<img width="659" height="278" alt="image" src="https://github.com/user-attachments/assets/8a0af4ad-77c4-412d-84cb-b2f793d0f5d1" />

after successfully getting to the password of the key pair, I moved on with some commands:



<img width="648" height="230" alt="image" src="https://github.com/user-attachments/assets/a84d198c-998e-4141-aa3f-bcf3779b4e70" />

command:
cat restored.txt (verification of the initial message)

# result:

<img width="782" height="169" alt="image" src="https://github.com/user-attachments/assets/ecedebce-90e4-44b7-99d9-483e85812b33" />

This task took a little longer because things didn't always work the first time I entered them, so I had to use additional commands. However, it was also kind of fun to see how it works in practice.

## Sources:

Schneier (2015): Applied Cryptography: Protocols, Algorithms and Source Code in C, 20th Anniversary Edition

Karvinen (2023): PGP - Send Encrypted and Signed Message - gpg

Fido Alliance (n.d.). User Authentication Specifications Overview
https://fidoalliance.org/specifications/

World Wide Web Consortium (2026).Web Authentication: An API for accessing Public Key Credentials Level 3
https://www.w3.org/TR/webauthn-3/

EMVCo (n.d.). EMV® Contactless Chip. https://www.emvco.com/emv-technologies/emv-contactless-chip/

Signal (2016). The X3DH Key Agreement Protocol. https://signal.org/docs/specifications/x3dh/
