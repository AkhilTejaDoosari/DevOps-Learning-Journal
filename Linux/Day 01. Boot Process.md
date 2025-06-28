# 🐧 Day 01 – Boot Process

## Table of Contents
- [1. Power ON & Firmware Initialization](#1-power-on--firmware-initialization)
- [2. Partition Tables – MBR vs GPT](#2-partition-tables--mbr-vs-gpt)
- [3. Bootloader – GRUB/GRUB2](#3-bootloader--grubgrub2)
- [4. Kernel – The Linux Brain Wakes Up](#4-kernel--the-linux-brain-wakes-up)
- [5. systemd – The Linux Team Leader](#5-systemd--the-linux-team-leader)
- [6. Run Targets](#6-run-targets)
- [7. Startup Scripts and Login](#7-startup-scripts-and-login)
- [8. Boot Process Commands](#8-boot-process-commands)

---

<details>
<summary><strong>1. Power ON & Firmware Initialization</strong></summary>

When you press the power button, your system gets electricity but before Linux starts, the firmware must check if the hardware is okay.   

**What is BIOS/UEFI?**
- **BIOS** = Basic Input/Output System  
- **UEFI** = Unified Extensible Firmware Interface (modern replacement for BIOS)

These are built-in programs on your motherboard.

**What happens here?**
- Runs POST (Power-On Self-Test)  
- Checks hardware like RAM, disk, keyboard, etc.  
- Finds a bootable device (hard drive, USB, etc.)

> **Tip:** “BIOS/UEFI initializes hardware and identifies a bootable device.”

</details>

---

<details>
<summary><strong>2. Partition Tables – MBR vs GPT</strong></summary>

Before loading an OS, your disk must have a partition table to map where things are stored.   

- **MBR** = Master Boot Record (old, limited to 4 partitions and 2 TB disks)   
- **GPT** = GUID Partition Table (modern, used with UEFI, supports large disks and more partitions)   

MBR and GPT help locate the bootloader.   

</details>

---

<details>
<summary><strong>3. Bootloader – GRUB/GRUB2</strong></summary>

**What is a bootloader?**   
A small program that loads your operating system (Linux). In most Linux systems, it’s GRUB or GRUB2.   

**What does GRUB do?**   
- Shows OS selection menu (if dual-booting)    
- Loads the Linux kernel  
- Loads initramfs (temporary root filesystem)   

GRUB is like a train conductor — it gets the right Linux engine ready to go.

**GRUB Config Files**
- `/boot/grub2/` or `/boot/efi/EFI/...`  
- `/etc/default/grub`  
- `/etc/grub.d/`

> **Tip:** “GRUB loads the Linux kernel and initial RAM disk into memory.”

</details>

---

<details>
<summary><strong>4. Kernel – The Linux Brain Wakes Up</strong></summary>

**What is the kernel?**   
The core of Linux — it controls CPU, memory, devices, etc.

**What happens here?**
- Loads drivers for your hardware  
- Loads initramfs (mini root filesystem)  
- Mounts the real root filesystem (e.g., `/dev/sda1`)

The kernel is like the factory manager making sure everything runs.

> **Tip:** “initramfs provides essential drivers to help the kernel mount the actual root partition.”

</details>

---

<details>
<summary><strong>5. systemd – The Linux Team Leader</strong></summary>

**What is systemd?**   
The first user-space process spawned by the kernel. It’s PID 1.   

**What does systemd do?**   
- Starts services (network, sound, login, etc.)  
- Decides boot mode (CLI or GUI)  
- Manages the order and status of services   

> **Tip:** “systemd is the first user-space process. It manages services and controls boot targets.”

</details>

---

<details>
<summary><strong>6. Run Targets</strong></summary>

Linux now uses systemd targets instead of traditional runlevels.

| Runlevel | systemd Target    | Purpose                  |
|:--------:|:-----------------:|:-------------------------|
| 0        | poweroff.target   | Shutdown                 |
| 1        | rescue.target     | Single-user mode         |
| 3        | multi-user.target | CLI, multi-user (no GUI) |
| 5        | graphical.target  | Multi-user with GUI      |
| 6        | reboot.target     | Reboot                   |

</details>

---

<details>
<summary><strong>7. Startup Scripts and Login</strong></summary>

Once systemd completes startup:

- You see a text login (for CLI systems)  
- Or a graphical login window (for desktop systems)

After login, Linux is ready to use.

</details>

---

<details>
<summary><strong>8. Boot Process Commands</strong></summary>

```bash
uname -r                              # Show kernel version
dmesg | less                          # View boot-time system messages
systemctl list-units --type=service   # Show running services
ls /boot                              # List kernel and GRUB files
````

</details>