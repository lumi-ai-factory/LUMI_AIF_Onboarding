---
title: "Chapter 1: Welcome to LUMI AIF Onboarding"
nav_order: 1
---

# LUMI AIF Onboarding Materials
Welcome! You are here because you are ready to take a massive leap in AI development using [LUMI](https://lumi-supercomputer.eu/), one of the most powerful and green supercomputers% in the world.

The LUMI AI Factory is designed to bridge the gap between supercomputers and their usage by the industry. Whether you are part of a startup scaling your first Large Language Model (LLM%) or an established company optimising complex simulations, this guide is your first step toward mastering the machine. 

> [!warning] We would love to hear your feedback!
> These materials are in active development. If you have suggestions, encounter difficulties, or want to share your experience, please fill out our [short feedback form](https://link.webropolsurveys.com/S/574AE9D8B276E808). It only takes a few minutes and helps us improve the guide for everyone.

## About this guide & what to expect

The official LUMI documentation is excellent, but it can be a bit "sink or swim" for those who aren't used to High-Performance Computing (HPC) and the Command Line. In this guide, we translate complex HPC jargon into plain English and take you on a step-by-step journey from your first login to running your first AI model.

Upon completion of this guide, you will:
- **Understand secure access:** Set up SSH Keys to connect to LUMI safely (Chapter 2).
- **Master Command Line "survival skills":** Navigate the system and manage files without a mouse (Chapter 3).
- **Navigate hardware & storage tiers:** Understand CPU/GPU Partitions and where your datasets live (Chapter 4).
- **Manage AI environments:** Use Containers for reproducible software setups (Chapter 5).
- **Retrieve ready-to-use code:** Download and update pre-written code and examples directly on the supercomputer (Chapter 6).
- **Order compute power:** Schedule resources and run AI Jobs using Slurm (Chapter 7).
- **Be prepared to scale:** Know where to find help, access example scripts, and easily transition to the advanced [LUMI AI Guide](https://github.com/Lumi-supercomputer/LUMI-AI-Guide) (Chapter 8).

At the end of each chapter, you will find **a quiz** to check your understanding before moving on.

![LUMI](./assets/LUMI_supercomputer.jpg)

## AI vs. LLMs
Before we dive into the supercomputer%, let's clarify an important distinction. The terms "AI" and "LLM" are often used interchangeably in the media, but they are not the same thing:

- **Artificial Intelligence (AI)** is a broad field of computer science dedicated to creating systems capable of performing tasks that typically require human intelligence. This includes everything from computer vision (how self-driving cars "see") and predictive analytics, to speech recognition and complex game-playing algorithms.
- **Large Language Models (LLMs)** are just one specific subset of AI. They are massive neural networks - composed of billions of parameters - that have been trained on vast amounts of text data to mathematically predict the most likely next token (a piece of text) in a sequence.

So, while all LLMs are AI, not all AI is an LLM! LUMI is built to handle the entire spectrum of AI research, whether you are building the next big language model or training a neural network to discover new medical drugs.

## Why LUMI instead of the "cloud"?
If you’ve used commercial cloud services that provide computational resources for money, you might find LUMI a bit different.

- **Computational Power:** With LUMI, you don't need to worry about running out of processing power. You don't need a project requiring hundreds or thousands of processors to get started, but when you are ready to scale, LUMI provides access to thousands of powerful AMD CPUs% and GPUs%. This massive capacity allows you to run complex AI training and heavy simulations that would be cost-prohibitive on standard cloud platforms.

- **Flexibility:** Unlike the "one-size-fits-all" approach of commercial cloud providers, LUMI is highly customisable. You aren't locked into a specific provider's ecosystem; you have the freedom to fine-tune your software% environment to meet the exact requirements of your project.

- **Support for Innovation:** Scaling AI can be incredibly resource-intensive for small and medium-sized enterprises (SMEs) and startups. We offer learning materials, free compute resources (subject to application approval and eligibility), and expert guidance to eligible companies to ensure your ideas aren't limited by budget or hardware% constraints.

- **Data Services:** We are also actively developing comprehensive data services - such as [Dataset as a Service](https://lumi-ai-factory.eu/dataset-as-a-service/) - to further support your workflows and help you manage your data alongside compute resources.

- **The Trade-off:** With great power comes... a bit of a learning curve. LUMI requires more technical "hands-on" work - specifically using the Command Line%. This guide is here to make sure you have the skills to handle that power with confidence.


## Help

**1. Ask an LLM (For Basics)**
Don't be afraid to talk to your favourite AI chatbot. For general questions about how High-Performance Computing% (HPC) works, navigating the Command Line%, understanding a Slurm script%, or debugging Python code, an LLM is often your fastest and most accessible resource.

**2. Contact Us (For LUMI-Specifics & Complex Issues)**
While LLMs are great for general knowledge, they might not know the specific configurations or policies of LUMI. For LUMI-specific questions, or if you run into more complicated issues that an LLM can't solve, please turn to us! 

Contact [LUMI AI Factory user support](https://lumi-ai-factory.eu/user-support/)


## A note on sensitive data
Before we dive in, let’s talk about your data. While LUMI is a highly secure environment, it is a shared supercomputer. 

If your project involves processing **personal, identifiable, or highly sensitive data** (like health records), strict GDPR rules and specific contractual agreements apply. You must assess your use case and establish the necessary safeguards *before* uploading any sensitive datasets.

We will cover the specific rules and classifications for sensitive data in detail in Chapter 4.


## How to get started
If you haven't already applied for a project or requested compute time, your first stop should be establishing whether you're eligible for free resources:

[Pricing and eligibility](https://lumi-ai-factory.eu/pricing-and-eligibility/)

Then proceed to applying for resources by filling out this form:

[Get started with us](https://lumi-ai-factory.eu/enter-the-lumi-ai-factory/)

After you've been granted resources:

[Getting access to LUMI](https://docs.lumi-supercomputer.eu/firststeps/accessLUMI/)


## Prerequisites
No special technical skills necessary!
What you will need to get started:

- A Laptop/Workstation: Any modern Mac, Linux, or Windows machine.
- An Internet Connection: No special fiber-optics required!
- A Terminal%: Don’t worry, your computer already has one (Terminal on Mac/Linux, PowerShell or CMD on Windows). We’ll show you how to use it.
- A LUMI Project with compute resources: You'll get this after your application for resources is approved.

## Transparency note on AI assistance
This material was prepared with the support of AI tools (Google Gemini and Anthropic Claude), which were used exclusively to refine phrasing and ensure clarity. All core concepts, instructional content, and overarching structure were entirely conceived, drafted, and rigorously reviewed by the authors and the LUMI AI Factory staff.

## License
Content is licensed under the CC-BY 4.0 license. Code and examples are licensed under MIT License.

## Summary checklist
- You understand what LUMI AI Factory is and why it is used.
- You know how to apply for resources.
- You are ready to start learning.

## Knowledge check

Answer all questions correctly to get celebratory confetti!

```quiz
title: Chapter 1 Quiz

Q: Which of the following is true about the relationship between AI and LLMs?
- [ ] They are completely different and unrelated fields.
- [ ] All AI systems are basically Large Language Models.
- [x] LLMs are a specific subset of Artificial Intelligence.
- [ ] AI is a specific subset of Large Language Models.
> AI is a broad field of computer science, while LLMs are a specific type of AI - massive neural networks trained to predict text.

---

Q: Why might you choose LUMI over a standard commercial cloud provider?
- [x] LUMI provides massive computational power that would be cost-prohibitive (too expensive) on standard cloud platforms.
- [x] LUMI offers total flexibility to fine-tune your software environment.
- [ ] LUMI is simple and intuitive to navigate and use without additional learning materials. 
- [x] LUMI offers free compute resources and expert guidance to eligible SMEs and startups.
> LUMI provides massive scale, flexibility, and support for innovation, but the trade-off is a steeper learning curve requiring the Command Line.
```
