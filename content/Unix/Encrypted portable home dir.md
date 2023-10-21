---
title: Encrypted portable home dir
draft: false
tags:
  - ai-generated
  - cybersec
  - thinkpad
  - void-linux
---

## Q

Let's say I have two identical ThinkPad X1 Carbon laptops. On both, I've installed Void Linux. Now one of these two has a more up-to-date /home/my_user directory as I work on it on a daily basis. I want to be able to somehow sync this home dir onto a sd card and pull it out whenever I'm switchting to the second laptop without any losses.

How can I achieve this the best way? Also make sure, the contents of my sd disk are encrypted.

## A

To achieve this, you can use a combination of synchronization tools and encryption. Here's a step-by-step guide on how to sync your home directory onto an encrypted SD card and access it from both of your ThinkPad X1 Carbon laptops:

### Step 1: Set Up Your SD Card

Insert your SD card into one of the laptops.

Format the SD card if it's not already formatted. You can use a tool like gparted or fdisk for this purpose. Make sure to create a single partition on the SD card.

Encrypt the SD card partition using LUKS (Linux Unified Key Setup):

```
sudo cryptsetup luksFormat /dev/sdX1
```

Replace /dev/sdX1 with the appropriate device identifier for your SD card partition. You'll be prompted to set a passphrase; make sure to remember it.

Open the LUKS container:

```
sudo cryptsetup luksOpen /dev/sdX1 sdcard
```

You'll be prompted to enter the passphrase you set in the previous step.

Create an ext4 filesystem on the decrypted partition:

```
sudo mkfs.ext4 /dev/mapper/sdcard
```

### Step 2: Mount the Encrypted SD Card

Create a mount point for your SD card:

```
sudo mkdir /mnt/sdcard
```

Mount the encrypted SD card partition:

```
sudo mount /dev/mapper/sdcard /mnt/sdcard
```

### Step 3: Sync Your Home Directory

Use rsync to synchronize your home directory to the mounted SD card:

```
rsync -avz --exclude=".cache" /home/your_user/ /mnt/sdcard/
```

Replace /home/your_user/ with the path to your home directory.

### Step 4: Unmount the SD Card

After syncing, unmount the SD card to ensure all data is flushed:

```
sudo umount /mnt/sdcard
```

### Step 5: Eject the SD Card

Safely eject the SD card from the laptop.

### Step 6: Access Your Home Directory on the Second Laptop

To access your home directory on the second laptop, follow these steps:

Insert the encrypted SD card into the second laptop.

Decrypt the SD card:

```
sudo cryptsetup luksOpen /dev/sdX1 sdcard
```

Mount the decrypted SD card:

```
sudo mount /dev/mapper/sdcard /mnt/sdcard
```

You can now access your home directory as if it were on the local machine:

```
cd /mnt/sdcard
```

### Important Notes:

Make sure to keep your passphrase secure because it's essential for decrypting the SD card.

Be cautious when using rsync to sync your home directory. It's a powerful tool that can overwrite files, so double-check your command before running it.

Always unmount and eject the SD card before physically removing it from a laptop.

This setup should allow you to securely sync your home directory between your two ThinkPad laptops while keeping the contents of your SD card encrypted.
