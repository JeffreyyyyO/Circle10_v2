# **Lab: Encryption on Kali Linux**

This document walks through the terminal commands from a lab session covering three types of encryption. Each command is preceded by a short explanation of its purpose.

# **Symmetric / Public Key Encryption**

In this section, we use a single, shared secret key to encrypt and decrypt a file.

## **Lab Setup and Navigation**

Check the present working directory.

### pwd

### **/home/kali** 

List the contents of the current directory.

### ls

### **Desktop  Documents  Downloads  Music  Pictures  Public  Templates  Videos** 

Navigate into the Desktop directory.

### cd Desktop 

Create a new directory named Karatu for this exercise.

### **\[\~/Desktop\]**

### mkdir Karatu 

Enter the newly created Karatu directory.

### cd Karatu 

List the contents. (Note: The log shows a secret.key file, which we will overwrite in a later step).

### **\[\~/Desktop/Karatu\]** 	ls

### **secret.key** 

Create a new, empty text file named altfile.txt. This will be the file we encrypt.

### touch altfile.txt  

List the contents again to confirm altfile.txt was created.

### ls 

### **altfile.txt  secret.key** 

## **Key Generation and Encryption**

Generate a 32-byte (256-bit) random, Base64-encoded key and save it to secret.key. This is our symmetric key.  
openssl rand \-base64 32 \>secret.key

Encrypt altfile.txt.

* enc: Use the OpenSSL encryption tool.  
* \-aes-256-cbc: Specify the AES-256-CBC cipher.  
* \-salt: Use a salt to make the encryption more secure.  
* \-in altfile.txt: The input file to encrypt.  
* \-out encrypted.bin: The name of the encrypted output file.  
* \-pass file:secret.key: Use the contents of secret.key as the password.

### openssl enc \-aes-256-cbc \-salt \-in altfile.txt \-out encrypted.bin \-pass file:secret.key 

### **\*\*\* WARNING : deprecated key derivation used.** 	**Using \-iter or \-pbkdf2 would be better.** 

List the directory contents to see the new encrypted file.

### ls

### **altfile.txt  encrypted.bin  secret.key** 

## **Decryption and Verification**

Decrypt encrypted.bin.

* \-d: Specifies decryption mode.  
* \-in encrypted.bin: The encrypted file to decrypt.  
* \-out clear.txt: The name of the file to save the decrypted content to.  
* \-pass file:secret.key: Use the same secret key to decrypt.

### openssl enc \-d \-aes-256-cbc \-salt \-in encrypted.bin \-out clear.txt \-pass file:secret.key 

### **\*\*\* WARNING : deprecated key derivation used.** 	**Using \-iter or \-pbkdf2 would be better.** 

List contents to see the decrypted clear.txt file.

### ls 	**altfile.txt  clear.txt  encrypted.bin  secret.key** 

Display the contents of the decrypted file.

### cat clear.txt 	Today is a very happy day 

Display the contents of the original file to verify they match.

### cat altfile.txt 	**Today is a very happy day** 

Attempt to display the contents of the encrypted file (shows garbled binary data).

### cat encrypted.bin 	**Salted\_\_Yk6m\*\<...\[and so on\]** 

# **Asymmetric / Private Key Encryption**

In this section, we use a pair of keys: a public key to encrypt and a private key to decrypt.

## **Lab Setup and Key Pair Generation**

List the contents of the Home directory.

### ls 	**Desktop  Documents  Downloads  Music  Pictures  Public  Templates  Videos** 

Navigate back to the Desktop.

### cd Desktop 

Create a new directory for the asymmetric lab.

### mkdir Karatu-Asymmetric 

List contents to see the new directory.

### ls

### **Karatu  Karatu-Asymmetric** 

Enter the new directory.

### cd Karatu-Asymmetric 

Generate an RSA private key and save it as private.pem. This is the secret key.

### **\[\~/Desktop/Karatu-Asymmetric\]** 	openssl genpkey \-algorithm RSA \-out private.pem **..+............+.........+.....+....+..+...+.......+...+...\[and so on\]** 

List contents to confirm the private key was created.

### ls

### **private.pem** 

Extract the corresponding public key from the private key and save it as public.pem. This key can be shared.

### openssl rsa \-in private.pem \-pubout \-out public.pem 	**writing RSA key** 

Create a new file theSpice.txt that we will encrypt.

### touch theSpice.txt 

Add content to the file using echo and redirection (\>\>).

### echo "This spice is the bringer of life. He who possesses the spice, possesses the power\!" \>\> theSpice.txt 

## **Encryption and Decryption**

Encrypt theSpice.txt using the *public key*.

* pkeyutl \-encrypt: Use the public key utility to encrypt.  
* \-inkey public.pem \-pubin: Specify that the key is a public key.  
* \-in theSpice.txt: The input file.  
* \-out encrypted\_theSpice.bin: The encrypted output file.

### openssl pkeyutl \-encrypt \-inkey public.pem \-pubin \-in theSpice.txt \-out encrypted\_theSpice.bin 

List the contents to see all the files.

### ls

### **encrypted\_theSpice.bin  private.pem  public.pem  theSpice.txt** 

Attempt to display the encrypted content (shows garbled binary data).

### cat encrypted\_theSpice.bin

### **\+)%ZUT3MIUȢFtK-Cu\[I:P...\[and so on\]** 

Decrypt the file using the *private key*.

* pkeyutl \-decrypt: Use the public key utility to decrypt.  
* \-inkey private.pem: Specify the private key to use for decryption.  
* \-in encrypted\_theSpice.bin: The encrypted input file.  
  (Note: Without an \-out flag, the decrypted content is printed directly to the terminal).

### openssl pkeyutl \-decrypt \-inkey private.pem \-in encrypted\_theSpice.bin

### **This spice is the bringer of life. He who possesses the spice, possesses the power\!** 

# **Public Key Infrastructure**

This section demonstrates creating a simple Public Key Infrastructure (PKI), including a Certificate Authority (CA) and a user certificate request.

## **Lab Setup and CA Creation**

Navigate back to the Desktop.

### cd Desktop 

Create a directory for the PKI lab.

### mkdir Karatu-PKI 

List contents to see all lab directories.

### ls

### Karatu  Karatu-Asymmetric  Karatu-PKI 

Enter the PKI directory.

### cd Karatu-PKI 

Generate an encrypted (AES-256) RSA private key for the Certificate Authority (CA).

* \-aes256: Encrypt the generated key.  
* \-pass pass:gambino: Provides the password "gambino" on the command line.

### **\[\~/Desktop/Karatu-PKI\]** 	openssl genpkey \-algorithm RSA \-out ca\_private\_key.pem \-aes256  \-pass pass:gambino 	**....+.........+...++++++++++++++++++++++++++++++...\[and so on\]** 

Create a new self-signed root certificate for the CA.

* req \-x509: Create a self-signed X.509 certificate.  
* \-new: This is a new request.  
* \-key ca\_private\_key.pem: Use the CA's private key to sign it.  
* \-days 365: Set the certificate's validity period.  
* \-out ca\_certificate.pem: The output file (the CA's public certificate).  
* \-passin pass:gambino: Provide the password for the private key.  
  (This command prompts for Distinguished Name (DN) information for the CA).

### openssl req \-x509 \-new \-key ca\_private\_key.pem \-days 365 \-out ca\_certificate.pem \-passin pass:gambino 

### **You are about to be asked to enter information that will be incorporated** **into your certificate request.** **What you are about to enter is what is called a Distinguished Name or a DN.** **There are quite a few fields but you can leave some blank** **For some fields there will be a default value,** **If you enter '.', the field will be left blank.** **\-----** **Country Name (2 letter code) \[AU\]:NG** **State or Province Name (full name) \[Some-State\]:Lagos**    **Locality Name (eg, city) \[\]:Aso-Rock** **Organization Name (eg, company) \[Internet Widgits Pty Ltd\]:AltStudio** **Organizational Unit Name (eg, section) \[\]:Design** **Common Name (e.g. server FQDN or YOUR name) \[\]:JeffreyO**                **Email Address \[\]:ginormous99@mozmail.com** 

List contents to see the CA's private key and public certificate.

### ls

### **ca\_certificate.pem  ca\_private\_key.pem** 

## **User Certificate Signing Request (CSR) Creation**

Generate an encrypted private key for a "user" (e.g., a server or person).

### openssl genpkey \-algorithm RSA \-out user\_private\_key.pem \-aes256 \-pass pass:gambino 

### **...+..........+...+.........+.........+...\[and so on\]** 

Create a Certificate Signing Request (CSR) for the user.

* req \-new: This is a new request (a CSR).  
* \-key user\_private\_key.pem: Use the user's private key to generate the request.  
* \-out user\_csr.pem: The output file (the CSR).  
  (This CSR would normally be sent to a CA to be signed).

### openssl req \-new \-key user\_private\_key.pem \-days 365 \-out user\_csr.pem 

### **Warning: Ignoring \-days without \-x509; not generating a certificate** **Enter pass phrase for user\_private\_key.pem:** **You are about to be asked to enter information that will be incorporated** **into your certificate request.** **What you are about to enter is what is called a Distinguished Name or a DN.** **There are quite a few fields but you can leave some blank** **For some fields there will be a default value,** **If you enter '.', the field will be left blank.** **\-----** **Country Name (2 letter code) \[AU\]:NG** **State or Province Name (full name) \[Some-State\]:Lagos** **Locality Name (eg, city) \[\]:Aso-Rock** **Organization Name (eg, company) \[Internet Widgits Pty Ltd\]:AltStudio** **Organizational Unit Name (eg, section) \[\]:Design** **Common Name (e.g. server FQDN or YOUR name) \[\]:JeffreyO** **Email Address \[\]:ginormous99@mozmail.com**  **Please enter the following 'extra' attributes** **to be sent with your certificate request** **A challenge password \[\]:gambinogambino** **An optional company name \[\]:AltS** 

List contents to see all CA and user files.

### ls

### **ca\_certificate.pem  ca\_private\_key.pem  user\_csr.pem  user\_private\_key.pem** 

Verify and display the contents of the CSR in human-readable text.

* \-text: Print the request in text form.  
* \-noout: Don't output the encoded version of the request.  
* \-verify: Verify the signature on the request.

### openssl req \-text \-noout \-verify \-in user\_csr.pem 

### **Certificate request self-signature verify OK** **Certificate Request:**     **Data:**         **Version: 1 (0x0)**         **Subject: C=NG, ST=Lagos, L=Aso-Rock, O=AltStudio, OU=Design, CN=JeffreyO, emailAddress=ginormous99@mozmail.com**         **Subject Public Key Info:**             **Public Key Algorithm: rsaEncryption**                 **Public-Key: (2048 bit)**                 **Modulus:**                     **00:9d:0c:ad:68:95:c2:b9:5e:08:3...\[and so on\]**