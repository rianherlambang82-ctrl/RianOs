# RianOS

**English** | [Bahasa Indonesia](README.id.md)

**RianOS 1.0-RC** is a lightweight Linux desktop distribution based on **Ubuntu 24.04 (Noble)**, featuring the **XFCE** desktop, the **Calamares** installer, and a preinstalled collection of networking and security tools.

RianOS provides the applications; advanced configuration (services, firewall, and so on) is left to the user.

> **Status:** Release Candidate. It can be installed and used, but it is still being tested.

---

## Overview

| | |
|---|---|
| Base | Ubuntu 24.04 LTS (Noble), Debian family |
| Architecture | amd64 |
| Desktop | XFCE + LightDM |
| Installer | Calamares |
| ISO build tool | [live-build](https://live-team.pages.debian.net/live-manual/) |
| ISO size | about 3.8 GiB |
| Boot | UEFI (`shim-signed` and `grub-efi-amd64-signed` are included) |

---

## What's included

**Desktop and office:** XFCE, LibreOffice, Evince, VLC, OBS Studio, FileZilla, GParted, GNOME Disks, Bluetooth (BlueZ + Blueman), SQLite Browser

**Command-line basics:** git, curl, wget, nano, vim, htop, tmux, net-tools, Python 3 (pip, venv), SQLite 3

**Networking:** NetworkManager, nmap, netcat, tcpdump, socat, dnsutils, traceroute, whois, nftables

**Security:** Wireshark, John the Ripper, OpenSSL, Metasploit Framework, Burp Suite Community

**Installed services:** nginx (web server), dropbear (SSH server)

---

## Download and verify

1. Download the ISO and the `SHA256SUMS` file from **[Google Drive](https://drive.google.com/drive/folders/1LmG6_M8X6-0EWgXnaPa4qiYnxmOzuj8L?usp=drive_link)**.
2. Verify the file:

   ```bash
   sha256sum -c SHA256SUMS
   ```

   The output must be `RianOS-1.0-RC-amd64.iso: OK`.

## Write it to a USB drive

Use a USB drive of **8 GB or larger** (the ISO is about 3.8 GiB and will not fit on a "4 GB" drive).

- **Windows:** [Rufus](https://rufus.ie) (choose *DD Image* mode) or [Ventoy](https://www.ventoy.net)
- **Linux:**

  ```bash
  lsblk   # confirm the name of the USB device
  sudo dd if=RianOS-1.0-RC-amd64.iso of=/dev/sdX bs=4M status=progress conv=fsync
  ```

  Replace `/dev/sdX` with the correct USB device. Choosing the wrong device will erase its contents.

Then boot from the USB drive and run **Calamares** to install RianOS to disk.

---

## Services enabled by default

Some services start automatically after RianOS is installed. They are listed here so you know what is open:

| Service | Port | Description |
|---|---|---|
| `dropbear` | 22 (TCP) | SSH server |
| `nginx` | 80 (TCP) | Web server |
| `avahi-daemon` | 5353 (UDP) | Local network device discovery (mDNS) |
| `cups` | 631 (loopback only) | Printing service |

**No firewall is enabled by default.** `nftables` is installed, and you configure the rules yourself.

To disable services you do not use:

```bash
sudo systemctl disable --now nginx dropbear
```

To see which ports are listening:

```bash
sudo ss -tulpn
```

---

## Third-party software

- **Metasploit Framework** is installed from Rapid7's official repository when the ISO is built, so updates arrive through `apt`.
- **Burp Suite Community** is installed from PortSwigger's installer.
- Paid licenses (for example Burp Suite Professional) are **not included**. Users buy and enter their own license.
- All third-party software remains subject to the licenses and terms of use of its respective owners.

---

## Building the ISO yourself

You need Ubuntu (or Ubuntu on WSL) with `live-build`, enough disk space, and an **active internet connection**, because some hooks download software during the build.

```bash
git clone https://github.com/rianherlambang82-ctrl/RianOs.git
cd RianOs
sudo ./scripts/02-configure.sh && sudo ./scripts/03-build.sh
```

Main configuration layout:

```
config/
├── package-lists/     # package lists (custom.list.chroot, live.list.chroot)
├── includes.chroot/   # files copied into the system (scripts, configs, branding)
└── hooks/             # scripts run during the build (live/ and normal/)
```

Make changes inside `config/` and rebuild the ISO. Edits made directly in a live session are lost on reboot.

---

## Roadmap

- A server/headless variant for security work.
- Optional AI assistant integration (the API key is supplied by the user and is not shipped in the ISO).
- Persistence for live USB.

---

## Responsible use

The security tools in RianOS may only be used on systems that you **own** or have **written permission** to test. Misuse is entirely the user's responsibility.

---

## License

<!-- TODO: fill in your chosen license, for example GPL-3.0 or MIT, and add a LICENSE file -->
This project's license: _not yet specified_.

Third-party components have their own licenses (see *Third-party software*).

---

Created by **Rian**.
