---
title: "Chapter 7: Ordering Compute Power (The Slurm Workload Manager)"
nav_order: 7
---

# Chapter 7: Ordering Compute Power (The Slurm Workload Manager)
Now you have all the pieces of the puzzle: your data is uploaded (Chapter 4), your software container is ready and you know how to use it (Chapter 5), and your AI code has been cloned from GitHub (Chapter 6). 

But if you try to run your AI script on the login node right now, the system will stop it. To actually run your code on LUMI's powerful GPUs, you need to ask the system's supervisor for permission and resources. That supervisor is called **Slurm**.


## 📋 What is Slurm and Why Do We Need It?
LUMI is shared by thousands of users. If everyone clicked "Run" at the exact same time, the supercomputer would crash. 

Slurm is a **Workload Manager** (often called a **Job Scheduler**). Think of Slurm like an exceptionally organized restaurant kitchen manager. 
1. You don't walk into the kitchen and cook the food yourself. 
2. Instead, you write your request on a **ticket** (a **Batch Script**) and hand it to the manager (Slurm).
3. The manager looks at what resources your meal needs (e.g., 4 GPUs for 2 hours), checks which kitchen stations are empty (and 4 GPUs are available), and runs your order as soon as a slot opens up.

The beauty of this system is that you do not need to sit at your computer waiting. Once you hand your ticket to Slurm, you can log out of LUMI, close your laptop, and go have dinner. LUMI will start running your job as soon as enough free resources (GPUs, CPUs, etc.) are available for your job. It will save all the outputs of the script into a text file for you to read later.


## The Anatomy of a Slurm Ticket (Batch Script)
To talk to Slurm, we write a simple text file (ending in `.sh`). This file always starts with a standard script header (`#!/bin/bash`) and contains two main sections right after it:
1. Special instructions for Slurm: lines starting with `#SBATCH` that tell Slurm exactly what resources your job is asking to book and use.
2. The actual Command Line commands: the specific instructions to load your environment, set necessary variables, and run e.g., a Python AI script from within a container.

Here is what a simple AI script may look like:

```
#!/bin/bash
#SBATCH --project=project_462xxxxxx      # Identifies your organization's project
#SBATCH --partition=small-g              # Choosing the Slurm partition and the hardware partition (-g for GPU)
#SBATCH --nodes=1                        # Requesting 1 physical server node
#SBATCH --ntasks-per-node=1              # Run one instance ('task') of the script at a time 
#SBATCH --cpus-per-task=14               # The number of CPU cores, in this case per task
#SBATCH --gpus-per-node=2                # Requesting 2 AMD GCDs (1 full GPU) on that node
#SBATCH --mem-per-gpu=60G                # 60GB of RAM per GPU
#SBATCH --time=02:00:00                  # Asking for 2 hours of compute time


# 1. Remove any previously loaded modules and load the necessary ones
module purge
module use /appl/local/laifs/modules
module load lumi-aif-singularity-bindings


# 2. Set necessary environment variables
MIOPEN_DIR=$(mktemp -d)
export MIOPEN_CUSTOM_CACHE_DIR=$MIOPEN_DIR/cache
export MIOPEN_USER_DB=$MIOPEN_DIR/config


# 3. Run your Python AI script from inside the container
singularity run /appl/local/laifs/containers/lumi-multitorch-latest.sif run_ai.py
```

> [!note] Reminder: GPUs vs. GCDs in Slurm
> Notice the `--gpus-per-node=2` flag above? Remember from Chapter 4 that Slurm considers each **GCD** (half of a physical MI250X chip) as one GPU. So requesting 2 "GPUs" gives you exactly 1 physical MI250X chip.


## Handing the Ticket to Slurm
LUMI AI Guide and examples of AI scripts usually contain this Slurm script and you only need to edit it with your actual `--project=` which will be 'billed' for the job.
Once you've edited this file (let's call it `run_ai.sh`), you submit it to the queue using the `sbatch` command in your terminal:

```bash
sbatch run_ai.sh
```


## Exercise: run your first AI job on LUMI

First, let's create the batch script. Navigate to your project's `/scratch` directory and create a new file named `my_first_slurm_script.sh` (you can use `nano` like we did in Chapter 3). Copy and paste the following code, making sure to replace `project_462xxxxxx` with your actual project ID.

```bash title="my_first_slurm_script.sh"
#!/bin/bash
#SBATCH --project=project_462xxxxxx      # Identifies your organization's project
#SBATCH --partition=dev-g                # Choosing the Slurm partition and the hardware partition (-g for GPU)
#SBATCH --nodes=1                        # Requesting 1 physical server node
#SBATCH --ntasks-per-node=1              # Run one instance ('task') of the script at a time 
#SBATCH --cpus-per-task=14               # The number of CPU cores, in this case per task
#SBATCH --gpus-per-node=2                # Requesting 2 AMD GCDs (1 full GPU) on that node
#SBATCH --mem-per-gpu=60G                # 60GB of RAM per GPU
#SBATCH --time=02:00:00                  # Asking for 2 hours of compute time


# 1. Remove any previously loaded modules and load the necessary ones
module purge
module use /appl/local/laifs/modules
module load lumi-aif-singularity-bindings # bindings give LUMI containers access to the file system of the working directory


# 2. Set necessary environment variables
MIOPEN_DIR=$(mktemp -d)
export MIOPEN_CUSTOM_CACHE_DIR=$MIOPEN_DIR/cache
export MIOPEN_USER_DB=$MIOPEN_DIR/config

# We use the PyTorch container provided by the LUMI AI Factory Services, which contains vLLM - an engine library for running LLMs.
export SIF=/appl/local/laifs/containers/lumi-multitorch-u24r70f21m50t210-20260415_130625/lumi-multitorch-full-u24r70f21m50t210-20260415_130625.sif

# Redirect all vLLM cache files from $HOME to scratch.
export VLLM_CACHE_ROOT=/scratch/$SLURM_JOB_ACCOUNT/vllm-cache

# Where the models are downloaded (the 'weights'). $SLURM_JOB_ACCOUNT automatically finds your project ID on LUMI. 
export HF_HOME=/scratch/$SLURM_JOB_ACCOUNT/hf-cache/

# Choose the LLM to run (from https://huggingface.co/ )
export MODEL_NAME="Qwen/Qwen3.6-35B-A3B"


# 3. Run the python script inside the container
srun singularity run $SIF python3 generate_text.py
```

Like earlier, the Slurm script books resources, sets the environment variables necessary for the AI code, and starts a script called `generate_text.py`. For that to work, the Python script needs to exist in the exact same directory. **Create a new file** named `generate_text.py` and paste the following code into it:

```python title="generate_text.py"
import os
from vllm import LLM, SamplingParams

# Read the variable we exported in the Slurm script
model_name = os.environ.get("MODEL_NAME")

print(f"Loading {model_name}...")
# Initialize the model (vLLM will automatically distribute it across our 2 requested GCDs)
llm = LLM(model=model_name, tensor_parallel_size=2)

# Set up our prompt
prompts = ["The LUMI supercomputer is"]
sampling_params = SamplingParams(temperature=0.7, max_tokens=100)

print("Generating text...")
outputs = llm.generate(prompts, sampling_params)

# Save the generated text to the project's scratch directory
with open("./my_first_ai_output.txt", "w") as f:
    f.write(outputs[0].outputs[0].text)
```

### Step 1: Submit the job
Finally, submit the script to the queue using `sbatch`:

```bash
sbatch my_first_slurm_script.sh
``` 
The terminal will respond with something like: `Submitted batch job 1234567`. Congratulations, your job is officially in the Slurm queue!

> [!note]
> If you see an error message after submitting, check the [page of common batch job errors](https://docs.csc.fi/support/faq/why-does-my-batch-job-fail/).


### Step 2: Monitor the queue
You can check the status of your job using the `squeue` command:

```bash
squeue -u $USER
```
If your job is running, you will see `R` under the `ST` (State) column. If it is waiting for resources, you will see `PD` (Pending).


### Step 3: Check the live output
Unlike your laptop where text prints directly to your screen, Slurm captures everything your Python script "prints" and saves it to a log file in the same directory. It will be named `slurm-<your_job_id>.out` (e.g., `slurm-1234567.out`).

To watch the model weights load in real-time, you can "follow" the log file:
```bash
tail -f slurm-1234567.out
```
*(Press `Ctrl+C` to stop watching the file. This doesn't stop the job!)*


### Step 4: Review the final results
Once the job finishes (it will disappear from `squeue`), you can read the complete log file:
```bash
less slurm-1234567.out
```

More importantly, your Python script created a brand new file with the AI's answer! Let's read it:
```bash
less my_first_ai_output.txt
```


## Interactive Jobs: Running Code and AI Models in Real-Time
Writing a script and waiting in a queue can be frustrating if you are just trying a new model or testing if it runs properly. For this, Slurm offers **Interactive Sessions**.

Instead of submitting a Batch Script to run later, you ask Slurm to give you a live connection to a node right now. 

To spin up a quick, 30-minute test environment on a GPU node, you use srun:

```bash
srun --partition=dev-g --nodes=1 --ntasks-per-node=1 --cpus-per-task=14 --gpus-per-node=2 --mem-per-gpu=60G --time=00:30:00 --account=project_xxxxxxxxx --pty bash
```

Once Slurm grants you the requested resources, your command prompt will change from the Login Node (such as `uan01`) to the Compute Node `nid002134` where all the code you run will use the GPUs and the rest of the hardware you requested. You can now run commands interactively.


> [!warning]
> When you are finished testing, always type exit to release the node so other users can use it!

> [!warning]
> Note to self: need some interactive job exercise here. 

[👉 Learn more about Interactive Usage on LUMI](https://docs.lumi-supercomputer.eu/runjobs/scheduled-jobs/interactive/)


## 💰 How Billing Works on LUMI
Compute power on LUMI isn't billed in euros — it's billed in **Billing Units (BUs)**. When your project was granted access to LUMI, it received a specific allocation of BUs. Every job you run spends from that allocation.

BUs come in two currencies: **GPU-hours** (for LUMI-G) and **CPU-hours** (for LUMI-C). Since GPUs are the most valuable resource on LUMI, GPU-hours are significantly more expensive.

[👉 How to check your project's remaining Billing Units](https://docs.lumi-supercomputer.eu/runjobs/lumi_env/dailymanagement/)

- **What you're billed for.** You are billed for the resources that have been allocated to you, not the part of the resources you've actually used (such as 20% GPU utilisation). If your script reserves 4 GCDs but your code only utilizes 1, you still pay for all 4 for the duration of your job. However, if you request 2 hours of walltime but your job finishes in 1 hour, the allocated resources are released and you are only **billed for the 1 hour**.

> [!warning] Efficient resource usage is your responsibility. 
> Always match your resource requests to what your workload actually needs. Running a tiny model on many GPUs wastes your project's billing allocation. You're billed for allocated resources regardless of how efficiently you have been utilising them.

- **GPU Billing Rates.**
In standard-g (full-node allocation): each LUMI-G node contains 4 MI250X GPUs (8 GCDs), so 1 node-hour costs 4 GPU-hours. For example, if you allocate 4 nodes and your job runs for 24 hours:

```text
4 nodes × 4 GPU-hours/node × 24 hours = 384 GPU-hours
```

In `small-g` and `dev-g` (per-GCD allocation, not full node): each GCD is billed at 0.5 GPU-hours per hour. However, if you request more than 8 CPU cores or more than 64 GB of memory per GCD, you will be billed per additional slice of 8 cores or 64 GB.

> [!info] Slice logic
> You're effectively billed per "proportion" of the node you're using. GPU nodes are split into 8 equal parts. 


- **Walltime is a hard limit:** If you ask Slurm for 2 hours (`#SBATCH --time=02:00:00`), but your job isn't complete when time runs out, the job will be strictly terminated. Always give your jobs a little bit of "buffer time" to ensure they complete cleanly!

To find out more about GPU, CPU and storage billing:
[👉 Read the official Breakdown of LUMI Billing Policies](https://docs.lumi-supercomputer.eu/runjobs/lumi_env/billing/)