# [**Circle10 Study App**](https://jeffreyyyyo.github.io/Circle10_v2/)

**Circle10** is a lightweight, mobile-first, and highly optimized web application designed to help Cybersecurity students at AltSchool Africa access course materials, lecture transcripts, and test quizzes on-the-go, unbound by the limitations of location, resources, or bandwidth.

<img width="2000" height="1256" alt="1" src="https://github.com/user-attachments/assets/3d88ef12-7fe2-4726-9e90-5baa7bce5af0" />

*From left to right: **1\.** The minimalist and welcoming frontpage of the Circle10 Study App, inviting students to challenge themselves today. **2\.** The main lobby interface providing structured, quick access to Notes, Decks, Tests, and Miscellaneous study materials. **3\.** The Notes Lobby organized by semester and month, making it easy to locate specific domains like Network Security or Cryptography.*

## **The Backstory: From Problem to Solution**

As a Cybersecurity student at AltSchool Africa (Karatu 2025 cohort), I noticed a recurring challenge: the course's Learning Management System (LMS) relied heavily on video lectures. This made studying incredibly difficult when away from a desk or in areas with limited data and network connectivity. Scrubbing through hours of video just to find a specific concept was tedious, and many of my classmates voiced similar frustrations during town hall meetings.

**Iteration 1: "Panels" (July 2025\)** My first solution was manual. I began transcribing the weekly lecture videos and sharing them with my colleagues. Shortly after, I built "Panels"—the very first iteration of this app. It was a basic HTML application that hosted test quizzes for the first two weeks of material. As schoolwork intensified, I paused app development but continued releasing the weekly transcripts.

**Iteration 2: Circle10 (October 2025 \- Present)** I decided to start from scratch, embarking on a self-taught journey through UI/UX design, database management, software architecture, and resource management. Inspired by my background in Architecture, I adopted a philosophy of **Minimalist Modernism**. I prioritized core functionality while engineering the system architecture to severely reduce bloat, weight, and management complexity.

Today, Circle10 serves as a comprehensive study companion that not only helps my current cohort but will remain a valuable resource for future learners.

<img width="2000" height="1256" alt="2" src="https://github.com/user-attachments/assets/11156fba-6aad-4832-a6ec-7aadc59b3fa1" />

*From left to right: **1\.** The beautifully formatted Notes Reader page, parsing markdown files dynamically into a clean, highly readable layout.* **2\.** A seamless Dark Mode experience on the Notes Reader page, fully optimized for comfortable late-night study sessions without straining the eyes. **3\.** The Tests Lobby where users can select specific cybersecurity modules to put their knowledge base to the test.

## **Features**

* **Offline-Friendly Architecture:** By migrating away from heavy video files to structured text (Markdown) and local JSON data, the app uses a fraction of the bandwidth, making it accessible anywhere.  
* **Rich Markdown Reader:** Seamlessly read through domain-specific notes, live-class deck transcriptions, lab guides, and official checklists. Includes a dynamically generated Table of Contents and a **Dark Mode** toggle for late-night studying.  
* **Customizable Quizzes:** Test your knowledge with dynamically loaded question pools. Choose your format:  
  * *Mini Quiz* (5 questions / 2 mins)  
  * *Pop Quiz* (10 questions / 5 mins)  
  * *Test* (15 questions / 10 mins)  
  * *Exam* (40 questions / 30 mins)  
  * *Surfing Mode* (Unlimited time and questions)

<img width="2000" height="1256" alt="3" src="https://github.com/user-attachments/assets/3a9381a0-75cf-4b34-b944-db4b8fb36d2d" />

*From left to right: **1\.** The Test Configuration Page allowing students to tailor their assessment format, from a quick Mini Quiz to a full Exam or an unlimited Surfing Mode.* **2\.** *A clean, distraction-free test screen interface focusing entirely on the current question alongside a built-in timer.* **3\.** *The final Test Score Screen delivering instant performance metrics, including percentage scores, correct answers tally, and total time taken.*

* **Performance Tracking:** Get immediate post-quiz breakdowns, showing exactly what you got right, what you missed, and the correct answers.  
* **Distraction-Free UI:** A fluid, app-like mobile experience with smooth screen transitions, fullscreen toggles, and zero image bloat (exclusively using SVGs and CSS).

<img width="2000" height="1256" alt="4" src="https://github.com/user-attachments/assets/2e26c494-ee1a-417f-ab64-2fe38008cfe0" />

*From left to right: **1\.** The Your Performance Page showing students the details of their test performance, providing an opportunity for review.* **2\.** *The Decks Lobby where users can select the class decks sorted by Cybersecurity modules.* **3\.** *The Miscellaneous Lobby where students can access documented steps for Cybersecurity labs, guides for project execution, and other helpful school and learning materials.*

## **Technical Architecture & Design Philosophy**

### **Minimalist Modernism**

The design eliminates unnecessary weight. Instead of using external image files for icons, the app utilizes inline SVGs. The UI is built with **Tailwind CSS** to maintain a clean, modern aesthetic without the overhead of heavy component libraries.

### **The Migration from Firebase to GitHub**

Initially, Circle10 used Firebase to host external dependencies. This taught me a lot about API keys, Node.js, OOP, and database security (a lesson underscored by the infamous 'Tea' app data breach). However, to prioritize ease of management and rapid content updates, I migrated the entire stack to GitHub.

### **Data Management**

The app functions as a Single Page Application (SPA) that fetches its content dynamically. New weeks of learning transcripts, test question pools, cybersecurity labs, and project guides can be added simply by dropping `.md` or `.json` files into the local `/data/` directory—requiring zero changes to the core HTML engine.

* **Core Engine:** Pure HTML, CSS (Tailwind), and Vanilla JavaScript.  
* **Content Rendering:** [Marked.js](https://marked.js.org/) for parsing Markdown files into HTML.  
* **Hosting:** GitHub Pages.

## **Impact**

As of December 2025, Circle10 has integrated Google Analytics to measure usage and reach. The platform has successfully supported **133 unique users across 9 different countries**, proving its value as a decentralized, low-bandwidth educational tool.

*Built with love for the AltSchool Africa Cybersecurity Cohort.*

[Try it out here!](https://jeffreyyyyo.github.io/Circle10_v2/)
