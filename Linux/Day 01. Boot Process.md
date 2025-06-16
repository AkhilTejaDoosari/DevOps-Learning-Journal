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

## 1. Power ON & Firmware Initialization

When you press the power button, your system gets electricity — but before Linux starts, the firmware must check if the hardware is okay.

### What is BIOS/UEFI?

- BIOS = Basic Input/Output System  
- UEFI = Unified Extensible Firmware Interface (modern replacement for BIOS)

These are built-in programs on your motherboard.

### What happens here?

- Runs POST (Power-On Self-Test)  
- Checks hardware like RAM, disk, keyboard, etc.  
- Finds a bootable device (hard drive, USB, etc.)

Interview Tip: “BIOS/UEFI initializes hardware and identifies a bootable device.”

---

## 2. Partition Tables – MBR vs GPT

Before loading an OS, your disk must have a partition table to map where things are stored.

- MBR = Master Boot Record (old, limited to 4 partitions and 2TB disks)  
- GPT = GUID Partition Table (modern, used with UEFI, supports large disks and more partitions)

MBR and GPT help locate the bootloader.

---

## 3. Bootloader – GRUB/GRUB2

### What is a bootloader?

A small program that loads your operating system (Linux). In most Linux systems, it's GRUB or GRUB2.

### What does GRUB do?

- Shows OS selection menu (if dual-booting)  
- Loads the Linux kernel  
- Loads initramfs (temporary root file system)

GRUB is like a train conductor — it gets the right Linux engine ready to go.

### GRUB Config Files

- `/boot/grub2/` or `/boot/efi/EFI/...`  
- `/etc/default/grub`  
- `/etc/grub.d/`

Interview Tip: “GRUB loads the Linux kernel and initial RAM disk into memory.”

---

## 4. Kernel – The Linux Brain Wakes Up

### What is the kernel?

The core of Linux — it controls CPU, memory, devices, etc.

### What happens here?

- Loads drivers for your hardware  
- Loads initramfs (mini root filesystem)  
- Mounts the real root filesystem (e.g., `/dev/sda1`)

The kernel is like the factory manager making sure everything runs.

Interview Tip: “initramfs provides essential drivers to help the kernel mount the actual root partition.”

---

## 5. systemd – The Linux Team Leader

### What is systemd?

The first real process started by the kernel. It’s PID 1.

### What does systemd do?

- Starts services (network, sound, login, etc.)  
- Decides boot mode (CLI or GUI)  
- Manages the order and status of services

Interview Tip: “systemd is the first user-space process. It manages services and controls boot targets.”

---

## 6. Run Targets

Linux now uses systemd targets instead of traditional runlevels.

| Runlevel | systemd Target      | Purpose                     |
|----------|---------------------|-----------------------------|
| 0        | poweroff.target     | Shutdown                    |
| 1        | rescue.target       | Single-user mode            |
| 3        | multi-user.target   | CLI, multi-user (no GUI)    |
| 5        | graphical.target    | Multi-user with GUI         |
| 6        | reboot.target       | Reboot                      |

---

## 7. Startup Scripts and Login

Once systemd completes startup:

- You see a text login (for CLI systems)  
- Or a graphical login window (for desktop systems)

After login, Linux is ready to use.

---

## 8. Boot Process Commands

```bash
uname -r                              # Show kernel version
dmesg | less                          # View boot-time system messages
systemctl list-units --type=service   # Show running services
ls /boot                              # List kernel and GRUB files
