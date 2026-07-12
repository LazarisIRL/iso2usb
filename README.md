# iso2usb
Makes a bootable USB from an ISO URL

Stream an ISO straight from a URL onto a USB device. No need to download the file first, no intermediate storage, and the write is verified with a SHA-256 checksum afterwards.
Uses `dd` to write a bootable USB drive, but with several checks to prevent a user from "disc destoying" their boot partition or something.

How is this better than Ventoy, Netboot or even BalenaEtcher you ask? It isn't. Use those tools instead. 
This is a personal project for me to learn bash scripting. AI was not used except for some regex syntax, and sanity checking the finished code.

## Features
- **No intermediate file**
    -The ISO is piped directly from the download into `dd`
- **Checksum verification**
    After writing, the script rereads the exact number of bytes reported by the server's `Content-Length` HTTP header response and compares SHA256 sums.
- **Safety checks before writing:**
    - Confirms the target is a valid block device
    - Warns if the device isn't flagged as removable
    - Warns if the device is unusually large for a USB stick
    - Warns if the device's transport isn't USB
    - Checks if the device is currently mounted and offers to unmount if so.
    - Requires the user to type the exact device name (e.g. `/dev/sda`) to confirm before it overwrites anything

## WARNING

This script permanently overwrites the target device with `dd`. There is no undo. Double check the device path (uing lsblk or similar) before confirming. The script's built in checks are there to catch common mistakes, but they are not a substitute for checking `lsblk` output yourself.

## Installation

Superuser privilages are required for this script to run (writing to a raw block device requires it). Script can be installed in /usr/local/bin.

```bash
curl -O https://raw.githubusercontent.com/LazarisIRL/iso2usb/refs/heads/main/iso2usb
chmod +x iso2usb
sudo install -m 744 iso2usb /usr/local/bin/iso2usb
```
This puts it on your `$PATH` so it can be run from anywhere as `sudo iso2usb <url> <device>`. To make the script runable by any user, change the last line to:

```bash
sudo install -m 755 iso2usb /usr/local/bin/iso2usb
```
## Usage

```bash
sudo iso2usb <url> <device>
```
**Example:**

```bash
sudo iso2usb https://releases.example.com/distro.iso /dev/sda
```
Run with option flags `-h` or `--help`

## Requirements

- curl
- dd
- sha256sum
- lsblk
- numfmt 
- root privileges 
