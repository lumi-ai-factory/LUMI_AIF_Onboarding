---
title: "Chapter 5: AI Software on LUMI (Modules and Containers)"
nav_order: 5
---

# Chapter 5: AI Software on LUMI (Modules and Containers)
When you download an AI project from the internet, the instructions usually say: "Just run pip install -r requirements.txt."

> [!warning]
> **On LUMI, you should almost never run `pip install`.**


## 🚫 Why `pip install` is the "Forbidden Command"
LUMI uses a specialized high-performance storage system called **Lustre**. Lustre is designed for "Big Data" - it is incredibly good at reading and writing massive files (like gigabytes of model weights or terabytes of text) at lightning speeds.

> [!warning]
> Lustre has a weakness: **many small files**.

A typical `pip install` of a library like PyTorch creates tens of thousands of tiny files. If everyone ran it, the filesystem would struggle to keep track of millions of tiny files, slowing down the entire supercomputer for everyone.

## 📦 The Solution: Containers
To avoid the "Million File" problem, we use Containers. On LUMI, our container tool of choice is called 'Apptainer' (previously 'singularity').

**What is a Container?** 

Think of a shipping container. Instead of loading thousands of loose items onto a ship, you pack everything - your software, your specific version of Python, and your AI libraries - into one large, sealed "box."

On LUMI, this "box" is a single file (usually ending in `.sif`).

- **For Lustre:** Instead of tracking 20,000 tiny files, it only has to track one big container file. This keeps the system fast.
- **For You:** Your software is "frozen" inside that box. This means it will work exactly the same way every time, regardless of what updates happen to the rest of the supercomputer.

> [!note] Why Apptainer instead of Docker?
> Apptainer was built specifically for supercomputers. It allows you to run the same "boxes" as Docker, but it does so securely without needing administrative privileges ("root").


## 📥 How to get your AI software
You don't necessarily need to learn how to build these containers from scratch. The LUMI AI Factory provides them for you. Most of the example scripts and guides already contain code necessary to use a container and you don't need to do anything. 

1) If you have the `.sif` file already on the system you can enter the container with an interactive shell. All the commands you execute in the container's shell will be executed using all the dependencies/libraries that are present in the container:

    ``` bash
    singularity shell <container.sif>
    ``` 

    Once you "enter" a container, you will see that the Command Line has changed. You can check what is inside just like you would on your own computer:

    ```bash
    pip list
    ```

2) The second way is to execute a command in the container with singularity exec, which enters the container, executes the command and immediately exits the container.

    ```bash
    singularity exec <container.sif> pip list
    ```

3) The third option is often called running a container, which is done with singularity run:

    ```bash
    singularity run <container.sif>
    ```

    It does require the container to have a built-in "runscript" — a set of default instructions baked in by whoever created the container. 
    
    > [!warning]
    > The LUMI AI Factory containers come with this already set up, so `singularity run` is the recommended way to use them. If you ever get an error saying no runscript is defined, fall back to `singularity exec` instead.

[👉 Read more about containers and interacting with them.](https://lumi-supercomputer.github.io/LUMI-training-materials/2day-20240502/09_Containers/#interacting-with-containers) **(Optional)**


## 🔄 Using a different container (Advanced)
However, if the guide or script uses an outdated container, or doesn't use the specific version of a library you need, you can find the full list of containers created and maintained by LUMI AI Factory at `/appl/local/laifs/containers/`. If you don't know which container to choose, use the latest one with the highest number of libraries, conveniently named `lumi-multitorch-latest.sif`.

![List of LAIFs containers](./assets/LAIFS_containers.png)

Besides `lumi-multitorch-latest.sif` you can see directories. The name of the directory contains the date when it was created. `20260415` stands for 2026/04/15. Within each directory you have a number of containers with different contents. `lumi-multitorch-full-...` contains the highest number of libraries and likely includes everything you might want to use. 

[👉 Read about the other types of containers that contain fewer libraries here](https://docs.lumi-supercomputer.eu/laif/software/ai-environment/).

## 🧩 What if I am missing a library? (Advanced)
If you find a container that is almost perfect but is missing one specific library, you can use a Python Virtual Environment (`venv`). You create this environment on top of the container. It stores the extra bits you need in a folder, allowing you to customize your workspace without creating millions of files. If you're sure this is the route you want to take, read this guide:

[👉 Guide on Python Virtual Environment](https://www.w3schools.com/python/python_virtualenv.asp)

[👉 Or watch this video on creating and using `venv` from within a container.](https://lumi-supercomputer.github.io/LUMI-training-materials/ai-20240529/extra_07_VirtualEnvironments/)


## 🔌 Modules
Besides Containers, we use something called 'Modules'. These are software packages already installed by the LUMI staff. You "load" them into your session with a simple command, similar to turning on a light switch:

```bash
module purge
module use /appl/local/laifs/modules
module load lumi-aif-singularity-bindings
```

- `module purge` — Clears any previously loaded modules, giving you a clean slate. This prevents conflicts between incompatible software.
- `module use /appl/local/laifs/modules` — Tells LUMI where to look for the AI Factory's modules. By default, LUMI only knows about its own system modules; this line adds our collection to the list.
- `module load` lumi-aif-singularity-bindings — Activates a specific module. In this case, it sets up the necessary "bindings" that allow your container to communicate with LUMI's hardware (GPUs, high-speed network, etc.).

> [!note] Do not fret
> Guides and example scripts will instruct you what modules to use. 


## ✅ Summary Checklist
- You understand that Lustre is great for big files, but struggles with many small files.
- You understand that pip install can slow down the system for everyone by creating too much "metadata."
- You understand that Apptainer is the secure, supercomputer-friendly alternative to Docker that turns thousands of files into one easy-to-manage .sif file.