# 🧠 Linux Swap Setup Guide (Full Tutorial for File & Partition-Based Swap) 🚀

Create and manage swap on Linux using **swap files** or **dedicated partitions**. This guide covers multiple methods and helps you decide which is best for your server or desktop system. Ideal for VPS and physical hardware.

---

## 📚 Table of Contents

1. [Swap File (Method 1 - via dd)](#-method-1-swap-file-via-dd)
2. [Swap File (Method 2 - via fallocate)](#-method-2-swap-file-via-fallocate)
3. [Swap Partition (Dedicated Disk Space)](#-method-3-swap-partition)
4. [Adjust Swappiness](#adjust-swappiness)
5. [Check Swap Status](#-check-swap-status)
6. [Disable or Resize Swap](#-disable-or-resize-swap)
7. [License](#-license)

---

## 🧱 Method 1: Swap File via dd

### 🔨 Create Swap File (Example: 8 GB)
```bash
sudo dd if=/dev/zero of=/swapfile bs=1M count=8192 status=progress
```

### 🔐 Set Permissions
```bash
sudo chmod 600 /swapfile
```

### 💾 Format as Swap
```bash
sudo mkswap /swapfile
```

### ⚡ Enable Swap
```bash
sudo swapon /swapfile
```

### 🔁 Make Persistent
```bash
echo '/swapfile none swap sw 0 0' | sudo tee -a /etc/fstab
```

---

## ⚡ Method 2: Swap File via fallocate

### ⚡ Create Swap File Quickly
```bash
sudo fallocate -l 8G /swapfile
```

> ⚠️ Note: On some VPS systems, `fallocate` may produce "holey" files that `mkswap` rejects. Use `dd` if issues arise.

### Continue with:
```bash
sudo chmod 600 /swapfile
sudo mkswap /swapfile
sudo swapon /swapfile
echo '/swapfile none swap sw 0 0' | sudo tee -a /etc/fstab
```

---

## 💽 Method 3: Swap Partition

### 🛠️ Step 1: Create Partition
Use `fdisk` or `parted` to create a new partition (e.g., `/dev/sdb1`) and set type to `Linux swap (82)`.

```bash
sudo fdisk /dev/sdX
# Create new partition, set type to 82, write changes
```

### 💾 Step 2: Format Partition
```bash
sudo mkswap /dev/sdXn
```

### ⚡ Step 3: Enable Swap
```bash
sudo swapon /dev/sdXn
```

### 🔁 Step 4: Add to /etc/fstab
```bash
echo '/dev/sdXn none swap sw 0 0' | sudo tee -a /etc/fstab
```

> Replace `/dev/sdXn` with your actual swap partition (e.g., `/dev/sda3`).

---

## Adjust Swappiness


Swappiness defines how often the system uses swap space. Lower = RAM preferred.

```bash
echo 'vm.swappiness=10' | sudo tee -a /etc/sysctl.conf
sudo sysctl -p
```

Default: `60`  
Recommended for high-RAM systems: `10`

---

## ✅ Check Swap Status

```bash
swapon --show
free -h
```

---

## 🧹 Disable or Resize Swap

### 🔻 Disable:
```bash
sudo swapoff /swapfile
```

### ❌ Remove:
```bash
sudo rm /swapfile
```

### 🔄 Resize:
Recreate using a new size (e.g., `count=16384` for 16 GB in `dd`).

---

## 📜 License

This project is licensed under the MIT License. See the [LICENSE](LICENSE) file for details.
