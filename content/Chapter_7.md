---
title: "Chapter 7: Ordering Compute Power (The Slurm Workload Manager)"
nav_order: 7
---

# Chapter 7: Ordering Compute Power (The Slurm Workload Manager)
Now you have all the pieces of the puzzle: your data is uploaded (Chapter 4), your software% Container is ready and you know how to use it (Chapter 5), and your AI code has been cloned from GitHub (Chapter 6). 

But if you try to run your AI script on the Login Node right now, the system will stop it. To actually run your code on LUMI's powerful GPUs%, you need to ask the system's supervisor for permission and resources. That supervisor is called **Slurm%**.


## 📋 What is Slurm and Why Do We Need It?
LUMI is shared by thousands of users. If everyone clicked "Run" at the exact same time, the supercomputer would crash. 

Slurm is a **Workload Manager** (often called a **Job Scheduler**). Think of Slurm like the manager of a massive, highly sought-after commercial kitchen. 
1. You don't just walk into the kitchen and start cooking whenever you want. 
2. Instead, you submit a **booking form** (a **Batch Script**%) to the manager (Slurm). On this form, you specify exactly what equipment you need (e.g., 4 ovens for 2 hours) AND provide the step-by-step recipe (commands) you want executed.
3. The manager looks at the schedule. As soon as the exact equipment you requested becomes available, they execute your recipe and save the final dish (your output results) for you to pick up later.

The beauty of this system is that you do not need to sit at your computer waiting. Once you hand your ticket to Slurm, you can log out of LUMI, close your laptop, and go have dinner. LUMI will start running your Job as soon as enough free resources (GPUs, CPUs, etc.) are available for your Job. It will save all the outputs of the script into a text file for you to read later.


## 📝 The Anatomy of a Slurm Ticket (Batch Script)
To talk to Slurm, we write a simple text file (ending in `.sh`). This file always starts with a standard script header (`#!/bin/bash`) and contains two main sections right after it:
1. Special instructions for Slurm: lines starting with `#SBATCH` that tell Slurm exactly what resources your Job is asking to book and use.
2. The actual Command Line% commands: the specific instructions to load your environment, set necessary Environment Variables%, and run e.g., a Python AI script from within a Container.

Here is what a simple AI script may look like:

```sh
#!/bin/bash
#SBATCH --account=project_462xxxxxx      # Identifies your organisation's project
#SBATCH --partition=small-g              # Choosing the Slurm Partition and the hardware Partition (-g for GPU)
#SBATCH --nodes=1                        # Requesting 1 physical server node
#SBATCH --ntasks-per-node=1              # Run one instance ('task') of the script at a time 
#SBATCH --cpus-per-task=14               # The number of CPU cores, in this case per task
#SBATCH --gpus-per-node=2                # Requesting 2 AMD GCDs (1 full GPU) on that node
#SBATCH --mem-per-gpu=60G                # 60GB of RAM per GPU
#SBATCH --time=02:00:00                  # Asking for 2 hours of compute time


# 1. Remove any previously loaded Modules and load the necessary ones
module purge
module use /appl/local/laifs/modules
module load lumi-aif-singularity-bindings


# 2. Set necessary Environment Variables
MIOPEN_DIR=$(mktemp -d)
export MIOPEN_CUSTOM_CACHE_DIR=$MIOPEN_DIR/cache
export MIOPEN_USER_DB=$MIOPEN_DIR/config


# 3. Run your Python AI script from inside the Container
srun singularity run /appl/local/laifs/containers/lumi-multitorch-latest.sif python3 run_ai.py
```

> [!note] Reminder: GPUs vs. GCDs in Slurm
> Notice the `--gpus-per-node=2` flag above? Remember from Chapter 4 that Slurm considers each **GCD**% as one GPU. So requesting 2 "GPUs" gives you exactly 1 physical MI250X chip.


## 🎫 Handing the Ticket to Slurm
LUMI AI Guide and examples of AI scripts usually contain this Slurm script and you only need to edit it with your actual `project_` which will be 'billed' for the Job.
Once you've edited this file (let's call it `run_ai.sh`), you submit it to the queue using the `sbatch` command in your Terminal:

```bash
sbatch run_ai.sh
```


## 🏋️ Exercise: Run Your First AI Job on LUMI

First, let's create the Batch Script%. Navigate to your project's `/scratch`% directory% and create a new file named `my_first_slurm_script.sh` (you can use `nano` like we did in Chapter 3). Copy and paste the following code, making sure to replace `project_462xxxxxx` with your actual project ID.

```sh
#!/bin/bash
#SBATCH --account=project_462xxxxxx      # Identifies your organisation's project
#SBATCH --partition=dev-g                # Choosing the Slurm Partition and the hardware Partition (-g for GPU)
#SBATCH --nodes=1                        # Requesting 1 physical server node
#SBATCH --ntasks-per-node=1              # Run one instance ('task') of the script at a time 
#SBATCH --cpus-per-task=14               # The number of CPU cores, in this case per task
#SBATCH --gpus-per-node=2                # Requesting 2 AMD GCDs (1 full GPU) on that node
#SBATCH --mem-per-gpu=60G                # 60GB of RAM per GPU
#SBATCH --time=00:30:00                  # Asking for 30 minutes of compute time


# 1. Remove any previously loaded Modules and load the necessary ones
module purge
module use /appl/local/laifs/modules
module load lumi-aif-singularity-bindings # Bindings give LUMI Containers access to the file system of the Working Directory


# 2. Set necessary Environment Variables
MIOPEN_DIR=$(mktemp -d)
export MIOPEN_CUSTOM_CACHE_DIR=$MIOPEN_DIR/cache
export MIOPEN_USER_DB=$MIOPEN_DIR/config

# We use the PyTorch Container provided by the LUMI AI Factory Services, which contains vLLM - an engine library for running LLMs.
export SIF=/appl/local/laifs/containers/lumi-multitorch-u24r70f21m50t210-20260415_130625/lumi-multitorch-full-u24r70f21m50t210-20260415_130625.sif

# Redirect all vLLM cache files from $HOME to scratch.
export VLLM_CACHE_ROOT=/scratch/$SLURM_JOB_ACCOUNT/vllm-cache

# Where the models are downloaded (the 'weights'). $SLURM_JOB_ACCOUNT automatically finds your project ID on LUMI. 
export HF_HOME=/scratch/$SLURM_JOB_ACCOUNT/hf-cache/

# Choose the LLM to run (from https://huggingface.co/ )
export MODEL_NAME="Qwen/Qwen3.6-35B-A3B"


# 3. Run the python script inside the Container
srun singularity run $SIF python3 generate_text.py
```

Like earlier, the Slurm script books resources, sets the Environment Variables% necessary for the AI code, and starts a script called `generate_text.py`. For that to work, the Python script needs to exist in the exact same directory. **Create a new file** named `generate_text.py` and paste the following code into it:

```python title="generate_text.py"
import os
from vllm import LLM, SamplingParams

def main():
    # Read the variable we exported in the Slurm script
    model_name = os.environ.get("MODEL_NAME")
    
    print(f"Loading {model_name}...")
    # Initialise the model (vLLM will automatically distribute it across our 2 requested GCDs)
    # We use load_format="runai_streamer" to speed up weight loading using the Run:ai Model Streamer
    llm = LLM(model=model_name, tensor_parallel_size=2, load_format="runai_streamer")
    
    # Set up our prompt
    prompts = ["What is the LUMI supercomputer?"]
    sampling_params = SamplingParams(temperature=0.7, max_tokens=1000)
    
    print("Generating text...")
    outputs = llm.generate(prompts, sampling_params)
    
    # Save the generated text to the project's scratch directory
    with open("./my_first_ai_output.txt", "w") as f:
        f.write(outputs[0].outputs[0].text)

if __name__ == "__main__":
    main()
```


### Step 1: Submit the Job
Finally, submit the script to the queue using `sbatch`:

```bash
sbatch my_first_slurm_script.sh
``` 
The Terminal will respond with something like: `Submitted batch job 1234567`. Congratulations, your Job is officially in the Slurm queue!

> [!note] Job submission errors
> If you see an error message after submitting, check the [troubleshooting guide for common Batch Job errors](https://docs.csc.fi/support/faq/why-does-my-batch-job-fail/).


### Step 2: Monitor the Queue
You can check the status of your Job using the `squeue` command:

```bash
squeue --me
```
If your Job is running, you will see `R` under the `ST` (State) column. If it is waiting for resources, you will see `PD` (Pending).


### Step 3: Check the Live Output
Unlike your laptop where text prints directly to your screen, Slurm captures everything your Python script "prints" and saves it to a log file in the same directory. This log file will be named `slurm-<your_job_id>.out` (e.g., `slurm-1234567.out`).

To watch the model weights load in real-time, you can "follow" the log file:
```bash
tail -f slurm-<your_job_id>.out
```
*(Press `Ctrl+C` to stop watching the file. This doesn't stop the Job!)*


### Step 4: Review the Final Results
Once the Job finishes, it will disappear from `squeue`. If you are watching the live log with `tail -f`, you'll know it's done when the text stops flowing and you see a "Shutdown complete" message. You can then read the complete log file:
```bash
less slurm-<your_job_id>.out
```

More importantly, your Python script created a brand new file with the AI's answer! Let's read it:
```bash
less my_first_ai_output.txt
```


> [!tip] Cancelling a Job
> Mistakes happen — maybe you spotted a bug in your script right after submitting, or you realised you requested the wrong resources. To cancel a Job, use `scancel` with the Job ID that `sbatch` gave you:
>
> ```bash
> scancel <your_job_id>
> ```


## 💬 Interactive Jobs: Running Code and AI Models in Real-Time
Writing a script and waiting in a queue can be frustrating if you are just trying a new model or testing if it runs properly. For this, Slurm also offers **Interactive Jobs**%. 

Instead of submitting a Batch Script to run later, you ask Slurm to give you a live connection to a node right now.

To spin up a quick, 30-minute test environment on a GPU node, you use `srun`:

```bash
srun --partition=dev-g --nodes=1 --ntasks-per-node=1 --cpus-per-task=14 --gpus-per-node=2 --mem-per-gpu=60G --time=00:30:00 --account=project_462xxxxxx --pty bash
```
Once Slurm grants you the requested resources, your Command Line prompt will change from the Login Node (such as `uan01`) to the Compute Node `nid002134` where all the code you run will use the GPUs and the rest of the hardware% you requested. You can now run commands interactively.

Let's test it by checking our allocated GPUs. Run this command:
```bash
rocm-smi
```
This will print out the status of the 2 GCDs (GPUs) that Slurm just gave you! You can now run Python scripts, test code, or download models interactively.

When you are finished testing, always type `exit` or press `Ctrl+D` to close your session and release your allocated resources!

[👉 Learn more about Interactive Usage on LUMI](https://docs.lumi-supercomputer.eu/runjobs/scheduled-jobs/interactive/)


## 💰 How Billing Works on LUMI
Compute power on LUMI isn't billed in euros — it's billed in **Billing Units (BUs)**. When your project was granted access to LUMI, it received a specific allocation of BUs. Every Job you run spends from that allocation.

BUs come in two currencies: **GPU-hours** (for LUMI-G) and **CPU-hours** (for LUMI-C). Since GPUs are the most valuable resource on LUMI, GPU-hours are significantly more expensive.

[👉 How to check your remaining Billing Units and storage quotas](https://docs.lumi-supercomputer.eu/runjobs/lumi_env/dailymanagement/)

- **What you're billed for.** You are billed for the resources that have been allocated to you, not the part of the resources you've actually used (such as 20% GPU utilisation). If your script reserves 4 GCDs but your code only utilises 1, you still pay for all 4 for the duration of your Job. However, if you request 2 hours of walltime but your Job finishes in 1 hour, the allocated resources are released and you are only **billed for the 1 hour**.

> [!warning] Efficient resource usage is your responsibility
> Always match your resource requests to what your workload actually needs. Running a tiny model on many GPUs wastes your project's billing allocation. You're billed for allocated resources regardless of how efficiently you have been utilising them.

- **Negative Billing Units.** If your project runs out of Billing Units while a Job is running, the Job will be allowed to finish and your project may go into negative billing units. You will not be charged for this negative balance.

- **GPU Billing Rates.**
In standard-g (full-node allocation): each LUMI-G node contains 4 MI250X GPUs (8 GCDs), so 1 node-hour equals 4 GPU-hours. GPU-hours are counted based on physical GPUs, 1 GCD = 0.5 GPU. For example, if you allocate 4 nodes and your Job runs for 24 hours:

$$
4 \text{ nodes} \times 4 \text{ GPU-hours/node} \times 24 \text{ hours} = 384 \text{ GPU-hours}
$$

In `small-g` and `dev-g` (per-GCD allocation, not full node): each GCD is billed at 0.5 GPU-hours per hour. 

> [!info] LUMI-G Slice logic
> You're effectively billed per "proportion" of the node you're using. A LUMI-G node is split into 8 equal slices. Each 1/8th slice contains:
> - **1 GCD** (half a physical GPU)
> - **7 CPU cores**
> - **60 GB of RAM**
> 
> You should always request resources in multiples of these full slices (e.g., 2 GCDs, 14 CPU cores, 120 GB RAM). If you request more CPU cores or memory than what fits in your GPU slices, you will be billed for the extra slices!
> 
> **Example:** If you request 1 GCD but ask for **180 GB of RAM** for 1 hour, your RAM request spans across 3 full slices (180 / 60 = 3). You will be billed for **3 slices (1.5 GPU-hours)**, even though you only used 1 GCD!

- **CPU Billing Rates.**
For **LUMI-C** CPU nodes, the slice logic is different from GPU nodes. A CPU slice is **1 CPU core and 2 GB of RAM**. You are billed based on whichever is higher: the number of cores requested or the amount of memory requested divided by 2 GB.


- **Walltime is a hard limit:** If you ask Slurm for 2 hours (`#SBATCH --time=02:00:00`), but your Job isn't complete when time runs out, the Job will be strictly terminated. Always give your Jobs a little bit of "buffer time" to ensure they complete cleanly!

To find out more about GPU, CPU and storage billing:

[👉 Read the official Breakdown of LUMI Billing Policies](https://docs.lumi-supercomputer.eu/runjobs/lumi_env/billing/)

## ✅ Summary Checklist
- You understand how Slurm schedules Jobs.
- You can write a basic Slurm Batch Script.
- You know how to submit, monitor, and review Jobs.
- You understand how LUMI billing works.

## 📝 Knowledge Check

```quiz
title: Chapter 7 Quiz

Q: What is Slurm?
- [ ] LUMI's high-speed storage filesystem.
- [x] The workload manager that schedules and allocates resources for your Jobs.
- [ ] The software Container used to run AI models.
- [ ] A tool for compiling Python code into GPU instructions.
> Slurm is the "manager" that takes your Batch Script, waits for the requested resources to become available, and then executes your commands on the Compute Nodes.

---

Q: Which of the following Slurm commands are correctly matched with their purpose?
- [x] `sbatch` -> Submits a Batch Script to the queue.
- [x] `squeue` -> Checks the status of queued or running Jobs.
- [ ] `srun` -> Cancels a running Job.
- [x] `srun` -> Starts an Interactive Job for real-time testing.
- [x] `scancel` -> Cancels a running or pending Job.
> `srun` is used to launch an Interactive Job (giving you a live Terminal on a Compute Node) or to launch parallel tasks within a Batch Script. To cancel a Job, use `scancel <job_id>`.

---

Q: In your Batch Script, you request `--gpus-per-node=2`. What hardware are you actually getting?
- [ ] Two complete physical MI250X GPU chips.
- [x] Two GCDs (which equals exactly one physical MI250X GPU chip).
- [ ] Two full LUMI-G nodes.
> Remember that Slurm counts each half of an MI250X (a GCD) as a single GPU.

---

Q: What happens if you request a walltime of 2 hours, but your script needs 2.5 hours to finish?
- [ ] Slurm automatically extends the time and bills you for the extra half hour.
- [ ] The script finishes successfully, but you are charged a penalty rate.
- [x] The Job is strictly terminated at the 2-hour mark, potentially losing unsaved progress.
> Walltime is a hard limit. Always add a buffer to your walltime to ensure your Job has time to finish cleanly!

---

Q: How does billing work if you request 1 GCD but ask for 180 GB of RAM for 1 hour?
- [ ] You are billed for 1 GCD (0.5 GPU-hours), because memory is free.
- [ ] You are billed for exactly the fraction of RAM you use during that hour.
- [x] You are billed for 3 full slices (1.5 GPU-hours).
> Because a single slice only contains 60 GB of RAM, requesting 180 GB forces Slurm to allocate 3 full slices to you (3 × 60 GB = 180 GB). You pay for the allocated resources, so you are billed for all 3 slices even though you only requested 1 GCD.

---

Q: How do you view the real-time "live" output of a running Slurm Job?
- [ ] By waiting for it to print directly to your Terminal screen.
- [ ] By running `cat slurm-<your_job_id>.out`.
- [x] By using `tail -f slurm-<your_job_id>.out`.
- [ ] By using the `squeue` command.
> Slurm redirects everything your script prints into a log file named `slurm-<your_job_id>.out`. The `tail -f` command lets you "follow" this file and see new text as it is written in real-time.

---

Q: You just finished testing your code in an Interactive Job (`srun ... --pty bash`). What is the correct way to release the GPUs so you stop getting billed?
- [ ] Close your laptop lid.
- [x] Press `Ctrl+D`.
- [ ] Do nothing, the Job will eventually time out.
- [x] Type `exit`.
> Closing your laptop or pressing `Ctrl+C` does NOT stop an interactive session! It will keep running in the background and burning your Billing Units until it hits its maximum walltime. You must use `exit` or `Ctrl+D` to terminate the session cleanly.

---

Q: If your script requests 4 GCDs but your code only ends up utilising 1 GCD (e.g., 25% GPU utilisation), how are you billed?
- [ ] You are billed for 1 GCD because Slurm detects the actual usage.
- [x] You are billed for all 4 GCDs for the duration of the Job.
- [ ] You are billed for 1 GCD but charged a penalty fee.
> You are always billed for what you **allocate**, regardless of how efficiently your code actually uses it. It is your responsibility to match your resource requests to your workload!
```