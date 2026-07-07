---
title: "Chapter 6: GitHub, the World’s Collaboration Platform"
nav_order: 6
---

# Chapter 6: GitHub, the World’s Collaboration Platform

If you’ve ever used Google Docs or a shared Dropbox folder to work on a document with a team, you already understand the spirit of GitHub.

GitHub is the world’s largest platform for hosting and sharing code. A lot of AI research, including the code for working with AI on LUMI, is hosted on GitHub. It isn't just a place to store files; it's a place where developers "track" every single change made to a project, allowing them to collaborate without accidentally overwriting each other's work.


## 🐺 Why Do We Use It on LUMI?
The AI community moves incredibly fast. Instead of downloading a static "version 1.0" zip file that becomes outdated in a week, we use GitHub to download the project in a way that lets us easily pull in (download) the latest updates.

The good news? Git% (the tool used to talk to GitHub) is already installed and configured for everyone on LUMI. You don't need to install anything to start using it.


## ⬇️ Getting the Code: `git clone`%
When you find an AI project on one of these platforms that you want to use, you "clone" it. Think of cloning simply as downloading the project directory, but with extra Git features attached.

The Command:
```bash
git clone https://github.com/username/project-name.git
```

You can easily get the link for `git clone` if you go to the Repository% of the project, click the button `code` and copy the link in the `HTTPS` window as such:

![Github clone link](./assets/Github_clone_link.png)

A new directory will appear on your LUMI storage (e.g., in your `/scratch`% directory%) containing all the scripts, configuration files, and instructions from that Repository. Once cloned, don't forget to use `cd repo-name` to step inside your new directory before trying to run any scripts.

> [!tip] Clone into `/scratch` or `/project`, not `$HOME`
> Your `$HOME` directory has a small 20 GB / 100k file quota. Always clone Repositories into your project's directories, which have much more space for code and data.


## 🔄 Staying Up to Date: `git pull`%
AI developers frequently update their code (including LUMI guides and examples) to fix bugs or improve performance. If there has been new work published in the Repository, it doesn't automatically get reflected in your LUMI directory of the Repository. To 'update' the directory and download the new changes, navigate inside your project directory and run:

```bash
git pull
```

This command checks GitHub for any new changes and "pulls" them down to your LUMI directory.

## 📚 Learn More
Git is a deep and powerful tool used by professional developers. While clone and pull are enough to get you started as a user, you may eventually want to learn how to save your own changes and contribute back. For that, we recommend reading GitHub's short official introduction to Git and its basic commands:

[👉 About Git](https://docs.github.com/en/get-started/using-git/about-git) **(Optional)**

## ✅ Summary Checklist
- You understand what GitHub is and why it is used on LUMI.
- You know how to download a project from GitHub using `git clone`.
- You know how to update your local copy with `git pull`.

## 📝 Knowledge Check

```quiz
title: Chapter 6 Quiz

Q: Why do we use Git and GitHub to download code on LUMI instead of just downloading a standard `.zip` file?
- [ ] Because GitHub automatically runs the code on LUMI's Compute Nodes for you.
- [x] Because Git makes it extremely easy to update your downloaded project with a single command whenever developers release new changes.
- [ ] Because LUMI's security policies strictly block all `.zip` file downloads.
- [ ] Because downloading `.zip` files creates too many small files for the Lustre filesystem.
> Downloading a project with Git allows you to maintain a connection to the Repository's history, making it trivial to fetch new updates later without having to redownload everything from scratch.

---

Q: What exactly does the `git clone` command do?
- [ ] It creates an exact backup duplicate of your LUMI workspace on the GitHub servers.
- [x] It downloads a Repository from GitHub into a new directory on your LUMI storage, along with its version history.
- [ ] It updates an already existing project directory with the newest changes.
- [ ] It creates a clone of your files on a different supercomputer node.
> `git clone` is your starting point. It "clones" (downloads) the GitHub Repository and all its files into a new directory inside your current directory.

---

Q: You cloned an AI project a few weeks ago, and the developers just announced a major bug fix. How do you apply this fix to your LUMI workspace?
- [ ] Run `git clone` again in the exact same directory.
- [ ] Delete the directory completely and download the new `.zip` file.
- [x] Navigate inside your project directory and run `git pull`.
- [ ] Run the `module load git-update` command.
> The `git pull` command checks the remote GitHub Repository for any new changes and "pulls" them down into your local directory, effortlessly keeping your code up to date.
```