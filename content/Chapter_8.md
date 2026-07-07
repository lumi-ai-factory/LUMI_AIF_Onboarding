---
title: "Chapter 8: Graduation & Next Steps"
nav_order: 8
---

# Chapter 8: Graduation & Next Steps

Congratulations! You have successfully completed the LUMI AI Factory onboarding. You've progressed from learning how to securely log in with SSH Keys (Chapter 2) and navigating the Command Line% (Chapter 3), to understanding LUMI's powerful hardware% and storage architecture (Chapter 4). You also learned how to use Apptainer% / Singularity% Containers% for reproducible AI environments (Chapter 5), how to retrieve code from GitHub% (Chapter 6), and finally, how to request compute resources from Slurm% to run your first AI Job (Chapter 7). You now possess the foundational "survival skills" needed to harness one of the world's most powerful supercomputers.

> [!warning] We would love to hear your feedback!
> These materials are in active development. If you have suggestions, encounter difficulties, or want to share your experience, please fill out our [short feedback form](https://link.webropolsurveys.com/S/574AE9D8B276E808). It only takes a few minutes and helps us improve the guide for everyone.

## 🎓 Graduation Exercise

To truly graduate, let's put everything you've learned into practice with one final, comprehensive exercise. Your task is to complete the [LUMI AI Guide](https://github.com/Lumi-supercomputer/LUMI-AI-Guide). 

The LUMI AI Guide is a 10-chapter deep dive designed to help you migrate machine learning applications from your local computer to LUMI. Instead of dry theory, you will work through a hands-on, end-to-end example: training an image classification model (PyTorch's Vision Transformer) on the ImageNet dataset.

You will start with a basic Python script that runs on a laptop, and chapter-by-chapter, you will modify it to run efficiently at scale on LUMI's AMD GPUs. Along the way, you will practise everything from this onboarding—using Containers%, navigating storage, and submitting Slurm% Jobs—while learning advanced concepts like distributed multi-node training. Finally, the guide covers the transition from training to production, concluding with a chapter on memory-efficient LLM inference hosting using vLLM.

Take your time with it, and remember to use the foundational "survival skills" you've acquired here!

> [!note] Do I need to be a Python expert for this?
> The LUMI AI Guide provides all the completed Python and Slurm scripts in its Repository. If you do not have a programming background, you can easily follow along, run the provided commands, and watch the AI models train without writing a single line of code! However, to truly understand *why* the Python code must be modified to scale across multiple GPUs, a basic understanding of Python and Machine Learning concepts is still highly recommended.


## 📚 Next Steps: Advanced Materials

Now that you've mastered the basics, here are some excellent resources to continue your journey — pick whichever match your goals:

- [👉 Working with LLMs](https://docs.csc.fi/support/tutorials/ml-llm/): Tutorials and examples on LLM fine-tuning, quantisation and Retrieval-Augmented Generation (RAG).
- [👉 Technical Primer on Large Language Models](https://arbruiser.github.io/Technical-primer-on-LLMs/): A deep dive into the theory of LLMs: model architectures (MoE vs. Dense), attention mechanisms (GQA), memory bottlenecks, and scaling strategies like Tensor Parallelism.
- [👉 HPC Guides Collection](https://github.com/lumi-ai-factory/ai-hpc-guide-collection): A curated Repository of various High-Performance Computing and AI guides.


## 🤖 Give Your AI Assistant LUMI Knowledge

Out of the box, AI chatbots often don't know LUMI's specific configurations and policies. To fix this, the LUMI AI Factory hosts a free, public MCP% server that lets AI assistants search the official LUMI documentation directly. If your AI tool supports the Model Context Protocol — like Claude, ChatGPT (Developer mode), VS Code Copilot, or coding agents such as OpenCode — you can connect it using this address: `https://lumi-aif-agents.2.rahtiapp.fi/mcp` (no authentication needed).

For setup details — and for how to run AI coding agents directly on LUMI — see [Infrastructure for AI Agents](https://docs.lumi-supercomputer.eu/laif/software/agent-infrastructure/).


## 🔔 Stay Updated

The AI landscape and the LUMI ecosystem evolve rapidly. Keep yourself in the loop:

- **Attend LUMI User Coffee:** Join the [regular informal virtual meetings](https://lumi-supercomputer.eu/events/usercoffeebreaks/) to discuss issues, share experiences, and hear announcements.
- **Read the Blogs:** Stay inspired by reading the [LUMI Supercomputer Blog](https://lumi-supercomputer.eu/category/blog-posts/) and the [LUMI AI Factory Blog](https://lumi-ai-factory.eu/about-us/news/).
- **Trainings:** Check the [Trainings Calendar](https://lumi-ai-factory.eu/trainings/) for upcoming workshops, courses and webinars.
- **Services:** Explore the LUMI AI Factory [Service Catalogue](https://lumi-ai-factory.eu/service-catalogue/) to see what other expert support and resources are available.


## 🆘 Getting Help

Even the most experienced developers get stuck. When you do, don't struggle in silence!

- **AI Factory Support:** reach out via the [LUMI AI Factory User Support](https://lumi-ai-factory.eu/user-support/).

## ✅ Summary Checklist
- You are ready for the Graduation Exercise.
- You have completed the LUMI AIF Onboarding.
- You know where to find advanced guides and help.

## 📝 Final Knowledge Check

```quiz
title: Final Quiz: LUMI Survival Guide

Q: When typing your SSH passphrase in the Terminal during login, what should you expect to see?
- [ ] Asterisks (***) hiding your password.
- [ ] A progress bar.
- [x] Absolutely nothing appears on the screen as you type.
- [ ] You see the characters that you've typed.
> This is a standard Linux security feature called the "Silent Passphrase". Just type your phrase and hit Enter!

---

Q: If you want to move up one directory level (e.g., from `/users/smithmar/first_dir` to `/users/smithmar`), which command do you use?
- [ ] `cd up`
- [ ] `ls -l`
- [x] `cd ..`
- [ ] `cd /`
> `cd` stands for Change Directory and the `..` symbol represents the Parent Directory.

---

Q: You just logged into LUMI and need to edit some text files and organise your folders before training. Where should you do this?
- [x] Directly on the Login Node.
- [ ] You must request an interactive Compute Node via Slurm.
- [ ] On the LUMI-D Partition.
- [ ] You should edit code exclusively on your local laptop before uploading.
> Lightweight tasks like organising files, editing code, and submitting Jobs are exactly what the Login Nodes are for!

---

Q: Which of the following actions should you **avoid** doing on the Login Nodes?
- [ ] Writing or editing code in `nano`.
- [x] Training a machine learning model.
- [x] Running a script that processes heavy amounts of data for a long time.
- [ ] Submitting a Batch Job using Slurm.
> The Login Nodes are shared by hundreds of users. Training models or running heavy data processing scripts there will slow down the entire system and your processes will be killed by automatic guards.

---

Q: What happens if you try to bypass Slurm and run your heavy AI training script directly on the Command Line of a Login Node?
- [ ] The script will run normally, just slightly slower.
- [x] The system's automatic guards will detect the heavy load and kill your process to protect the node for other users.
- [ ] Slurm will automatically move the script to a Compute Node for you.
- [ ] You will be permanently banned from LUMI.
> Login Nodes are shared lobbies. Heavy computations are strictly forbidden and will be automatically terminated.

---

Q: If your Slurm Batch Script requests `--gpus-per-node=4` on the LUMI-G Partition, how much physical hardware are you actually reserving?
- [ ] Four full AMD MI250X chips.
- [x] Two full AMD MI250X chips.
- [ ] Half of a node.
- [ ] Four CPU cores.
> Always remember the golden rule: 1 Slurm GPU = 1 GCD (half a physical chip).

---

Q: Your script needs to rapidly read and write temporary checkpoint data during a massive AI training run. Which storage tier provides the absolute fastest reading and writing speeds?
- [ ] User `$HOME`
- [ ] Project Space (`/project`)
- [ ] Project Scratch (`/scratch`)
- [x] Project Flash (`/flash`)
> Flash uses high-speed NVMe drives for the most demanding tasks (but it is 3x more expensive!).

---

Q: LUMI is a shared environment. Which of the following statements about what others can see are **TRUE**?
- [x] Other members of your project can view and access the files you place in the project's `/scratch` directory.
- [ ] Your personal `$HOME` directory is visible to everyone on the supercomputer.
- [x] If you run a script on a shared Compute Node, other users on that node can see the name of the command you ran.
- [x] Details about your submitted Slurm Jobs (like the Job name) are visible to anyone monitoring the system queue.
- [ ] Other users can read the output and contents of the scripts you are running on shared nodes.
> Project spaces and Job queues are collaborative and visible. Shared nodes expose process names (but not file contents!).

---

Q: When preparing to process datasets containing personal data on LUMI, which of the following is the correct approach?
- [ ] You can freely upload any dataset because LUMI is a highly secure supercomputer.
- [ ] Personal data is strictly forbidden; only fully anonymised data can ever be uploaded.
- [x] You can process identifiable data if the right contractual agreements (like a Data Processing Agreement) are in place, but highly sensitive data (like health records) requires contacting the LUMI team in advance.
- [ ] You only need to encrypt the dataset before uploading it, and then all GDPR restrictions are waived.
> While anonymised data is generally safe, processing identifiable data requires a Data Processing Agreement, and highly sensitive data requires a prior assessment with the LUMI team.

---

Q: Which tools does the guide recommend for moving data onto LUMI?
- [x] The "Upload" button in the LUMI Web Interface for small files.
- [x] `scp` or `rsync` from your Terminal for large datasets.
- [ ] Mailing a hard drive directly to the CSC data centre.
- [ ] Installing a cloud syncing service like Dropbox directly on LUMI.
> For small files, the Web Interface is easiest. For large datasets, `scp` or `rsync` are the reliable, professional tools of choice.

---

Q: Instead of using `pip install` to build your environment directly on the Lustre file system, LUMI recommends using Apptainer. What is the primary benefit of Apptainer on a supercomputer?
- [x] It packages your entire environment into a single `.sif` file, bypassing Lustre's weakness with thousands of tiny files.
- [ ] It automatically converts Python code to C++ for faster execution.
- [ ] It is the only way to request GPU resources from Slurm.
- [ ] It provides a graphical desktop interface.
> Lustre hates millions of tiny files. Apptainer solves this by bundling them into one large, efficient file.

---

Q: You found a great open-source AI model on GitHub. What is the recommended way to get that code onto LUMI?
- [ ] Download it as a `.zip` file on your laptop, then use the Web Interface to upload it.
- [ ] Email the code to the LUMI support team.
- [x] Use `git clone` directly in your LUMI Terminal to download the Repository.
- [ ] Copy and paste the code manually into a new `nano` file.
> `git clone` is the fastest and easiest way to pull code, and it makes updating it later with `git pull` a breeze!

---

Q: You submit a Batch Job to Slurm, and it finishes almost immediately. Where is the first place you should look to see what went wrong?
- [ ] The LUMI Web Interface dashboard.
- [ ] Your `$HOME` directory.
- [x] The `slurm-<jobid>.out` file generated in the directory where you submitted the Job.
- [ ] The `/tmp` folder on the Compute Node.
> Slurm writes all the standard output and errors from your script into the `.out` file. It's your primary debugging tool!
```
