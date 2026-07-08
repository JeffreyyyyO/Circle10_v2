# **CYS 532—WEEK 2—LAB**

## **ADVANCED CLOUD SECURITY**

# **Introduction to Cloud-Native Security: Docker**

## **1\. Overview and Objectives**

In modern cloud-native environments, container images serve as the static blueprints for our runtime infrastructure. However, deploying application code along with outdated libraries and system packages presents a significant attack surface.

This hands-on lab demonstrates how to:

* Set up and configure a local **Docker** environment securely.  
* Clone and prepare a legacy Node.js application.  
* Build a local container image using a custom `Dockerfile`.  
* Utilize **Trivy** (Aqua Security's open-source vulnerability scanner) to detect, categorize, and analyze image vulnerabilities prior to deployment.  
* Troubleshoot system-level issues during scans, such as storage constraints.

## **2\. Prerequisites and Environment Setup**

This lab is performed in a Linux environment (e.g., Ubuntu, Kali Linux, or Debian).

### **Step 1: Update the Local Package Index**

Ensure all local system packages are updated to prevent installation conflicts:

`sudo apt update`

### **Step 2: Install Docker Engine**

Install the native Docker runtime environment:

`sudo apt install -y docker.io`

### **Step 3: Manage Docker Services**

Start the Docker engine and configure it to launch automatically at system boot using `systemctl`:

`# Start Docker service`  
`sudo systemctl start docker`  
`# Enable Docker to run on startup`  
`sudo systemctl enable docker`

### **Step 4: Configure Non-Root Permissions**

By default, executing Docker commands requires administrative (`sudo`) privileges. To add your current user to the `docker` group (allowing execution of Docker commands natively):

`sudo usermod -aG docker $USER`

*Note: After running this command, you must log out of your session and log back in (or reboot the machine) for the group configuration changes to take effect.*

`sudo reboot`

### **Step 5: Verify Docker Installation**

Confirm Docker is working correctly by checking the version and running a test container:

`# Verify the installed version`  
`docker version`

`# Run a test "Hello World" container`  
`docker run hello-world`

## **3\. Installing Trivy**

Trivy is a lightweight, fast, and highly effective security scanner. It identifies vulnerabilities in operating system packages (APT, APK, YUM) and application-level dependencies (NPM, Pip, Cargo, etc.).

Install Trivy on your Debian/Ubuntu-based machine:

`sudo apt install -y trivy`

## **4\. Preparing the Application and Dockerfile**

### **Step 1: Clone the Sample Project**

To demonstrate vulnerability detection, clone a legacy public Node.js sample application from GitHub into a dedicated project directory:

`# Create and navigate to a project directory`  
`mkdir -p ~/docker-security-lab && cd ~/docker-security-lab`

`# Clone the sample codebase`  
`git clone https://github.com/heroku/node-js-sample.git`  
`cd node-js-sample`

### **Step 2: Create the Dockerfile**

Because the cloned repository does not contain packaging instructions, create a new `Dockerfile`:

`touch Dockerfile`  
`nano Dockerfile`

Add the following configuration to the `Dockerfile`:

`# Use a highly legacy Node.js base image (perfect for showcasing vulnerability detection)`  
`FROM node:8`  
`# Set the working directory inside the container`  
`WORKDIR /usr/src/app`  
`# Copy dependency configuration files`  
`COPY package*.json ./`  
`# Install application dependencies via NPM`  
`RUN npm install`  
`# Expose the application port`  
`EXPOSE 5000`  
`# Copy the rest of the application files`  
`COPY . .`  
`# Define the default command to start the application`  
`CMD ["npm", "start"]`

*Save the file and exit (`Ctrl + X`, then `Y`, then `Enter`).*

## **5\. Building the Container Image**

Package the Node.js application into a local container image tagged as `my-old-school-app:latest`:

`docker build -t my-old-school-app:latest .`

*Note: The trailing dot (`.`) specifies the build context (the current directory) and is mandatory.*

## **6\. Scanning the Image with Trivy**

### **Running a Standard Scan**

Run a vulnerability scan against the newly compiled container image:

`trivy image my-old-school-app:latest`

### **Troubleshooting: "No Space Left on Device"**

During database downloads and scanning, Trivy caches massive datasets on your machine. If your default home directories run out of space, direct Trivy to run using a temporary directory mapped to a larger system partition (e.g., `/var/tmp`):

`TMPDIR=/var/tmp/trivy trivy image my-old-school-app:latest`

## **7\. Analyzing the Vulnerability Report**

Trivy generates detailed severity-based classifications, grouping findings into five key levels:

1. **Critical**  
2. **High**  
3. **Medium**  
4. **Low**  
5. **Unknown**

Because we leveraged a highly outdated base runtime (`node:8`), the output exposes a massive risk profile (e.g., thousands of vulnerabilities).

### **Key Vulnerability Vectors Explored in This Lab**

#### **1\. Symlink / Link Following Weakness (CWE-59 / High Severity)**

* **Description:** An issue where the software attempts to access a file based on its path or filename, but fails to prevent that path from pointing to a symbolic link (symlink) or shortcut. This allows attackers to redirect file read/write operations to unintended, highly sensitive files.  
* **Mitigation:** Adhere to the *Principle of Least Privilege* by restricting write permissions on directories containing configurations and assets, preventing the replacement of legitimate files with malicious symbolic links.

#### **2\. Heap-Based Buffer Overflow (CWE-122 / Critical Severity)**

* **Description:** A memory corruption vulnerability occurring in heap memory (typically when buffers are allocated using routines like `malloc()`). When the application writes more data to a buffer than it was allocated to hold, adjacent memory is overwritten, potentially leading to arbitrary remote code execution (RCE).  
* **Mitigation:** Ensure library dependencies are compiled with modern safe buffer mechanisms, restrict raw memory execution permissions, and prioritize upgrading the runtime base container image.

## **8\. DevSecOps Strategy: Shifting Left**

Integrating scanning tools like Trivy into daily workflows helps build a **Secure Software Development Life Cycle (SSDLC)**:

* **Automate Scan Actions:** Embed Trivy scans into continuous integration pipelines (e.g., GitHub Actions, GitLab CI, AWS CodePipeline).  
* **Fail Builds Early:** Configure automated build policies to reject, block, or fail deployment runs when **Critical** or **High** vulnerabilities are detected.  
* **Base Image Lifecycles:** Prevent the deployment of "black box" legacy dependencies. Consistently evaluate and upgrade base environments to slim, hardened images like *Alpine* or *Distroless*.

