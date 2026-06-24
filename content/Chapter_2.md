---
title: "Chapter 2: The Keys to the Castle (Access & Security)"
nav_order: 2
---

# Chapter 2: The Keys to the Castle (Access & Security)

When you have your LUMI project, account and computational resources ("compute") from Chapter 1, it is time to actually step inside. Unlike logging into a website with a username and password, supercomputers use a much more secure "handshake" method called SSH% Keys.

If this sounds like secret agent territory, don't worry—it is simply a more robust way to prove who you are without sending a password over the internet where it could be intercepted.


## 🔑 What are SSH Keys? (The Lock and the Key)

Think of an SSH key pair like a physical lock and its matching key, but digital:
- **The Private Key (Your Key):** This stays on your laptop. You should never share it, email it, or move it. It is your digital identity.
- **The Public Key (The Lock):** You upload this to the LUMI user portal. It acts as a "lock" that only your specific Private Key can open.

> [!note] Why don't we just use passwords?
> Passwords can be guessed, stolen via "phishing%", or intercepted. A cryptographic% SSH key is virtually impossible to crack. Because LUMI is a billion-euro piece of infrastructure, we take this security very seriously to protect both the machine and your data.


## 🛠️ Step 1: Generating Your Keys
Before you can log in, you need to create this "Key and Lock" pair on your own computer. This process is quick and usually involves typing one command into your computer's Terminal%.

The exact command and steps vary slightly depending on whether you are using Windows, macOS, or Linux:

[👉 Follow the Official guide to create and upload your SSH keys](https://docs.lumi-supercomputer.eu/firststeps/SSH-keys/)

> [!note] Choose a strong passphrase
> When the guide asks you for a "passphrase%," choose something you can remember. This adds a second layer of security: even if someone stole your laptop, they couldn't use your key without that phrase.

> [!tip] Using multiple devices
> If you use multiple computers (e.g., a laptop and a desktop), generate a new, separate key pair on each device. Uploading multiple public keys to the LUMI portal simply gives each device its own independent access — any of them will log you into the exact same LUMI account.

Make sure that you have uploaded your public key to the correct portal as described in the guide before you proceed.


## 🚪 Step 2: Logging into LUMI
Once your "Lock" (Public Key) is uploaded to the portal, you are ready to enter the "Lobby" of the supercomputer. We do this by "SSH%ing" into the system.

### Option A: The Professional Way (Terminal)

This is how AI developers typically work. You open your terminal and type a command to 'enter' LUMI. Once the connection is established, your terminal window is no longer talking to your laptop; it is talking directly to LUMI.

[👉 How to Log In via Terminal for the first time (SSH Client)](https://docs.lumi-supercomputer.eu/firststeps/loggingin/)

When you type the command to log in and hit enter, it will look something like this:
> [!command]
> ssh <your_lumi_username>@lumi.csc.fi

> [!note] Placeholder values
> In this guide, `<angle brackets>` indicate a placeholder — replace the entire thing, brackets included, with your own value. For example, `<your_lumi_username>` becomes `smithmar`.

If you prefer to copy and paste commands from this guide rather than typing them out, keep in mind that standard pasting shortcuts might not work in your terminal:
- **Windows / Linux:** Try **Ctrl+Shift+V** (or right-clicking) instead of the usual Ctrl+V.
- **Mac:** Standard **Cmd+V** usually works.

**Don't Panic: The First-Time Warning**

When you connect for the first time, you will be asked to check the host key fingerprint of the system and need to type `yes` in order to accept it. The fingerprints of the LUMI login nodes are listed in the table below. Please make sure that the host key fingerprint matches one of these:

| Key type | Fingerprint |
| :--- | :--- |
| ED25519 | `SHA256:qCFZThjRw8nf0CiZ9rU7b6Zirjq8slAIl5r0xWaVoD0` |
| RSA | `SHA256:ypbqdMWtk9ZdXEROkeEpv+3PCEXWjPLGI79IXGHe9ac` |
| ECDSA | `SHA256:hY4mnRCYb8bRchTnVcFo7SqoHHHEsUh9Ym38F4sHN1Y` |

**Where is my username and where do I upload my key?**

Because LUMI is a pan-European consortium, your account and public key are managed by the specific portal you used to gain access. This depends on where your project resources were granted from, rather than your nationality:
- **Projects granted via CSC (Finland):** You upload your SSH key and manage your account in your [MyCSC user profile](https://my.csc.fi/). You can also find your 8-character username (e.g., `smithmar`) by logging into [MyLUMI](https://www.lumi.csc.fi).
- **Projects granted via EuroHPC:** You manage your access through the EuroHPC Federation Platform (EFP). You can find detailed instructions for uploading your SSH key in the [EuroHPC AAI documentation](https://docs.my-eurohpc.eu/aai/aai/).
- **Projects granted via other Consortium countries:** Most other consortium countries manage their access and SSH keys through the [Puhuri portal](https://puhuri.io/).
- **Unsure where to go?** Please refer to the official [Access to LUMI guide](https://docs.lumi-supercomputer.eu/firststeps/accessLUMI/) to find the correct portal for your specific country or organisation.

**The Silent Passphrase**

When you are asked for your passphrase, you won't see any characters appear as you type (not even stars). This is a security feature. Just type your phrase and hit Enter!

> [!note] Secure disconnection
> When you are done with your session, simply type `exit` or press "Ctrl+D" to securely disconnect from LUMI.

![Logging into LUMI in terminal](./assets/LUMI_logged_in_terminal.png)

### Option B: The Web Interface (Browser-Based)

If you are on a restricted work laptop where you cannot configure SSH, or if you just want a more familiar visual environment to start, LUMI offers a web-based portal. This includes a "Web Terminal" that runs directly in your browser. It behaves the same as the SSH terminal for all subsequent chapters.

[👉 Accessing LUMI via the Web Interface](https://docs.lumi-supercomputer.eu/firststeps/loggingin-webui/)

Once you have logged into the web interface, click on the **Login node shell** button to open the Command Line% directly in your browser.

![Logging into LUMI Command Line via web interface](./assets/LUMI_logged_in_browser.png)


## 🛡️ Staying Secure

As an industry partner, your code and data are your most valuable assets. To keep them safe:
- Never share your Private Key. With anyone.
- Use a Passphrase. It’s the 'password' for your digital key.
- One Key per Device. If you work from both a desktop and a laptop, generate a new key pair on each device and upload each public key to the portal.
- If a device is lost or compromised, you should remove its public key from the portal.


## ✅ Summary Checklist
- You have generated an SSH key pair on your computer.
- You have uploaded your Public Key to the LUMI user portal.
- You have successfully logged in and saw the "LUMI" welcome text in your terminal.

## 📝 Knowledge Check

```quiz
title: Chapter 2 Quiz

Q: What is the difference between your Private Key and your Public Key?
- [ ] The Private Key is uploaded to LUMI, while the Public Key stays on your laptop.
- [ ] Both keys must be uploaded to the LUMI portal to work.
- [x] The Private Key stays on your laptop and must never be shared, while the Public Key is uploaded to LUMI.
- [ ] The Public Key acts as a password, and the Private Key is your username.
> Your Private Key is your secure digital identity and must remain solely on your machine. You only upload the Public Key (the "lock") to LUMI.

---

Q: When logging into LUMI via the terminal, what happens when you type your passphrase?
- [ ] The terminal displays asterisks (***) for each character.
- [ ] A graphical pop-up window appears.
- [x] Nothing appears on the screen as you type.
- [ ] The terminal skips the passphrase if your key is correct.
> To protect against screen recording or shoulder-surfing, the terminal will remain completely blank as you type your passphrase. This is a normal security feature!

---

Q: If you work from both a desktop computer and a laptop, how should you handle your SSH keys? (select all)
- [ ] Copy your single Private Key to both computers.
- [x] Generate a new, unique SSH key pair on each computer.
- [x] Upload both Public Keys to the LUMI user portal.
- [ ] You can only access LUMI from a single registered device.
> For maximum security, follow the "One Key per Device" rule. Generate a new pair on each device and upload both public keys to the portal.
```
