# Yocto QEMU ARM Bring-up Lab

This repo documents my hands-on practice building a custom ARM Linux image with the Yocto Project, running it in QEMU, and verifying a working shell environment -> without any physical hardware.

---

## ✅ What I did

- Spun up a cloud Ubuntu VM on Google Cloud with free credits.
- Installed Yocto build dependencies.
- Cloned `poky` and `meta-raspberrypi` layers for practice.
- Set `MACHINE = "qemuarm"` to use QEMU for ARM emulation.
- Built `core-image-sato` with BitBake.
- Booted the image in QEMU using `runqemu`.
- Logged in, verified networking and kernel version.

---

## 🔑 Why it matters

This demonstrates:
- Bring up a Yocto build system on a clean Linux host.
- Configure layers and machine targets.
- Debug build issues (`bitbake` errors, missing host tools).
- Run QEMU in headless mode on a remote cloud VM.
- Use QEMU output as a safe test bench before flashing real hardware (e.g., PYNQ-Z2).

---

## 🗂️ What’s in this repo

| File/Folder | Description |
| ------------ | ----------- |
| `conf/` | My working Yocto `local.conf` and `bblayers.conf` |
| `screenshots/` | build running, QEMU boot logs, shell login |
| `README.md` | This guide |

---

## 🏁 Next steps

✅ Switch `MACHINE` to my real board’s BSP layer (PYNQ-Z2).  
✅ Flash the image to SD and bring up my Zynq FPGA board.  
✅ Document real hardware bring-up in a separate branch.

---

## 🙌 Credits

Yocto Project Docs: [docs.yoctoproject.org](https://docs.yoctoproject.org)  
Hands-on practice by me, built & tested in June 2025.
