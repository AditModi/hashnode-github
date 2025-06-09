---
title: "🎮 Level Up Your Coding: Building a Texas Hold’em Game with Amazon Q CLI"
datePublished: Sat Jun 07 2025 18:30:00 GMT+0000 (Coordinated Universal Time)
cuid: cmbovohcr000g02ky5scs1wej
slug: level-up-your-coding-building-a-texas-holdem-game-with-amazon-q-cli
tags: aws, amazon-q-developer-cli

---

## **🃏 What I Built: Classic Texas Hold’em, Powered by AI**

As part of the “Build Games with Amazon Q CLI and score a T-shirt” challenge, I set out to create a fully playable, classic Texas Hold’em poker game in Python. Here’s what the game delivers:

* **A Real Deck of Cards:**  
    Shuffle and deal cards just like in a real casino, thanks to a dedicated deck module.
    
* **Hand Evaluator:**  
    Robust logic to rank poker hands—pairs, straights, flushes, and more—so every round feels authentic.
    
* **Betting Rounds:**  
    Experience all the action with Pre-flop, Flop, Turn, and River betting, where you and AI opponents can call, raise, or fold.
    
* **Four Unique AI Opponents:**  
    Each AI player has its own chip balance and decision-making strategy, making every game unpredictable and fun.
    
* **Continuous Gameplay:**  
    Play as many hands as you like—the game keeps going until you decide to walk away from the table.
    
* **Welcoming User Experience:**  
    The game greets you with a polished welcome screen and a clear explanation of the rules, so even beginners can jump right in.
    

**All of this was structured in a clean, modular project folder, with Amazon Q CLI guiding me every step of the way.**

---

## **🚀 Why This Guide?**

Ever wondered how fast you can turn an idea into a working game with the help of AI? I recently took on the “Build Games with Amazon Q CLI and score a T-shirt” challenge, and built a classic Texas Hold’em poker game—right from scratch—using Amazon Q CLI.  
In this deep-dive, I’ll walk you through every step: from setting up Amazon Q CLI, to architecting the game, to the real lessons I learned (and where Q CLI can do even better).

If you’re a developer, hobbyist, or just curious about the future of AI-powered coding, you’ll find actionable tips, honest feedback, and code insights here.

---

## **🛠️ Step 1: Installing Amazon Q CLI (The Right Way)**

**Why start here?**  
Because the right setup means you spend more time building and less time troubleshooting.

## **On Windows (using WSL for best compatibility):**

1. **Enable WSL (Windows Subsystem for Linux):**
    
    * Open PowerShell as Administrator:
        
        ```bash
        wsl --install
        ```
        
    * Restart your computer if prompted.
        
2. **Launch Ubuntu:**
    
    * Open the Ubuntu app from your Start Menu, or run:
        
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
    
    * Follow the browser prompt to log in with your AWS Builder ID.
        

> ***Pro Tip: If you’re on Mac or Linux, the steps are nearly identical—just skip the WSL part.***

---

## **🧠 Step 2: Planning the Game with Amazon Q CLI**

Before you write a single line of code, let the AI help you plan!

* **Prompt Q CLI:**
    
    ```bash
    q chat
    ```
    
    > ***“Help me design a modular Texas Hold’em poker game in Python. What files and classes should I create?”***
    
* **Suggested Structure:**
    
    ```typescript
    texttexas_holdem/
      ├── deck.py
      ├── hand_evaluator.py
      ├── player.py
      ├── game.py
      ├── ui.py
      └── README.md
    ```
    

**Why modular?**  
It keeps your code clean, testable, and easy to expand—plus, Q CLI is great at helping you break things down.

---

## **🏗️ Step 3: Building, One Module at a Time**

Here’s how I tackled each part, with Q CLI as my co-pilot:

## **A. The Deck (**[**deck.py**](http://deck.py)**)**

* **Prompt:**
    
    > ***“Write a Python class for a deck of cards that can shuffle and deal.”***
    
* **What I got:**  
    A ready-to-use `Deck` class with shuffle and deal methods.
    

---

## **B. Hand Evaluation (hand\_**[**evaluator.py**](http://evaluator.py)**)**

* **Prompt:**
    
    > ***“Create a function to rank poker hands for Texas Hold’em.”***
    
* **What I got:**  
    Logic to detect pairs, straights, flushes, and more—saving hours of manual coding.
    

---

## **C. Players and AI (**[**player.py**](http://player.py)**)**

* **Prompt:**
    
    > ***“Build classes for human and AI players, each with chip balances and betting logic.”***
    
* **What I got:**  
    A base player class, plus AI opponents with decision-making logic.
    

---

## **D. The Game Loop (**[**game.py**](http://game.py)**)**

* **Prompt:**
    
    > ***“Write the main game loop for Texas Hold’em, including betting rounds and winner determination.”***
    
* **What I got:**  
    A continuous gameplay loop—Pre-flop, Flop, Turn, River—where the action never stops until you say so.
    

---

## **E. User Interface (**[**ui.py**](http://ui.py)**)**

* **Prompt:**
    
    > ***“Add a welcome screen and explain the rules to the player.”***
    
* **What I got:**  
    A friendly, informative start screen that sets the tone for the game.
    

---

## **🧩 Step 4: Stitching It All Together**

While Q CLI excels at building modules, integrating them can be tricky. Here’s what worked for me:

* **Ask Q CLI for integration help:**
    
    > ***“How do I connect all these modules into a playable game?”***
    
* **Manually review and tweak:**  
    Sometimes, you’ll need to manually wire up imports and function calls—think of it as your final quality check.
    

---

## **🔍 Step 5: Testing, Debugging, and Iterating**

* **Manual playtesting:**  
    Run the game, try different scenarios, and note where things break.
    
* **Ask Q CLI for help:**
    
    > ***“I’m getting an error in the betting round. Can you help me debug?”***
    
* **Write unit tests:**  
    Q CLI can generate test cases if you ask, but you’ll still want to run them and interpret results.
    

---

## **🌟 What Makes Amazon Q CLI Stand Out**

* **Step-by-step guidance:**  
    It’s like pair programming with an expert.
    
* **Rapid prototyping:**  
    Instantly see your ideas come to life.
    
* **Modular thinking:**  
    Q CLI nudges you toward best practices.
    
* **Creative freedom:**  
    Want to add a new AI opponent? Just ask!
    

---

## **🛠️ Where Amazon Q CLI Can Improve**

* **Context carryover:**  
    Sometimes, Q CLI forgets previous code or context. Remind it by pasting relevant snippets.
    
* **Testing support:**  
    Automated debugging and test integration would be a game-changer.
    
* **Integrated output:**  
    A more cohesive, project-aware output would save time on integration.
    

---

## **💡 Final Takeaways**

**Amazon Q CLI is a fantastic tool for rapid prototyping, learning, and creative coding.**  
It’s not perfect—yet—but it’s evolving fast. For hackathons, side projects, or just exploring new ideas, it’s a must-try.

---

## **🎲 Ready to Play?**

* **Try my Texas Hold’em game:** [Link to your repo](https://github.com/AditModi)  
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1749460572986/d3358f4a-f6dd-4182-9efe-9c2b44f7eff0.png align="center")
    
      
      
    
* **Join the challenge:** Build your own game, share your experience, and maybe win some swag!
    
* **Let’s connect:** Drop your questions, feedback, or your own Q CLI stories in the comments.
    

---

## **👕 Bonus: How to Win That T-shirt**

1. Build a game with Amazon Q CLI.
    
2. Share your experience (blog, social, or demo).
    
3. Submit your entry and claim your exclusive Amazon Q T-shirt!
    

---

**Thanks for reading! If this helped you, share it with a friend or your dev community. Happy coding!**