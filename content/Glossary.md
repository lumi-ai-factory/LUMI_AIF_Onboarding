---
title: "Glossary"
nav_order: 9
---

# Glossary

A quick reference for all technical terms used in this guide, listed in the order they appear.

---

## Chapter 1: Welcome

| Term | Definition |
|:-----|:-----------|
| **Supercomputer** | A very powerful computing system made up of thousands of interconnected computers (nodes) working together. |
| **Command Line** | A text-based interface where you type commands to interact with a computer, instead of clicking with a mouse. |
| **HPC (High-Performance Computing)** | The use of supercomputers and parallel processing to solve large computational problems. |
| **Terminal** | The application on your computer that provides access to the command line (Terminal on Mac/Linux, PowerShell on Windows). |

---

## Chapter 2: The Keys to the Castle

| Term | Definition |
|:-----|:-----------|
| **SSH (Secure Shell)** | A protocol for securely connecting to a remote computer over the internet. |
| **SSH Key Pair** | A pair of cryptographic keys (one public, one private) used to authenticate your identity when connecting to LUMI. |
| **Private Key** | The secret half of your SSH key pair. It stays on your computer and should never be shared. |
| **Public Key** | The shareable half of your SSH key pair. You upload it to the LUMI portal so LUMI can verify your identity. |
| **Passphrase** | A password that protects your private key, adding a second layer of security. |

---

## Chapter 3: The Command Line

| Term | Definition |
|:-----|:-----------|
| **Script** | A list of commands saved in a file that LUMI can execute automatically, even while you're away. |
| **Shell** | The program that interprets and executes your typed commands. |
| **CLI (Command Line Interface)** | Another name for the command line — a text-based way to interact with a computer. |
| **Directory** | The command-line term for "folder." |
| **Working Directory** | The directory you are currently "inside" in the command line. Shown by the `pwd` command. |
| **`$HOME`** | Your personal user directory on LUMI, located at `/users/<username>`. Where you land after logging in. |
| **Parent Directory** | The directory one level "above" the current one. Reached with `cd ..`. |

---

## Chapter 4: Navigating LUMI

| Term | Definition |
|:-----|:-----------|
| **Node** | An individual computer within the LUMI supercomputer. |
| **Login Node** | A shared "lobby" node where you manage files and submit jobs. Not for heavy computation. Named `uan01`, `uan02`, etc. |
| **Compute Node** | A powerful node reserved exclusively for running your jobs. Named like `nid007628`. |
| **Job** | Any task you ask the supercomputer to run — a script that trains a model, processes data, or runs a simulation. |
| **CPU (Central Processing Unit)** | The general-purpose processor of a computer. Handles complex tasks sequentially. |
| **GPU (Graphics Processing Unit)** | Device designed for massive parallel computation. Essential for AI. |
| **Partition** | A group of nodes with specific hardware and resource limits (e.g., `small-g`, `dev-g`). |
| **`/project`** | Shared project storage (50 GB). Good for files the whole team needs. |
| **`/scratch`** | High-capacity temporary storage (50 TB). Your main working area for datasets, outputs, and checkpoints. |
| **`/flash`** | Ultra-fast temporary storage (2 TB). 3x more expensive — use only when your job needs very fast I/O. |
| **LUMI-C** | The CPU partition of LUMI — nodes with powerful processors but no GPUs. |
| **LUMI-G** | The GPU partition of LUMI — nodes equipped with AMD MI250X GPUs for AI workloads. |
| **Walltime** | The maximum duration a job is allowed to run before it is stopped. |
| **I/O (Input/Output)** | The speed of reading from and writing to a storage drive. |
| **GCD (Graphics Compute Die)** | One independent half of an AMD MI250X GPU chip. Each MI250X contains 2 GCDs, each with its own 64 GB of HBM. Slurm treats each GCD as a separate GPU. |

---

## Chapter 5: AI Software on LUMI

| Term | Definition |
|:-----|:-----------|
| **Lustre** | LUMI's high-performance filesystem, optimized for large files but sensitive to many small files. |
| **Container** | A single `.sif` file that packages all your software, libraries, and dependencies into one "box." |
| **Apptainer (Singularity)** | The container tool used on LUMI. A secure, supercomputer-friendly alternative to Docker. |
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
| **Repository (repo)** | A project folder on GitHub that contains code, configuration files, and history of all changes. |
| **`git clone`** | Downloads a repository from GitHub to your LUMI storage. |
| **`git pull`** | Updates your local copy of a repository with the latest changes from GitHub. |

---

## Chapter 7: Slurm

| Term | Definition |
|:-----|:-----------|
| **Slurm** | The job scheduler that manages who gets to use which compute nodes and when. |
| **Batch Job Script** | A text file containing Slurm options and commands that define what resources you need and what to run. |
| **`sbatch`** | The command to submit a batch job script to Slurm. |
| **`squeue`** | The command to check the status of your queued or running jobs. |
| **Interactive Job** | A job where you get a live terminal session on a compute node for real-time work (debugging, testing). |
| **Billing Unit (BU)** | The unit of currency for LUMI compute time. Different resources (CPU, GPU, storage) cost different amounts of BUs. |
| **`srun`** | The command to start an interactive job or to launch a task within an allocated job. |
| **GPU-hours / CPU-hours** | The two currencies of Billing Units on LUMI. GPU-hours are for LUMI-G, CPU-hours are for LUMI-C. |
