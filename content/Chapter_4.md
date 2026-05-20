---
title: "Chapter 4: Navigating LUMI (Nodes and Storage)"
nav_order: 4
---

# Chapter 4: Navigating LUMI (Nodes and Storage)

Think of LUMI not as one giant entity, but as a massive *collection* of computers connected by an incredibly fast network.


## The "Two-Room" Rule: Login vs. Compute
When you log in via SSH (as we did in Chapter 2), you aren't actually on the "super" part of the supercomputer yet. You are in the **Lobby**.

### 1. The Login Nodes (The Lobby)
The login nodes are shared by hundreds of people at once. Login nodes are named `uan01`, `uan02`, etc.
*   **What to do here:** Organize files, write code in `nano`, check your project balance, and submit your "work orders" (jobs).
*   **What NOT to do:** Do not run anything computationally heavy, such as AI training or heavy data processing here. If the "Lobby" gets too crowded or someone starts heavy machinery there, it slows down for everyone. LUMI has automatic guards that will stop your processes if they use too much power in the Lobby. For such heavy processes, use the Compute Nodes.

### 2. The Compute Nodes (The Factory room)
This is where the magic happens. These are thousands of individual computers equipped with world-class GPUs and CPUs.
*   **How to get there:** You cannot "log in" to a compute node directly. Instead, you "book" time on them using the Slurm scheduler (which we will cover in Chapter 7). Unlike the Login Node, you don't share CPUs and GPUs with other users; the resources you request are exclusively yours for the duration of your job.

> [!note]
> You can see your current location from the Command Line: 
> ![uan2 command line](./assets/uan2_command_line.png) 
> The `@uan2` in the command line indicates that you are that login node. Compute nodes will have longer names like `nid007628`.


## 🏎️ Choosing Your Engine: CPU vs. GPU

**What is a CPU?** The CPU (Central Processing Unit) is the general-purpose brain of a computer. It is designed to handle a few complex tasks very quickly, one after another in a sequence.
**What is a GPU?** The GPU (Graphics Processing Unit) is designed to handle thousands of simple tasks simultaneously. Originally created to calculate the color of millions of pixels on a screen all at once for video games, researchers realized that this ability to do massive simultaneous (parallel) calculations is exactly the kind of math required for Artificial Intelligence. While you technically *can* run AI applications on a CPU, it would be incredibly slow.

LUMI is divided into two main "partitions" (sections) depending on what kind of hardware you need:

*   **LUMI-C (The CPU Workhorse):** These nodes have powerful processors but no GPUs. They are great for data preprocessing, traditional statistics, or "cleaning" your data before the AI training begins.
*   **LUMI-G (The GPU Powerhouse):** This is the heart of the AI Factory. It contains 11,000 of **AMD MI250X GPUs**. If you are training or using Large Language Models (LLMs) or computer vision models, this is where you want to be.

Within those **hardware** partitions, there are something known as **Slurm partitions**. They are "subpartitions" of hardware partitions.

**LUMI-C (CPU partitions)**
| Name | Max walltime (duration) | Max jobs | Max resources/job | Purpose |
| :--- | :--- | :--- | :--- | :--- |
| standard | 2 days | 120 | 512 nodes | Larger jobs |
| small | 3 days | 220 | 4 nodes | Small or memory intense jobs |
| debug | 30 minutes | 2 | 4 nodes | Debugging and testing |

> [!note]
> **A job** is any task you ask the supercomputer to run — a script that trains a model, processes a dataset, or runs a simulation. You write the instructions, submit them, and LUMI executes them for you.

**LUMI-G (GPU partitions)**
| Name | Max walltime (duration) | Max jobs | Max resources/job | Purpose |
| :--- | :--- | :--- | :--- | :--- |
| standard-g | 2 days | 210 | 1024 nodes | Larger jobs |
| small-g | 3 days | 210 | 4 nodes | Small GPU jobs |
| dev-g | 2 hours | 2 | 8 nodes | Debugging |

[👉 Full list of LUMI Partitions](https://docs.lumi-supercomputer.eu/runjobs/scheduled-jobs/partitions/)

### 🖥️ Inside a LUMI-G Compute Node (The Hardware)

When you book a full GPU node on LUMI-G, here is exactly what you get inside that single physical server:

*   **1 × 64-core CPU:** An AMD EPYC processor that acts as the manager, handling data loading and feeding it to the GPUs. However, out of these 64 cores, only 56 are available fo you to use as the rest are reserved for the operating system. 
*   **8 × 64gb DDR4 RAM:** totaling 512GB per node used by the CPU to load datasets, models, run data preprocessing and hold information before sending it to the GPU.  
*   **4 × AMD MI250X GPUs:** The incredibly powerful accelerators where your AI models are actually trained.
*   **Zero Local Storage:** There are no hard drives inside the compute nodes! They are connected directly to LUMI's massive, high-speed network storage.

![LUMI-G Hardware](./assets/LUMI_G_Hardware.svg)

#### The "GPU vs. GCD" Confusion (Important!)
This is one of the most important concepts to understand about LUMI's hardware. 

Each AMD MI250X is physically one large chip, but inside, it is actually split into **two independent halves** called **Graphics Compute Dies (GCDs)**. Each GCD acts as its own separate GPU with its own 64 GB of dedicated video memory (HBM).

Because they operate independently, the software and the Slurm scheduler (covered in Chapter 7) treat every GCD as a completely separate GPU. 

*   1 physical MI250X chip = **2 GCDs**
*   1 full LUMI-G Node (4 physical chips) = **8 GCDs**

**Why does this matter?** Because when Slurm refers to "GPUs", it actually means "GCDs". If you look at a full LUMI-G node, Slurm will say it has **8 GPUs available**, not 4. When you request resources for your job, remember: **1 Slurm GPU = 1 GCD = half of a physical MI250X.**

[👉 LUMI-G (GPU partition) hardware description](https://docs.lumi-supercomputer.eu/hardware/lumig/)

[👉 LUMI-C (CPU partition) hardware description](https://docs.lumi-supercomputer.eu/hardware/lumic/)


## 📂 Where Does My Data Live? (Storage Tiers)

On your laptop, everything is usually on one "C: Drive." On LUMI, storage is split into different tiers. Using the wrong one can make your AI training run 10x slower!

| Storage Name | Path | Intended Use | Limits (Quota / Max Files) | Retention | Billing Rate |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **User home** | `/users/<username>` | User home directory (`$HOME`) for personal and configuration files | 20 GB / 100k | User lifetime | NA |
| **Project space** | `/project/<project>` | Project home directory for shared project files | 50 GB / 100k | Project lifetime | 1x |
| **Project scratch** | `/scratch/<project>` | Temporary storage for input, output or checkpoint data | 50 TB / 2000k | Project lifetime* | 1x |
| **Project flash** | `/flash/<project>` | High performance temporary storage for input and output data | 2 TB / 1000k | Project lifetime* | 3x |

> [!warning]
> **\*** - Please remove the files that are no longer needed by your project on a regular basis. If the storage space on LUMI gets too full at some point, automatic cleaning of project scratch and flash might be enabled. In this case information would be sent to LUMI users at least three months in advance. 

> [!note]
> Flash is 3x more expensive per TB because it uses faster NVMe drives. Use it only when your job needs very fast Input and Output (I/O), i.e., the speed of reading from and writing to the drive.

[👉 More info on LUMI Storage](https://docs.lumi-supercomputer.eu/storage/)


## 🚚 Moving Your Data to LUMI

As an industry customer, you likely have your own datasets ready to go. To get them onto LUMI:

1.  **Small Files:** Use the "Upload" button in the [LUMI Web Interface](https://www.lumi.csc.fi).
2.  **Large Datasets:** Use `scp` or `rsync` from your terminal. These tools are designed to handle gigabytes or even terabytes of data reliably.

[👉 How to move your data to LUMI](https://docs.lumi-supercomputer.eu/firststeps/movingdata/)


## Summary Checklist
- You understand the difference between login nodes and compute nodes.
- You understand the difference between LUMI-C and LUMI-G.
- You know which storage tier to use for your data.
