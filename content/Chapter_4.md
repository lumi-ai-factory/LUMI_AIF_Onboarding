---
title: "Chapter 4: Navigating LUMI (nodes and storage)"
nav_order: 4
---

# Chapter 4: Navigating LUMI (nodes and storage)

Think of LUMI not as one giant entity, but as a massive *collection* of computers connected by an incredibly fast network.

> [!info] Deep dive: Why supercomputing?
> You might wonder why we connect thousands of computers together instead of just building one giant one. To learn about how supercomputers use **parallel processing** to solve massive AI challenges (and how they differ from your laptop), check out the article: [Why supercomputing and LUMI?](https://lumi-ai-factory.eu/articles/blog-why-supercomputing-and-lumi/) **(Optional)**


## The "two-room" rule: Login vs. compute
When you log in via SSH (as we did in Chapter 2), you aren't actually on the "super" part of the supercomputer yet. You are in the **Lobby**.

### 1. The Login Nodes (The lobby)
The Login Nodes are shared by hundreds of people at once. Login Nodes are named `uan01`, `uan02`, etc.
*   **What to do here:** Organise files, write code in `nano`, check your project balance, and submit your "work orders" (Jobs).
*   **What NOT to do:** Do not run anything computationally heavy, such as AI training or heavy data processing here. If the "Lobby" gets too crowded or someone starts heavy machinery there, it slows down for everyone. LUMI has automatic guards that will stop your processes if they use too much power in the Lobby. For such heavy processes, such as AI workloads, we use the Compute Nodes.

### 2. The Compute Nodes (The factory room)
This is where the magic happens. These are thousands of individual computers equipped with world-class GPUs and CPUs.
*   **How to get there:** You cannot "log in" to a Compute Node% directly. Instead, you "book" time on them using the Slurm scheduler (which we will cover in Chapter 7). Unlike the Login Node%, you don't share CPUs and GPUs with other users; the resources you request are exclusively yours for the duration of your Job.

> [!note] Finding your location
> You can see your current location from the Command Line: 
> ![uan4 Command Line](./assets/lumi_uan4.png) 
> The `@uan4` in the Command Line indicates that you are on that Login Node. Compute Nodes will have longer names like `nid007628`.


## Choosing your engine: CPU vs. GPU

**What is a CPU?** The CPU (Central Processing Unit) is the general-purpose brain of a computer. Think of it as a small team of brilliant professors - it can solve very complex problems, but only a few at a time, one after another.

**What is a GPU?** The GPU (Graphics Processing Unit) is more like a stadium full of students, each doing simple arithmetic simultaneously. Originally designed to calculate the colour of millions of pixels at once for video games, researchers discovered that this massive parallel math power is exactly what AI needs. While you can train AI models on a CPU, it would be impractically slow - a Job that takes hours on GPUs could take weeks on CPUs.

LUMI is divided into several "Partitions%" (sections) depending on what kind of hardware% or service you need. The two main Partitions are:

*   **LUMI-C (The CPU% Workhorse):** These nodes% have powerful processors but no GPUs%. They are great for data preprocessing, traditional statistics, or "cleaning" your data before the AI training begins.
*   **LUMI-G (The GPU% Powerhouse):** This is where the AI happens. It contains 11,000 **AMD MI250X GPUs%**. If you are training or using Large Language Models (LLMs) or computer vision models, this is where you want to be.

> [!info] Other specialised Partitions
> While this guide focuses on LUMI-C and LUMI-G, the supercomputer also has:
> - **[LUMI-D (Data Analytics)](https://docs.lumi-supercomputer.eu/hardware/lumid/):** Specialised nodes with massive amounts of memory (up to 4TB per node).
> - **[LUMI-K (Container Cloud)](https://docs.lumi-supercomputer.eu/runjobs/lumi-k/getting-started/lumi_k_what_is/):** A Kubernetes environment for running persistent web services, databases, or APIs instead of standard compute Jobs.

Within each of those hardware Partitions%, the Slurm scheduler further divides resources into smaller **Slurm Partitions** based on Job size and duration.

**LUMI-C (CPU% Partitions)**
| Name | Max walltime% (duration) | Max Jobs% | Max resources/job | Purpose |
| :--- | :--- | :--- | :--- | :--- |
| standard | 2 days | 120 | 512 nodes | Larger Jobs |
| small | 3 days | 220 | 4 nodes | Small or memory intense Jobs |
| debug | 30 minutes | 2 | 4 nodes | Debugging and testing |

> [!note] What is a Job?
> **A Job** is any task you ask the supercomputer to run - a script that trains a model, processes a dataset, or runs a simulation. You write the instructions, submit them, and LUMI executes them for you.

**LUMI-G (GPU% Partitions)**
| Name | Max walltime% (duration) | Max Jobs% | Max resources/job | Purpose |
| :--- | :--- | :--- | :--- | :--- |
| standard-g | 2 days | 210 | 1024 nodes | Larger Jobs |
| small-g | 3 days | 210 | 4 nodes | Small GPU Jobs |
| dev-g | 2 hours | 2 | 8 nodes | Debugging |

[Full list of Slurm Partitions](https://docs.lumi-supercomputer.eu/runjobs/scheduled-jobs/partitions/) **(Optional)**

### Exclusive vs. shared nodes
When you book resources from the `standard-g` Partition, Slurm allocates **entire physical nodes exclusively to you**. You are the only person running code on that machine, and you are billed for all 8 GPUs on it, even if your code only uses 1.

However, if you only need 1 or 2 GPUs for a smaller task, booking a whole node would be a massive waste of resources and credits. This is where `small-g` and `dev-g` come in. These are **shared Partitions**. When you request just 1 GPU on `small-g`, Slurm might place your Job on the exact same physical server as another user who requested 2 GPUs. You are "sharing" the node, meaning you are both running programs on the exact same underlying operating system at the exact same time!

### Inside a LUMI-G Compute Node (the hardware%)

When you book a full GPU node on LUMI-G, here is exactly what you get inside that single physical server:

*   **1 × 64-core CPU:** An AMD EPYC processor that acts as the manager, handling data loading and feeding it to the GPUs. However, out of these 64 cores, only 56 are available for you to use as the rest are reserved for the operating system. 
*   **8 × 64GB DDR4 RAM:** totalling 512GB per node, used by the CPU to load datasets, models, run data preprocessing and hold information before sending it to the GPU. However, only 60GB per stick (480GB total) is available to you as the rest is reserved for the operating system.
*   **4 × AMD MI250X GPUs:** The incredibly powerful accelerators where your AI models are actually trained.
*   **Zero Local Storage:** There are no hard drives inside the Compute Nodes - all your data is accessed directly via LUMI's massive, high-speed network storage.

![LUMI-G Hardware](./assets/LUMI_G_Hardware.svg)

#### The "GPU vs. GCD" confusion (important!)
This is one of the most important concepts to understand about LUMI's hardware%. 

Each AMD MI250X is physically one large chip, but inside, it is actually split into **two independent halves** called **Graphics Compute Dies (GCDs%)**. Each GCD acts as its own separate GPU with its own 64 GB of dedicated video memory (VRAM).

Because they operate independently, the software% and the Slurm% scheduler (covered in Chapter 7) treat every GCD as a completely separate GPU. 

*   1 physical MI250X chip = **2 GCDs**
*   1 full LUMI-G Node (4 physical chips) = **8 GCDs**

**Why does this matter?** Because when Slurm% refers to "GPUs", it actually means "GCDs". If you look at a full LUMI-G node, Slurm will say it has **8 GPUs available**, not 4. When you request resources for your Job, remember: **1 Slurm GPU = 1 GCD = half of a physical MI250X.**

[LUMI-G (GPU Partition) hardware description](https://docs.lumi-supercomputer.eu/hardware/lumig/) **(Optional)**

[LUMI-C (CPU Partition) hardware description](https://docs.lumi-supercomputer.eu/hardware/lumic/) **(Optional)**


## Where does my data live? (Storage tiers)

On your laptop, everything is usually on one "C: Drive." On LUMI, storage is split into different tiers. Using the wrong one can make your AI training run much more slowly!

| Storage Name | Path | Intended Use | Limits (Quota / Max Files) | Retention | Billing Rate |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **User home** | `/users/<username>` | User home directory% (`$HOME`) for personal and configuration files | 20 GB / 100k | User lifetime | NA |
| **Project space** | `/project/<project>` | Project home directory for shared project files | 50 GB / 100k | Project lifetime | 1x |
| **Project scratch** | `/scratch/<project>` | Temporary storage for input, output or checkpoint data | 50 TB / 2000k | Project lifetime | 1x |
| **Project flash** | `/flash/<project>` | High performance temporary storage for input and output data | 2 TB / 1000k | Project lifetime | 3x |

> [!warning] Remove old data
> Please remove the files that are no longer needed by your project on a regular basis. If the storage space on LUMI gets too full at some point, automatic cleaning of project scratch and flash might be enabled. In this case information would be sent to LUMI users at least three months in advance. 

> [!note] Flash is fast but expensive
> Flash is 3x more expensive per TB because it uses faster NVMe drives. Use it only when your Job needs very fast Input and Output (I/O), i.e., the speed of reading from and writing to the drive.

[More info on LUMI Storage](https://docs.lumi-supercomputer.eu/storage/) **(Optional)**

### Jobs and data privacy
On a shared supercomputer, privacy behaves differently than on a personal computer:
*   **Storage Access:** Your home directory (`/users`) is private to you. However, project directories (`/project`, `/scratch`, `/flash`) are by default **fully visible and accessible** to all members of your project.
*   **Shared Compute Nodes:** When using shared Partitions (such as `small-g` or `dev-g`), other users running on the same node can see the *names* of the commands and programs you are running (like seeing `python my_script.py` in a task manager). They cannot see the actual contents of your files or the output of your programs.
*   **Job Details:** Information about your submitted Slurm Jobs (including Job names and submission paths) is visible to all users on the system via monitoring commands.

[Read the official LUMI Jobs and Data Privacy Guide](https://docs.lumi-supercomputer.eu/runjobs/lumi_env/privacy-on-system/) to understand visibility rules and best practices for securing sensitive information. **(Optional - recommended if you work with confidential code or data)**

## GDPR and sensitive data
Beyond internal system visibility, you must also consider legal compliance before uploading datasets. When it comes to personal and sensitive information, it is important to distinguish between two types of data processing:

1. **LUMI User Data:** LUMI processes personal data related to its users (such as names, organisational affiliations, and contact information) for account management and service operation. For this, LUMI acts as the Data Controller%. You can read more in the [Privacy Notice for processing of user data on the LUMI Service](https://lumi-supercomputer.eu/privacy-notice-for-processing-of-user-data-on-the-lumi-service/).
2. **Personal Data in User Datasets:** You might use LUMI to process datasets that contain personal data. In this context, your organisation acts as the GDPR% Data Controller%, and LUMI acts as the Data Processor%. As the Data Controller, your organisation is responsible for assessing whether the selected LUMI service provides an adequate level of security and compliance for your specific dataset. Depending on your dataset, the requirements differ:
   - **Anonymised Data:** Fully anonymised datasets are generally well suited for processing in LUMI as they fall outside the scope of GDPR%.
   - **Identifiable Data:** Identifiable personal data may be processed provided that appropriate security controls and contractual arrangements, including a Data Processing Agreement (DPA)% where applicable, are in place.
   - **Highly Sensitive Data (Special Categories):** You must exercise extreme caution before processing special categories of personal data, such as health or biometric records. While the Terms of Use may allow this under specific conditions, LUMI's shared architecture means that guaranteeing the secure deletion of data after processing is not straightforward. Therefore, if your project requires handling such sensitive information, you are strongly encouraged to contact the LUMI team in advance to assess your use case and establish the necessary safeguards before uploading any data.

[Read the LUMI General Terms of Use](https://lumi-supercomputer.eu/wp-content/uploads/2026/03/LUMI-General-Terms-of-Use_2026.pdf) **(Optional - read it before processing personal data)**

## Moving your data to LUMI

As an industry customer, you likely have your own datasets ready to go. To get them onto LUMI:

1.  **Small Files:** Use the "Upload" button in the [LUMI Web Interface](https://www.lumi.csc.fi).
2.  **Large Datasets:** Use `scp` or `rsync` from your Terminal. These tools are designed to handle gigabytes or even terabytes of data reliably.

> [!warning] Data encryption%
> Although data traffic is encrypted, data is **not** stored encrypted by default on LUMI. Sensitive data should therefore be encrypted before being added to the service.

[How to move your data to LUMI](https://docs.lumi-supercomputer.eu/firststeps/movingdata/)


## Summary checklist
- You understand the difference between Login Nodes and Compute Nodes.
- You understand the difference between LUMI-C and LUMI-G.
- You know which storage tier to use for your data.
- You understand how privacy works on shared nodes and directories.
- You know the rules for processing personal and sensitive data on LUMI.

## Knowledge check

```quiz
title: Chapter 4 Quiz

Q: What is the primary difference between a Login Node and a Compute Node?
- [x] Login Nodes are for managing files and submitting Jobs, while Compute Nodes are reserved for running computationally heavy tasks like AI training.
- [ ] Login Nodes are used for CPU processing, while Compute Nodes are used exclusively for GPUs.
- [ ] Login Nodes are private to you, while Compute Nodes are shared with all LUMI users.
- [ ] You run your AI scripts directly on the Login Node, and the Compute Node handles the data storage.
> You should never run heavy computations on the Login Node (the "Lobby"). You use it to organise your work and ask Slurm to run your heavy Jobs on the Compute Nodes.

---

Q: When you ask the Slurm scheduler for "1 GPU" on LUMI-G, what exact hardware are you allocated?
- [ ] One full AMD MI250X chip.
- [x] One GCD (Graphics Compute Die), which is exactly half of an AMD MI250X chip.
- [ ] A small slice of a single GCD.
- [ ] Four physical MI250X chips (a full node).
> Because the two halves of the MI250X chip (the GCDs) operate completely independently, Slurm treats every single GCD as its own separate GPU. 1 Slurm GPU = 1 GCD.

---

Q: You need to store 500 GB of dataset files and model weights for your AI training. Which storage directory is the best place for this data?
- [ ] Your user home directory (`/users/<username>`)
- [ ] The shared project directory (`/project/<project>`)
- [x] The project scratch directory (`/scratch/<project>`)
- [ ] The project flash directory (`/flash/<project>`)
> The `/scratch` directory is designed specifically as your main, high-capacity working area for datasets and checkpoints.

---

Q: Which of the following is true regarding data privacy on LUMI?
- [ ] Everything you do on LUMI is completely invisible to all other users.
- [ ] Your project's `/scratch` directory is private and only visible to you.
- [x] Project directories (like `/scratch`, `/project` and `/flash`) are visible to all members of your project, and Slurm Job details are visible to everyone on the system.
- [ ] Data stored on LUMI is automatically encrypted at rest by the system.
> Project spaces are shared among all project members by default, and information about running Jobs can be seen by any user monitoring the system. Data is not encrypted at rest by default.

---

Q: When you run a program on a shared Compute Node (like `small-g`), what can other users logged into that same node see?
- [ ] Absolutely nothing; your session is completely isolated.
- [ ] They can see the output of your programs and the contents of your files.
- [x] They can see the names of the commands you are running (e.g., `python train.py`), but not the file contents or the program's output.
- [ ] They can view your screen directly.
> Just like a task manager on your personal computer, shared nodes allow users to see the names of running processes, but your actual code and data contents remain secure from other users on the node.

---

Q: Under what conditions can you process datasets containing personal data on LUMI?
- [ ] You can upload any personal data since LUMI is a highly secure environment.
- [ ] You can only upload fully anonymised data; identifiable data is strictly prohibited.
- [ ] You can never upload special categories of personal data, such as health records.
- [x] You can process identifiable data if appropriate security controls and a Data Processing Agreement are in place.
> While fully anonymised data is generally well-suited, you can process identifiable data with the right contractual agreements. Highly sensitive data requires contacting the LUMI team in advance to assess the use case.
```
