# iso2usb
Makes a bootable USB from an ISO URL

Stream an ISO straight from a URL onto a USB device. No need to download the file first, no intermediate storage, and the write is verified with a SHA-256 checksum afterwards.
Uses dd to write a bootable USB drive, but with several checks to prevent a user from "disc destoying" their boot partition or something.

This is a personal project for me to learn bash scripting. AI was not used except for some regex syntax, and sanity checking the finished code.

## WARNING

This script permanently overwrites the target device with dd. There is no undo. Double check the device path (uing lsblk or similar) before confirming. The script's built in checks (removable flag, size, transport type, mount status) are there to catch common mistakes, but they are not a substitute for checking lsblk output yourself.

## Install

```bash
curl -O https://raw.githubusercontent.com/LazarisIRL/iso2usb/refs/heads/main/iso2usb
chmod +x iso2usb
```

## Usage

```bash
sudo ./iso2usb <url> <device>
```

**Example:**

```bash
sudo ./iso2usb https://releases.example.com/distro.iso /dev/sdb
```
Run with `-h` or `--help` for a quick usage summary.

## Requirements

- curl
- dd
- sha256sum
- lsblk
- numfmt 
- root privileges (writing to a raw block device requires it)
