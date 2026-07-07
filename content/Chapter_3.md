---
title: "Chapter 3: The Command Line – Your Primary Tool"
nav_order: 3
---

# Chapter 3: The Command Line – Your Primary Tool

In the previous chapter, you've logged in to LUMI. If you were successful, you saw LUMI's login wolf, a "Did you know?" fact, and a Command Line% that looked similar to this: 

```bash
     
```

**Welcome to the Command Line Interface (CLI).** The underlying program that actually reads and executes your commands here is known as the **Shell%**.


## 🖱️ Navigation Without a Mouse
When you use Windows or macOS, you see folders and icons. You double-click a folder to see what’s inside.
On LUMI, those folders (called directories in the Command Line) and files are still there, but you are "navigating by description" rather than "navigating by sight."

**Why do we use this for AI?**
- Flexibility: Graphical interfaces are often just "wrappers" that sit on top of Command Line% tools. By working directly in the Shell, you're removing the middleman. You gain the ability to use every piece of software% in the LUMI ecosystem exactly how the developers intended, without being restricted by visual menu options. 
- Automation: You can write a list of commands (a script%) that LUMI can follow while you are asleep. You can't "script" a mouse click easily.
- Efficiency: LUMI is optimised for math and data processing. Displaying a fancy visual desktop for thousands of users would waste massive amounts of power that should be used for your AI models instead.

## 📚 Command Line Study Materials
To learn about the Command Line and the commands to use it, we recommend reading [Linux Command Line for beginners](https://ubuntu.com/desktop/docs/en/latest/tutorial/the-linux-command-line-for-beginners/). 

If you are more of a visual learner or if this is your first time learning about the Command Line%, watch this [video guide](https://www.youtube.com/watch?v=16d2lHc0Pe8).

> [!warning] There is no "Undo"
> The Command Line is unforgiving and does not have a "Recycle Bin" or "Revert the previous command" functionality.
> If you use the command to delete files (`rm`), the file is gone instantly and permanently. We must always be careful and double-check our commands before hitting Enter.


## 🏋️ Practice
The best way to learn after having studied some theory is to **practise**. Log in to LUMI (see Chapter 2) and let's apply what you've learned in the guides in our Command Line Terminal on LUMI. Follow these step-by-step instructions:

> [!tip] Build muscle memory
> We highly recommend typing the commands out yourself rather than copy-pasting them. Writing them manually helps build muscle memory so you can use them naturally later!

1. **Where am I?**
    When you log into LUMI using `ssh` as we did in Chapter 2, you end up in your user's `$HOME` folder (this is the current 'Working Directory%'). Check it by running:

    ```bash
    pwd
    ```

    The output (what is printed/shown on the screen) should be `/users/<username>`. This is "where you are" now. 

2. **Create a directory%** and name it 'first_dir':

    ```bash
    mkdir first_dir
    ```

    If it was successful, `mkdir` has no output (it won't print a "Success!" message, it will just drop you to a new, empty prompt). Check the result by **l**i**s**ting all the files and directories in the current Working Directory%:

    ```bash
    ls
    ```

    You should see your new directory% there. 

3. **Move Inside.** Change your Working Directory% to the new directory: 

    ```bash
    cd first_dir
    ```

    Notice that the text at the beginning of your line has updated to include `~/first_dir>`. This indicates that you are now inside the new folder.

4. **Create a Document.** While in the new directory, create a new text file using `nano`, a very simple text editor: 

    ```bash
    nano first_file.txt
    ```

    Once the editor opens, follow these steps to add content and save it:
    - **First, type or paste** some text into the editor (if pressing **ctrl+V** doesn't work, try **ctrl+shift+V** or right-click to paste). 
    - **Then, save** the file by pressing **ctrl+S**. To help you remember, "S" in this case stands for "**s**ave".
    - **Finally, exit** the editor by pressing **ctrl+X**. "X" stands for "e**x**it". 

    > [!info] Using `nano`
    > The command `nano <filename>` is used to open an existing file or create a new file with that name if it doesn't exist. 

5. **View the file:**
    ```bash
    less first_file.txt
    ```
    
    *Press **q** to exit the `less` viewer.*

    > [!tip] Autocomplete
    > Instead of typing the whole name of an existing file (such as in step 5), you can type the first few characters of its name (such as `less fir`) and press TAB, it will automatically finish the name. If there are multiple files that start with the same few characters, press TAB twice to see available options. 

6. **Upload a file.** To upload an image from your machine (PC/laptop) to LUMI, one way is to use the web interface:
    - Open [www.lumi.csc.fi](https://www.lumi.csc.fi) in your browser (don't close your Terminal!), log in and go to `Home directory`. This is your `$HOME` user directory. There you should see our `first_dir` folder (do **not** click into it so your upload lands in `$HOME`, not inside `first_dir`).
    - Click "Upload" and upload your image directly to your `$HOME` directory. 

    > [!tip] Using `scp`
    > Another, more 'professional' way of uploading files is to use `scp` command as [described here](https://docs.lumi-supercomputer.eu/firststeps/movingdata/).

7. **Go back.** Open your Terminal. Go back to the "Parent Directory%" of the current Working Directory: 

    ```bash
    cd ..
    ```

    Now, when you're in your `$HOME` directory you should see the file that you uploaded there (Hint: use `ls` to check that it's there).

8. **Copy the uploaded image** to the new directory. Replace `<your_image.png>` with the actual name of your file (using its actual extension, like `.jpg` or `.jpeg` if it's not a `.png`), and run:

    ```bash
    cp <your_image.png> first_dir
    ```

    Change your Working Directory to `first_dir` and list the files in it. Hint: look at steps 2 and 3.

9. **Rename the copied image**. Let's rename it to `renamed_image.png` (using your file's actual extension if it's not `.png`, like `renamed_image.jpg`). We use the `mv` (move) command for this, as renaming is essentially moving a file to the same location under a new name:

    ```bash
    mv <your_image.png> renamed_image.png
    ```

10. **Delete the original image**. Remove the original image that we left in the Parent Directory:

    ```bash
    rm ../<your_image.png>
    ```

    Make sure that the original image has been removed by navigating to the Parent Directory and listing the files there.

    > [!warning] Gone for good!
    > After this command, there is no way to recover the removed image as there are no backups on LUMI. Make sure to make a backup of your data to avoid accidents with `rm`. 

11. **Disconnect**. Great job! You can now disconnect from LUMI by pressing **Ctrl+D**. You will notice the Command Line prompt change back to your local computer's format (such as `username@your_computer` on Mac/Linux, or `C:\Users\Name` on Windows). This confirms you are safely back on **your machine**.

You can learn more about Linux Command Line on the [Linux basics tutorial for CSC](https://docs.csc.fi/support/tutorials/env-guide/) page. 

## 📝 Your Command Line Cheatsheet

| Command / Key | Action | Quick Note |
| :--- | :--- | :--- |
| `pwd` | **P**rint **W**orking **D**irectory | "Where am I right now?" |
| `ls` | **L**i**s**t | Show all files and folders here. |
| `mkdir <name>` | **M**ake **D**irectory | Create a new folder. |
| `cd <name>` | **C**hange **D**irectory | Enter a folder. |
| `cd ..` | Go Back | Move up to the "parent" folder. |
| `nano <file>` | Text Editor | Create or edit text files. |
| `less <file>` | View File | Read a file without editing it (press **q** to exit). |
| `cp <old> <new>` | **C**o**p**y | Copy a file to a new location (or/and under a new name). |
| `mv <old> <new>` | **M**o**v**e / Rename | Move a file or change its name. |
| `rm <file>` | **r**e**m**ove | Irreversibly delete a file |
| `TAB` | **Autocomplete** | Type a few letters and hit Tab to finish the name. |
| `Ctrl + S` | Save | Save changes in the `nano` editor. |
| `Ctrl + X` | Exit | Close the `nano` editor. |
| `Ctrl + D` | Disconnect | Logout from LUMI and return to your local machine. |

## ✅ Summary Checklist
- You know what a Command Line is and why it is used.
- You have learned the basics of the Command Line.
- You have practised learned commands on LUMI.

## 📝 Knowledge Check

```quiz
title: Chapter 3 Quiz

Q: Why do AI developers primarily use the Command Line Interface (CLI) instead of graphical desktops on a supercomputer?
- [x] To write scripts that automate tasks.
- [x] Because graphical desktops waste massive amounts of computational power.
- [ ] Because graphical interfaces are more secure against hackers.
- [x] To gain direct, flexible access to software without being restricted by visual menus.
> The Command Line offers automation, efficiency, and flexibility. Visual desktops are rarely used on supercomputers because they waste power and act as unnecessary "middlemen."

---

Q: What is the relationship between the Command Line Interface (CLI) and the Shell?
- [ ] They are completely unrelated tools used for different things.
- [ ] The CLI is the program that interprets commands, and the Shell is the text box on your screen.
- [x] The CLI is the text-based environment, and the Shell is the underlying program that reads and executes your commands.
- [ ] The Shell is a graphical wrapper for the CLI.
> The CLI is the interface where you type, and the Shell is the invisible engine underneath that actually understands and runs what you typed.

---

Q: Which command is used to step out of your current directory and move up to the "Parent Directory"?
- [ ] `cd parent`
- [x] `cd ..`
- [ ] `mv ..`
- [ ] `pwd`
> The command `cd` stands for Change Directory, and the two dots (`..`) specifically refer to the Parent Directory located one level above your current location.

---

Q: How do you find out "where you are" (your current Working Directory) in the Command Line?
- [ ] Look at the graphical folder icon at the top of the window.
- [x] Type `pwd` and press Enter.
- [ ] Type `whereami` and press Enter.
- [ ] Type `ls` to see the folder name.
> The command `pwd` stands for **P**rint **W**orking **D**irectory. It outputs the exact path of the directory you are currently inside.

---

Q: What are the two primary ways to upload files from your computer to LUMI?
- [x] Using the LUMI web interface.
- [ ] Dragging and dropping files into the SSH Terminal window.
- [x] Using the `scp` command from your local Terminal.
- [ ] Emailing the files to LUMI support.
> You can upload files visually via the web interface, or use the `scp` command in your Terminal for a more robust transfer.

---

Q: What is the "Undo" shortcut (like Ctrl+Z) if you make a mistake and run a destructive command (like `rm`) in the Command Line?
- [ ] You can type `undo` to revert the last command.
- [ ] You can find deleted or changed files in the "Recycle Bin" or "Trash" folder.
- [x] There is no "Undo" functionality for the Command Line.
- [ ] You can contact LUMI support to restore it from an automatic backup.
> The Command Line is unforgiving and does not have a "Revert" or "Recycle Bin" feature for *any* command. You must always double-check your commands before pressing Enter!
```
