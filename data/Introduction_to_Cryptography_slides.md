# **Month 1, Week 3:**

# **Introduction to Cryptography**

### **Overview**

* Basics of Cryptography  
* Encryption and Decryption Processes  
* Symmetric and Asymmetric Encryption  
* Public Key Infrastructure (PKI)  
* Digital Signatures and Certificates  
* Cryptography Hash Functions

# **Basics Of Cryptography**

## **Cryptography**

* "Cryptography is the science of using mathematics to encrypt and decrypt data" \- Phil Zimmermann  
* "Cryptography is the art and science of keeping messages secure" \- Bruce Schneier  
* The art and science of concealing the messages to introduce secrecy in information security is recognized as cryptography.

In Cybersecurity, Cryptography is the process of hiding or coding information so that only the person a message was intended for can read it. The art of cryptography has been used to code messages for thousands of years and continues to be used in bank cards, computer passwords, and ecommerce.

## **Cryptography Terminologies**

* A message is **plaintext** (sometimes called cleartext). The process of disguising a message in such a way as to hide its substance is **encryption**.  
* An encrypted message is **ciphertext**.  
* The process of turning ciphertext back into plaintext is **decryption**.  
* A **cipher** (or cypher) is an algorithm for performing encryption or decryption-a series of well-defined steps that can be followed as a procedure.  
* A **cryptosystem** is an implementation of cryptographic techniques and their accompanying infrastructure to provide information security services. A cryptosystem is also referred to as a cipher system. The various components of a basic cryptosystem are as follows:  
  * Plaintext  
  * Encryption Algorithm  
  * Ciphertext  
  * Decryption Algorithm  
  * Encryption Key  
  * Decryption Key

## **Goal of Cryptography**

The primary goal of cryptography is to secure important data on the hard disk or as it passes through a medium that may not be secure itself. Usually, that medium is a computer network.  
Cryptography can provide the following services:

* Confidentiality (secrecy)  
* Integrity (anti-tampering)  
* Authentication  
* Non-repudiation.

### **Confidentiality (secrecy):**

* Ensuring that no one can read the message except the intended receiver.  
* Data is kept secret from those without the proper credentials, even if that data travels through an insecure medium.

### **Integrity (anti-tampering):**

* Assuring the receiver that the received message has not been altered in any way from the original.

### **Authentication:**

* Cryptography can help establish identity for authentication purposes. The process of proving one's identity. (The primary forms of host-to-host authentication on the Internet today are name-based or address-based, both of which are notoriously weak.)

### **Non-repudiation:**

* A mechanism to prove that the sender really sent this message.

# **Encryption And Decryption Processes**

## **Encryption And Decryption**

* **Encryption** is the process of converting a normal message (plain text) into a meaningless message (ciphertext).  
* **Decryption** is the process of converting a meaningless message (ciphertext) into its original form (plaintext).

### **What Is Encryption?**

Data can be secured with encryption by being changed into an unintelligible format that can only be interpreted by a person with the proper decryption key. Sensitive data, including financial and personal information as well as communications over the internet, is frequently protected with it.

### **What Is Decryption?**

To make encrypted data comprehensible again, it must first be decrypted and then put back into its original format. To access and utilize the protected information, authorized parties must follow this procedure.

## **Applications of Encryption**

Many different fields employ encryption, including:

* **Online Banking:** To secure transactions, use online banking.  
* **Email security:** To safeguard the contents of emails.  
* **Secure Messaging:** To protect the privacy of discussions.  
* **Data Storage:** To prevent unwanted access to data that has been stored.

## **Real-Life Examples Of Encryption And Decryption**

* **WhatsApp Messaging:** It encrypts communications from beginning to end so that only the sender and recipient can read them.  
* **HTTPS websites:** Encrypt user data to prevent third parties from intercepting it.  
* **Encrypted Email Services:** Email services that use encryption, like ProtonMail, protect email contents.

# **Symmetric And Asymmetric Encryption**

## **Symmetric Encryption**

**Symmetric encryption** is a widely used data encryption technique whereby data is encrypted and decrypted using a single, secret cryptographic key.  
Specifically, the key is used to encrypt plaintext \- the data's pre-encryption or post-decryption state \- and decrypt ciphertext \- the data's post-encryption or pre-decryption state.

Symmetric encryption is one of the most widely used encryption techniques and also one of the oldest, dating back to the days of the Roman Empire. Caesar's cipher, named after none other than Julius Caesar, who used it to encrypt his military correspondence, is a famous historical example of symmetric encryption in action.

The goal of symmetric encryption is to secure sensitive information. It's used daily in many major industries, including defense, aerospace, banking, health care, and other industries in which securing a person's, business', or organization's sensitive data is of the utmost importance.

## **How Does Symmetric Encryption Work?**

Symmetric encryption works by using either a **stream cipher** or **block cipher** to encrypt and decrypt data. A stream cipher converts plaintext into ciphertext one byte at a time, and a block cipher converts entire units, or blocks, of plaintext using a predetermined key length, such as 128, 192, or 256 bits.

Senders and recipients using symmetric encryption to transfer data to each other must know the secret key to, in the case of senders, encrypt the data they intend to share with recipients, and in the case of recipients, decrypt and read the encrypted data the senders share with them, as well as encrypt any necessary responses.

If Anthony Obed, the sender, wants to send Ashley-Megan, the recipient, a confidential document, Anthony Obed would use the secret key to encrypt the file and send it to Ashley-Megan, who would be unable to read its contents until she entered the same key that Anthony Obed just used to encrypt the file. Conversely, if Ashley-Megan makes changes to the document and wishes to share them with Anthony Obed, she'd use the same key to re-encrypt the file and send it back to Anthony Obed, who will use the same key to decrypt the file and access its contents, and the process repeats itself.

## **Examples Of Symmetric Encryption**

* Advanced Encryption Standard (AES)  
* Data Encryption Standard (DES)  
* Triple Data Encryption Standard (Triple DES)  
* International Data Encryption Algorithm (IDEA)  
* TLS/SSL protocol

### **AES encryption**

**AES encryption**, which uses block ciphers of 128, 192, or 256 bits to encrypt and decrypt data, is one of the most well-known and effective symmetric encryption techniques in use today. It would take billions of years to crack, and that's why it's used to secure sensitive information in government, healthcare, banking, and other industries. It is **more secure** than DES, Triple DES, and IDEA.

### **DES encryption**

**DES encryption** is now considered by the National Institute of Standards and Technology (NIST) to be a legacy symmetric encryption algorithm because it has long been ineffective at safeguarding sensitive information from brute-force attacks. In fact, the NIST has withdrawn the standard entirely, and its more secure big brother, **Triple DES encryption**, will have the same fate. Although still in use today, Triple DES encryption is being withdrawn and disallowed by the NIST in 2023 because of mounting security concerns.

### **IDEA Encryption**

**IDEA encryption** was developed as a replacement for DES in the 1990s, but AES was ultimately deemed more secure. The IDEA is now an open and free block-cipher algorithm, so anyone can use it, but it's generally considered to be obsolete and ineffective at securing sensitive information today. AES encryption is the gold standard for both purposes.

### **Transport Layer Security (TLS)**

**Transport Layer Security (TLS)**, as well as its predecessor, **Secure Sockets Layer (SSL)**, uses symmetric encryption. Basically, when a client accesses a server, unique symmetric keys, called session keys, are generated. These session keys are used to encrypt and decrypt the data shared between the client and the server in that specific client-server session at that specific point in time. A new client-server session would generate new, unique session keys.

TLS/SSL uses not only symmetric encryption but both symmetric and asymmetric encryption, to ensure the security of client-server sessions and the information exchanged within them.

## **Advantages Of Symmetric Encryption**

* **Security:** symmetric encryption algorithms like AES take billions of years to crack using brute-force attacks.  
* **Speed:** symmetric encryption, because of its shorter key lengths and relative simplicity compared to asymmetric encryption, is much faster to execute.  
* **Industry adoption and acceptance:** symmetric encryption algorithms like AES have become the gold standard of data encryption because of their security and speed benefits, and as such, have enjoyed decades of industry adoption and acceptance.

## **Disadvantages Of Symmetric Encryption**

By far the biggest disadvantage of symmetric encryption is its use of a single, secret cryptographic key to encrypt and decrypt information.

Why?

Well, if this secret key is stored in an insecure location on a computer, then hackers could gain access to it using software-based attacks, allowing them to decrypt the encrypted data and thereby defeating the entire purpose of symmetric encryption.

In addition, if one party or entity is encrypting at one location and a separate party or entity decrypting at a second, then the key will need to be transmitted, leaving it vulnerable to interception if the transmission channel is compromised.

## **Asymmetric Encryption**

Unlike symmetric encryption, which uses the same secret key to encrypt and decrypt sensitive information, **asymmetric encryption**, also known as **public-key cryptography** or **public-key encryption**, uses mathematically linked public- and private-key pairs to encrypt and decrypt senders' and recipients' sensitive data.

As with symmetric encryption, plaintext is still converted into ciphertext and vice versa during encryption and decryption, respectively. The main difference is that two unique key pairs are used to encrypt data asymmetrically.

## **How Does Asymmetric Encryption Work?**

If Anthony Obed, the sender, and Ashley-Megan, the recipient, want to continually send a confidential file back and forth to each other, Anthony Obed and Ashley-Megan will give their unique and respective public keys to each other. Anthony Obed will then use Ashley-Megan's public key to encrypt the file, since it's intended for Ashley-Megan only, and send the file to Ashley-Megan.

Upon receipt of the file, Ashley-Megan will use her private key \- keyword, "private," meaning no one else other than Ashley-Megan knows it \- to decrypt the file and access its contents. No one other than Ashley-Megan, not even Anthony Obed, can decrypt this file, because no one other than Ashley-Megan knows Ashley-Megan's private key.

The same process applies when Ashley-Megan wants to send the file back to Anthony Obed. Ashley-Megan ties it to Anthony Obed's public key, and Anthony Obed uses her private key to decrypt the file.

One reason asymmetric encryption is often regarded as more secure than symmetric encryption is that asymmetric encryption, unlike its counterpart, does not require the exchange of the same encrypt-decrypt key between two or more parties.

Yes, public keys are exchanged, but users sharing data in an asymmetric cryptosystem have unique public and private key pairs, and their public keys, because they're used for encryption only, pose no risk of unauthorized decryption by hackers should they become known, because the hackers, assuming private keys are kept private, don't know the users' private keys and thus cannot decrypt the encrypted data.

## **Examples Of Asymmetric Encryption**

* Rivest Shamir Adleman (RSA)  
* The Digital Signature Standard (DSS), which incorporates the Digital Signature Algorithm (DSA)  
* Elliptical Curve Cryptography (ECC)  
* The Diffie-Hellman Exchange Method  
* TLS/SSL Protocol

### **Rivest Shamir Adleman (RSA)**

Published in 1977, **RSA** is one of the oldest examples of asymmetric encryption. Developed by Ron Rivest, Adi Shamir, and Leonard Adleman, RSA encryption generates a public key by multiplying two large, random prime numbers together, and using these same prime numbers, generates a private key. From there, standard asymmetric encryption takes place: information is encrypted using the public key and decrypted using the private key.

### **The Digital Signature Standard (DSS)**

The **DSS**, which incorporates the **Digital Signature Algorithm (DSA)**, is the perfect example of asymmetric digital signature authentication. A sender's private key is used to digitally sign a message or file, and the recipient uses the sender's corresponding public key to confirm that the signature originated from the correct sender and not a suspicious or unauthorized source.

### **Elliptical Curve Cryptography (ECC)**

**ECC** is an RSA alternative that uses smaller key sizes and mathematical elliptic curves to execute asymmetric encryption. It's frequently used to digitally sign cryptocurrency transactions; in fact, the popular cryptocurrency Bitcoin uses ECC \- the Elliptic Curve Digital Signature Algorithm (ECDSA), to be exact \- to digitally sign transactions and ensure that funds are spent by authorized users only. ECC is much faster than RSA in terms of key and signature generation, and many consider it the future of asymmetric encryption, mainly for web traffic and cryptocurrency but for other applications as well.

### **The Diffie-Hellman Exchange Method**

**Diffie-Hellman**, one of cryptography's greatest breakthroughs, is a key exchange method that two parties who have never met can use to exchange public and private key pairs over public, insecure communication channels. Prior to Diffie-Hellman, two parties seeking to encrypt their communications between each other had to physically pre-exchange encryption keys so that both parties could decipher each other's encrypted messages. Diffie-Hellman made it so that these keys could be securely exchanged over public communication channels, where third parties normally extract sensitive information and encryption keys.

### **TLS/SSL**

**TLS/SSL** uses asymmetric encryption to establish a secure client-server session while the client and server are generating symmetric encryption keys. This is known as a TLS handshake. After the TLS handshake is complete, the client-server session keys are used to encrypt the information exchanged in that session.

## **Advantages Of Asymmetric Encryption**

* **Key distribution not necessary:** securing key distribution channels has long been a headache in cryptography. Asymmetric encryption eliminates key distribution entirely. The needed public keys are exchanged through public-key servers, and the disclosure of public keys is not, at this time, detrimental to the security of encrypted messages, because they cannot be used to derive private keys.  
* **Exchange of private keys not necessary:** with asymmetric encryption, private keys should remain stored in a secure location and thus private to the entities using them. Basically, the keys needed to decrypt sensitive information are never, and should not ever be, exchanged over a potentially compromised communication channel, and that's a major plus for the security and integrity of encrypted messages.  
* **Digital signature/message authentication:** with asymmetric encryption, senders can use their private keys to digitally sign and verify that a message or file originated from them and not an untrusted third party.

## **Disadvantages Of Asymmetric Encryption**

The main disadvantage of asymmetric encryption is that it's slower than symmetric encryption because of its longer key lengths, not to mention that asymmetric encryption calculations tend to be much more complex than their symmetric counterparts.

Why? Because, in theory, public keys can be used to crack private keys \- again, they're mathematically linked \- but asymmetric encryption uses extraordinarily long key lengths to make this virtually impossible, at least for now.

Now, this is not to say that symmetric encryption is insecure; however, the very foundation of asymmetric encryption eliminates several information security risks that still exist within poorly managed symmetric encryption cryptosystems.

## **Summary Of Key Differences**

The key differences between symmetric and asymmetric encryption are speed and security preferences. Generally speaking, symmetric encryption is faster and simpler but is often viewed as less secure than asymmetric encryption. But as we've discussed, encryption really boils down to two things: key size and the security of the media storing encryption keys.

Symmetric encryption is much faster to execute because of its shorter key lengths. Asymmetric encryption has a tendency to bog down networks because of its longer key lengths and complex algorithms. These are the tradeoffs worth considering when deciding which type of encryption to employ.

# **Public Key Infrastructure (PKI)**

## **Public Key Infrastructure (PKI)**

**Public key infrastructure (PKI)** refers to tools used to create and manage public keys for encryption which is a common method of securing data transfers on the internet. PKI is built into all web browsers used today, and it helps secure public internet traffic. Organizations can use it to secure the communications they send back and forth internally and also to make sure connected devices can connect securely.

The most important concept associated with PKI is the cryptographic keys that are part of the encryption process and serve to authenticate different people or devices attempting to communicate with the network.

## **Why Is Public Key Infrastructure (PKI) Important**

PKI is crucial because the encryption and authentication it manages and makes possible ensures trustworthy, secure communication online. For an enterprise, PKI can make the difference between an intruder gaining access to the network through a connected device and keeping a potentially dangerous threat away from the organization.

## **How Does PKI Work?**

PKI works through the implementation of two technologies: certificates and keys. A key is a long number used to encrypt data. Each element of a message gets encrypted using the key formula. For example, if you want to write a message where every letter is replaced by the letter after it, then A will become B, C will be D, etc. If someone is to have this key, they will get what will look like a nonsensical message and decrypt it.

With PKI, the key involves advanced mathematical concepts that are much more complicated. With the alphabetic example above, there is one key, and if the recipient has it, they can easily decrypt the message. With PKI, on the other hand, there are two keys: a private and a public one.

The public key is available to anyone who wants it and is used to encode a message that someone sends to you. A private key is what you use to decrypt the message after you get it. The keys are connected using a complex mathematical equation. Even though the private and public keys are connected, the connection is facilitated by this complex equation. It is therefore extremely difficult to ascertain the private key by using data from the public key.

## **Common Uses Of Pki Certificates**

### **Hypertext Transfer Protocol Secure (HTTPS):**

In HTTPS, the certificates identify each website the user tries to reach to make sure the messages sent back and forth are not intercepted or changed. If someone gains unauthorized access, they can engage in fraudulent activity, such as sending fake wire transfers or taking people's credit card information. HTTPS can also help prevent a range of man-in-the-middle (MITM) attacks because the hacker has to know how to decrypt the information to effectively intercept and then change or steal the data being sent between two entities. When the communicating parties have to encrypt their messages, not only is key data kept secure but so are passwords and other private information that hackers want to get their hands on.

### **Secure Shell (SSH)**

SSH provides authentication for computers and users, and uses X.509 certificates. Although different SSH protocols can use different certificate formats, they all perform the same basic function: making sure users and computers are who they claim to be.

### **Signing and encrypting emails**

Certificates also come into play when you have to make sure your email communications are secure. As with SSH, there are different options for the implementation of PKI certificates for sending emails, but they perform the same essential functions: securing emails that get sent and received, while also ensuring the sender and receiver are who they claim to be.

# **Digital Signature And Certificate**

## **Digital Signature**

More and more people and organizations are using digital documents instead of paper documents to conduct day-to-day transactions. By reducing dependency on paper documents, we are protecting the environment and saving the planet's resources. Digital signatures support this change by providing assurances about the validity and authenticity of a digital document.

A **DIGITAL SIGNATURE** is an electronic, encrypted, stamp of authentication on digital information such as email messages, macros, or electronic documents. A signature confirms that the information originated from the signer and has not been altered.

## **Signing Certificate**

To create a digital signature, you need a **SIGNING CERTIFICATE**, which proves identity. When you send a digitally-signed macro or document, you also send your certificate and public key. Certificates are issued by a **certification authority**, and like a driver's license, can be revoked. A certificate is usually valid for a year, after which, the signer must renew, or get a new, signing certificate to establish identity.

## **Certificate Authority (CA)**

A **certificate authority** is an entity similar to a notary public. It issues digital certificates, signs certificates to verify their validity and tracks which certificates have been revoked or have expired.

Digital certificates are issued by CAs, which sign a certificate to prove the authenticity of the individual or organization that issued the request. A CA is responsible for managing domain control verification and verifying that the public key attached to the certificate belongs to the user or organization that requested it. They play an important part in the PKI process and keeping internet traffic secure.

## **Digital Signature Assurances**

The following terms and definitions show what assurances are provided by digital signatures.

* **Authenticity:** The signer is confirmed as the signer.  
* **Integrity:** The content has not been changed or tampered with since it was digitally signed.  
* **Non-repudiation:** Proves to all parties the origin of the signed content. Repudiation refers to the act of a signer denying any association with the signed content.  
* **Notarization:** Signatures in Microsoft Word, Microsoft Excel, or Microsoft PowerPoint files, which are time stamped by a secure time-stamp server, under certain circumstances, have the validity of a notarization.

To make these assurances, the content creator must digitally sign the content by using a signature that satisfies the following criteria:

* The digital signature is valid.  
* The certificate associated with the digital signature is current (not expired).  
* The signing person or organization, known as the publisher, is trusted.  
* The certificate associated with the digital signature is issued to the signing publisher by a reputable certificate authority (CA).

(Image of a sample certificate)

## **Types Of Digital Certificates**

There are three different types of public key certificates:

* A transport layer security (TLS/SSL) certificate  
* A code signing certificate  
* A client certificate.

### **TLS/SSL Certificate**

A **TLS/SSL certificate** sits on a server- such as an application, mail, or web server-to ensure communication with its clients is private and encrypted. The certificate provides authentication for the server to send and receive encrypted messages to clients. The existence of a TLS/SSL certificate is signified by the Hypertext Transfer Protocol Secure (HTTPS) designation at the start of a Uniform Resource Locator (URL) or web address.

* **Domain validated:** A domain validated certificate is a quick validation method that is acceptable for any website. It is cheap to obtain and can be issued in a matter of minutes.  
* **Organization validated:** This provides light business authentication and is ideal for organizations selling products online through e-commerce.  
* **Extended validation:** This offers full business authentication, which is required by larger organizations or any business dealing with highly sensitive information. It is typically used by... (text cut off)

### **Code signing certificate**

A code signing certificate is used to confirm the authenticity of software or files downloaded through the internet. The developer or publisher signs the software to confirm that it is genuine to users that download it. This is useful for software providers that make their programs available on third-party sites to prove that files have not been tampered with.

### **Client certificate**

A client certificate is a digital ID that identifies an individual user to another user or machine, or one machine to another. A common example of this is email, where a sender signs a communication digitally and its signature is verified by the recipient. Client certificates can also be used to help users access protected databases.

## **Beneficial Features Of Digital Certificates**

Digital certificates are becoming increasingly important, as cyberattacks continue to increase in both volume and sophistication. Key benefits of digital certificates include:

* **Security:** Digital certificates encrypt internal and external communications to prevent attackers from intercepting and stealing sensitive data. For example, a TLS/SSL certificate encrypts data between a web server and a web browser, ensuring an attacker cannot intercept website visitors' data.  
* **Scalability:** Digital certificates provide businesses of all shapes and sizes with the same encryption quality. They are highly scalable, which means they can easily be issued, revoked, and renewed in seconds, used to secure user devices, and managed through a centralized platform.  
* **Authenticity:** Digital certificates are crucial to ensuring the authenticity of online communication in the age of widespread cyberattacks. They make sure that users' messages will always reach their intended recipient-and only reach their intended recipient. TLS/SSL certificates encrypt websites, Secure/Multipurpose Internet Mail Extensions (S/MIME) encrypt email communication, and document-signing certificates can be used for digital document sharing.  
* **Reliability:** Only publicly trusted CAs can issue recognized digital certificates. Obtaining one requires rigorous vetting, which ensures hackers or fake organizations cannot trick victims that use a digital certificate.  
* **Public trust:** Using a digital certificate provides confirmation that a website is genuine and that documents and emails are authentic. This projects public trust, assuring clients that they are dealing with a genuine company that values their security and privacy.

## **Digital Certificate Vs Digital Signature**

A **digital certificate** is a file that verifies the identity of a device or user and enables encrypted connections. A **digital signature** is a hashing approach that uses a numeric string to provide authenticity and validate identity.

A digital signature is typically fixed to a document or email using a cryptographic key. The signature is hashed, and when the recipient receives it, it performs that same hash function to confirm that the information from the signer has not been altered.

# **Cryptography Hash Functions**

## **Cryptography Hash Functions**

A **cryptographic hash function** is a specialized type of hash function designed for use in various cryptographic applications, including digital signatures, message authentication codes, and other forms of authentication. These functions play a crucial role in modern information security practices, particularly in protocols like SSL/TLS.

Hash functions are fundamental to modern cryptography. These functions map binary strings of an arbitrary length to small binary strings of a fixed length, known as hash values.

A cryptographic hash function has the property that it is computationally infeasible to find two distinct inputs that hash to the same value.

Hash functions are commonly used with digital signatures and for data integrity.

The hash is used as a unique value of fixed size representing a large amount of data. Hashes of two sets of data should match if the corresponding data also matches. Small changes to the data result in large unpredictable changes in the hash.

Due to collision problems with SHA-1, best practice recommends a security model based on SHA-256 or better.

## **Key Properties Of Cryptographic Hash Functions**

Cryptographic hash functions possess several essential properties that distinguish them from other hash functions:

* **Deterministic:** The same input message always produces the same hash value.  
* **Efficiency:** The hash value is computed quickly, regardless of the input size.  
* **Collision Resistance:** It is computationally infeasible to find two different messages that produce the same hash value.  
* **Preimage Resistance:** Given a hash value, it is infeasible to create a message that produces that specific hash.  
* **Avalanche Effect:** Small changes in the input message result in significant, seemingly uncorrelated changes in the output hash.

## **Common Cryptographic Hash Functions**

Several cryptographic hash functions have been widely used over the years:

* **(Message Digest Algorithm 5\) MD5:** Once popular but now considered cryptographically broken and unsuitable for security applications.  
* **(Secure Hash Algorithm 1\) SHA-1:** Formerly widely used but now deprecated due to security vulnerabilities.  
* **SHA-2:** The SHA-2 family is currently the industry standard for cryptographic hashing, offering robust security with longer hash outputs of 256 and 512 bits. These algorithms provide strong collision and pre-image resistance, making them the preferred choice for secure communication protocols, blockchain technologies, and password hashing.  
* **SHA-3:** Adopted as the latest NIST standard, SHA-3 uses a unique sponge construction different from SHA-2, enhancing security and flexibility. It offers comparable hash lengths with improved resistance to certain types of attacks, making it suitable for applications demanding long-term security.  
* **BLAKE2 & BLAKE3:** Designed as high-speed, secure alternatives to SHA-2 and SHA-3, BLAKE2 and BLAKE3 deliver faster hashing without compromising security. BLAKE3, in particular, supports parallel processing and incremental updates, making it ideal for modern systems requiring both speed and strong cryptographic guarantees.

## **Applications Of Cryptographic Hash Functions**

Cryptographic hash functions have numerous applications in cybersecurity:

* **Digital Signatures:** Used to create a fixed-size digest of a message, which is then encrypted with the sender's private key.  
* **File Integrity Verification:** Websites often publish hash values for downloadable files, allowing users to verify the file's integrity after download.  
* **Password Security:** Passwords are typically stored as hashes rather than plaintext, enhancing security.  
* **Blockchain Technology:** Cryptocurrencies like Bitcoin use cryptographic hash functions (e.g., SHA-256) to maintain the integrity and security of transaction records.  
* **SSL/TLS Protocols:** These secure communication protocols rely heavily on cryptographic hash functions for various security mechanisms.