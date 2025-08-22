# Persistent Live USB for Pop!_OS

This guide walks you through the steps to create a persistent Live USB with **Pop!_OS** by **System76**.

## Prerequisites

1. **Pop!_OS ISO**  
   Download the latest Pop!_OS ISO from the official [System76 website](https://pop.system76.com).

2. **USB Drive**  
   You will need a USB drive with at least 8GB of storage (preferably 16GB or more for a better experience).

3. **Partitioning Tool**  
   - **GParted** (Linux) or **Disk Utility** (macOS) to manage partitions.
   - **Rufus** (Windows) or **Etcher** (Linux/Windows/macOS) to create the live USB.

4. **Persistent Storage**  
   You'll need to create a second partition on the USB drive for the persistent storage.

## Step 1: Prepare the USB Drive

1. Insert your USB drive into your computer.

2. Format the USB drive:
   - Use **GParted** (Linux) or **Disk Utility** (macOS) to format the USB drive to **FAT32**.
   - Create two partitions:
     - **1st Partition (FAT32)**: This will be for the bootable Pop!_OS.
     - **2nd Partition (ext4)**: This will be for persistent storage. Make it as large as you need for persistence (e.g., 4GB or more).

3. Label the 1st partition as `boot` and the 2nd partition as `ext4`.

## Step 2: Write Pop!_OS to the USB Drive

1. Open **Rufus** (Windows) or **Etcher** (Linux/macOS) and select the Pop!_OS ISO file.

2. Choose the USB drive as the target device and start the process of burning the ISO to the **1st FAT32 partition**.

   **Warning:** Do not overwrite the second `ext4` partition while writing the ISO.

## Step 3: Enable Persistence

1. After the Pop!_OS ISO is written, you need to enable persistence.

2. Open the USB drive and navigate to the `EFI/BOOT` folder.

3. Find the `grub.cfg` (for GRUB bootloader) or `syslinux.cfg` (for Syslinux bootloader) file.

4. Edit the file and find the line starting with `linux`. Add the following to the end of the line:
   
   For **GRUB**:
   ```bash
   persistence
   ```
   or
   ```bash
   persistence persistent-storage=UUID=<UUID_of_your_ext4_partition>
   ```
5. You can find the UUID of the partition by using blkid (Linux) or diskutil (macOS).
If you're using Syslinux, look for the APPEND line and add the persistence options there.

## Step 4: Boot from the USB Drive

1. Reboot your computer and boot from the USB drive.

2. Select Pop!_OS to boot in "live" mode.

3. Test persistence by creating a file or modifying settings, then reboot and check if changes are still present.


