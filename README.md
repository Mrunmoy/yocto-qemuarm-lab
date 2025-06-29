# Yocto QEMU ARM Bring-up Lab

This repo documents my hands-on practice building a custom ARM Linux image with the Yocto Project, running it in QEMU, and verifying a working shell environment -> without any physical hardware.

---

## Summary

- Spun up a cloud Ubuntu VM on Google Cloud with free credits.
- Installed Yocto build dependencies.
- Cloned `poky` and `meta-raspberrypi` layers for practice.
- Set `MACHINE = "qemuarm"` to use QEMU for ARM emulation.
- Built `core-image-sato` with BitBake.
- Booted the image in QEMU using `runqemu`.
- Logged in, verified networking and kernel version.

---

## This demonstrates
- Bring up a Yocto build system on a clean Linux host.
- Configure layers and machine targets.
- Debug build issues (`bitbake` errors, missing host tools).
- Run QEMU in headless mode on a remote cloud VM.
- Use QEMU output as a safe test bench before flashing real hardware (e.g., PYNQ-Z2).

---

## Directory Structure

| File/Folder | Description |
| ------------ | ----------- |
| `conf/` | My working Yocto `local.conf` and `bblayers.conf` |
| `screenshots/` | build running, QEMU boot logs, shell login |
| `README.md` | This guide |

---

## Next steps

✅ Switch `MACHINE` to my real board’s BSP layer (PYNQ-Z2).  
✅ Flash the image to SD and bring up my Zynq FPGA board.  
✅ Document real hardware bring-up in a separate branch.

---

## Credits

Yocto Project Docs: [docs.yoctoproject.org](https://docs.yoctoproject.org)  
Hands-on practice by me, built & tested in June 2025.


# Steps

## Step-by-Step Guide: Yocto ARM Bring-up with QEMU

This section is a complete walkthrough of what I did — from spinning up a free Google Cloud VM to booting my custom Yocto ARM image in QEMU.

---

### 1️⃣ Create a free Google Cloud VM

- Sign up at https://console.cloud.google.com
- Activate your free trial ($300 credits)
- Create a project (I named mine `yocto-lab`)
- Enable **Compute Engine API** when asked
- Create a VM:
  - Ubuntu 22.04 LTS
  - Region close to you
  - `e2-standard-4` (4 vCPU, 16 GB RAM)
  - 100 GB SSD disk
  - Allow HTTP/HTTPS firewall rules

*Screenshots:*
## 🚀 1️⃣ Create Your Yocto Build VM on Google Cloud

This guide shows exactly how I created a free-tier Google Cloud VM to build Yocto without messing up my local machine.

---

### Step 1: Open Compute Engine

In your Google Cloud Console, go to **Compute Engine → VM Instances**.

![Welcome Screen](screenshots/Screenshot_2025-06-30_091824.png)

*Use the left sidebar to navigate to Compute Engine.*

![Compute Engine menu](screenshots/Screenshot_2025-06-30_091926.png)

---

### Step 2: See your VM list

Once Compute Engine is enabled, you’ll see all your VM instances.  
Click **“Create Instance”** to make a new VM.

![VM Instances overview](screenshots/Screenshot_2025-06-30_092038.png)

*Here you can see my `yocto-builder` VM ready to go.*

---

### Step 3: Choose machine configuration

Pick a machine series that fits the free credits but has enough RAM for a Yocto build.

- **Series:** E2 (low cost, general-purpose)
- **Machine type:** `e2-standard-4` (4 vCPU, 16 GB RAM)

![Machine configuration](screenshots/Screenshot_2025-06-30_093018.png)

*This is affordable but fast enough for a small build.*

---

### Step 4: Select OS & Storage

For your boot disk, pick:

- **Operating System:** Ubuntu 22.04 LTS
- **Disk Size:** 100 GB SSD

![OS and Storage](screenshots/Screenshot_2025-06-30_093125.png)

*A larger disk is better because Yocto build directories can be huge.*

---

### Step 5: Launch your VM & connect

Once it’s created, you’ll see your new VM in the list with its external IP.  
Click **SSH** to connect directly in your browser.

![Connect to VM over SSH](screenshots/Screenshot_2025-06-30_091926.png)

*Click SSH to open a terminal window to your VM.*

---

### Tips

✅ **Allow HTTP/HTTPS traffic** if you need to download packages.  
✅ Monitor your cost — shut down the VM when done.  
✅ Pin Compute Engine in the Cloud Hub for quick access.

![Pinned Compute Engine in Cloud Hub](screenshots/Screenshot_2025-06-30_095200.png)

---


### 2️⃣ SSH into the VM

- Use the GCP Console `SSH` button.


### Install Yocto build dependencies
```bash
sudo apt update
sudo apt upgrade -y

sudo apt install -y \
    gawk wget git-core diffstat unzip texinfo gcc-multilib \
    build-essential chrpath socat cpio python3 python3-pip python3-pexpect \
    xz-utils debianutils iputils-ping libsdl1.2-dev coreutils vim
```
#### Extra host tools you might need
During the build, you may see BitBake complain about missing tools. For example, I needed to install lz4 because lz4c was missing, I also like to install `htop` and `tmux` so I can monitor the build and reconnect easily.
```bash
sudo apt update
sudo apt install -y htop tree tmux lz4 zip
```

✅ Note: Keep an eye on any bitbake errors — they’ll always tell you what host tools are missing!





