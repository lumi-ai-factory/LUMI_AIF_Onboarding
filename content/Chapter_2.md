---
title: "Chapter 2: The keys to the castle (access & security)"
nav_order: 2
---

# Chapter 2: The keys to the castle (access & security)

When you have your LUMI project, account and computational resources ("compute") from Chapter 1, it is time to actually step inside. Unlike logging into a website with a username and password, supercomputers use a much more secure "handshake" method called SSH% Keys.

If this sounds like secret agent territory, don't worry - it is simply a more robust way to prove who you are without sending a password over the internet where it could be intercepted.


## What are SSH Keys? (The lock and the key)

Think of an SSH Key Pair% like a physical lock and its matching key, but digital:
- **The Private Key% (Your key):** This stays on your laptop. You should never share it, email it, or move it. It is your digital identity.
- **The Public Key% (The lock):** You upload this to the LUMI user portal. It acts as a "lock" that only your specific Private Key can open.

> [!note] Why don't we just use passwords?
> Passwords can be guessed, stolen via "phishing%", or intercepted. A cryptographic% SSH Key is virtually impossible to crack. Because LUMI is a billion-euro piece of infrastructure, we take this security very seriously to protect both the machine and your data.


## Step 1: Generating your keys
Before you can log in, you need to create this "Key and Lock" pair on your own computer. This process is quick and usually involves typing one command into your computer's Terminal% (Windows users need to open "Windows PowerShell").

Before you generate your keys and follow the official guide, please read these important rules and details:
- **Unique keys:** You will be using the `ed25519` encryption algorithm. Both your Public and Private keys are completely unique to you. The file name is usually just `ed25519`, but the actual content of the files is unique to you.
- **File extensions:** The Public Key file name always ends in `.pub`, but the Private Key file name has no extension at all. You will need to know this when providing paths later.
- **Finding the paths:** The exact path where your keys are saved will be written on the screen during generation. There is no need to actually open and look inside either of these files.
- **Strong passphrase:** When the guide asks you for a "passphrase%," choose something you can remember. This adds a second layer of security: even if someone stole your laptop, they couldn't use your Key without that phrase.
- **The silent passphrase:** When you are asked for your passphrase in the Terminal, you won't see any characters appear as you type (not even stars). This is a normal security feature so that people who see your screen can't know even the number of characters in your passphrase. Just type your phrase and hit Enter!
- **Multiple devices:** If you use multiple computers (e.g., a laptop and a desktop), generate a new, separate Key Pair on each device. Uploading multiple Public Keys to the LUMI portal simply gives each device its own independent access - any of them will log you into the exact same LUMI account.
- **Placeholder values:** In this guide, `<angle brackets>` indicate a placeholder - replace the entire thing, **brackets included**, with your own value. Make sure to remove the `< >` brackets! For example, `<your_lumi_username>` becomes `smithmar`.

The exact command and steps vary slightly depending on whether you are using Windows, macOS, or Linux. Make sure to select the "From a terminal (all OS%)" tab in the guide below, even if you are on Windows.

[Follow the Official guide to create and upload your SSH Keys](https://docs.lumi-supercomputer.eu/firststeps/SSH-keys/)

Make sure that you have uploaded your Public Key to the correct portal as described in the guide before you proceed. Once uploaded, keep in mind that it might take a few hours for your public key to be registered on LUMI. Until then, attempting to log in might result in a `Permission denied (publickey)` error.


## Step 2: Logging into LUMI
Once your "Lock" (Public Key) is uploaded to the portal, you are ready to enter the "Lobby" of the supercomputer. We do this by "SSH%ing" into the system.

### Option A: The professional way (Terminal)

This is how AI developers typically work. You open your Terminal and type a command to 'enter' LUMI. Once the connection is established, your Terminal window is no longer talking to your laptop; it is talking directly to LUMI.

[How to Log In via Terminal for the first time (SSH Client)](https://docs.lumi-supercomputer.eu/firststeps/loggingin/)

To connect to LUMI, type the following command into your Terminal and hit enter:
> [!command]
> ssh -i <path_to_your_private_key> <your_lumi_username>@lumi.csc.fi

If you prefer to copy and paste commands from this guide rather than typing them out, keep in mind that standard pasting shortcuts might not work in your Terminal:
- **Windows / Linux:** Try **Ctrl+Shift+V** (or right-clicking) instead of the usual Ctrl+V.
- **Mac:** Standard **Cmd+V** usually works.

**Don't Panic: The First-Time Warning**

When you connect for the first time, you will be asked to check the host Key fingerprint% of the system and need to type `yes` in order to accept it. Before you do, please make sure that the fingerprint shown in your Terminal matches one of the up-to-date fingerprints of the LUMI Login Nodes listed in the [official login documentation](https://docs.lumi-supercomputer.eu/firststeps/loggingin/).

**Where is my username and where do I upload my Key?**

Because LUMI is a pan-European consortium, your account and Public Key are managed by the specific portal you used to gain access. This depends on where your project resources were granted from, rather than your nationality:
- **Projects granted via CSC (Finland):** You upload your SSH Key and manage your account in your [MyCSC user profile](https://my.csc.fi/). You can also find your 8-character username (e.g., `smithmar`) by logging into [MyLUMI](https://www.lumi.csc.fi).
- **Projects granted via EuroHPC:** You manage your access through the EuroHPC Federation Platform (EFP). You can find detailed instructions for uploading your SSH Key in the [EuroHPC AAI documentation](https://docs.my-eurohpc.eu/aai/aai/).
- **Projects granted via other Consortium countries:** Most other consortium countries manage their access and SSH Keys through the Puhuri portal.
- **Unsure where to go?** Please refer to the official [Access to LUMI guide](https://docs.lumi-supercomputer.eu/firststeps/accessLUMI/) to find the correct portal for your specific country or organisation.

This illustrates what happens if you've successfully logged into LUMI:

![Logging into LUMI in Terminal](./assets/LUMI_logged_in_terminal.png)

> [!note] Secure disconnection
> Once you have finished your work and want to exit LUMI (for example, after you complete Chapter 3), type `exit` and hit Enter or just press "Ctrl+D" to securely disconnect.

### Option B: The web interface (browser-based)

If you are on a restricted work laptop where you cannot configure SSH, or if you just want a more familiar visual environment to start, LUMI offers a web-based portal. This includes a "Web Terminal" that runs directly in your browser. It behaves the same as the SSH Terminal for all subsequent chapters.

[Accessing LUMI via the Web Interface](https://docs.lumi-supercomputer.eu/firststeps/loggingin-webui/)

Once you have logged into the web interface, click on the **Login Node Shell** button to open the Command Line% directly in your browser.

![Logging into LUMI Command Line via web interface](./assets/LUMI_logged_in_browser.png)


## Staying secure

As an industry partner, your code and data are your most valuable assets. To keep them safe:
- Never share your Private Key. With anyone.
- Use a passphrase. It’s the 'password' for your digital key.
- One Key per Device. If you work from both a desktop and a laptop, generate a new Key Pair on each device and upload each Public Key to the portal.
- If a device is lost or compromised, you should remove its Public Key from the portal.


## Summary checklist
- You have generated an SSH Key Pair% on your computer.
- You have uploaded your Public Key to the LUMI user portal.
- You have successfully logged in and saw the "LUMI" welcome text in your Terminal.

## Knowledge check

```quiz
title: Chapter 2 Quiz

Q: What is the difference between your Private Key and your Public Key?
- [ ] The Private Key is uploaded to LUMI, while the Public Key stays on your laptop.
- [ ] Both Keys must be uploaded to the LUMI portal to work.
- [x] The Private Key stays on your laptop and must never be shared, while the Public Key is uploaded to LUMI.
- [ ] The Public Key acts as a password, and the Private Key is your username.
> Your Private Key is your secure digital identity and must remain solely on your machine. You only upload the Public Key (the "lock") to LUMI.

---

Q: When logging into LUMI via the Terminal, what happens when you type your passphrase?
- [ ] The Terminal displays asterisks (***) for each character.
- [ ] A graphical pop-up window appears.
- [x] Nothing appears on the screen as you type.
- [ ] The Terminal skips the passphrase if your Key is correct.
> To protect against screen recording or shoulder-surfing, the Terminal will remain completely blank as you type your passphrase. This is a normal security feature!

---

Q: If you work from both a desktop computer and a laptop, how should you handle your SSH Keys?
- [ ] Copy your single Private Key to both computers.
- [x] Generate a new, unique SSH Key Pair% on each computer.
- [x] Upload both Public Keys to the LUMI user portal.
- [ ] You can only access LUMI from a single registered device.
> For maximum security, follow the "One Key per Device" rule. Generate a new pair on each device and upload both Public Keys to the portal.
```
