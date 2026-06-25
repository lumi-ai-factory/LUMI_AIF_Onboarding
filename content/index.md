---
title: "Chapter 1: Welcome to LUMI AIF Onboarding"
nav_order: 1
---

# Chapter 1: Welcome to LUMI AIF Onboarding
Welcome! You are here because you are ready to take a massive leap in AI development using [LUMI](https://lumi-supercomputer.eu/), one of the most powerful and green supercomputers% in the world.

The LUMI AI Factory is designed to bridge the gap between supercomputers and their usage by the industry. Whether you are part of a startup scaling your first Large Language Model (LLM%) or an established company optimising complex simulations, this guide is your first step toward mastering the machine. 

> [!warning] We would love to hear your feedback!
> These materials are in active development. If you have suggestions, encounter difficulties, or want to share your experience, please fill out our [short feedback form](https://link.webropolsurveys.com/S/574AE9D8B276E808). It only takes a few minutes and helps us improve the guide for everyone.

## 🎯 What to Expect
Upon completion of this guide, you will: 
- Understand the basics of working on a supercomputer% using the Command Line% interface. 
- Become familiar with Slurm% job scheduler and be able to run your own Jobs% on LUMI.
- Be prepared to follow a more advanced guide on using LUMI - [LUMI AI Guide](https://github.com/Lumi-supercomputer/LUMI-AI-Guide), which covers setting up your environment, data storage, running LLMs% and much more.
- Know where to turn for help and where to look for example scripts% for your use case.

![LUMI](./assets/LUMI_supercomputer.jpg)

## 🤖 AI vs. LLMs
Before we dive into the supercomputer%, let's clarify an important distinction. The terms "AI" and "LLM" are often used interchangeably in the media, but they are not the same thing:

- **Artificial Intelligence (AI)** is a broad field of computer science dedicated to creating systems capable of performing tasks that typically require human intelligence. This includes everything from computer vision (how self-driving cars "see") and predictive analytics, to speech recognition and complex game-playing algorithms.
- **Large Language Models (LLMs)** are just one specific subset of AI. They are massive neural networks—composed of billions of parameters—that have been trained on vast amounts of text data to mathematically predict the most likely next token (a piece of text) in a sequence.

So, while all LLMs are AI, not all AI is an LLM! LUMI is built to handle the entire spectrum of AI research, whether you are building the next big language model or training a neural network to discover new medical drugs.

## 🐺 Why LUMI Instead of the "Cloud"?
If you’ve used commercial cloud services that provide computational resources for money, you might find LUMI a bit different.

- **Computational Power:** With LUMI, you don't need to worry about running out of processing power. You don't need a project requiring hundreds or thousands of processors to get started, but when you are ready to scale, LUMI provides access to thousands of powerful AMD CPUs% and GPUs%. This massive capacity allows you to run complex AI training and heavy simulations that would be cost-prohibitive on standard cloud platforms.

- **Flexibility:** Unlike the "one-size-fits-all" approach of commercial cloud providers, LUMI is highly customisable. You aren't locked into a specific provider's ecosystem; you have the freedom to fine-tune your software% environment to meet the exact requirements of your project.

- **Support for Innovation:** Scaling AI can be incredibly resource-intensive for small and medium-sized enterprises (SMEs) and startups. We offer learning materials, free compute resources (subject to application approval and eligibility), and expert guidance to eligible companies to ensure your ideas aren't limited by budget or hardware% constraints.

- **Data Services:** We are also actively developing comprehensive data services—such as [Dataset as a Service](https://lumi-ai-factory.eu/dataset-as-a-service/)—to further support your workflows and help you manage your data alongside compute resources.

- **The Trade-off:** With great power comes... a bit of a learning curve. LUMI requires more technical "hands-on" work — specifically using the Command Line%. This guide is here to make sure you have the skills to handle that power with confidence.


## 🆘 Help

**1. Ask an LLM (For Basics)**
Don't be afraid to talk to your favourite AI chatbot. For general questions about how High-Performance Computing (HPC) works, navigating the Command Line%, understanding a Slurm script%, or debugging Python code, an LLM is often your fastest and most accessible resource.

**2. Contact Us (For LUMI-Specifics & Complex Issues)**
While LLMs are great for general knowledge, they might not know the specific configurations or policies of LUMI. For LUMI-specific questions, or if you run into more complicated issues that an LLM can't solve, please turn to us! 

Contact [LUMI AI Factory user support](https://lumi-ai-factory.eu/user-support/)


## 🛡️ A Note on Sensitive Data
Before we dive in, let’s talk about your data. While LUMI is a secure environment, it is a shared supercomputer.

**The "Shared" Nature:** The hardware% itself (the physical supercomputer) is shared among many users across Europe. However, your files are protected by standard permissions. In practice, this is like having multiple user accounts on a single computer: User A cannot open the files belonging to User B, even though they are using the exact same physical machine.

That said, file-level permissions are only one part of the picture. When it comes to personal and sensitive information, it is important to distinguish between two types of data processing:

1. **LUMI User Data:** LUMI processes personal data related to its users (such as names, organisational affiliations, and contact information) for account management and service operation. For this, LUMI acts as the data controller%. You can read more in the [Privacy Notice for processing of user data on the LUMI Service](https://lumi-supercomputer.eu/privacy-notice-for-processing-of-user-data-on-the-lumi-service/).
2. **Personal Data in User Datasets:** You might use LUMI to process datasets that contain personal data. In this context, your organisation acts as the GDPR% data controller%, and LUMI acts as the data processor%. In accordance with the [LUMI General Terms of Use](https://lumi-supercomputer.eu/wp-content/uploads/2026/03/LUMI-General-Terms-of-Use_2026.pdf), it is your responsibility to assess and ensure that the LUMI environment provides an adequate level of security for the specific data you are processing. Depending on your dataset, the requirements differ:
   - **Anonymised Data:** Fully anonymised datasets are generally well suited for processing in LUMI as they fall outside the scope of GDPR%.
   - **Identifiable Data:** Identifiable personal data may be processed provided that appropriate security controls and contractual arrangements (such as a Data Processing Agreement (DPA)%) are strictly in place.
   - **Highly Sensitive Data (Special Categories):** You must exercise extreme caution before processing special categories of personal data, such as health or biometric records. While the Terms of Use may allow this under specific conditions, LUMI's shared architecture means that guaranteeing the secure deletion of data after processing is not straightforward. Therefore, if your project requires handling such sensitive information, you must contact the LUMI team in advance to assess your use case and establish the necessary safeguards before uploading any data.


## 🚀 How to Get Started
If you haven't already applied for a project or requested compute time, your first stop should be establishing whether you're eligible for free resources:

[👉 Pricing and eligibility](https://lumi-ai-factory.eu/pricing-and-eligibility/)

Then proceed to applying for resources by filling out this form:

[👉 Get started with us](https://lumi-ai-factory.eu/enter-the-lumi-ai-factory/)

After you've been granted resources:

[👉 Getting access to LUMI](https://docs.lumi-supercomputer.eu/firststeps/accessLUMI/)


## 📖 About This Guide

The official LUMI documentation is excellent, but it can be a bit "sink or swim" for those who aren't used to High-Performance Computing (HPC) and the Command Line.

In the following chapters, we will:
1. Translate the jargon into plain English.
2. Teach you the "Survival Skills" for the Command Line%.
3. Link you to the technical steps in the official documentation once you're ready.

At the end of each chapter, you will find **a quiz** that helps you check your understanding of the material before moving on. 

> [!note] The mechanics of LLM models
> This onboarding guide focuses on the practical skills needed to operate the supercomputer. To understand the underlying mechanics of the LLMs you will be running—including the technical differences between Dense and MoE architectures, memory bottlenecks, and scaling strategies like Tensor Parallelism—please read our [Technical Primer on Large Language Models](https://arbruiser.github.io/Technical-primer-on-LLMs/).


## 🛠️ Prerequisites
No special technical skills necessary!
What you will need to get started:

- A Laptop/Workstation: Any modern Mac, Linux, or Windows machine.
- An Internet Connection: No special fiber-optics required!
- A Terminal%: Don’t worry, your computer already has one (Terminal on Mac/Linux, PowerShell or CMD on Windows). We’ll show you how to use it.
- A LUMI Project with compute resources: You'll get this after your application for resources is approved.

## ✅ Summary Checklist
- You understand what LUMI AI Factory is and why it is used.
- You know how to apply for resources.
- You are ready to start learning.

## 📝 Knowledge Check

Answer all questions correctly to get celebratory confetti! 🎉

```quiz
title: Chapter 1 Quiz

Q: Which of the following is true about the relationship between AI and LLMs?
- [ ] They are completely different and unrelated fields.
- [ ] All AI systems are basically Large Language Models.
- [x] LLMs are a specific subset of Artificial Intelligence.
- [ ] AI is a specific subset of Large Language Models.
> AI is a broad field of computer science, while LLMs are a specific type of AI—massive neural networks trained to predict text.

---

Q: Why might you choose LUMI over a standard commercial cloud provider?
- [x] LUMI provides massive computational power that would be cost-prohibitive (too expensive) on standard cloud platforms.
- [x] LUMI offers total flexibility to fine-tune your software environment.
- [ ] LUMI is simple and intuitive to navigate and use without additional learning materials. 
- [x] LUMI offers free compute resources and expert guidance to eligible SMEs and startups.
> LUMI provides massive scale, flexibility, and support for innovation, but the trade-off is a steeper learning curve requiring the Command Line.

---

Q: Under what conditions can you process datasets containing personal data on LUMI?
- [ ] You can upload any personal data since LUMI is a highly secure environment.
- [ ] You can only upload fully anonymised data; identifiable data is strictly prohibited.
- [ ] You can never upload special categories of personal data, such as health records.
- [x] You can process identifiable data if appropriate security controls and a Data Processing Agreement are in place.
> While fully anonymised data is generally well-suited, you can process identifiable data with the right contractual agreements. Highly sensitive data requires contacting the LUMI team in advance to assess the use case.
```
