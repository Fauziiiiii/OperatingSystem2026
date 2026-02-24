# Praktikum Sistem Operasi – Pertemuan 1  
### Pengenalan Sistem Operasi & Instalasi Ubuntu Server

> Nama   : Muhammad Fauzi Fadillah
>
> NIM    : 254107020085
>
> Kelas  : TI_1G

---

# LATIHAN 1.1  
## Lima Fungsi Utama Sistem Operasi

Sistem operasi adalah perangkat lunak yang berfungsi sebagai **resource manager** dan **extended machine**. Berikut lima fungsi utamanya beserta contoh pada Windows dan Ubuntu (Linux).


## 1. Process Management

Mengatur proses yang berjalan agar CPU digunakan secara efisien.

Fungsi:
- Process scheduling
- Process creation & termination
- Sinkronisasi proses
- Inter-process communication

Contoh:

Pada Windows:
- Task Manager menampilkan proses aktif
- End Task untuk menghentikan aplikasi

Pada Linux:
- `ps`, `top`, `htop` untuk melihat proses
- `kill` untuk menghentikan proses


## 2. Memory Management

Mengatur penggunaan RAM dan virtual memory.

Fungsi:
- Memory allocation
- Virtual memory
- Memory protection
- Paging dan swapping

Contoh:

Windows:
- Menggunakan pagefile.sys sebagai virtual memory
- Monitoring RAM melalui Task Manager

Ubuntu:
- Menggunakan swap partition
- `free -h` untuk melihat penggunaan memori


## 3. File Management

Mengatur penyimpanan dan struktur file.

Fungsi:
- Operasi file (create, read, write, delete)
- Struktur direktori
- Permission
- Mounting filesystem

Contoh:

Windows:
- File Explorer
- Filesystem NTFS

Ubuntu:
- `ls`, `cp`, `mv`, `rm`
- Filesystem ext4


## 4. I/O Management

Mengatur komunikasi antara sistem dan perangkat keras.

Fungsi:
- Device drivers
- Buffering
- Interrupt handling
- Spooling

Contoh:

Windows:
- Device Manager
- Printer queue

Ubuntu:
- Driver dikelola kernel Linux
- `lsusb`, `lspci` untuk melihat perangkat


## 5. Security dan Protection

Melindungi sistem dari akses tidak sah.

Fungsi:
- Authentication
- Authorization
- Encryption
- Logging

Contoh:

Windows:
- Login password/PIN
- BitLocker

Ubuntu:
- Sistem permission rwx
- sudo untuk hak akses administrator

---

# LATIHAN 1.2  
## Analisis Pemilihan Sistem Operasi Berdasarkan Use Case


## 1. Gaming

Rekomendasi utama: Windows

Alasan:
- Kompatibilitas game paling luas
- Dukungan DirectX
- Driver GPU optimal

Linux dapat digunakan melalui Steam Proton, namun tidak semua game berjalan sempurna.


## 2. Software Development

Rekomendasi:
- Ubuntu
- macOS

Alasan:
- Berbasis Unix
- Tools development lengkap
- Package manager kuat (apt, brew)

Windows cocok untuk:
- .NET
- Game development
- Aplikasi berbasis Windows


## 3. Server

Rekomendasi utama: Linux (Ubuntu Server, Debian, RHEL)

Alasan:
- Stabil
- Ringan
- Dominan di cloud computing
- Open source

Windows Server cocok untuk:
- Active Directory
- Infrastruktur berbasis Microsoft


## 4. Creative Work

Rekomendasi utama: macOS

Alasan:
- Integrasi hardware dan software
- Stabil untuk editing video dan audio
- Standar industri kreatif

Windows juga banyak digunakan untuk Adobe dan software desain lainnya.


## 5. Enterprise

Kombinasi:
- Windows untuk client dan domain
- Linux untuk server dan backend


## Kesimpulan Latihan 1.2

Pemilihan sistem operasi harus disesuaikan dengan kebutuhan:

- Gaming → Windows  
- Development → Linux / macOS  
- Server → Linux  
- Creative Work → macOS  
- Enterprise → Windows + Linux  

Tidak ada OS yang terbaik untuk semua kebutuhan.

---

# LATIHAN 1.3  
## Instalasi Ubuntu Server 22.04 LTS di VirtualBox


## 1. Download ISO

Unduh Ubuntu Server:
https://ubuntu.com/download/server  
![ubuntu1.png](image/image-1.3.1-1.png)


## 2. Membuat dan Konfigurasi Virtual Machine

- ![alt text](image/image-1.3.2-1.png)

- ![alt text](image/image-1.3.2-2.png)

- ![alt text](image/image-1.3.2-3.png)

- ![alt text](image/image-1.3.2-4.png)

- ![alt text](image/image-1.3.2-5.png)


## 3. Reboot dan Login

- ![alt text](image/image-1.3.3-1.png)

---

# LATIHAN 1.4  
## Setelah instalasi Ubuntu Server, lakukan tasks berikut:


### 1. Update package list:

```bash
sudo apt update
```
- ![alt text](image/image-1.4.1-1.png)


### 2.  Upgrade packages: 

```bash
sudo apt upgrade
```

- ![alt text](image/image-1.4.2-1.png)


### 3. Install neofetch:

```bash
sudo apt install neofetch
```

- ![alt text](image/image-1.4.3-1.png)


### 4. Jalankan neofetch dan screenshot hasilnya

```bash
neofetch
```

- ![alt text](image/image-1.4.4-1.png)


### 5. Check disk usage dengan:

```bash
df -h
```

- ![alt text](image/image-1.4.5-1.png)


### 6. Check memory dengan free -h

```bash
free -h
```

- ![alt text](image/image-1.4.6-1.png)

---

# LATIHAN 1.5
## Eksplorasi sistem yang baru diinstall


### 1. Tampilkan informasi OS

```bash
cat /etc/os-release
```

- ![alt text](image/image-1.5.1-1.png)


### 2. Tampilkan versi kernel: 

```bash
uname -r
```

- ![alt text](image/image-1.5.2-1.png)


### 3.  List partisi: 

```bash
lsblk
```

- ![alt text](image/image-1.5.3-1.png)


### 4. Check network connectivity: 

```bash
ping -c 4 google.com
```

- ![alt text](image/image-1.5.4-1.png)


### 5. Install dan jalankan htop untuk melihat resource usage

```bash
htop
```

- ![alt text](image/image-1.5.4-1.png)


### 6. Buat laporan singkat tentang konfigurasi sistem anda

#### Sistem

- OS: Ubuntu Server 24.04 LTS

- Kernel: 6.8.0-100

- RAM: 4 GB

- Storage: 25 GB

- Network: NAT (IP otomatis via DHCP)

- Swap: Aktif

#### Sistem berhasil:

- Login normal

- Terkoneksi internet

- Update package berhasil

- Monitoring sistem berjalan dengan baik


---

# LATIHAN 1.6  
## Refleksi Pengalaman Menggunakan Sistem Operasi

### Sistem Operasi yang Digunakan Sehari-hari

Sistem operasi yang saya gunakan sehari-hari adalah **Windows 11**. Sistem ini saya gunakan untuk kebutuhan perkuliahan, mengerjakan tugas, game, menonton film, serta menjalankan beberapa aplikasi pemrograman seperti Visual Studio Code dan software MSOffice.

Saya telah menggunakan Windows sejak awal SMK (sekitar 4 tahun) sehingga sudah cukup terbiasa dengan ui dan sistem pengaturannya

---

### Hal yang Disukai

Beberapa hal yang saya sukai dari Windows:

1. UI yang mudah dipahami.
2. Dukungan software yang sangat lengkap.
3. Kompatibilitas hardware yang luas.
4. Cocok untuk kebutuhan umum bahkan ngegame.

Selain itu, hampir semua aplikasi yang dibutuhkan untuk perkuliahan tersedia dan berjalan dengan baik di Windows.

---

### Tantangan atau Masalah yang Pernah Dihadapi

Beberapa kendala yang pernah saya alami antara lain:

- Update sistem yang terkadang memakan waktu lama
- Performa menurun jika terlalu banyak aplikasi berjalan
- Penggunaan RAM yang cukup besar dibanding sistem operasi berbasis Linux

Namun, masalah tersebut masih bisa diatasi dengan melakukan maintenance rutin seperti membersihkan aplikasi startup dan melakukan update secara berkala

---

### Pengalaman Menggunakan Sistem Operasi Lain

Melalui praktikum ini, saya mencoba menginstal **Linux Ubuntu** di VirtualBox. Ini adalah pengalaman pertama saya menggunakan sistem operasi berbasis Linux secara langsung.

Perbedaan yang saya rasakan:

- Ubuntu Server berbasis command line tanpa GUI.
- Sistem terasa lebih ringan dan minimalis.
- Konfigurasi dilakukan melalui terminal.
- Memberikan pemahaman lebih dalam tentang manajemen sistem.

Pengalaman ini membantu saya memahami bahwa sistem operasi tidak hanya soal tampilan grafis, tetapi juga tentang bagaimana sistem mengelola resource seperti CPU, memori, dan storage.

---

### Sistem Operasi yang Ingin Dicoba

Setelah mempelajari materi ini, saya tertarik untuk mencoba:

- Ubuntu Desktop untuk merasakan Linux dengan GUI.
- Distribusi Linux lain seperti Debian atau Arch Linux untuk memperdalam pemahaman.
- macOS untuk mengetahui bagaimana integrasi hardware dan software pada perangkat Apple.

Alasan utamanya adalah untuk memperluas wawasan dan meningkatkan kemampuan teknis, terutama karena Linux banyak digunakan pada server dan cloud computing.

---

## Kesimpulan Refleksi

Dari pengalaman penggunaan Windows dan praktik instalasi Ubuntu Server, saya menyimpulkan bahwa setiap sistem operasi memiliki keunggulan masing-masing sesuai kebutuhan pengguna.

Windows sangat cocok untuk penggunaan umum dan kompatibilitas aplikasi. Ubuntu Server memberikan pengalaman teknis yang lebih mendalam serta relevan untuk bidang sistem administrasi, DevOps, dan cloud.

Pembelajaran pada pertemuan ini membantu saya memahami konsep dasar sistem operasi secara teori dan praktik, serta memberikan gambaran pentingnya penguasaan Linux dalam dunia industri teknologi.
