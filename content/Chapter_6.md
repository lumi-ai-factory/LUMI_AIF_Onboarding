---
title: "Chapter 6: GitHub, the world’s collaboration platform"
nav_order: 6
---

# Chapter 6: GitHub, the world’s collaboration platform

If you’ve ever used Google Docs or a shared Dropbox folder to work on a document with a team, you already understand the spirit of GitHub.

GitHub is the world’s largest platform for hosting and sharing code. A lot of AI research, including the code for working with AI on LUMI, is hosted on GitHub. It isn't just a place to store files; it's a place where developers "track" every single change made to a project, allowing them to collaborate without accidentally overwriting each other's work.


## 🐺 Why do we use it on LUMI?
The AI community moves incredibly fast. Instead of downloading a "version 1.0" zip file that becomes outdated in a week, we use GitHub to "link" our workspace on LUMI to the latest version of the code.

The good news? Git (the tool used to talk to GitHub) is already installed and configured for everyone on LUMI. You don't need to install anything to start using it.


## ⬇️ Getting the Code: `git clone`
When you find an AI project on one of these platforms that you want to use, you "clone" it. Think of cloning simply as downloading the project folder, but with extra Git features attached.

The Command:
```bash
git clone https://github.com/username/project-name.git
```

You can easily get the link for `git clone` if you go to the repository (GitHub folder) of the project, click the button `code` and copy the link in the `HTTPS` window as such:

![Github clone link](./assets/Github_clone_link.png)

A new folder will appear on your LUMI storage (e.g., in your `/scratch` directory) containing all the scripts, configuration files, and instructions from that repository. Once cloned, don't forget to use `cd repo-name` to step inside your new directory before trying to run any scripts.


## 🔄 Staying Up to Date: `git pull`
AI developers frequently update their code (including LUMI guides and examples) to fix bugs or improve performance. If there has been new work published in the repository, it doesn't automatically get reflected in your LUMI folder of the repository. To 'update' the directory and download the new changes, navigate inside your project folder and run:

```bash
git pull
```

This command checks GitHub for any new changes and "pulls" them down to your LUMI folder.


## 📚 Learn More
Git is a deep and powerful tool used by professional developers. While clone and pull are enough to get you started as a user, you may eventually want to learn how to save your own changes and contribute back. For that, we recommend going through this guide for beginners:

[👉 Git and GitHub](https://www.atlassian.com/git/tutorials/setting-up-a-repository)

## ✅ Summary Checklist
- You understand what GitHub is and why we use it on LUMI.
- You know how to download a project from GitHub using `git clone`.
- You know how to update your local copy with `git pull`.