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

cat secret.txt

Step 2: Generate own GPG Key Pair

command:
gpg --full-generate-key

Selections in the assistant:
By default, RSA and RSA (Option 1).
Select the key length (e.g., 3072 or 4096 bits).
Specify how long the key should be valid (in this case, I entered 0 for unlimited).
Enter your name and email address, then confirm by pressing O (OK).
Create a secure passphrase (password) for the private key.

gpg --list-keys (this is for reassurance)

Step 3: Encrypt and sign the file

gpg --armor --sign --encrypt --recipient luise@debian secret.txt

Step 4: Inspect or Decrypt the Result

command:

cat secret.txt.asc (View the contents of the encrypted file)

gpg --decrypt secret.txt.asc > decrypted.txt (To decrypt the file and verify the signature)

<img width="471" height="32" alt="image" src="https://github.com/user-attachments/assets/dcc0ad30-b3e5-46f8-a5bf-7eee1d46f3bb" />

as seen above, the message could successfully been decrypted!


Passkey for the installation of the key pair:

<img width="659" height="278" alt="image" src="https://github.com/user-attachments/assets/8a0af4ad-77c4-412d-84cb-b2f793d0f5d1" />

after successfully getting to the password of the key pair, I moved on with some commands:



<img width="648" height="230" alt="image" src="https://github.com/user-attachments/assets/a84d198c-998e-4141-aa3f-bcf3779b4e70" />

command:
cat restored.txt (verification of the initial message)

# result:

<img width="782" height="169" alt="image" src="https://github.com/user-attachments/assets/ecedebce-90e4-44b7-99d9-483e85812b33" />

This task took a little longer because things didn't always work the first time I entered them, so I had to use additional commands. However, it was also kind of fun to see how it works in practice.

How different parties use different keys at different stages of operation:
In public-key cryptography, such as GnuPG / OpenPGP, confidentiality (encryption) and authenticity/integrity (digital signatures) rely on distinct keys used by different parties at different stages. Each party holds a private key (kept secret) and a public key (shared openly).
Each party holds a private key that stays secret and a public key that is shared openly, and different keys are used depending on whether a message is being kept confidential or authenticated. When Alice wants to send a secret message to Bob, she encrypts it using Bob’s public key so that only Bob can decrypt it using Bob’s private key. To prove that the message genuinely came from her and wasn't altered, Alice signs it using Alice’s private key, allowing Bob to verify the signature with Alice’s public key. When both signing and encrypting are combined, Alice signs with her own private key and encrypts with Bob's public key, while Bob first decrypts with his private key and then verifies the signature with Alice's public key (cf. GNU Privacy Handbook, n.d).

Evaluation of the chosen security tool:
For a learning and lab environment on a local VM, GnuPG with Curve 25519 is well-suited and cryptographically strong for demonstrating encryption, signing, decryption, and signature verification. For real-world production or sensitive data, security also depends on keeping software patched, protecting the private key (ideally with a strong passphrase and/or hardware token), and verifying recipients' key fingerprints out-of-band

# c) Password management

Demonstration of the use of a password manager:
Updating the package list and installing "pass"

commands:

apt update && apt install -y pass

<img width="583" height="403" alt="image" src="https://github.com/user-attachments/assets/b73c9c38-6581-4975-88f8-86d2fabd4cd9" />

Initializing the Password Store with my GPG Key

Linking "pass" to my existing GPG identity so all stored passwords are encrypted with my public key:

pass init luise@debian

<img width="506" height="75" alt="image" src="https://github.com/user-attachments/assets/f62300c1-4517-45bf-b8b1-3f02bee1a686" />

verification can be seen here (password store initialized for luisedebian)

Generation and Store a Strong Random Password

command:
pass generate university/portal 20

<img width="509" height="112" alt="image" src="https://github.com/user-attachments/assets/cf491b85-da94-4803-9c4b-ca885008bced" />

A newly generated 20-character password is displayed in the terminal and stored in encrypted form.

command:

pass insert email/personal

<img width="425" height="195" alt="image" src="https://github.com/user-attachments/assets/38cfba82-fa38-4745-8ab9-4f001889d88c" />

Both entries (email/personal and university/portal) are now securely stored in the Password Store.


- What kind of attacks take advantage of people not using password managers?

When users do not use a password manager, they often rely on weak, predictable passwords and reuse the same credentials across multiple accounts, making them vulnerable to credential stuffing, brute-force, and password spraying attacks. Additionally, manually typing passwords from memory exposes users to phishing attacks on lookalike domains—whereas a password manager's autofill only triggers on the exact legitimate URL—and storing passwords in unencrypted notes or spreadsheets leaves them open to local plaintext theft by malware or unauthorized local access.

# d) Voluntary: Details of PGP

1. Set Up Separate Folders & Keyrings for Two Users
Creation of two isolated directories (~/alice and ~/bob) so each party has its own files and its own --homedir keyring

command:

mkdir -p ~/alice/gnupg ~/bob/gnupg
chmod 700 ~/alice/gnupg ~/bob/gnupg

2. Generation of a Second Key Pair for the Other Party (bob)
Keeping my existing (luise@debian) identity as the sender (alice), and generating a key pair for the recipient (bob) in Bob's separate keyring

gpg --homedir ~/bob/gnupg --full-generate-key

<img width="672" height="290" alt="image" src="https://github.com/user-attachments/assets/47657677-33b0-4e7a-88df-f73bc5ad624a" />

Choosing ECC and ECC (Curve 25519) and set an expiration/name such as bob <bob@debian> with a passphrase

<img width="798" height="396" alt="image" src="https://github.com/user-attachments/assets/626a47b7-63a0-4ba4-8b62-62fffcdde461" />

3. Exchange Public Keys (Out-of-Band Simulation)

Export Bob's public key and import it into Alice's keyring
gpg --homedir ~/bob/gnupg --armor --export bob@debian > ~/bob/bob.pub.asc
gpg --homedir ~/alice/gnupg --import ~/bob/bob.pub.asc

Export Luise's public key from your existing keyring and import it into Bob's keyring
gpg --armor --export luise@debian > ~/alice/luise.pub.asc
gpg --homedir ~/bob/gnupg --import ~/alice/luise.pub.asc

<img width="797" height="249" alt="image" src="https://github.com/user-attachments/assets/e3525277-3508-4dab-b7fb-020326254c0c" />

4. Alice Creates, Signs, and Encrypts a Message for Bob
Creating a message in ~/alice/message.txt, then sign it with Alice's (luise@debian) private key and encrypt it with Bob's (bob@debian) public key:

command:

echo "Secret lab note from Luise to Bob." > ~/alice/message.txt

gpg --homedir ~/alice/gnupg --armor --sign --encrypt \
  --local-user luise@debian \
  --recipient bob@debian \
  --output ~/bob/incoming.asc \
  ~/alice/message.txt

<img width="805" height="309" alt="image" src="https://github.com/user-attachments/assets/ffc4d921-4d39-4f91-91d3-230de99b9c6e" />

command:

1. Import Luise's Public Key into Bob's Keyring
   
gpg --armor --export luise@debian > ~/alice/luise.pub.asc && gpg --homedir ~/bob/gnupg --import ~/alice/luise.pub.asc

2. Decrypt and Verify the Message as Bob

gpg --homedir ~/bob/gnupg --decrypt ~/bob/incoming.asc > ~/bob/decrypted_message.txt && cat ~/bob/decrypted_message.txt

<img width="803" height="417" alt="image" src="https://github.com/user-attachments/assets/b2ad4335-c614-4054-8efa-ff20a12f01a7" />


Encryption and signing succeeded—after confirming y, GnuPG created ~/bob/incoming.asc using bob <bob@debian>'s cv25519 encryption subkey and luise <luise@debian>'s signing key.

## Sources:

Schneier (2015): Applied Cryptography: Protocols, Algorithms and Source Code in C, 20th Anniversary Edition

Karvinen (2023): PGP - Send Encrypted and Signed Message - gpg

Fido Alliance (n.d.). User Authentication Specifications Overview
https://fidoalliance.org/specifications/

World Wide Web Consortium (2026).Web Authentication: An API for accessing Public Key Credentials Level 3
https://www.w3.org/TR/webauthn-3/

EMVCo (n.d.). EMV® Contactless Chip. https://www.emvco.com/emv-technologies/emv-contactless-chip/

Signal (2016). The X3DH Key Agreement Protocol. https://signal.org/docs/specifications/x3dh/

The GNU privacy Handbook (n.d.). https://www.gnupg.org/gph/en/manual.html 
