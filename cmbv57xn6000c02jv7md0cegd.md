---
title: "Building a Tambola Game with Amazon Q CLI: My AI-Powered Coding Adventure"
datePublished: Fri Jun 13 2025 18:30:35 GMT+0000 (Coordinated Universal Time)
cuid: cmbv57xn6000c02jv7md0cegd
slug: building-a-tambola-game-with-amazon-q-cli-my-ai-powered-coding-adventure
tags: aws, amazon-q-developer-cli, amazonqcli

---

## **🃏 What I Built: Bringing Tambola (Housie) to Life with AI**

As part of the “Build Games with Amazon Q CLI and score a T-shirt 🏆👕” challenge, I set out to digitize Tambola—a game that’s a staple at Indian gatherings and loved by players of all ages. My goal: make a fun, replayable Tambola game in Python, powered by Amazon Q CLI.

Here’s what my Tambola game delivers:

* **Unique Ticket Generator:**  
    Every player gets a randomly generated, valid Tambola ticket—no two are the same!
    
* **Automated Caller:**  
    Numbers from 1 to 90 are drawn randomly and announced, just like in a real game.
    
* **Simple User Interface:**  
    Players can easily mark off called numbers on their tickets.
    
* **Prize Pattern Detection:**  
    The game checks for classic wins—top line, middle line, bottom line, and full house.
    
* **Scoreboard:**  
    Tracks which player claimed which prize, updating in real time.
    
* **Endless Fun:**  
    Players can keep playing or start a new game with a single click.
    

All these features are organized into neat, modular files—ticket generation, number calling, and prize checking—thanks to Amazon Q CLI’s step-by-step guidance.

---

## **🚀 Why This Guide?**

If you’re curious about what it’s like to build a culturally relevant game with the help of AI, or if you want to see how Amazon Q CLI can turbocharge your coding workflow, this blog is for you.  
I’ll share my process, what worked, what I learned, and where Q CLI still has room to grow.

---

## **🛠️ Step 1: Installing Amazon Q CLI (Quick Start)**

## **On Windows (using WSL for best compatibility):**

1. **Enable WSL:**
    
    ```bash
    wsl --install
    ```
    
    Restart your computer if prompted.
    
2. **Launch Ubuntu:**
    
    ```bash
    wsl -d Ubuntu
    ```
    
3. **Update your packages:**
    
    ```bash
    sudo apt update && sudo apt upgrade -y
    sudo apt install curl unzip
    ```
    
4. **Download and install Amazon Q CLI:**
    
    ```bash
    curl --proto '=https' --tlsv1.2 -sSf https://desktop-release.codewhisperer.us-east-1.amazonaws.com/latest/q-x86_64-linux-musl.zip -o q.zip
    unzip q.zip
    cd q
    chmod +x install.sh
    ./install.sh
    ```
    
5. **Authenticate:**
    
    ```bash
    q login
    ```
    
    Follow the browser prompt to log in with your AWS Builder ID.
    

> ***Tip: On Mac or Linux, just skip the WSL part—the rest is the same!***

---

## **🧠 Step 2: Planning Tambola with Amazon Q CLI**

Before writing code, I let Q CLI help me break down the project:

* **Prompt:**
    
    ```bash
    q chat
    ```
    
    > ***“Help me design a modular Tambola (Housie) game in Python. What files and classes should I create?”***
    
* **Suggested Structure:**
    
    ```typescript
    texttambola/
      ├── ticket.py         # Ticket generation logic
      ├── caller.py         # Number drawing and announcing
      ├── prize_checker.py  # Prize pattern detection
      ├── scoreboard.py     # Prize tracking and display
      ├── game.py           # Game loop and UI
      └── README.md         # Game rules and setup
    ```
    

**Why modular?**  
It keeps your code organized, testable, and easy to expand—plus, Q CLI excels at helping you break things down.

---

## **🏗️ Step 3: Building Tambola, Module by Module**

Here’s how I built each part, with Q CLI as my coding partner:

## **A. Ticket Generator (**[`ticket.py`](http://ticket.py)**)**

* **Prompt:**
    
    > ***“Write a Python function to generate a unique Tambola ticket for each player.”***
    
* **Result:**  
    A function that creates a valid 3x9 Tambola ticket with no repeats.
    

---

## **B. Automated Caller (**[`caller.py`](http://caller.py)**)**

* **Prompt:**
    
    > ***“Create a module that randomly draws numbers from 1 to 90 and announces them.”***
    
* **Result:**  
    An automated caller that ensures no number is called twice.
    

---

## **C. User Interface & Marking (**[`game.py`](http://game.py)**)**

* **Prompt:**
    
    > ***“Add a simple UI so players can mark off called numbers on their ticket.”***
    
* **Result:**  
    A clear, interactive interface for marking tickets and viewing progress.
    

---

## **D. Prize Pattern Detection (**`prize_`[`checker.py`](http://checker.py)**)**

* **Prompt:**
    
    > ***“Write logic to check for top line, middle line, bottom line, and full house wins.”***
    
* **Result:**  
    Instant feedback when a player claims a prize.
    

---

## **E. Scoreboard (**[`scoreboard.py`](http://scoreboard.py)**)**

* **Prompt:**
    
    > ***“Track and display which player has won which prize.”***
    
* **Result:**  
    A live scoreboard updating after every win.
    

---

## **F. Game Loop and Replayability**

* **Prompt:**
    
    > ***“Let players keep playing or start a new game after one finishes.”***
    
* **Result:**  
    Endless Tambola fun—no need to restart the program!
    

---

## **🧩 Step 4: Integrating Everything**

While Q CLI is great at generating modules, I sometimes had to manually connect them—especially when linking ticket generation with the caller and scoreboard.  
**Tip:**

> ***“Ask Q CLI for integration help, or paste snippets from different modules to give it more context.”***

---

## **🔍 Step 5: Testing, Debugging, and Iterating**

* **Manual Testing:**  
    I played through several games, checked all prize patterns, and made sure the scoreboard updated correctly.
    
* **Debugging:**  
    When issues popped up (like prize detection edge cases), I asked Q CLI for debugging tips and fixes.
    
* **Iterative Improvement:**  
    I refined prompts to add features like a new game button, clearer UI, and more robust error handling.
    

---

## **🌟 What Makes Amazon Q CLI Stand Out**

* **Step-by-step guidance:**  
    Q CLI helped me break the project into manageable chunks.
    
* **Rapid prototyping:**  
    I could quickly try new features and see results.
    
* **Modular thinking:**  
    Q CLI nudged me to keep code clean and organized.
    
* **Creative freedom:**  
    Adding new features—like a live scoreboard—was a breeze.
    

---

## **🛠️ Where Amazon Q CLI Can Improve**

* **Context awareness:**  
    Sometimes, Q CLI forgot earlier code or context. I got better results by pasting relevant code into my prompts.
    
* **Testing support:**  
    Built-in test generation and debugging tools would make development even smoother.
    
* **Integrated output:**  
    A more project-aware output would help stitch modules together automatically.
    

---

## **💡 Final Takeaways**

**Amazon Q CLI is a fantastic tool for creative, rapid prototyping—especially for games that blend cultural fun with modern tech.**  
It’s not perfect, but it’s evolving fast. For hackathons, side projects, or learning, it’s a must-try.

---

## **🎲 Ready to Play?**

* **Try my Tambola game:** [Link to your repo](https://github.com/AditModi)  
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1749463260549/69d40dd6-1872-40b7-a7b0-d14977af8697.png align="center")
    
      
    
* **Join the challenge:** Build your own game, share your experience, and maybe win some swag!
    
* **Let’s connect:** Drop your questions, feedback, or your own Q CLI stories in the comments.
    

---

## **👕 Bonus: How to Win That T-shirt**

1. Build a game with Amazon Q CLI.
    
2. Share your experience (blog, social, or demo).
    
3. Submit your entry and claim your exclusive Amazon Q T-shirt!
    

---

**Thanks for reading! If this helped you, share it with a friend or your dev community. Happy coding!**