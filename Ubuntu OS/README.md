# Persistent Live USB for Ubuntu

This guide will walk you through the steps to create a persistent Live USB with **Ubuntu**.

## Prerequisites

1. **Ubuntu ISO**
   Download the latest Ubuntu ISO from the official [Ubuntu website](https://ubuntu.com/download/desktop).

2. **USB Drive**
   A USB drive with at least **8GB** of storage (preferably 16GB or more for a better experience).

3. **Partitioning Tool**

   * **GParted** (Linux) or **Disk Utility** (macOS) to manage partitions.
   * **Rufus** (Windows) or **Etcher** (Linux/Windows/macOS) to create the live USB.

4. **Persistent Storage Partition**
   You will need to create a second partition on the USB drive to store persistent data.

## Step 1: Prepare the USB Drive

1. **Insert the USB Drive** into your computer.

2. **Format the USB Drive**:

   * Use **GParted** (Linux) or **Disk Utility** (macOS) to format the USB drive to **FAT32**.
   * Create two partitions:

     * **1st Partition (FAT32)**: For the bootable Ubuntu system.
     * **2nd Partition (ext4)**: For persistent storage. Make it large enough to hold your data (e.g., 4GB or more).

     Ensure the **ext4** partition is labeled `casper-rw` (this is the name that Ubuntu uses for persistent storage).

3. **Label the Partitions**:

   * The 1st partition should be `boot` and formatted as **FAT32**.
   * The 2nd partition should be `ext4` and labeled `casper-rw`.

## Step 2: Write Ubuntu to the USB Drive

1. Open **Rufus** (Windows) or **Etcher** (macOS/Linux) and select the Ubuntu ISO file.

2. Choose the USB drive as the target device, and burn the **Ubuntu ISO** to the **1st partition** (the FAT32 partition).

3. **Important**: When using **Rufus**, ensure that the "Partition scheme" is set to **MBR** and the file system to **FAT32** for compatibility.

---

## Step 3: Enable Persistence

1. After writing the ISO, you need to enable persistence.

2. **Edit the Boot Parameters**:

   * Open the USB drive, navigate to the `boot/grub` folder.
   * Look for the `grub.cfg` file (if using GRUB).

3. Edit the `grub.cfg` file:

   * Find the line that starts with `linux` (this is the kernel boot line).
   * Add the following to the end of the line:

     ```bash
     persistent
     ```

   If you want to specify the persistence partition manually (in case it's not detected automatically), add the UUID of the `ext4` partition:

   ```bash
   persistent persistent-storage=UUID=<UUID_of_your_casper-rw_partition>
   ```

   * You can find the UUID of the `casper-rw` partition by running:

     ```bash
     sudo blkid
     ```

     Or, on macOS:

     ```bash
     diskutil info <disk identifier>
     ```

4. **Save and Close** the `grub.cfg` file.

---

## Step 4: Boot from the USB Drive

1. **Reboot** your computer and boot from the USB drive.

2. Select **Ubuntu** to boot into the live environment.

3. **Test Persistence**:

   * Create a file or make system changes.
   * Reboot the system.
   * Check if your changes persist across reboots.

