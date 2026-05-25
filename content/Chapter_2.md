---
title: "Chapter 2: The Keys to the Castle (Access & Security)"
nav_order: 2
---

# Chapter 2: The Keys to the Castle (Access & Security)

When you have your LUMI project, account and computational resources ("compute") from Chapter 1, it is time to actually step inside. Unlike logging into a website with a username and password, supercomputers use a much more secure "handshake" method called SSH Keys.

If this sounds like secret agent territory, don't worry—it is simply a more robust way to prove who you are without sending a password over the internet where it could be intercepted.


## 🔑 What are SSH Keys? (The Lock and the Key)

Think of an SSH key pair like a physical lock and its matching key, but digital:
- **The Private Key (Your Key):** This stays on your laptop. You should never share it, email it, or move it. It is your digital identity.
- **The Public Key (The Lock):** You upload this to the LUMI user portal. It acts as a "lock" that only your specific Private Key can open.

> [!note] Why don't we just use passwords?
> Passwords can be guessed, stolen via "phishing," or intercepted. A cryptographic SSH key is virtually impossible to crack. Because LUMI is a billion-euro piece of infrastructure, we take this security very seriously to protect both the machine and your proprietary company data.


## 🛠️ Step 1: Generating Your Keys
Before you can log in, you need to create this "Key and Lock" pair on your own computer. This process takes about two minutes and usually involves typing one command into your computer's terminal.

The exact command and steps vary slightly depending on whether you are using Windows, macOS, or Linux:

[👉 Follow the Official guide to create and upload your SSH keys](https://docs.lumi-supercomputer.eu/firststeps/SSH-keys/)

> [!note]
> When the guide asks you for a "passphrase," choose something you can remember. This adds a second layer of security: even if someone stole your laptop, they couldn't use your key without that phrase.

Make sure that you have uploaded your public key to the correct portal as described in the guide before you proceed.


## 🚪 Step 2: Logging into LUMI
Once your "Lock" (Public Key) is uploaded to the portal, you are ready to enter the "Lobby" of the supercomputer. We do this by "SSHing" into the system.

### Option A: The Professional Way (Terminal)

This is how most AI developers work. You open your terminal and type a command to "call" LUMI. Once the connection is established, your terminal window is no longer talking to your laptop; it is talking directly to LUMI.

[👉 How to Log In via Terminal (SSH Client)](https://docs.lumi-supercomputer.eu/firststeps/loggingin/)

When you type the command to log in, it will look something like this: `ssh your_lumi_username@lumi.csc.fi`.

**Don't Panic: The First-Time Warning**

The very first time you connect, you will see a message asking if you want to continue connecting. Type yes and press Enter. This is just your computer "shaking hands" with LUMI for the first time.

**Where is my username?**

Your LUMI username is assigned to you when your account is created. It is usually a short string of characters (e.g., stubbale). You can find your 8-character username on https://www.lumi.csc.fi in the top right corner: "Logged in as `username`".

**The Silent Passphrase**

When you are asked for your passphrase, you won't see any characters appear as you type (not even stars). This is a security feature. Just type your phrase and hit Enter!

> [!note]
> When you are done with your session, simply type `exit` in the terminal or press "ctrl+d" to securely disconnect from LUMI.

![Logging into LUMI in terminal](./assets/LUMI_logged_in_terminal.png)

### Option B: The Web Interface (Browser-based)

If you are on a restricted work laptop where you cannot configure SSH, or if you just want a more familiar visual environment to start, LUMI offers a web-based portal. This includes a "Web Terminal" that runs directly in your browser. It behaves the same as the SSH terminal for all subsequent chapters.

[👉 Accessing LUMI via the Web Interface](https://docs.lumi-supercomputer.eu/firststeps/loggingin-webui/)

![Logging into LUMI command line via web interface](./assets/LUMI_logged_in_browser.png)


## 🛡️ Staying Secure

As an industry partner, your code and data are your most valuable assets. To keep them safe:
- Never share your Private Key. With anyone.
- Use a Passphrase. It’s the 'password' for your digital key.
- One Key per Device. If you work from both a desktop and a laptop, generate a new key pair on each device and upload each public key to the portal.


## ✅ Summary Checklist
- You have generated an SSH key pair on your computer.
- You have uploaded your Public Key to the LUMI user portal.
- You have successfully logged in and saw the "LUMI" welcome text in your terminal.
