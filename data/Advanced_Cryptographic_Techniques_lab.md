# **CYS 535—WEEK 1—LAB**

## **ADVANCED CYPTOGRAPHIC TECHNIQUES**

# **How to use OpenSSL to generate an ECC key pair**

## **Lab Overview**

In this hands-on cybersecurity lab, you will learn how to configure a secure, encrypted HTTPS web server from scratch. This lab is broken down into four core phases:

1. **Generating an Elliptic Curve Cryptography (ECC) Private Key**  
2. **Creating a Certificate Signing Request (CSR)**  
3. **Self-Signing the TLS Certificate**  
4. **Configuring, Debugging, and Testing a Python HTTPS Secure Web Server**

## **Technical Concept: Why ECC?**

**Elliptic Curve Cryptography (ECC)** is a modern public-key cryptography framework based on the algebraic structure of elliptic curves over finite fields.

| Feature | Elliptic Curve Cryptography (ECC) | Rivest–Shamir–Adleman (RSA) |
| :---- | :---- | :---- |
| **Key Size** | Smaller (e.g., 256-bit) | Larger (e.g., 2048-bit or 3072-bit) |
| **Computational Overhead** | Highly efficient, lighter, and faster | Heavy computational requirements |
| **Security Equivalent** | A 256-bit ECC key offers the same security as a 3072-bit RSA key | Requires huge keys to maintain modern security standards |
| **Best Use Cases** | Mobile devices, IoT, high-performance web servers | Legacy systems, compatibility-focused networks |

## **Phase 1: Environment Setup & Key Generation**

### **Step 1.1: Create a Dedicated Workspace**

Before executing cryptographic commands, create a clean directory to store your keys, requests, and certificates.

`# Navigate to your Documents folder`  
`cd ~/Documents`

`# Create a workspace directory for the Elliptic Curve Cryptography assets`  
`mkdir ecc`

`# Enter the newly created workspace`  
`cd ecc`

### **Step 1.2: Generate the ECC Private Key**

We will use OpenSSL to generate the private key. We use the prime256v1 elliptic curve (also structurally known as **NIST P-256**), which is widely trusted and optimized for modern security protocols.  
`openssl ecparam -genkey -name prime256v1 -out ecc_private.key`

**Command Breakdown:**

* ecparam: Instructs OpenSSL to use Elliptic Curve parameter generation.  
* \-genkey: Generates an EC private key using the specified parameters.  
* \-name prime256v1: Selects the standard, widely trusted curve profile (P-256).  
* \-out ecc\_private.key: Specifies the file path where the generated private key will be saved.

\[\!NOTE\]  
Running this command will not produce any output on your terminal screen. This is standard secure behavior to avoid exposing cryptographic key parameters.

### **Step 1.3: Verify Key Generation**

Verify that the private key file has been successfully written to the directory:  
`ls -l`

You should see ecc\_private.key listed in the output.

## **Phase 2: Creating a Certificate Signing Request (CSR)**

A Certificate Signing Request (CSR) is a block of encoded text given to a Certificate Authority (CA) to apply for a TLS certificate. It contains identity details (such as organization and domain name) and the public key that will be associated with the certificate.

### **Step 2.1: Run the CSR Generation Command**

Generate the CSR by pairing it with your generated ECC private key:  
`openssl req -new -key ecc_private.key -out ecc_request.csr`

**Command Breakdown:**

* req: Activates the PKCS\#10 Certificate Request and Certification Generating utility.  
* \-new: Generates a new certificate request.  
* \-key ecc\_private.key: Specifies the private key used to sign the request.  
* \-out ecc\_request.csr: Specifies the file destination for the output request.

### **Step 2.2: Complete the Identity Prompts**

When prompted by OpenSSL, input the following details. For local testing, ensure you use localhost as the **Common Name (CN)**:  
`Country Name (2 letter code) [XX]: NG`  
`State or Province Name (full name) []: Lagos`  
`Locality Name (eg, city) [Default City]: Lagos`  
`Organization Name (eg, company) [Default Company Ltd]: AltSchool`  
`Organizational Unit Name (eg, section) []: Highland`  
`Common Name (eg, your name or your server's hostname) []: localhost`  
`Email Address []: admin@localhost.com`

`Please enter the following 'extra' attributes`  
`to be sent with your certificate request`  
`A challenge password []: [Optional - Press Enter to leave blank]`  
`An optional company name []: AltSchool Africa`

\[\!IMPORTANT\]  
The **Common Name (CN)** must match your server's domain name or IP address. Since we are hosting this web server locally for testing purposes, we define the Common Name as localhost.

### **Step 2.3: Verify the CSR Generation**

Verify that the CSR file is written to your workspace directory:  
`ls -l`

You should now see ecc\_request.csr in your workspace.

## **Phase 3: Self-Signing the TLS Certificate**

In a production environment, you would send your CSR file to a reputable public Certificate Authority (such as Let's Encrypt, DigiCert, or Sectigo) to get a signed public certificate. For development, training, and internal testing, we can act as our own authority and **self-sign** the certificate using our private key.

### **Step 3.1: Run the Self-Signing Command**

Generate a certificate valid for one year (365 days):  
`openssl x509 -req -days 365 -in ecc_request.csr -signkey ecc_private.key -out ecc_cert.crt`

**Command Breakdown:**

* x509: Activates the X.509 certificate data management utility.  
* \-req: Tells OpenSSL to expect a CSR input.  
* \-days 365: Specifies the certificate validity period (1 year).  
* \-in ecc\_request.csr: Identifies the source Certificate Signing Request.  
* \-signkey ecc\_private.key: Signs the certificate using your own ECC private key.  
* \-out ecc\_cert.crt: Designates the file path for the resulting public TLS certificate.

### **Step 3.2: Verify the TLS Certificate**

Confirm that the certificate is generated:  
`ls -l`

You should now see three files in your workspace directory:

1. ecc\_private.key (Your private key)  
2. ecc\_request.csr (Your request file)  
3. ecc\_cert.crt (Your self-signed public certificate)

## **Phase 4: Building the Python Secure Web Server**

Now that the cryptographic credentials are ready, we will use Python’s built-in standard libraries to create an HTTPS server without needing external frameworks (like Flask or Django).

### **Step 4.1: Create the Server File**

Generate a file named server.py and open it inside the nano terminal text editor:  
`touch server.py`  
`nano server.py`

### **Step 4.2: Start Writing the Server Script**

In the nano editor, start constructing the script by writing the following initial logic:  
`# Import the built-in HTTP server module to handle HTTP/HTTPS protocols`  
`import http.server`

`# Import the SSL module to wrap connections with TLS encryption`  
`import ssl`

`# Define the network address and custom port the server will bind to`  
`# 4443 is a standard non-privileged port alternative to HTTPS default port 443`  
`server_address = ('localhost', 4443)`

`# Create an instance of the HTTPServer using Python's standard Request Handler`  
`httpd = http.server.HTTPServer(server_address, http.server.SimpleHTTPRequestHandler)`

### **Step 4.3: Configure the TLS Cryptographic Context**

We must transition our plain HTTP connections into encrypted HTTPS transactions. We do this by creating an SSL/TLS context wrapper and binding our ECC credentials to it.  
Add the following code to your server.py script:

\# Create an SSL/TLS context explicitly configured for server-side usage.  
\# This configures modern secure defaults and blocks obsolete, vulnerable protocols.  
context \= ssl.SSLContext(ssl.PROTOCOL\_TLS\_SERVER)

\# Load the public certificate and the secure private key into the TLS context.  
\# These keys are parsed during the TLS handshake to authenticate the server to the browser.  
context.load\_cert\_chain(certfile="ecc\_cert.crt", keyfile="ecc\_private.key")

\# Wrap the server's primary socket with our configured TLS context.  
\# Setting server\_side=True designates that this socket acts as the secure host.  
httpd.socket \= context.wrap\_socket(httpd.socket, server\_side=True)

\# Print a tracking output to terminal to confirm server status and interface  
print("Serving secure HTTPS at https://localhost:4443")

\# Initialize the persistent server loop. This keeps the daemon listening indefinitely.  
\# To exit the runtime, you must terminate it manually using Ctrl+C.  
httpd.serve\_forever()

## **Phase 5: Building the Web Application Content**

To test our cryptographic server, we need front-end files to serve. We will copy a responsive HTML template representing a styled, food-focused web interface into an index.html file located in the same directory.

### **Step 5.1: Create the HTML Entry Point**

Create a blank index.html file and open it inside nano for writing:  
touch index.html  
nano index.html

### **Step 5.2: Write the Web Application Code**

Paste the following complete, responsive, styled HTML code inside your index.html file. This is configured to work with the mock food images downloaded for local display:  
`<!DOCTYPE html>`  
`<html lang="en">`  
`<head>`  
    `<meta charset="UTF-8">`  
    `<meta name="viewport" content="width=device-width, initial-scale=1.0">`  
    `<title>Gourmet Bytes - Local Host</title>`  
    `<style>`  
        `body {`  
            `font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;`  
            `margin: 0;`  
            `padding: 0;`  
            `background-color: #fcf8f2;`  
            `color: #333;`  
        `}`  
        `header {`  
            `background-color: #d35400;`  
            `color: white;`  
            `text-align: center;`  
            `padding: 2em 1em;`  
            `box-shadow: 0 4px 6px rgba(0,0,0,0.1);`  
        `}`  
        `h1 {`  
            `margin: 0;`  
            `font-size: 2.5em;`  
        `}`  
        `p.subtitle {`  
            `margin: 5px 0 0 0;`  
            `font-size: 1.1em;`  
            `color: #fbeee6;`  
        `}`  
        `.container {`  
            `max-width: 1200px;`  
            `margin: 20px auto;`  
            `padding: 0 20px;`  
        `}`  
        `.grid {`  
            `display: grid;`  
            `grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));`  
            `gap: 20px;`  
            `margin-top: 30px;`  
        `}`  
        `.card {`  
            `background: white;`  
            `border-radius: 8px;`  
            `overflow: hidden;`  
            `box-shadow: 0 4px 8px rgba(0,0,0,0.05);`  
            `transition: transform 0.2s;`  
        `}`  
        `.card:hover {`  
            `transform: translateY(-5px);`  
        `}`  
        `.card img {`  
            `width: 100%;`  
            `height: 200px;`  
            `object-fit: cover;`  
            `display: block;`  
        `}`  
        `.card-content {`  
            `padding: 15px;`  
        `}`  
        `.card-title {`  
            `font-size: 1.3em;`  
            `margin: 0 0 10px 0;`  
            `color: #2c3e50;`  
        `}`  
        `.card-desc {`  
            `font-size: 0.95em;`  
            `line-height: 1.5;`  
            `color: #555;`  
        `}`  
        `footer {`  
            `text-align: center;`  
            `padding: 20px;`  
            `margin-top: 40px;`  
            `color: #7f8c8d;`  
            `border-top: 1px solid #eee;`  
        `}`  
    `</style>`  
`</head>`  
`<body>`

    `<header>`  
        `<h1>Gourmet Bytes</h1>`  
        `<p class="subtitle">Secure local delivery platform running over ECC-encrypted TLS</p>`  
    `</header>`

    `<div class="container">`  
        `<h2>Today's Specials</h2>`  
        `<div class="grid">`  
            `<!-- Card 1 -->`  
            `<div class="card">`  
                `<img src="food1.jpg" alt="Artisanal Sandwich">`  
                `<div class="card-content">`  
                    `<h3 class="card-title">Artisanal Sandwich</h3>`  
                    `<p class="card-desc">Freshly baked sourdough stacked with farm-fresh organic ingredients, avocado, and homemade pestos.</p>`  
                `</div>`  
            `</div>`

            `<!-- Card 2 -->`  
            `<div class="card">`  
                `<img src="food2.webp" alt="Fresh Sea Harvest">`  
                `<div class="card-content">`  
                    `<h3 class="card-title">Fresh Sea Harvest</h3>`  
                    `<p class="card-desc">Responsibly sourced salmon salad infused with organic citrus dressing, capers, and dill leaves.</p>`  
                `</div>`  
            `</div>`

            `<!-- Card 3 -->`  
            `<div class="card">`  
                `<img src="food3.jpg" alt="Special Berry Dessert">`  
                `<div class="card-content">`  
                    `<h3 class="card-title">Summer Berry Parfait</h3>`  
                    `<p class="card-desc">Layers of wild handpicked summer berries, thick organic Greek yogurt, and zero-calorie clean sweeteners.</p>`  
                `</div>`  
            `</div>`  
        `</div>`  
    `</div>`

    `<footer>`  
        `<p>© 2026 Gourmet Bytes. Serving Local Host Securely.</p>`  
    `</footer>`

`</body>`  
`</html>`

### **Step 5.3: Save and Exit**

Once the file contents have been copied:

1. Press Ctrl \+ X on your keyboard.  
2. When prompted to save modified buffer, press Y (Yes).  
3. Hit Enter to confirm the file name as index.html.

## **Phase 6: Running the Server & Troubleshooting Syntax Errors**

### **Step 6.1: Run the Server Command**

Start your newly constructed HTTPS server using Python 3:  
`python3 server.py`

### **Step 6.2: Troubleshooting Common Script Errors (SyntaxError)**

If your python daemon fails to start with a traceback resembling:  
  `File "server.py", line 8`  
    `print("Serving secure HTTPS at https://localhost:4443)`  
                                                        `^`  
`SyntaxError: unterminated string literal (detected at line 8)`

This indicates that a string delimiter (a double quote ") was left unclosed on or around line 8\.  
**To resolve this issue:**

1. Re-open your file: nano server.py  
2. Navigate to line 8 and ensure your print statement is fully enclosed:  
   `print("Serving secure HTTPS at https://localhost:4443")`

3. Save, exit (Ctrl \+ X, Y, Enter), and clear your console interface using the clear command to keep a clean terminal view.  
4. Execute python3 server.py again. Your terminal should successfully log:  
   `Serving secure HTTPS at https://localhost:4443`

## **Phase 7: Serving the Site & Bypassing Self-Signed Warnings**

### **Step 7.1: Connect to your Server via Browser**

Open your web browser and navigate explicitly to:  
`https://localhost:4443`

### **Step 7.2: Bypass the Self-Signed Security Warn**

Your browser will intercept your request and display a prominent warning stating **"Potential Security Risk Ahead"** or **"Your connection is not private"**.  
**Why does this happen?** Browsers verify TLS certificates using an internal pre-installed store of trusted **Root Certificate Authorities** (such as DigiCert or Let's Encrypt). Since you self-signed this certificate, your machine acted as the Root CA. The browser does not recognize your local private key as an authenticated public trust anchor.  
**How to safely bypass this during development:**

1. Click on the **Advanced** or **More Information** button.  
2. Click on **Accept the Risk and Continue** or **Proceed to localhost (unsafe)**.  
3. The page will successfully render the Gourmet Bytes layout.

## **Phase 8: Setting Up Local Media Assets (Images)**

Upon loading the page, you might notice that the three product cards show broken images. This is because the template's relative image references (food1.jpg, food2.webp, food3.jpg) do not exist in your local workspace directory.

### **Step 8.1: Download and Save Local Mock Images**

To fix this, keep your server running in the current terminal window. Open a new terminal instance (Right-click on terminal \-\> *Launch a new instance*) and download replacement images into your target directory.  
For example, if you download images using browser downloads, locate them in your standard system directory and transfer them to the ecc workspace:  
`# Move downloaded assets into your ECC folder`  
`mv ~/Downloads/food1.jpg ~/Documents/ecc/`  
`mv ~/Downloads/food2.webp ~/Documents/ecc/`  
`mv ~/Downloads/food3.jpg ~/Documents/ecc/`

### **Step 8.2: Verify Web Directory Assets**

Ensure all required assets sit in the same working directory before testing:  
`cd ~/Documents/ecc`  
`ls -l`

**Expected files inside /ecc directory:**

* `ecc_cert.crt`  
* `ecc_private.key`  
* `ecc_request.csr`  
* `food1.jpg`  
* `food2.webp`  
* `food3.jpg`  
* `index.html`  
* `server.py`

Reload the browser tab at https://localhost:4443. The webpage will now pull and cleanly render all three special dishes with zero broken assets.

## **Phase 9: Verifying the ECC Certificate Details**

To verify that your server is properly encrypting connections using Elliptic Curve Cryptography:

### **Step 9.1: Access the Certificate Viewer**

1. In the browser address bar, click the padlock icon adjacent to the URL (which may display a red warning indicator due to the self-signed status).  
2. Select **Connection Not Secure** \-\> **More Information**.  
3. Under the Security Tab, click the **View Certificate** button.

### **Step 9.2: Inspect Cryptographic Attributes**

Verify the certificate fields:

* **Subject Name (Common Name):** localhost  
* **Issuer Name (Organization):** AltSchool  
* **Validity Period:** 365 Days (from today's date through next year)  
* **Public Key Algorithm:** Elliptic Curve Public Key  
* **Elliptic Curve Parameter:** prime256v1 (NIST P-256)  
* **Key Size:** 256 bits (equivalent to a massive, computationally expensive 3072-bit RSA key)  
* **Fingerprint Hashes:** SHA-256 / SHA-1 cryptographic fingerprints of the certificate file.

## **Phase 10: Verifying over CLI with cURL**

We can test connections directly from the CLI. This is standard procedure for automating secure system checks.

### **Step 10.1: Default cURL Verification Fail**

Run a baseline cURL request against your secure local host:  
`curl https://localhost:4443`

You will receive an error warning:  
`curl: (60) SSL certificate problem: self signed certificate`  
`More details here: [https://curl.se/docs/sslcerts.html](https://curl.se/docs/sslcerts.html)`

This happens for the same reason the browser generated an alert—curl rejects self-signed certificates by default to protect you from spoofing attacks.

### **Step 10.2: Bypassing TLS Checks securely on the CLI**

During testing, bypass verification using the \-k or \--insecure flag:  
`curl -k https://localhost:4443`

Your terminal will ignore the self-signed status and output the full raw HTML payload of your index.html file safely.

## **Phase 11: Production Best Practices & Security Audit Notes**

While our built-in Python HTTPS server is excellent for educational purposes and rapid prototyping, it is **highly discouraged for production use**:

1. **Single-Threaded Architecture:** The Python http.server library operates on a single thread. This means it processes incoming HTTP requests sequentially rather than concurrently. If a client downloads a large image asset, all other concurrent visitors are placed in a queue and blocked until the current request completes.  
2. **Lack of Performance Optimization:** It does not natively support advanced HTTP methods, HTTP/2 multiplexing, keep-alive connections, or Gzip compression.  
3. **Production Recommendation:** To deploy ECC TLS servers at scale, use production-grade software:  
   * Use **Nginx** or **Apache** to terminate the SSL/TLS connection.  
   * Bind the upstream application to standard WSGI/ASGI application servers (such as **Gunicorn** or **Uvicorn**).  
   * Implement automated certificate renewal protocols using **ACME** clients (such as Certbot with Let's Encrypt).