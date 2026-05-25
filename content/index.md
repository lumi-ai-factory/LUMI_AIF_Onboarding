---
title: "Chapter 1: Welcome"
nav_order: 1
---

# LUMI AIF onboarding materials
Welcome! You are here because you are ready to take a massive leap in AI development using [LUMI](https://lumi-supercomputer.eu/), one of the most powerful and "green" supercomputers in the world.

The LUMI AI Factory is designed to bridge the gap between supercomputers and their usage by the industry. Whether you are part of a startup scaling your first Large Language Model (LLM) or an established company optimizing complex simulations, this guide is your first step toward mastering the machine. 

Upon completion of this guide, you will: 
- Understand the basics of working on a supercomputer using the Command Line interface. 
- Become familiar with Slurm job scheduler and be able to run your own jobs on LUMI.
- Be prepared to follow a more advanced guide on using LUMI - [LUMI AI Guide](https://github.com/Lumi-supercomputer/LUMI-AI-Guide), which covers setting up your environment, data storage, running LLMs and much more.
- Know where to turn for help and where to look for example scripts for your use case.

![LUMI](./assets/LUMI_supercomputer.jpg)

## 🐺 Why LUMI instead of the "Cloud"?
If you’ve used services that provide computational resources for money such as AWS, Google Cloud, or Azure, you might find LUMI a bit different.

- **Computational Power:** LUMI provides access to thousands of powerful AMD processors and GPUs. This massive scale allows you to run complex AI training and heavy simulations that would be cost-prohibitive on standard cloud platforms.

- **Flexibility:** Unlike the "one-size-fits-all" approach of commercial cloud providers, LUMI is highly customizable. You aren't locked into a specific provider's ecosystem; you have the freedom to fine-tune your software environment to meet the exact requirements of your project.

- **Support for Innovation:** For small and medium-sized enterprises (SMEs) and startups, the barriers to entry can be high. We offer learning materials, free compute resources (subject to application approval and eligibility), and expert guidance to eligible companies to ensure that European innovation isn't limited by a budget.

- **The Trade-off:** With great power comes... a bit of a learning curve. LUMI requires more technical "hands-on" work — specifically using the command line. This guide is here to make sure you have the skills to handle that power with confidence.


## 🆘 Help
Contact [LUMI AI Factory user support](https://lumi-ai-factory.eu/user-support/)

Don't be afraid to talk to AI and ask it questions about how something works and why we do this or that, and why something doesn't work. It might not know LUMI-specific things (turn to us for that!), but it can very accessibly explain you what's happening in a Slurm script or in a Python code.




## 🛡️ A Note on Sensitive Data
Before we dive in, let’s talk about your data. While LUMI is a secure environment, it is a shared supercomputer.

- **The "Shared" Nature:** Your files are protected by standard permissions—users you haven't authorized cannot see your work. However, the hardware itself (the nodes) is shared among many users across Europe.
- **Sensitive Data:** LUMI lacks the specific legal certifications such as those required for handling raw medical records with patient names or classified government data.
- **The Rule of Thumb:** LUMI is not suitable for "raw" sensitive data containing Personally Identifiable Information (PII) such as names or addresses. However, it is an excellent choice for anonymized or pseudonymized data. If you are working in healthcare or finance, ensure your data is properly stripped of identifying markers before uploading it to LUMI.


## 🚀 How to Get Started
If you haven't already applied for a project or requested compute time, your first stop should be establishing whether you're eligible for free resources:

[👉 Pricing and eligibility](https://lumi-ai-factory.eu/pricing-and-eligibility/)

Then proceed to applying for resources by filling out this form:

[👉 Get started with us](https://lumi-ai-factory.eu/enter-the-lumi-ai-factory/)

After you've been granted resources:

[👉 Read more about identification](https://docs.lumi-supercomputer.eu/firststeps/accessLUMI/)


## 📖 About This Guide

The official LUMI documentation is excellent, but it can be a bit "sink or swim" for those who aren't used to High-Performance Computing (HPC) and the command line.

In the following chapters, we will:
1. Translate the jargon into plain English.
2. Teach you the "Survival Skills" for the command line.
3. Link you to the technical steps in the official documentation once you're ready.


## 🛠️ Prerequisites
No special technical skills necessary!
What you will need to get started:

- A Laptop/Workstation: Any modern Mac, Linux, or Windows machine.
- An Internet Connection: No special fiber-optics required!
- A Terminal: Don’t worry, your computer already has one (Terminal on Mac/Linux, PowerShell or CMD on Windows). We’ll show you how to use it.
- A LUMI Project with compute resources: You'll get this after your application is approved.

## ✅ Summary Checklist
- You understand what LUMI AI Factory is and why we use it.
- You know how to apply for resources.
- You are ready to start learning.