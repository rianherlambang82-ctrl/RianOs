# RianOS

[English](README.md) | **Bahasa Indonesia**

**RianOS 1.0-RC** adalah distro Linux desktop ringan berbasis **Ubuntu 24.04 (Noble)**, dengan desktop **XFCE**, installer **Calamares**, dan kumpulan aplikasi jaringan serta keamanan siber yang sudah terpasang.

RianOS menyediakan aplikasinya, dan pengaturan lanjutan (layanan, firewall, dan sebagainya) diserahkan kepada pengguna.

> **Status:** Release Candidate. Sudah bisa dipasang dan dipakai, tetapi masih dalam tahap pengujian.

---

## Ringkasan

| | |
|---|---|
| Basis | Ubuntu 24.04 LTS (Noble), keluarga Debian |
| Arsitektur | amd64 |
| Desktop | XFCE + LightDM |
| Installer | Calamares |
| Pembuatan ISO | [live-build](https://live-team.pages.debian.net/live-manual/) |
| Ukuran ISO | sekitar 3,8 GiB |
| Boot | UEFI (paket `shim-signed` dan `grub-efi-amd64-signed` disertakan) |

---

## Isi

**Desktop dan kantor:** XFCE, LibreOffice, Evince, VLC, OBS Studio, FileZilla, GParted, GNOME Disks, Bluetooth (BlueZ + Blueman), SQLite Browser

**Alat dasar (CLI):** git, curl, wget, nano, vim, htop, tmux, net-tools, Python 3 (pip, venv), SQLite 3

**Jaringan:** NetworkManager, nmap, netcat, tcpdump, socat, dnsutils, traceroute, whois, nftables

**Keamanan:** Wireshark, John the Ripper, OpenSSL, Metasploit Framework, Burp Suite Community

**Layanan terpasang:** nginx (web server), dropbear (server SSH)

---

## Unduh dan verifikasi

[⬇️ Unduh RianOS-1.0-RC-amd64.iso](https://drive.google.com/uc?export=download&id=15YkaPj4-rdN_dwC3aaiUorxf_qKXSWMs)

Kalau unduhan tidak mulai (Google Drive bisa menampilkan halaman peringatan untuk berkas besar), buka [folder Google Drive](https://drive.google.com/drive/folders/1LmG6_M8X6-0EWgXnaPa4qiYnxmOzuj8L?usp=drive_link) dan unduh dari sana.

### Checksum SHA256

Lihat [`SHA256SUMS`](SHA256SUMS) untuk memverifikasi keutuhan ISO yang diunduh. Taruh di folder yang sama dengan ISO, lalu jalankan:

```bash
sha256sum -c SHA256SUMS
```

Hasilnya harus `RianOS-1.0-RC-amd64.iso: OK`.

Di Windows (PowerShell), bandingkan hasil perintah ini dengan hash di dalam `SHA256SUMS`:

```powershell
Get-FileHash .\RianOS-1.0-RC-amd64.iso -Algorithm SHA256
```

## Menulis ke USB

Gunakan flashdisk **8 GB atau lebih** (ISO sekitar 3,8 GiB, tidak muat di flashdisk "4 GB").

- **Windows:** [Rufus](https://rufus.ie) (pilih mode *DD Image*) atau [Ventoy](https://www.ventoy.net)
- **Linux:**

  ```bash
  lsblk   # pastikan nama perangkat USB
  sudo dd if=RianOS-1.0-RC-amd64.iso of=/dev/sdX bs=4M status=progress conv=fsync
  ```

  Ganti `/dev/sdX` dengan perangkat USB yang benar. Salah memilih perangkat akan menghapus isinya.

Setelah itu boot dari USB dan jalankan **Calamares** untuk memasang RianOS ke disk.

---

## Layanan yang aktif secara bawaan

Beberapa layanan menyala otomatis setelah RianOS dipasang. Berikut daftarnya supaya kamu tahu apa yang terbuka:

| Layanan | Port | Keterangan |
|---|---|---|
| `dropbear` | 22 (TCP) | Server SSH |
| `nginx` | 80 (TCP) | Web server |
| `avahi-daemon` | 5353 (UDP) | Penemuan perangkat di jaringan lokal (mDNS) |
| `cups` | 631 (hanya loopback) | Layanan cetak |

**Firewall tidak diaktifkan secara bawaan.** `nftables` sudah terpasang, dan aturannya kamu atur sendiri.

Mematikan layanan yang tidak dipakai:

```bash
sudo systemctl disable --now nginx dropbear
```

Melihat port yang sedang mendengar:

```bash
sudo ss -tulpn
```

---

## Perangkat lunak pihak ketiga

- **Metasploit Framework** dipasang dari repositori resmi Rapid7 saat ISO dibuat, sehingga pembaruannya datang lewat `apt`.
- **Burp Suite Community** dipasang dari installer PortSwigger.
- Lisensi berbayar (misalnya Burp Suite Professional) **tidak disertakan**. Pengguna membeli dan memasukkan lisensinya sendiri.
- Seluruh perangkat lunak pihak ketiga tetap tunduk pada lisensi dan syarat penggunaan masing-masing pemiliknya.

---

## Membangun ISO sendiri

Dibutuhkan Ubuntu (atau WSL Ubuntu) dengan `live-build`, ruang disk yang cukup, dan **koneksi internet aktif**, karena beberapa hook mengunduh paket saat build.

```bash
git clone https://github.com/rianherlambang82-ctrl/RianOs.git
cd RianOs
sudo ./scripts/02-configure.sh && sudo ./scripts/03-build.sh
```

Struktur konfigurasi utama:

```
config/
├── package-lists/     # daftar paket (custom.list.chroot, live.list.chroot)
├── includes.chroot/   # file yang disalin ke sistem (skrip, konfigurasi, branding)
└── hooks/             # skrip yang dijalankan saat build (live/ dan normal/)
```

Perubahan harus dibuat di dalam `config/`, lalu ISO dibangun ulang. Edit langsung di sesi live akan hilang saat reboot.

---

## Rencana

- Varian server/headless untuk keamanan.
- Integrasi asisten AI opsional (kunci API dimasukkan pengguna sendiri, tidak disertakan di ISO).
- Persistence untuk live USB.

---

## Penggunaan yang bertanggung jawab

Alat keamanan di RianOS hanya boleh dipakai pada sistem **milik sendiri** atau yang kamu punya **izin tertulis** untuk mengujinya. Penyalahgunaan sepenuhnya tanggung jawab pengguna.

---

## Lisensi

<!-- TODO: isi dengan lisensi pilihanmu, misalnya GPL-3.0 atau MIT, lalu tambahkan file LICENSE -->
Lisensi proyek ini: _belum ditentukan_.

Komponen pihak ketiga memiliki lisensinya sendiri (lihat bagian *Perangkat lunak pihak ketiga*).

---

Dibuat oleh **Rian**.
