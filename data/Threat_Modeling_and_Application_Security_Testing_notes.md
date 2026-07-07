# **CYS 532—WEEK 5**

## **THREAT MODELING AND APPLICATION SECURITY TESTING**

1. # **Introduction**

## **Introduction to Threat Modeling and Application Security Testing**

As software becomes increasingly critical to everything we do—powering banking apps, healthcare systems, e-commerce platforms, and smart home devices—our digital footprint grows. Consequently, the potential attack surface for cybercriminals expands alongside it.

Unfortunately, in many development environments, security is still treated as an afterthought—something tacked onto the end of the software development lifecycle. This reactive approach often allows vulnerabilities to slip through the cracks, exposing organizations to significant risks and sometimes irreparable damage.

Application security is not a "nice-to-have"; it is an essential component of any serious engineering process.

## **Moving from Reactive to Proactive Security**

To shift from a reactive posture to a proactive one, engineering teams must embrace **Threat Modeling** and **Application Security Testing**. These practices shift the focus from fixing issues late in the development game to preventing them from occurring in the first place.

### **What is Threat Modeling?**

Think of threat modeling as an early warning system. It is a structured way of thinking through how a system could be attacked *before* it is even built. The process starts by asking simple but powerful questions:

1. **What are we building?**  
2. **What could go wrong?**  
3. **What are we doing to defend against those risks?**  
4. **How well are we doing it?**

**Example Scenario:** Imagine designing a web application that handles sensitive user data, like credit card information. Before writing a single line of code, threat modeling allows your team to sketch out the architecture, identify where data flows, where it is stored, and where the weak spots might be.

You might realize that an API endpoint is publicly exposed or that session data isn't being encrypted. Armed with this knowledge, you can implement safeguards before the system goes live—saving time, reducing costs, and protecting the company's reputation.

## **Application Security Testing**

In addition to threat modeling, teams need ways to validate that their security assumptions actually hold up. This validation is achieved through various types of Application Security Testing, each designed for different stages of development:

* **SAST (Static Application Security Testing):** Analyzes your application's source code without executing it to find structural flaws and vulnerabilities.  
* **DAST (Dynamic Application Security Testing):** Evaluates your application while it is running. It simulates real-world attacks on a live system to uncover runtime issues, such as broken authentication or improper error handling.  
* **Penetration Testing:** Mimics real-world attacks to assess how well an application holds up under pressure. This is akin to hiring an ethical hacker to try and break into your system before a malicious actor does.

## **DevSecOps and the Secure SDLC**

In fast-paced development environments, there is a major shift toward integrating security directly into the software development process.

This shift is known as **DevSecOps**—a cultural and technical movement that embeds security into every phase of development, from planning and coding to testing and deployment. It is no longer enough to just build software fast; we must build it securely, continuously, and collaboratively.

This philosophy is the foundation of the **Secure Software Development Lifecycle (Secure SDLC)**. By baking security controls into every stage, teams can catch issues early, reduce costs, and deliver trustworthy applications without compromising on speed.

## **Conclusion: Security as a Mindset**

Securing applications isn't just about tools or checklists; it is about mindset. It requires:

* Anticipating threats before they strike.  
* Testing continuously and learning constantly.  
* Owning security as a **shared responsibility** across the entire development team.

2. # **Threat Modelling**

## **Deep Dive into Threat Modeling**

Threat modeling is a strategic practice in secure software development. It is the process of proactively identifying, understanding, and prioritizing potential threats to an application before they can be exploited.

It provides a structured way of thinking like an attacker. Rather than waiting for a vulnerability to show up during testing—or worse, in production—threat modeling asks, *"What could go wrong?"* and pushes teams to design defenses early in the development process.

The core purpose of threat modeling is to predict and prepare for possible security issues in the application's architecture or logic long before a single line of code is written or deployed. By doing this, we significantly reduce the chances of those issues becoming real-world threats down the line.

**The Building Analogy:** If you are building a house, you don't wait until the final inspection to start thinking about safety. Instead, you plan for it from the start. You install secure locks, position your windows appropriately, and decide how the electrical and plumbing should run to avoid future problems. Threat modeling does the same for software—it provides architectural foresight for cybersecurity.

## **Why Threat Modeling Matters**

Imagine a financial web application that allows users to transfer money. Applying threat modeling early in development prompts critical questions:

* *What if a user tries to access someone else's account?*  
* *What happens if the session times out?*  
* *Could an attacker inject malicious scripts into the transaction page?*

Based on those questions, teams can introduce controls like session management, input validation, and authorization checks *before* building the transaction feature. Modeling these potential threats at the design level allows action to be taken before vulnerabilities reach the user.

Most importantly, threat modeling fosters a **security-first mindset** across teams. Developers begin to think critically about risks and attack paths, making security a shared responsibility rather than an afterthought.

## **The Pillars of Effective Threat Modeling**

While the concept is simple, anticipating threats requires structure, clarity, and the ability to think like both a developer and an attacker. The process rests on three foundational pillars:

### **1\. Identifying Assets**

The first step is identifying the valuable components you want to protect. If you don't know what is valuable, you cannot defend it. Assets can include:

* User data  
* Authentication credentials  
* Payment information  
* Internal APIs  
* The application's reputation

### **2\. Identifying Entry Points**

Next, you must map the places where data enters or leaves the system. Entry points often represent the initial access vectors for attackers. Examples include:

* User login forms  
* APIs and Webhooks  
* Uploaded files  
* Third-party integrations

*Returning to the house analogy: If the application is a building, the entry points are the doors and windows. You need to know where they are and how they might be breached.*

### **3\. Understanding Attackers**

With assets and entry points mapped, turn your attention to potential attackers. Consider their motives, capabilities, and the techniques they might use. Attackers could be:

* External hackers trying to steal data.  
* Internal employees misusing access privileges.  
* Automated bots probing for weak points.

## **Creating Realistic Attack Scenarios**

Once you establish the assets, entry points, and potential attackers, the next phase is to create realistic, plausible attack scenarios where identified threats come to life.

**Example Scenario:** If an app allows file downloads, a realistic scenario might involve an attacker uploading a malicious file disguised as a resume, which then executes a script when opened by an admin. This creates a concrete scenario the team can test, plan for, and defend against.

Creating these scenarios moves teams from abstract worries to actionable insights, pushing the conversation toward mitigation strategies and concrete security controls.

## **Threat Modeling Tools: OWASP Threat Dragon**

To make this process structured, repeatable, and accessible in fast-paced DevSecOps environments, teams utilize specialized tools. One of the most widely recommended is **OWASP Threat Dragon**.

Threat Dragon is a free, open-source tool developed by the OWASP Foundation. It is designed to help developers and security teams visually model applications, identify potential threats, and document mitigation strategies in a single interface.

### **Creating a Threat Model with Threat Dragon**

1. **Create a New Project:** Name the application, select the environment (Dev, Staging, or Prod), and define the boundaries of the model (e.g., focusing on just a login module, a payment gateway, or the entire app).  
2. **Draw a Data Flow Diagram (DFD):** Use simple shapes to map out the system architecture (detailed below).  
3. **Define Threats Using STRIDE:** Threat Dragon will analyze the DFD and suggest threats based on the **STRIDE** model:  
   * **S**poofing  
   * **T**ampering  
   * **R**epudiation  
   * **I**nformation Disclosure  
   * **D**enial of Service  
   * **E**levation of Privilege  
4. **Document Mitigation Steps:** For every identified risk, describe how the application will handle or prevent it (e.g., via encryption, input validation, or monitoring).  
5. **Review and Share:** Export the model to share with the team or include in project documentation to maintain transparency and aid in compliance.

## **Understanding Data Flow Diagrams (DFDs)**

A Data Flow Diagram (DFD) is a simple visual representation of how data moves through a system. It helps teams understand system architecture in terms of how information is handled, where it originates, where it goes, and how it is processed. This is crucial because attackers often exploit gaps where data crosses trust boundaries, interacts with external systems, or is stored insecurely.

### **The Four Basic Components of a DFD:**

1. **External Entities:** Users or external systems interacting with the application (e.g., a customer or a 3rd-party payment service).  
2. **Processes:** Functions or logic within the application that handle or transform data (e.g., a login handler, order processing).  
3. **Data Stores:** Repositories where data is held (e.g., databases, file systems, caches).  
4. **Data Flows:** Arrows showing how data moves between the other elements. Every data flow has a source and a destination.

By mapping architecture this way, teams can ask targeted questions: *Is data validated before processing? Is sensitive information encrypted in transit? Where are the trust boundaries?*

## **Analyzing Attack Vectors**

The final step in the threat modeling process is analyzing attack vectors. An attack vector is a path or means by which an attacker gains access to a system to carry out malicious activities.

To analyze vectors, step into the attacker's shoes and ask:

* What is the attacker's goal?  
* What assets are they trying to reach?  
* What entry points are available?  
* How might they chain together system weaknesses to reach their goal?

For example, an attacker might use SQL injection in a login form to access a database, exploit a misconfigured API to retrieve records, or use insecure file uploads to execute malicious scripts. Each is a potential attack vector tracing a path through the DFD.

### **Assessing Risk: Likelihood vs. Impact**

Once potential vectors are identified, you must estimate the **likelihood** of the attack (how easy is it to execute?) and the **impact** if it were successful (would it cause financial damage or harm to users?).

A low-likelihood, low-impact vector may not need urgent attention, but a high-likelihood, high-impact vector becomes a top priority. This risk-based approach ensures mitigation efforts are focused where they matter most.

3. # **Penetration Testing**

## **Penetration Testing and the OWASP Top 10**

Penetration testing, often referred to as "pen testing," is one of the most hands-on forms of application security assessment. It is the practice of simulating real-world attacks on a system, application, or network to uncover vulnerabilities that malicious actors could exploit.

Unlike automated scanners or code analyzers, pen testing combines manual techniques, automated tools, and an attacker's mindset to dig deep into security flaws.

## **Approaches to Penetration Testing**

There are two major approaches to penetration testing, and effective security assessments typically utilize a combination of both.

### **1\. Automated Penetration Testing**

These are tool-driven assessments that quickly scan and probe an application for known vulnerabilities.

* **Purpose:** Tools like Burp Suite Pro, Nessus, and Metasploit can automate parts of the process, saving time and offering broad coverage.  
* **Strengths:** Automation is excellent for identifying "low-hanging fruit," such as outdated software versions, open ports, or missing security headers.  
* **Limitations:** Automated testing can only go so far and often misses complex, multi-step exploits or business logic errors.

### **2\. Manual Penetration Testing**

Manual pen testing is performed by skilled ethical hackers—professionals who think like attackers but act ethically.

* **Purpose:** Testers go beyond surface-level checks, leveraging creativity and domain knowledge.  
* **Strengths:** They identify business logic flaws, chain multiple exploits together, and find privilege escalation opportunities.  
* **Example:** An automated tool might detect a login form vulnerable to SQL injection. A manual tester, however, could then exploit that vulnerability to extract an entire user database, bypass authentication, or escalate privileges—demonstrating the true risk in a way tools cannot fully replicate.

Penetration testing is ultimately about **validation**—verifying that security controls actually work as expected. It is not just about finding bugs; it is about proving the real-world impact those bugs represent. Furthermore, pen testing is not a one-time checkbox. For applications in active development or those exposed to the internet, regular testing is essential to keep pace with evolving threats.

## **Identifying the OWASP Top 10 Vulnerabilities**

When conducting penetration testing on web applications, one of the most important guiding frameworks is the OWASP Top 10\.

**What is the OWASP Top 10?** It is a globally recognized list of the top ten most critical web application security risks. Published and maintained by the Open Worldwide Application Security Project (OWASP), this list is based on data from real-world incidents, industry research, and expert consensus. It is updated periodically to reflect the evolving threat landscape.

### **Notable Vulnerabilities in the OWASP Top 10**

* **Injection Attacks:** Includes SQL injection, command injection, etc. These occur when untrusted data is sent to an interpreter, tricking it into executing unintended commands.  
* **Broken Authentication:** Weak or poorly implemented authentication mechanisms allow attackers to compromise passwords, session tokens, or exploit flaws to impersonate users.  
* **Sensitive Data Exposure:** When data like credit card numbers, passwords, or personal information is not properly protected or encrypted, it can be exposed.  
* **Security Misconfigurations:** Default settings, open cloud storage, outdated libraries, and exposed error messages can be exploited even on otherwise secure systems.  
* **Cross-Site Scripting (XSS):** Occurs when attackers inject malicious scripts into trusted websites, affecting users who visit those sites.  
* **Broken Access Control:** Attackers gain access to data or functionality they shouldn't possess due to insufficient access restrictions.  
* **Insecure Design & Vulnerable/Outdated Components:** Highlighted in recent versions, these remind us that security must be embedded in the design phase and components must be kept up to date.

**Why the OWASP Top 10 Matters:** These vulnerabilities represent the most common and impactful weaknesses seen in real applications. They act as a roadmap for security testers, ensuring assessments cover the most likely entry points. Additionally, many compliance frameworks and secure coding standards require adherence to OWASP principles. As a penetration tester, your job is to recognize these vulnerabilities, actively test for them, and work clearly with developers on remediation.

## **Penetration Testing Tools**

Two of the most powerful and widely used tools in the penetration testing space are Metasploit and Kali Linux.

### **Metasploit Framework**

Metasploit is an advanced, open-source penetration testing framework that allows security professionals to develop, test, and execute exploits against known vulnerabilities.

* **Functionality:** It acts as an offensive testing lab, coming with a wide range of payloads, exploits, post-exploitation tools, and scanners.  
* **Modular Architecture:** You can scan for vulnerabilities using integrated tools, choose and configure exploit modules, launch attacks, and automate complex attack chains using scripting.  
* **Ethical Use:** It helps verify vulnerabilities in a controlled manner, which is critical for responsible disclosure and remediation planning.

### **Kali Linux**

Kali Linux is a complete penetration testing platform (a Linux distribution) developed and maintained by Offensive Security.

* **Pre-installed Tools:** Kali comes with hundreds of tools ready to use for:  
  * Network scanning (e.g., Nmap)  
  * Web application testing (e.g., Burp Suite)  
  * Wireless attacks  
  * Exploitation  
  * Forensics  
* **Advantage:** The primary strength is its ready-to-use environment. Testers don't have to waste time configuring tools from scratch. It works well with virtual machines and cloud environments, making it versatile for red teaming, Capture the Flag (CTF) events, and professional assessments.

Together, Metasploit and Kali Linux form the backbone of many professional pen testers' toolkits.

## **A Real-World Penetration Testing Scenario**

To bring this to life, imagine a penetration test on a FinTech web application that manages user accounts, processes payments, and provides dashboards. The goal is to identify vulnerabilities that could lead to unauthorized access or data leakage.

Here is a typical methodology for how the test might play out:

1. **Reconnaissance:** Gathering information about the application—domain names, IP addresses, open ports, and technologies used (passive and active footprinting).  
2. **Scanning and Enumeration:** Using tools like Nmap and Nikto to detect services and look for misconfigurations or outdated components.  
3. **Exploitation:** Attempting to exploit common vulnerabilities. Examples include SQL injection in login forms, Cross-Site Scripting in search fields, or Insecure Direct Object References (IDOR) in user account URLs.  
4. **Simulated Attacks:** Utilizing tools like Burp Suite, OWASP ZAP, and Metasploit to perform deeper exploitation.  
5. **Privilege Escalation:** If initial access is gained, testing whether a normal user can escalate their rights to admin level or extract sensitive backend data.  
6. **Maintaining Access:** Mimicking attacker behavior to stay hidden, demonstrating the impact of poor detection mechanisms.  
7. **Post-Test (Reporting and Mitigation):** The test doesn't end with exploitation; the most valuable deliverable is the final report.

### **The Penetration Test Report**

The report typically includes:

1. A summary of discovered vulnerabilities.  
2. Screenshots or logs proving exploitation.  
3. A risk rating for each issue.  
4. Actionable mitigation strategies.

**Example Mitigations:**

* *If SQL injection is found:* Recommend input validation, parameterized queries, and updating database libraries.  
* *If weak session tokens are discovered:* Recommend stronger randomness, HTTPS enforcement, and strict session timeout policies.

Following the report, a debrief session with the development or security team ensures clear next steps. Real-world penetration testing isn't just about showing what is broken; it is about improving resilience before real attackers get the chance.

## **Module Conclusion: The Big Picture**

In a world where applications are prime targets for cyberattacks, building secure software requires proactive design and continuous testing.

* **Threat Modeling** helps teams stay ahead of risks by systematically identifying and prioritizing threats before they can be exploited, often utilizing visual aids like Data Flow Diagrams (DFDs) in tools like Threat Dragon.  
* **Penetration Testing** allows security professionals to dig deeper, finding and exploiting weaknesses—like those highlighted in the OWASP Top 10—using robust tools like Kali Linux and Metasploit.

Effective application security demands a blend of strategic threat modeling, automated testing (SAST/DAST), and practical penetration testing. Together, these practices shift security "left" in the development lifecycle, reducing risk, increasing resilience, and ensuring that secure code is an everyday reality.