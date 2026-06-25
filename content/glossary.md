---
title: "Glossary"
nav_order: 10
---

# Glossary

A quick reference for all technical terms used in this guide, listed by chapter.

---

## Chapter 1: Welcome

| Term | Definition |
|:-----|:-----------|
| **supercomputer** | A very powerful computing system made up of thousands of interconnected computers (nodes) working together. |
| **Command Line** | A text-based interface where you type commands to interact with a computer, instead of clicking with a mouse. |
| **HPC (High-Performance Computing)** | The use of supercomputers and parallel processing to solve large computational problems. |
| **Terminal** | The application on your computer that provides access to the Command Line (Terminal on Mac/Linux, PowerShell on Windows). |
| **LLM** |  Large Language Model. An AI model with billions of parameters trained on massive text datasets to understand and generate language. |
| **GDPR** | General Data Protection Regulation. A European Union regulation concerning data protection and privacy in the EU and the EEA. |
| **Data Controller** | Under GDPR, the entity that determines the purposes and means of processing personal data. |
| **Data Processor** | Under GDPR, the entity that processes personal data on behalf of the data controller. |
| **Data Processing Agreement (DPA)** | A legally binding contract between a Data Controller and a Data Processor regulating the secure handling of personal data. |

---

## Chapter 2: The Keys to the Castle

| Term | Definition |
|:-----|:-----------|
| **SSH** | Secure Shell. A protocol for securely connecting to a remote computer over the internet. |
| **phishing** | A type of cyberattack where attackers deceive people into revealing sensitive information, such as passwords, by pretending to be a trustworthy entity. |
| **cryptographic** | A secure way of protecting information using mathematical algorithms so that only the right people can access it. |
| **SSH Key Pair** | A pair of cryptographic keys (one public, one private) used to authenticate your identity when connecting to LUMI. |
| **Private Key** | The secret half of your SSH key pair. It stays on your computer and should never be shared. |
| **Public Key** | The shareable half of your SSH key pair. You upload it to the LUMI portal so LUMI can verify your identity. |
| **passphrase** | A password that protects your private key, adding a second layer of security. |
| **string** | A sequence of characters, such as letters, numbers, and symbols, used in computing to represent text. |

---

## Chapter 3: The Command Line

| Term | Definition |
|:-----|:-----------|
| **script** | A list of commands saved in a file that LUMI can execute automatically, even while you're away. |
| **Shell** | The program that interprets and executes your typed commands. |
| **CLI (Command Line Interface)** | Another name for the Command Line — a text-based way to interact with a computer. |
| **directory** | The command-line term for "folder." |
| **Working Directory** | The directory you are currently "inside" in the Command Line. Shown by the `pwd` command. |
| **`$HOME`** | Your personal user directory on LUMI, located at `/users/<username>`. Where you land after logging in. |
| **Parent Directory** | The directory one level "above" the current one. Reached with `cd ..` |

---

## Chapter 4: Navigating LUMI

| Term | Definition |
|:-----|:-----------|
| **node** | An individual computer within the LUMI supercomputer. |
| **Login Node** | A shared "lobby" node where you manage files and submit jobs. Not for heavy computation. Named `uan01`, `uan02`, etc. |
| **Compute Node** | A powerful node reserved exclusively for running your jobs. Named like `nid007628`. |
| **Job** | Any task you ask the supercomputer to run — a script that trains a model, processes data, or runs a simulation. |
| **processor** | The "brain" of the computer that carries out instructions and calculations. Often used interchangeably with CPU. |
| **CPU** | Central Processing Unit. The general-purpose processor of a computer. Handles complex tasks sequentially. |
| **GPU** | Graphics Processing Unit. A specialised processor that is incredibly fast at doing many simple calculations at the same time. Essential for AI. |
| **hardware** | The physical components of a computer system, such as processors, memory, and storage drives. |
| **Partition** | A group of nodes with specific hardware and resource limits (e.g., `small-g`, `dev-g`). |
| **Slurm Partition** | A subdivision within a hardware partition that sets limits on job size, duration, and number of concurrent jobs (e.g., `standard`, `small-g`, `dev-g`). |
| **`/project`** | Shared project storage (50 GB). Good for files the whole team needs. |
| **`/scratch`** | High-capacity temporary storage (50 TB). Your main working area for datasets, outputs, and checkpoints. |
| **`/flash`** | Ultra-fast temporary storage (2 TB). 3x more expensive — use only when your job needs very fast I/O. |
| **LUMI-C** | The CPU partition of LUMI — nodes with powerful processors but no GPUs. |
| **LUMI-G** | The GPU partition of LUMI — nodes equipped with AMD MI250X GPUs for AI workloads. |
| **walltime** | The maximum duration a job is allowed to run before it is stopped. |
| **I/O (Input/Output)** | The speed of reading from and writing to a storage drive. |
| **GCD** | Graphics Compute Die. One independent half of an AMD MI250X GPU chip. Each MI250X contains 2 GCDs, each with its own 64 GB of VRAM. Slurm treats each GCD as a separate GPU. |
| **encryption** | A method of protecting data by converting it into a secret format. Only people with the correct password or key can unlock and read it. |

---

## Chapter 5: AI Software on LUMI

| Term | Definition |
|:-----|:-----------|
| **Lustre** | LUMI's high-performance filesystem, optimised for large files but sensitive to many small files. |
| **pip** | Python's standard package manager — a tool that automatically downloads and installs Python libraries from the internet. |
| **software** | The programs, applications, and scripts that run on the hardware to perform tasks. |
| **container** | A single `.sif` file that packages all your software, libraries, and dependencies into one "box." |
| **Apptainer** | The container tool used on LUMI. A secure, supercomputer-friendly alternative to Docker. |
| **Singularity** | The container tool used on LUMI. A secure, supercomputer-friendly alternative to Docker. |
| **Bindings** | Configuration that allows a container to communicate with LUMI's hardware (GPUs, high-speed network). Set up by loading the `lumi-aif-singularity-bindings` module. |
| **Runscript** | A set of default instructions baked into a container that define what happens when you `singularity run` it. |
| **`venv` (Virtual Environment)** | A lightweight Python environment you can create on top of a container to add extra libraries. |
| **Module** | A pre-installed software package on LUMI that you activate with `module load`. |

---

## Chapter 6: GitHub

| Term | Definition |
|:-----|:-----------|
| **GitHub** | The world's largest platform for hosting and sharing code. |
| **Git** | The version-control tool that tracks changes to code. Pre-installed for every user on LUMI. |
| **Repository** | Aka repo. A project folder on GitHub that contains code, configuration files, and history of all changes. |
| **`git clone`** | Downloads a repository from GitHub to your LUMI storage. |
| **`git pull`** | Updates your local copy of a repository with the latest changes from GitHub. |

---

## Chapter 7: Slurm

| Term | Definition |
|:-----|:-----------|
| **Slurm** | The job scheduler that manages who gets to use which compute nodes and when. |
| **Batch Script** | A text file containing Slurm options and commands that define what resources you need and what to run. |
| **`sbatch`** | The command to submit a batch script to Slurm. |
| **`squeue`** | The command to check the status of your queued or running jobs. |
| **`scancel`** | The command to cancel a running or pending job. Use `scancel <job_id>` or `scancel --me` to cancel all your jobs. |
| **Interactive Job** | A job where you get a live terminal session on a compute node for real-time work (debugging, testing). |
| **Billing Unit (BU)** | The unit of currency for LUMI compute time. Different resources (CPU, GPU, storage) cost different amounts of BUs. |
| **`srun`** | The command to start an interactive job or to launch a task within an allocated job. |
| **GPU-hours / CPU-hours** | The two currencies of Billing Units on LUMI. GPU-hours are for LUMI-G, CPU-hours are for LUMI-C. |
| **Environment Variable** | A background setting you define before running a program, which tells the program how it should behave (such as telling it which AI model to load or where to save files). |
| **vLLM** | A fast software engine used to run LLMs efficiently. It is the recommended way to use large language models on LUMI. |

