# How to format usb storage device termux with root access

## Before proceeding!!!

- **I AM NOT RESPONSIBLE FOR THE DAMAGE THAT YOU MAY CAUSE IF YOU FOLLOW THIS STEPS INCORRECTLY**!!!
- Make sure to read this guide at least one time before proceeding, you have been warned.
- Make sure to have [Magisk](https://github.com/topjohnwu/Magisk), [Apatch](https://github.com/bmax121/APatch) or [KernelSU](https://github.com/tiann/KernelSU) installed on your device.
- Make sure to have sudo or tsu installed by either doing:

```bash
pkg update && pkg upgrade -y && pkg install sudo
```

- Or


```bash
pkg update && pkg upgrade -y && pkg install tsu
```

- **ATTENTION**: you cannot have both sudo and tsu installed, if you have sudo already installed tsu will fail to install and vice versa.

## Update and install binaries

- Update termux binaries and install blk-utils and dosfstools:

- **NOTE**: dosfstools is required to format drives as FAT32, if not needed do not install dosfstools, but its good to keep in hand, standard android should already come with (mkfs.erofs, mkfs.exfat, mkfs.ext2, mkfs.ext3, mkfs.ext4, mkfs.ntfs)

```bash 
pkg update && pkg upgrade -y && pkg install blk-utils dosfstools -y
```

## Find and identify usb storage device

- Find usb storage device path using lsblk from blk-utils:

```bash 
sudo lsblk
```

- Example output:

```bash
sdf       8:80   1  3.7G  0 disk
└─sdf1    8:81   1  3.7G  0 part /mnt/media_rw/0124-1790
```

- In my case its sdf1

## Identify the id of the usb storage device and unmount

- Identify the id of the usb storage device with sm and unmount:

```bash
sudo sm list-volumes
```

- Example output:

```bash
public:8,81 mounted 0124-1790
private mounted null
emulated;0 mounted null
```

- In my case the id is public:8,81

```
sudo sm unmount public:8,81
```

- After this the drive will be unmounted and we can proceed with formatting it.

## Format and remount usb storage device

- Format the usb storage device as you prefer with any of this: (mkfs.erofs, mkfs.exfat, mkfs.ext2, mkfs.ext3, mkfs.ext4, mkfs.ntfs).
- I will be using mkfs.vfat from dosfstools.

```bash
sudo mkfs.vfat -F 32 -n MYDRIVE /dev/block/sdf1 <-- Change this!!!
```

- **NOTE**: change the path of your usb storage device block to the one corresponding to yours, in my case its /dev/block/sdf1, also storage device blocks on Android are listed under: /dev/block not /dev.

- Example output:

```bash
mkfs.fat 4.2 (2021-01-31)
```

- Now we can mount the usb storage device with sm.

```bash
sudo sm mount public:8,81
```

- Or we can mount it as internal storage with:


```bash
sudo sm partition public:8,81 private
```
- Not recommended for external usb storage devices.
- Yay! You successfully made it through the guide!
