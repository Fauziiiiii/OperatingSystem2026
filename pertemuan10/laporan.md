# Praktikum Sistem Operasi – Pertemuan 10  
##  1 Manajemen Memori & System Call  


> Nama   : Muhammad Fauzi Fadillah
>
> NIM    : 254107020085
>
> Kelas  : TI_1G


---

# Praktikum 10.1 Melihat Penggunaan Memori  


### Langkah 1 - Jalankan free -h untuk melihat ringkasan RAM dan swap.:

Command:

```bash
free -h
```

Output:  

```bash
root@UbuntuServer:/home/fauzi# free -h
               total        used        free      shared  buff/cache   available
Mem:           3.7Gi       417Mi       3.2Gi       1.0Mi       304Mi       3.3Gi
Swap:          4.0Gi          0B       4.0Gi
```

### Langkah 2 - Lihat detail memori dari kernel melalui /proc/meminfo:

Command:

```bash
cat /proc/meminfo | head -n 20
```


Output:

```bash
root@UbuntuServer:/home/fauzi# cat /proc/meminfo | head -n 20
MemTotal:        3911664 kB
MemFree:         3374444 kB
MemAvailable:    3468068 kB
Buffers:           19724 kB
Cached:           277112 kB
SwapCached:            0 kB
Active:           279744 kB
Inactive:          55956 kB
Active(anon):      48772 kB
Inactive(anon):        0 kB
Active(file):     230972 kB
Inactive(file):    55956 kB
Unevictable:       27316 kB
Mlocked:           27316 kB
SwapTotal:       4194300 kB
SwapFree:        4194300 kB
Zswap:                 0 kB
Zswapped:              0 kB
Dirty:                 0 kB
Writeback:             0 kB
```


### Analisis

1. Hitung persentase memori tersedia: available / total × 100%. Jika hasilnya
di bawah 10%, sistem mulai kekurangan memori.  
Jawab:  
(3.3/3.7)×100%=89%  
Jadi, persentase memori tersedia kurang lebih 89%.Ini berada di atas 10%, jadi sistem tidak kekurangan memori (masih sangat aman).

2. Pada baris Swap, apakah kolom used bernilai 0? Jika lebih dari 0, kernel sudah
pernah memindahkan data ke disk karena RAM tidak cukup.  
Jawab:  
Swap total = 4.0 GiB
Swap used = 0 B  
Jadi, nilai used = 0, artinya sistem belum pernah menggunakan swap. RAM masih cukup untuk menjalankan proses. Belum ada pemindahan data dari RAM ke disk

3. Perhatikan field Cached dan Buffers di /proc/meminfo. Nilai ini sesuai
dengan kolom buff/cache pada free -h.  
Jawab:  
Buffers = 19,724 kB (19 MB)  
Cached = 277,112 kB (277 MB)  
Jika dijumlahkan: 19 MB+277 MB=296 MB  
buff/cache = 304 MB  
Sistem menggunakan RAM untuk cache agar akses data lebih cepat. Memori ini bisa dibebaskan kapan saja jika dibutuhkan


---

# Praktikum 10.1 Melihat Penggunaan Memori  

### Langkah 1: Periksa kondisi memori secara keseluruhan.

Command:

```bash
free -h
```

Output:  

```bash
root@UbuntuServer:/home/fauzi# free -h
               total        used        free      shared  buff/cache   available
Mem:           3.7Gi       417Mi       3.2Gi       1.0Mi       304Mi       3.3Gi
Swap:          4.0Gi          0B       4.0Gi
```


### Langkah 2: Pantau proses secara real-time

Command:

```bash
top
```

tekan M untuk mengurutkan berdasarkan memori, tekan q untuk keluar.


Output:  

```bash
root@UbuntuServer:/home/fauzi# top
top - 14:19:35 up 22 min,  2 users,  load average: 0.00, 0.00, 0.00
Tasks: 119 total,   1 running, 118 sleeping,   0 stopped,   0 zombie
%Cpu(s):  0.0 us,  0.0 sy,  0.0 ni, 99.8 id,  0.2 wa,  0.0 hi,  0.0 si,  0.0 st
MiB Mem :   3820.0 total,   3206.2 free,    446.7 used,    390.3 buff/cache
MiB Swap:   4096.0 total,   4096.0 free,      0.0 used.   3373.3 avail Mem

    PID USER      PR  NI    VIRT    RES    SHR S  %CPU  %MEM     TIME+ COMMAND
   1142 root      20   0  691508  44104  38296 S   0.0   1.1   0:01.74 fwupd
    367 root      rt   0  288988  27324   8760 S   0.0   0.7   0:00.28 multipathd
    651 root      20   0  109684  23180  13728 S   0.0   0.6   0:00.16 unattended-upgr
    312 root      19  -1   66856  17228  16052 S   0.0   0.4   0:00.36 systemd-journal
    623 root      20   0  468972  13616  11460 S   0.0   0.3   0:00.14 udisksd
      1 root      20   0   22000  13200   9488 S   0.0   0.3   0:01.26 systemd
    462 systemd+  20   0   21584  12964  10668 S   0.0   0.3   0:00.16 systemd-resolve
    681 root      20   0  392032  12912  10840 S   0.0   0.3   0:00.11 ModemManager
    967 fauzi     20   0   20100  11256   9352 S   0.0   0.3   0:00.09 systemd
   1037 root      20   0   14964  10652   8944 S   0.0   0.3   0:00.02 sshd
    436 systemd+  20   0   19012   9500   8336 S   0.0   0.2   0:00.10 systemd-network
   1149 root      20   0  314152   9284   7656 S   0.3   0.2   0:00.20 upowerd
    615 root      20   0   18128   9012   7896 S   0.0   0.2   0:00.08 systemd-logind
    792 root      20   0   12024   8132   6996 S   0.0   0.2   0:00.00 sshd
    607 polkitd   20   0  308164   8092   7196 S   0.0   0.2   0:00.09 polkitd
    466 systemd+  20   0   91028   7908   6932 S   0.0   0.2   0:00.04 systemd-timesyn
    376 root      20   0   29084   7804   4988 S   0.0   0.2   0:00.17 systemd-udevd
   1024 root      20   0   16904   7456   6196 S   0.0   0.2   0:00.03 sudo
   1105 root      20   0   16900   7444   6180 S   0.0   0.2   0:00.01 sudo
   1095 fauzi     20   0   15124   7140   5172 S   0.0   0.2   0:00.34 sshd
    652 syslog    20   0  222508   6200   4648 S   0.0   0.2   0:00.07 rsyslogd
   1198 root      20   0   12072   6052   3744 R   0.0   0.2   0:00.01 top
   1096 fauzi     20   0    8648   5660   3948 S   0.0   0.1   0:00.01 bash
    977 fauzi     20   0    8656   5640   3920 S   0.0   0.1   0:00.06 bash
    600 message+  20   0    9784   5620   4676 S   0.0   0.1   0:00.12 dbus-daemon
   1027 root      20   0    8032   5148   4100 S   0.0   0.1   0:00.01 bash
   1108 root      20   0    7992   4928   3912 S   0.0   0.1   0:00.05 bash
    824 root      20   0    6948   4892   4072 S   0.0   0.1   0:00.05 login
   1107 root      20   0    9376   4524   4084 S   0.0   0.1   0:00.00 su
   1026 root      20   0    9376   4520   4080 S   0.0   0.1   0:00.00 su
    968 fauzi     20   0   21156   3572   1844 S   0.0   0.1   0:00.00 (sd-pam)
    763 root      20   0    6824   2904   2664 S   0.0   0.1   0:00.00 cron
   1025 root      20   0   16904   2620   1356 S   0.0   0.1   0:00.00 sudo
   1106 root      20   0   16900   2612   1356 S   0.0   0.1   0:00.00 sudo
      2 root      20   0       0      0      0 S   0.0   0.0   0:00.01 kthreadd
      3 root      20   0       0      0      0 S   0.0   0.0   0:00.00 pool_workqueue_release
      4 root       0 -20       0      0      0 I   0.0   0.0   0:00.00 kworker/R-rcu_g
      5 root       0 -20       0      0      0 I   0.0   0.0   0:00.00 kworker/R-rcu_p
      6 root       0 -20       0      0      0 I   0.0   0.0   0:00.00 kworker/R-slub_
      7 root       0 -20       0      0      0 I   0.0   0.0   0:00.00 kworker/R-netns
      8 root      20   0       0      0      0 I   0.3   0.0   0:03.59 kworker/0:0-events
     11 root      20   0       0      0      0 I   0.0   0.0   0:00.00 kworker/u4:0-ipv6_addrconf
     12 root       0 -20       0      0      0 I   0.0   0.0   0:00.00 kworker/R-mm_pe
     13 root      20   0       0      0      0 I   0.0   0.0   0:00.00 rcu_tasks_kthread
     14 root      20   0       0      0      0 I   0.0   0.0   0:00.00 rcu_tasks_rude_kthread
     15 root      20   0       0      0      0 I   0.0   0.0   0:00.00 rcu_tasks_trace_kthread
     16 root      20   0       0      0      0 S   0.0   0.0   0:00.06 ksoftirqd/0
```


**Analisis**

1. Apakah nilai available sangat kecil (misalnya di bawah 200 MB pada server
dengan RAM 2 GB)? Jika ya, server kemungkinan kekurangan memori.  
Jawaban:  
Avail Mem = 3371.6 MiB (3.3 GB)  
Total RAM ≈ 3.8 GB  
hasilnya Nilai available sangat besar (±3.3 GB). Jauh di atas batas kritis (200 MB untuk RAM kecil)  
Jadi, Sistem tidak kekurangan memori. Kondisi RAM masih sangat longgar / aman

2. Apakah kolom used pada baris Swap lebih dari 0? Jika ya, kernel sedang
menggunakan swap, yang berarti performa menurun.  
Jawaban:  
Swap total = 4096 MiB  
Swap used = 0.0 MiB  
Jadi, Swap tidak digunakan sama sekali. Kernel tidak memindahkan data ke disk. Performa masih optimal (tidak ada bottleneck dari swap)

3. Di tampilan top, proses apa yang memiliki %MEM terbesar? Proses tersebut
menjadi kandidat utama penyebab lambatnya server.  
Jawaban:  
fwupd: 1.1% MEM (44 MB)  
multipathd: 0.7%  
unattended-upgr: 0.6%  
hasilnya Tidak ada proses yang menggunakan memori besar (semua < 2%). Proses fwupd adalah yang paling tinggi, tapi masih sangat kecil  
Jadi, Tidak ada proses yang mencurigakan atau membebani RAM. Tidak ada indikasi penyebab server lambat dari sisi memori


---

# Praktikum 10.2 Mengamati Aktivitas Paging  

### Langkah 1: Jalankan vmstat dengan interval 1 detik, 5 sampel

Command:

```bash
vmstat 1 5
```

Output:  

```bash
root@UbuntuServer:/home/fauzi# vmstat 1 5
procs -----------memory---------- ---swap-- -----io---- -system-- -------cpu-------
 r  b   swpd   free   buff  cache   si   so    bi    bo   in   cs us sy id wa st gu
 1  0      0 3286420  23856 375888    0    0   192    30  156    0  0  0 99  0  0  0
 0  0      0 3286420  23856 375928    0    0     0     0  372  136  0  2 98  0  0  0
 0  0      0 3286420  23856 375928    0    0     0     0  256   91  0  1 99  0  0  0
 0  0      0 3286420  23856 375928    0    0     0     0   95   52  0  0 100  0  0  0
 1  0      0 3286420  23856 375928    0    0     0     0  131   43  0  0 100  0  0  0
```

**Analisis**

1. Amati nilai si dan so pada kelima baris. Pada sistem normal dengan RAM
cukup, kedua nilai ini selalu 0.  
Jawaban:  
Tidak ada aktivitas swap sama sekali. Sistem tidak pernah memindahkan data antara RAM dan disk. Ini menandakan RAM masih sangat cukup

2. Jika nilai si atau so sesekali muncul lebih dari 0, artinya pernah ada aktivitas
swap. Ini masih wajar jika tidak terus-menerus.  
Jawaban:  
Tidak ada nilai si/so > 0, bahkan sekali pun

3. Jika si dan so terus-menerus lebih dari 0, sistem dalam kondisi memory
pressure serius — performa turun drastis karena akses disk jauh lebih lambat
dari RAM.  
Jawaban:  
Tidak ada memory pressure. Performa sistem tidak terganggu oleh memori

4. Perhatikan juga kolom free (RAM kosong) dan buff (buffer) untuk memahami
kondisi keseluruhan RAM saat itu.  
Jawaban:  
RAM kosong masih sangat banyak. Buffer & cache digunakan secara wajar. Cache ini bisa dilepas jika dibutuhkan


---

# Praktikum 10.3 Membuat dan Mengonfigurasi Swap File


### Langkah 1: Buat file berukuran 512 MB sebagai calon swap

Command:

```bash
sudo fallocate -l 512 M /swapfile-week10
```

Output:  

```bash
root@UbuntuServer:~/praktikum-os/week10# sudo fallocate -l 512M /swapfile-week10
root@UbuntuServer:~/praktikum-os/week10# ls -lh /swapfile-week10
-rw-r--r-- 1 root root 512M May  5 14:36 /swapfile-week10
```


### Langkah 2: Atur permission file menjadi 600 — hanya root yang boleh membaca dan menulis.

Command:

```bash
sudo chmod 600 /swapfile-week10
```

Output:  

```bash
root@UbuntuServer:~/praktikum-os/week10# sudo fallocate -l 512M /swapfile-week10
root@UbuntuServer:~/praktikum-os/week10# ls -lh /swapfile-week10
-rw-r--r-- 1 root root 512M May  5 14:36 /swapfile-week10
```


### Langkah 3: Format file sebagai area swap, lalu aktifkan

Command:

```bash
sudo mkswap /swapfile-week10
sudo swapon /swapfile-week10
```

Output:  

```bash
root@UbuntuServer:~/praktikum-os/week10# sudo mkswap /swapfile-week10
mkswap: /swapfile-week10: insecure permissions 0644, fix with: chmod 0600 /swapfile-week10
Setting up swapspace version 1, size = 512 MiB (536866816 bytes)
no label, UUID=7154ea82-e334-473f-b618-4e3da64532ba
root@UbuntuServer:~/praktikum-os/week10# sudo swapon /swapfile-week10
swapon: /swapfile-week10: insecure permissions 0644, 0600 suggested.
```


### Langkah 4: Verifikasi swap aktif. Anda akan melihat entri /swapfile-week10 dengan ukuran 512M, dan nilai total pada baris Swap di free -h bertambah 512M.


Command:

```bash
swapon --show
free -h
```

Output:  

```bash
root@UbuntuServer:~/praktikum-os/week10# swapon --show
NAME             TYPE SIZE USED PRIO
/swapfile        file   4G   0B   -2
/swapfile-week10 file 512M   0B   -3
root@UbuntuServer:~/praktikum-os/week10# free -h
               total        used        free      shared  buff/cache   available
Mem:           3.7Gi       459Mi       3.1Gi       1.0Mi       393Mi       3.3Gi
Swap:          4.5Gi          0B       4.5Gi
```


### Langkah 5: Periksa nilai swappiness, ubah sementara, dan verifikasi perubahan.


Command:

```bash
cat /proc/sys/vm/swappiness
sudo sysctl vm.swappiness=10
cat /proc/sys/vm/swappiness
```

Output:  

```bash
root@UbuntuServer:~/praktikum-os/week10# swapon --show
NAME             TYPE SIZE USED PRIO
/swapfile        file   4G   0B   -2
/swapfile-week10 file 512M   0B   -3
root@UbuntuServer:~/praktikum-os/week10# free -h
               total        used        free      shared  buff/cache   available
Mem:           3.7Gi       459Mi       3.1Gi       1.0Mi       393Mi       3.3Gi
Swap:          4.5Gi          0B       4.5Gi
```

**Analisis**:
1. Berapa nilai swappiness default? Apa artinya bagi perilaku kernel dalam
menggunakan swap?  
Jawaban:  
Nilai default 60
Kernel cukup seimbang:  
Tidak terlalu cepat pakai swap tapi juga tidak terlalu menahan penggunaan swap. Swap mulai digunakan saat RAM mulai terpakai cukup banyak
Jadi, Nilai 60 = kompromi antara performa dan efisiensi memori

2. Setelah diubah ke 10, konfirmasi nilai berubah pada output cat kedua. Apa
dampak nilai 10 terhadap penggunaan swap dibanding nilai 60?
Jawaban:  
Nilai berhasil berubah dari 60 → 10  
Dampaknya:  
Kernel akan lebih mengutamakan RAM. swap hanya digunakan jika RAM benar-benar mulai penuh  
Swappiness 60:  
Lebih cepat menggunakan swap
RAM lebih “lega”, tapi akses bisa lebih lambat (karena disk)  
Swappiness 10:  
Sangat jarang menggunakan swap
Performa lebih cepat (karena fokus di RAM)
Cocok untuk server ringan seperti punyamu  
Jadi, Nilai 10 → lebih optimal untuk performa, tapi butuh RAM cukup (dan kamu punya, jadi aman)

3. Apakah entri /swapfile-week10 muncul di swapon –show? Jika tidak,
pastikan Langkah 2 (chmod 600) sudah dijalankan sebelum Langkah 3.
Jawaban:  
/swapfile-week10 muncul berarti swap berhasil diaktifkan


---

# Praktikum 10.4 Monitoring Memory


### Langkah 1: Ambil snapshot proses diurutkan dari penggunaan memori terbesar

Command:

```bash
ps aux --sort=-%mem | head
```

Output:  

```bash
root@UbuntuServer:~/praktikum-os/week10# ps aux --sort=-%mem | head
USER         PID %CPU %MEM    VSZ   RSS TTY      STAT START   TIME COMMAND
root        1781 22.7  3.3 370288 130736 ?       Sl   15:01   0:19 /usr/bin/python3 /usr/bin/unattended-upgrade --download-only
root        1142  0.0  1.1 691508 44104 ?        Ssl  14:08   0:01 /usr/libexec/fwupd/fwupd
root         367  0.0  0.6 288988 27324 ?        SLsl 13:57   0:00 /sbin/multipathd -d -s
root         651  0.0  0.5 109684 23180 ?        Ssl  13:57   0:00 /usr/bin/python3 /usr/share/unattended-upgrades/unattended-upgrade-shutdown --wait-for-signal
root        1666  0.0  0.5 372600 21004 ?        Ssl  15:01   0:00 /usr/libexec/packagekitd
root         312  0.0  0.4  66856 17288 ?        S<s  13:57   0:00 /usr/lib/systemd/systemd-journald
root         623  0.0  0.3 468972 13616 ?        Ssl  13:57   0:00 /usr/libexec/udisks2/udisksd
root           1  0.0  0.3  22000 13244 ?        Ss   13:57   0:01 /sbin/init splash noprompt noshell automatic-ubiquity
systemd+     462  0.0  0.3  21584 12996 ?        Ss   13:57   0:00 /usr/lib/systemd/systemd-resolved
```


### Langkah 2: Pantau secara real-time dengan top

Command:

```bash
top
```

Di dalam top: M = urutkan berdasarkan memori, P = urutkan berdasarkan
CPU, q = keluar.


Output:  

```bash
root@UbuntuServer:~/praktikum-os/week10# top
top - 15:10:25 up  1:13,  2 users,  load average: 0.00, 0.00, 0.00
Tasks: 127 total,   1 running, 126 sleeping,   0 stopped,   0 zombie
%Cpu(s):  0.0 us,  0.2 sy,  0.0 ni, 99.5 id,  0.0 wa,  0.0 hi,  0.3 si,  0.0 st
MiB Mem :   3820.0 total,   2744.4 free,    479.0 used,    825.7 buff/cache
MiB Swap:   4608.0 total,   4608.0 free,      0.0 used.   3341.0 avail Mem

    PID USER      PR  NI    VIRT    RES    SHR S  %CPU  %MEM     TIME+ COMMAND
   1846 _apt      20   0   22904  10840   9712 S   1.0   0.3   0:10.61 http
   1847 _apt      20   0   22912  10920   9780 S   0.7   0.3   0:03.35 http
     17 root      20   0       0      0      0 I   0.3   0.0   0:00.29 rcu_preempt
   1216 root      20   0       0      0      0 I   0.3   0.0   0:07.42 kworker/0:1-mm_percpu_wq
   1860 root      20   0   11960   5996   3740 R   0.3   0.2   0:00.04 top
      1 root      20   0   22000  13244   9488 S   0.0   0.3   0:01.51 systemd
      2 root      20   0       0      0      0 S   0.0   0.0   0:00.02 kthreadd
      3 root      20   0       0      0      0 S   0.0   0.0   0:00.00 pool_workqueue_release
      4 root       0 -20       0      0      0 I   0.0   0.0   0:00.00 kworker/R-rcu_g
      5 root       0 -20       0      0      0 I   0.0   0.0   0:00.00 kworker/R-rcu_p
      6 root       0 -20       0      0      0 I   0.0   0.0   0:00.00 kworker/R-slub_
      7 root       0 -20       0      0      0 I   0.0   0.0   0:00.00 kworker/R-netns
     11 root      20   0       0      0      0 I   0.0   0.0   0:00.00 kworker/u4:0-ipv6_addrconf
     12 root       0 -20       0      0      0 I   0.0   0.0   0:00.00 kworker/R-mm_pe
     13 root      20   0       0      0      0 I   0.0   0.0   0:00.00 rcu_tasks_kthread
     14 root      20   0       0      0      0 I   0.0   0.0   0:00.00 rcu_tasks_rude_kthread
     15 root      20   0       0      0      0 I   0.0   0.0   0:00.00 rcu_tasks_trace_kthread
     16 root      20   0       0      0      0 S   0.0   0.0   0:00.08 ksoftirqd/0
     18 root      rt   0       0      0      0 S   0.0   0.0   0:00.02 migration/0
     19 root     -51   0       0      0      0 S   0.0   0.0   0:00.00 idle_inject/0
     20 root      20   0       0      0      0 S   0.0   0.0   0:00.00 cpuhp/0
     21 root      20   0       0      0      0 S   0.0   0.0   0:00.00 cpuhp/1
     22 root     -51   0       0      0      0 S   0.0   0.0   0:00.00 idle_inject/1
     23 root      rt   0       0      0      0 S   0.0   0.0   0:00.57 migration/1
     24 root      20   0       0      0      0 S   0.0   0.0   0:00.24 ksoftirqd/1
     26 root       0 -20       0      0      0 I   0.0   0.0   0:00.00 kworker/1:0H-events_highpri
     29 root      20   0       0      0      0 S   0.0   0.0   0:00.00 kdevtmpfs
     30 root       0 -20       0      0      0 I   0.0   0.0   0:00.00 kworker/R-inet_
     31 root      20   0       0      0      0 S   0.0   0.0   0:00.00 kauditd
     32 root      20   0       0      0      0 S   0.0   0.0   0:00.00 khungtaskd
     33 root      20   0       0      0      0 S   0.0   0.0   0:00.00 oom_reaper
     35 root       0 -20       0      0      0 I   0.0   0.0   0:00.00 kworker/R-write
     36 root      20   0       0      0      0 S   0.0   0.0   0:00.11 kcompactd0
     37 root      20   0       0      0      0 I   0.0   0.0   0:00.43 kworker/u5:2-events_power_efficient
     38 root      25   5       0      0      0 S   0.0   0.0   0:00.00 ksmd
     40 root      39  19       0      0      0 S   0.0   0.0   0:00.00 khugepaged
     41 root       0 -20       0      0      0 I   0.0   0.0   0:00.00 kworker/R-kinte
     42 root       0 -20       0      0      0 I   0.0   0.0   0:00.00 kworker/R-kbloc
     43 root       0 -20       0      0      0 I   0.0   0.0   0:00.00 kworker/R-blkcg
     44 root     -51   0       0      0      0 S   0.0   0.0   0:00.00 irq/9-acpi
     45 root       0 -20       0      0      0 I   0.0   0.0   0:00.00 kworker/R-tpm_d
     46 root       0 -20       0      0      0 I   0.0   0.0   0:00.00 kworker/R-ata_s
     47 root       0 -20       0      0      0 I   0.0   0.0   0:00.00 kworker/R-md
     48 root       0 -20       0      0      0 I   0.0   0.0   0:00.00 kworker/R-md_bi
     49 root       0 -20       0      0      0 I   0.0   0.0   0:00.00 kworker/R-edac-
     50 root       0 -20       0      0      0 I   0.0   0.0   0:00.00 kworker/R-devfr
     51 root     -51   0       0      0      0 S   0.0   0.0   0:00.00 watchdogd
```

**Analisis**:
1. Proses apa yang berada di urutan pertama? Catat nilai %MEM dan RSS-nya.  
Jawaban:  
Proses: unattended-upgrade (python3),
%MEM = 3.3%,
RSS = 130736 KB

2. Konversikan RSS dari KB ke MB (bagi 1024). Misalnya, RSS=524288 berarti proses menggunakan 512 MB RAM. Apakah wajar untuk jenis program
tersebut?  
Jawaban:  
RSS (KB)÷1024:  
130736÷1024=127.7 MB  
Ya, wajar
Karena unattended-upgrade adalah proses update sistem otomatis Proses Python memang cenderung memakai memori lebih besar dibanding program kecil. 128 MB masih kecil untuk server dengan RAM ±3.7 G

3. Mengapa VSZ selalu lebih besar dari RSS pada proses yang sama?  
Jawaban:  
VSZ selalu lebih besar karena Termasuk library, memory mapping, dll Tidak semuanya aktif di RAM Jadi yang paling real itu RSS

4. Apakah urutan proses di ps konsisten dengan tampilan top saat diurutkan
berdasarkan %MEM?  
Jawaban:  
Dari ps:
Peringkat 1: unattended-upgrade (3.3%)
Dari top (saat kamu jalankan):
Paling besar terlihat:
fwupd → 1.1%
http → 0.3%  
Analisis:
Tidak konsisten 100%, karena ps = snapshot (sekali ambil data) top = real-time (berubah terus)  
Kemungkinan: Proses unattended-upgrade sudah selesai / turun saat top dijalankan  
Kesimpulan:
Perbedaan ini normal Karena kondisi sistem berubah setiap detik


---

# Praktikum 10.5 Script Monitor Memori


### Langkah 1:Masuk ke direktori kerja dan buat file script:

Command:

```bash
cd ~/praktikum-os/week10-memory
nano monitor-memori.sh

```


### Langkah 2: Ketik script berikut:

Command:

```bash
#!/bin/bash
set -euo pipefail

THRESHOLD=20

echo "=== Monitor Memori ==="
date
echo

free -h

echo

AVAIL=$(free | awk '/Mem/ {printf "%d", $7/$2*100}')
if [ "$AVAIL" -lt "$THRESHOLD" ]; then
    echo "PERINGATAN: Memori tersedia hanya ${AVAIL}%"
else
    echo "Status: Memori tersedia ${AVAIL}% (normal)"
fi

echo
echo "--- 5 Proses Memori Tertinggi ---"
ps aux --sort=-%mem | head -n 6 | tail -n 5
```


### Langkah 3: Menjalankan script monitor

Command:

```bash
chmod +x monitor-memori.sh
bash monitor-memori.sh
```

Output:  

```bash
root@UbuntuServer:~/praktikum-os/week10-memory# chmod +x monitor-memori.sh
bash monitor-memori.sh
=== Monitor Memori ===
Tue May  5 03:39:04 PM UTC 2026

               total        used        free      shared  buff/cache   available
Mem:           3.7Gi       476Mi       2.4Gi       1.1Mi       1.1Gi       3.3Gi
Swap:          4.5Gi          0B       4.5Gi

Status: Memori tersedia 87% (normal)

--- 5 Proses Memori Tertinggi ---
root        1781  0.8  3.3 370288 130736 ?       Sl   15:01   0:19 /usr/bin/python3 /usr/bin/unattended-upgrade --download-only
root        1142  0.0  1.1 691508 44200 ?        Ssl  14:08   0:01 /usr/libexec/fwupd/fwupd
root         367  0.0  0.6 288988 27324 ?        SLsl 13:57   0:01 /sbin/multipathd -d -s
root         651  0.0  0.5 109684 23180 ?        Ssl  13:57   0:00 /usr/bin/python3 /usr/share/unattended-upgrades/unattended-upgrade-shutdown --wait-for-signal
root         312  0.0  0.4  66856 17312 ?        S<s  13:57   0:00 /usr/lib/systemd/systemd-journald
```


**Analisis**:
1. Variabel THRESHOLD=20 menetapkan batas persentase. Perintah free | awk
’/Mem/ {printf "%d", $7/$2*100}’ mengambil kolom ke-7 (available) dibagi
kolom ke-2 (total) dari baris Mem, lalu dikalikan 100 untuk menghasilkan
persentase bilangan bulat.

2. Kondisi if [ "$AVAIL" -lt "$THRESHOLD" ] bernilai benar jika persentase
memori tersedia di bawah 20.

3. Ubah THRESHOLD menjadi 90 dan jalankan ulang. Apa yang berubah pada
output? Mengapa demikian?  
Jawaban:  
Maka AVAIL menjadi 87 dan 87 < 90 bernilai TRUE  
Output akan berubah menjadi PERINGATAN: Memori tersedia hanya 87%


---

# Studi Kasus 10.2 Gagal Akses File

Skenario: Program tidak dapat membaca file konfigurasi. Penyebab umum: file
tidak ada, path salah, atau permission tidak sesuai. Kita akan mensimulasikan
kondisi ini dan mengamati pesan error yang dihasilkan.


### Langkah 1: Buat direktori dan file konfigurasi contoh

Command:

```bash
mkdir -p ~/praktikum-os/week10-memory/syscall-case
cd ~/praktikum-os/week10-memory/syscall-case
echo "PORT=8080" > app.conf
ls -l app.conf
cat app.conf
```


Output:

```bash
root@UbuntuServer:~/praktikum-os/week10-memory# mkdir -p ~/praktikum-os/week10-memory/syscall-case
cd ~/praktikum-os/week10-memory/syscall-case
echo "PORT=8080" > app.conf
ls -l app.conf
cat app.conf
-rw-r--r-- 1 root root 10 May  5 15:48 app.conf
PORT=8080
```


### Langkah 2: Simulasikan permission bermasalah

Command:

```bash
chmod 000 app.conf
cat app.conf
```


Output:

```bash
root@UbuntuServer:~/praktikum-os/week10-memory/syscall-case# chmod 000 app.conf
cat app.conf
PORT=8080
```


### Langkah 3: Kembalikan permission dan verifikasi.

Command:

```bash
chmod 644 app.conf
cat app.conf
```


Output:

```bash
root@UbuntuServer:~/praktikum-os/week10-memory/syscall-case# chmod 644 app.conf
cat app.conf
PORT=8080
```


**Analisis**:
1. Mengapa cat menghasilkan Permission denied setelah chmod 000? System
call apa yang gagal?  
Jawaban:  
Secara teori harusnya memunculkan error Permission denied. Karena menjalankannya di root, maka punya hak akses penuh (superuser). Permission file (000 sekalipun) tidak membatasi root. Jadi cat tetap berhasil membaca file

2. Apa perbedaan pesan error Permission denied vs No such file or directory?
Coba rm app.conf lalu cat app.conf untuk melihat perbedaannya.  
Jawaban:  
Permission denied: Terjadi jika File ada Tapi tidak punya izin akses  
No such file or directory: Terjadi jika file tidak ada

3. Permission 644 berarti apa untuk owner, group, dan others?  
Jawaban:  
Permission 644 berarti Owner bisa baca/tulis dan yang lain hanya bisa baca


---

# Praktikum 10.6 Mengamati System Call dengan strace

Tujuan: melihat dan menganalisis system call yang dilakukan suatu perintah.


### Langkah 1: Lihat 30 baris pertama system call dari perintah ls.

Command:

```bash
strace ls 2>&1 | head -n 30
```


Output:

```bash
root@UbuntuServer:~/praktikum-os/week10-memory/syscall-case# strace ls 2>&1 | head -n 30
execve("/usr/bin/ls", ["ls"], 0x7fff504bbb60 /* 19 vars */) = 0
brk(NULL)                               = 0x591be62a6000
mmap(NULL, 8192, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_ANONYMOUS, -1, 0) = 0x789eb4509000
access("/etc/ld.so.preload", R_OK)      = -1 ENOENT (No such file or directory)
openat(AT_FDCWD, "/etc/ld.so.cache", O_RDONLY|O_CLOEXEC) = 3
fstat(3, {st_mode=S_IFREG|0644, st_size=32431, ...}) = 0
mmap(NULL, 32431, PROT_READ, MAP_PRIVATE, 3, 0) = 0x789eb4501000
close(3)                                = 0
openat(AT_FDCWD, "/lib/x86_64-linux-gnu/libselinux.so.1", O_RDONLY|O_CLOEXEC) = 3
read(3, "\177ELF\2\1\1\0\0\0\0\0\0\0\0\0\3\0>\0\1\0\0\0\0\0\0\0\0\0\0\0"..., 832) = 832
fstat(3, {st_mode=S_IFREG|0644, st_size=174472, ...}) = 0
mmap(NULL, 181960, PROT_READ, MAP_PRIVATE|MAP_DENYWRITE, 3, 0) = 0x789eb44d4000
mmap(0x789eb44da000, 118784, PROT_READ|PROT_EXEC, MAP_PRIVATE|MAP_FIXED|MAP_DENYWRITE, 3, 0x6000) = 0x789eb44da000
mmap(0x789eb44f7000, 24576, PROT_READ, MAP_PRIVATE|MAP_FIXED|MAP_DENYWRITE, 3, 0x23000) = 0x789eb44f7000
mmap(0x789eb44fd000, 8192, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_FIXED|MAP_DENYWRITE, 3, 0x29000) = 0x789eb44fd000
mmap(0x789eb44ff000, 5832, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_FIXED|MAP_ANONYMOUS, -1, 0) = 0x789eb44ff000
close(3)                                = 0
openat(AT_FDCWD, "/lib/x86_64-linux-gnu/libc.so.6", O_RDONLY|O_CLOEXEC) = 3
read(3, "\177ELF\2\1\1\3\0\0\0\0\0\0\0\0\3\0>\0\1\0\0\0\220\243\2\0\0\0\0\0"..., 832) = 832
pread64(3, "\6\0\0\0\4\0\0\0@\0\0\0\0\0\0\0@\0\0\0\0\0\0\0@\0\0\0\0\0\0\0"..., 784, 64) = 784
fstat(3, {st_mode=S_IFREG|0755, st_size=2125328, ...}) = 0
pread64(3, "\6\0\0\0\4\0\0\0@\0\0\0\0\0\0\0@\0\0\0\0\0\0\0@\0\0\0\0\0\0\0"..., 784, 64) = 784
mmap(NULL, 2170256, PROT_READ, MAP_PRIVATE|MAP_DENYWRITE, 3, 0) = 0x789eb4200000
mmap(0x789eb4228000, 1605632, PROT_READ|PROT_EXEC, MAP_PRIVATE|MAP_FIXED|MAP_DENYWRITE, 3, 0x28000) = 0x789eb4228000
mmap(0x789eb43b0000, 323584, PROT_READ, MAP_PRIVATE|MAP_FIXED|MAP_DENYWRITE, 3, 0x1b0000) = 0x789eb43b0000
mmap(0x789eb43ff000, 24576, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_FIXED|MAP_DENYWRITE, 3, 0x1fe000) = 0x789eb43ff000
mmap(0x789eb4405000, 52624, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_FIXED|MAP_ANONYMOUS, -1, 0) = 0x789eb4405000
close(3)                                = 0
openat(AT_FDCWD, "/lib/x86_64-linux-gnu/libpcre2-8.so.0", O_RDONLY|O_CLOEXEC) = 3
read(3, "\177ELF\2\1\1\0\0\0\0\0\0\0\0\0\3\0>\0\1\0\0\0\0\0\0\0\0\0\0\0"..., 832) = 832
```


### Langkah 2: Lihat ringkasan statistik dan bandingkan dua direktori berbeda

Command:

```bash
strace -c ls
strace -c ls /etc 2>&1 | tail -5
```


Output:

```bash
root@UbuntuServer:~/praktikum-os/week10-memory/syscall-case# strace -c ls
app.conf
% time     seconds  usecs/call     calls    errors syscall
------ ----------- ----------- --------- --------- ----------------
 35.74    0.000460          25        18           mmap
 23.93    0.000308         154         2           pread64
 13.13    0.000169          33         5           read
 12.04    0.000155          17         9           close
  5.67    0.000073           9         8           fstat
  2.80    0.000036           5         7           openat
  1.79    0.000023           4         5           mprotect
  1.63    0.000021          10         2           getdents64
  1.01    0.000013           6         2         2 statfs
  0.70    0.000009           9         1           write
  0.54    0.000007           7         1           munmap
  0.23    0.000003           1         3           brk
  0.23    0.000003           1         2           ioctl
  0.16    0.000002           1         2         2 access
  0.08    0.000001           1         1           arch_prctl
  0.08    0.000001           1         1           set_tid_address
  0.08    0.000001           1         1           prlimit64
  0.08    0.000001           1         1           getrandom
  0.08    0.000001           1         1           rseq
  0.00    0.000000           0         1           execve
  0.00    0.000000           0         1           set_robust_list
------ ----------- ----------- --------- --------- ----------------
100.00    0.001287          17        74         4 total
root@UbuntuServer:~/praktikum-os/week10-memory/syscall-case# ls
app.conf
root@UbuntuServer:~/praktikum-os/week10-memory/syscall-case# strace -c ls /etc 2>&1 | tail -5
  0.69    0.000035          35         1           set_robust_list
  0.56    0.000028          28         1           getrandom
  0.44    0.000022          22         1           arch_prctl
------ ----------- ----------- --------- --------- ----------------
100.00    0.005039          68        74         5 total
```


**Analisis**:
1. Dari output Langkah 1, identifikasi minimal 4 system call berbeda. Jelaskan
fungsi singkat masing-masing berdasarkan argumen yang terlihat.  
Jawaban:  
execve(): Menjalankan program ls  
openat(): Membuka file (misalnya library atau konfigurasi). Mengembalikan file descriptor  
read()  : Membaca isi file dari file descriptor  
mmap()  : Memetakan file atau memori ke address space proses\
close() : Menutup file descriptor setelah selesai digunakan 

2. Dari ringkasan strace -c, system call mana yang paling sering dipanggil?
Mengapa?  
Jawaban:  
System call paling sering yaitu mmap(). Saat ls dijalankan, banyak library yang harus diload (libc, libselinux, dll), library tersebut dimapping ke memori menggunakan mmap. Jadi mmap sering muncul karena proses loading program

3. Apakah ada system call dengan errors lebih dari 0? Apakah itu berarti
program bermasalah, ataukah bagian normal dari logika program?  
JAwaban:  
Error ada 4. Tidak berarti program bermasalah. Justru bagian dari logika program mencoba cek file kalau tidak ada → lanjut. Ini disebut expected failure (normal behavior)

4. Apakah jumlah system call berbeda antara ls dan ls /etc? Faktor apa yang
menyebabkan perbedaan tersebut?  
Jawban:  
Sama. Tetapi, walaupun jumlah call mirip ada perbedaannya yang dipengaruhi oleh jumlah file & kompleksitas direktori

---


# 1.6 Tugas Praktikum

## Tugas 10.1 Audit Penggunaan Memori Sistem

### Langkah 1: Buat script memory-audit.sh yang menghasilkan laporan kondisi memori sistem secara otomatis.

Command:

```bash
nano ~/praktikum-os/week10-memory/memory-audit.sh
```


### Langkah 2: Isi file memory-audit.sh

Command:

```bash
#!/bin/bash
set -euo pipefail

LAPORAN="memory-report.txt"

{
    echo "=== LAPORAN MEMORI SISTEM ==="
    date
    echo
    echo "--- Ringkasan free -h ---"
    free -h
    echo
    echo "--- /proc/meminfo ---"
    cat /proc/meminfo | head -n 20
} > "$LAPORAN"

echo "Laporan disimpan ke: $LAPORAN"
cat "$LAPORAN"
```


### Langkah 3: Jalankan script audit

Command:

```bash
chmod +x ~/praktikum-os/week10-memory/memory-audit.sh
cd ~/praktikum-os/week10-memory
bash memory-audit.sh
```


Output:

```bash
root@UbuntuServer:~/praktikum-os/week10-memory/syscall-case# chmod +x ~/praktikum-os/week10-memory/memory-audit.sh
root@UbuntuServer:~/praktikum-os/week10-memory/syscall-case# cd ~/praktikum-os/week10-memory
root@UbuntuServer:~/praktikum-os/week10-memory# bash memory-audit.sh
Laporan disimpan ke: memory-report.txt
=== LAPORAN MEMORI SISTEM ===
Tue May  5 04:35:27 PM UTC 2026

--- Ringkasan free -h ---
               total        used        free      shared  buff/cache   available
Mem:           3.7Gi       461Mi       2.0Gi       1.0Mi       1.5Gi       3.3Gi
Swap:          4.5Gi          0B       4.5Gi

--- /proc/meminfo ---
MemTotal:        3911664 kB
MemFree:         2071984 kB
MemAvailable:    3439320 kB
Buffers:           34644 kB
Cached:          1511428 kB
SwapCached:            0 kB
Active:           255692 kB
Inactive:        1337816 kB
Active(anon):      57268 kB
Inactive(anon):        0 kB
Active(file):     198424 kB
Inactive(file):  1337816 kB
Unevictable:       27316 kB
Mlocked:           27316 kB
SwapTotal:       4718584 kB
SwapFree:        4718584 kB
Zswap:                 0 kB
Zswapped:              0 kB
Dirty:               208 kB
Writeback:             0 kB
```


**Analisis**
1. Hitung persentase memori tersedia (available / total × 100%). Apakah
sistem dalam kondisi normal?  
Jawaban:  
(3.3/3.7)×100%=89%  
sistem dalam kondisi normal

2. Mengapa buff/cache tidak dihitung sebagai memori yang terpakai dari sudut
pandang ketersediaan untuk aplikasi?  
Jawaban:  
buff/cache tidak dihitung sebagai beban tetap. Karena sifatnya fleksibel

3. Dari /proc/meminfo, apakah SwapTotal lebih besar dari 0? Berapa nilai
SwapFree?  
Jawaban:  
SwapTotal: 4718584 kB  
SwapFree:  4718584 kB  


---


## Tugas 10.2 Identifikasi Proses dengan Memori Tertinggi

### Langkah 1:  Simpan daftar 10 proses pengguna memori terbesar ke file.

Command:

```bash
ps aux --sort=-%mem | head -n 10 > top-memory-process.txt
cat top-memory-process.txt
```


Output:

```bash
root@UbuntuServer:~/praktikum-os/week10-memory# ps aux --sort=-%mem | head -n 10 > top-memory-process.txt
root@UbuntuServer:~/praktikum-os/week10-memory# cat top-memory-process.txt
USER         PID %CPU %MEM    VSZ   RSS TTY      STAT START   TIME COMMAND
root        1142  0.0  1.1 691508 44200 ?        Ssl  14:08   0:02 /usr/libexec/fwupd/fwupd
root         367  0.0  0.6 288988 27324 ?        SLsl 13:57   0:01 /sbin/multipathd -d -s
root         651  0.0  0.5 109684 23180 ?        Ssl  13:57   0:00 /usr/bin/python3 /usr/share/unattended-upgrades/unattended-upgrade-shutdown --wait-for-signal
root         312  0.0  0.4  66856 17340 ?        S<s  13:57   0:00 /usr/lib/systemd/systemd-journald
root         623  0.0  0.3 468972 13620 ?        Ssl  13:57   0:00 /usr/libexec/udisks2/udisksd
root           1  0.0  0.3  22216 13248 ?        Ss   13:57   0:02 /sbin/init splash noprompt noshell automatic-ubiquity
systemd+     462  0.0  0.3  21584 12996 ?        Ss   13:57   0:00 /usr/lib/systemd/systemd-resolved
root         681  0.0  0.3 392032 12912 ?        Ssl  13:57   0:00 /usr/sbin/ModemManager
fauzi        967  0.0  0.2  20100 11256 ?        Ss   14:00   0:00 /usr/lib/systemd/systemd --user
```


**Analisis**
1. Proses apa di urutan pertama? Catat nilai %MEM dan RSS.  
Jawaban:  
Proses: fwupd  
%MEM = 1.1%  
RSS = 44,200 KB

2. Konversikan RSS ke MB (bagi 1024). Apakah wajar?  
Jawaban:  
44200÷1024=43.16 MB  
Ya, sangat wajar. Karena fwupd adalah service update firmware. Penggunaan 43 MB tergolong kecil. Server punya RAM kurang lebih 3.7 GB dan ini sangat ringan

3. Jumlahkan %MEM dari 5 proses teratas. Berapa persen RAM yang mereka
gunakan bersama?  
Jawaban:  
fwupd: 1.1  
multipathd:	0.6  
unattended-upgrade-shutdown: 0.5  
systemd-journald: 0.4  
udisksd:0.3

penjumlahan: 1.1+0.6+0.5+0.4+0.3=2.9%
Jadi total penggunaan dari 5 proses terbesar kurang lebih 2.9% RAM


---


## Tugas 10.3 Membuat dan Memverifikasi Swap File
Instruksi: Buat swap file khusus tugas sebesar 256 MB dan verifikasi.


### Langkah 1:  Buat dan aktifkan swap file tugas

Command:

```bash
sudo fallocate -l 256M /swapfile-tugas-week10
sudo chmod 600 /swapfile-tugas-week10
sudo mkswap /swapfile-tugas-week10
sudo swapon /swapfile-tugas-week10
```


Output:

```bash
root@UbuntuServer:~/praktikum-os/week10-memory# sudo fallocate -l 256M /swapfile-tugas-week10
root@UbuntuServer:~/praktikum-os/week10-memory# sudo chmod 600 /swapfile-tugas-week10
root@UbuntuServer:~/praktikum-os/week10-memory# sudo mkswap /swapfile-tugas-week10
Setting up swapspace version 1, size = 256 MiB (268431360 bytes)
no label, UUID=0831d044-a5f7-441a-acdc-d8f844afc280
root@UbuntuServer:~/praktikum-os/week10-memory# sudo swapon /swapfile-tugas-week10
root@UbuntuServer:~/praktikum-os/week10-memory#
```


### Langkah 2: Verifikasi dan simpan hasil

Command:

```bash
{
    echo "=== VERIFIKASI SWAP ==="
    swapon --show
    echo
    free -h
} > swap-check.txt

cat swap-check.txt
```


Output:

```bash
root@UbuntuServer:~/praktikum-os/week10-memory# {
    echo "=== VERIFIKASI SWAP ==="
    swapon --show
    echo
    free -h
} > swap-check.txt

cat swap-check.txt
=== VERIFIKASI SWAP ===
NAME                   TYPE SIZE USED PRIO
/swapfile              file   4G   0B   -2
/swapfile-week10       file 512M   0B   -3
/swapfile-tugas-week10 file 256M   0B   -4

               total        used        free      shared  buff/cache   available
Mem:           3.7Gi       460Mi       2.0Gi       1.0Mi       1.5Gi       3.3Gi
Swap:          4.7Gi          0B       4.7Gi
```


**Analisis**
1. Identifikasi kolom NAME, TYPE, SIZE, dan USED pada output swapon –show.  
Jawaban:  
=== VERIFIKASI SWAP ===  
NAME                   TYPE SIZE USED PRIO  
/swapfile              file   4G   0B   -2  
/swapfile-week10       file 512M   0B   -3  
/swapfile-tugas-week10 file 256M   0B   -4  

Penjelasan kolom:  
NAME: Nama file atau device swap  
TYPE: Jenis swap  
SIZE: Ukuran swap  
USED: Jumlah swap yang sedang digunakan  
PRIO: Prioritas penggunaan swap

2. Apakah nilai total pada baris Swap di free -h bertambah 256 MB?  
Jawaban:  
Ya, total swap bertambah sekitar 256 MB

3. Mengapa permission 600 penting? Apa risiko jika diatur ke 644?  
Jawaban:  
Permission 600 penting untuk swap yang bisa menyimpan:  
Data sementara dari RAM. Bisa termasuk password, data aplikasi, informasi sensitif  
Risiko jika permission 644 adalah User lain bisa membaca swap file menjadikan beberapa kemungkinan resikonya yaitu Kebocoran data sensitif, Potensi eksploitasi keamanan, Melanggar prinsip keamanan sistem Linux



---


## Tugas 10.4 Analisis System Call dengan strace
Instruksi: Analisis system call yang dipanggil perintah ls.


### Langkah 1:  Simpan ringkasan dan detail system call

Command:

```bash
strace -c ls 2> strace-summary.txt
strace ls /etc 2> strace-ls-etc.txt
cat strace-summary.txt
```


Output:

```bash
root@UbuntuServer:~/praktikum-os/week10-memory# strace -c ls 2> strace-summary.txt
memory-audit.sh  memory-report.txt  monitor-memori.sh  strace-summary.txt  swap-check.txt  syscall-case  top-memory-process.txt
root@UbuntuServer:~/praktikum-os/week10-memory# strace ls /etc 2> strace-ls-etc.txt
adduser.conf            cron.daily            ethertypes   hosts.allow      libibverbs.d    mke2fs.conf          os-release        rc0.d          sgml               sysstat              vmware-tools
alternatives            cron.hourly           fonts        hosts.deny       libnl-3         ModemManager         overlayroot.conf  rc1.d          shadow             systemd              vtrgb
apparmor                cron.monthly          fstab        ImageMagick-6    libpaper.d      modprobe.d           PackageKit        rc2.d          shadow-            terminfo             w3m
apparmor.d              crontab               fuse.conf    init.d           locale.alias    modules              pam.conf          rc3.d          shells             thermald             wgetrc
apport                  cron.weekly           fwupd        initramfs-tools  locale.conf     modules-load.d       pam.d             rc4.d          skel               timezone             X11
apt                     cron.yearly           gai.conf     inputrc          locale.gen      mtab                 papersize         rc5.d          sos                tmpfiles.d           xattr.conf
bash.bashrc             cryptsetup-initramfs  ghostscript  iproute2         localtime       multipath            passwd            rc6.d          ssh                ubuntu-advantage     xdg
bash_completion         crypttab              gnutls       iscsi            logcheck        multipath.conf       passwd-           rcS.d          ssl                ucf.conf             xml
bash_completion.d       dbus-1                groff        issue            login.defs      nanorc               perl              resolv.conf    subgid             udev                 zsh_command_not_found
bindresvport.blacklist  debconf.conf          group        issue.net        logrotate.conf  needrestart          pki               rmt            subgid-            udisks2
binfmt.d                debian_version        group-       kernel           logrotate.d     netconfig            plymouth          rpc            subuid             ufw
byobu                   default               grub.d       landscape        lsb-release     netplan              pm                rsyslog.conf   subuid-            update-manager
ca-certificates         deluser.conf          gshadow      ldap             lvm             network              polkit-1          rsyslog.d      sudo.conf          update-motd.d
ca-certificates.conf    depmod.d              gshadow-     ld.so.cache      machine-id      networkd-dispatcher  pollinate         screenrc       sudoers            update-notifier
cloud                   dhcp                  gss          ld.so.conf       magic           networks             profile           security       sudoers.d          UPower
console-setup           dhcpcd.conf           hdparm.conf  ld.so.conf.d     magic.mime      newt                 profile.d         selinux        sudo_logsrvd.conf  usb_modeswitch.conf
credstore               dpkg                  host.conf    legal            manpath.config  nftables.conf        protocols         sensors3.conf  supercat           usb_modeswitch.d
credstore.encrypted     e2scrub.conf          hostname     libaudit.conf    mdadm           nsswitch.conf        python3           sensors.d      sysctl.conf        vconsole.conf
cron.d                  environment           hosts        libblockdev      mime.types      opt                  python3.12        services       sysctl.d           vim
root@UbuntuServer:~/praktikum-os/week10-memory# cat strace-summary.txt
% time     seconds  usecs/call     calls    errors syscall
------ ----------- ----------- --------- --------- ----------------
 34.51    0.001153          64        18           mmap
 15.71    0.000525         105         5           read
 10.06    0.000336          67         5           mprotect
  8.98    0.000300          33         9           close
  6.70    0.000224          32         7           openat
  3.53    0.000118          59         2         2 statfs
  3.50    0.000117          14         8           fstat
  2.90    0.000097          32         3           brk
  2.69    0.000090          45         2           ioctl
  1.83    0.000061          61         1           munmap
  1.62    0.000054          27         2           pread64
  1.41    0.000047          23         2         2 access
  1.41    0.000047          47         1           prlimit64
  1.29    0.000043          43         1           set_tid_address
  1.29    0.000043          43         1           set_robust_list
  1.23    0.000041          41         1           rseq
  1.11    0.000037          37         1           arch_prctl
  0.24    0.000008           8         1           getrandom
  0.00    0.000000           0         1           write
  0.00    0.000000           0         1           execve
  0.00    0.000000           0         2           getdents64
------ ----------- ----------- --------- --------- ----------------
100.00    0.003341          45        74         4 total
```


**Analisis**
1. Sebutkan minimal 5 system call dari strace-summary.txt beserta fungsi
singkatnya.  
Jawaban:  
mmap: Memetakan file atau library ke memori. Digunakan saat program load library (misalnya libc)  
read: Membaca data dari file descriptor. Contohnya membaca isi file atau library  
openat: Membuka file (dengan path relatif/absolut). Digunakan untuk akses file seperti /etc, library, dll  
close: Menutup file descriptor setelah selesai digunakan  
fstat: Mengambil informasi metadata file  
getdents64: Membaca isi direktori. Digunakan oleh ls untuk menampilkan daftar file

2. System call mana yang paling sering dipanggil? Mengapa?  
Jawaban:  
System call paling sering: mmap  
Karena program ls membutuhkan banyak library. Library tersebut dimuat ke memori menggunakan mmap. Maka mmap paling dominan

3. Apakah ada errors lebih dari 0? Apakah program tetap berjalan normal
meskipun ada kegagalan tersebut?  
Jawaban:  
Ada, totalnya 4. Tapi normal, tidak mempengaruhi jalannya program. Errornya bisa disebut expected error atau bagian dari logika program



---


## Tugas 10.5 Studi Kasus Diagnosa Server Lambat
Skenario: Server terasa lambat. Buat script diagnosa yang menggabungkan semua
pemeriksaan dari bab ini menggunakan fungsi Bash.



### Langkah 1:  Buka file dengan nano

Command:

```bash
nano ~/praktikum-os/week10-memory/diagnosa-server.sh
```


### Langkah 2:  Isi file diagnosa-server.sh

Command:

```bash
#!/bin/bash
set -euo pipefail

LAPORAN="diagnosa-server-lambat.txt"
WARN_MEM=false
WARN_SWAP=0

cek_memori() {
    echo "--- Kondisi Memori ---"
    free -h
    echo
    AVAIL_PCT=$(free | awk '/Mem/ {printf "%d", $7/$2*100}')
    if [ "$AVAIL_PCT" -lt 20 ]; then
        echo "PERINGATAN: Memori tersedia hanya ${AVAIL_PCT}%"
        WARN_MEM=true
    fi
}

cek_swap() {
    echo "--- Penggunaan Swap ---"
    swapon --show 2>/dev/null || echo "Tidak ada swap aktif"
    echo
    WARN_SWAP=$(free | awk '/Swap/ {print $3}')
    if [ "$WARN_SWAP" -gt 0 ]; then
        echo "INFO: Swap digunakan (${WARN_SWAP} kB)"
    fi
}

cek_proses() {
    echo "--- 10 Proses Memori Tertinggi ---"
    ps aux --sort=-%mem | head -n 11
    echo
}

cek_paging() {
    echo "--- Aktivitas Paging (5 sampel) ---"
    vmstat 1 5
    echo
}

ringkasan() {
    echo "=== RINGKASAN ==="
    if [ "$WARN_MEM" = true ]; then
        echo "- Memori: KRITIS - perlu tindakan segera"
    else
        echo "- Memori: normal"
    fi
    if [ "$WARN_SWAP" -gt 0 ]; then
        echo "- Swap: aktif - pantau aktivitas paging"
    else
        echo "- Swap: tidak digunakan"
    fi
}

{
    echo "=== LAPORAN DIAGNOSA SERVER ==="
    date
    echo
    cek_memori
    cek_swap
    cek_proses
    cek_paging
    ringkasan
} | tee "$LAPORAN"

echo
echo "Laporan disimpan ke: $LAPORAN"
```

### Langkah 3:  Jalankan script diagnosa

Command:

```bash
chmod +x ~/praktikum-os/week10-memory/diagnosa-server.sh
cd ~/praktikum-os/week10-memory
bash diagnosa-server.sh
```


Output:

```bash
root@UbuntuServer:~/praktikum-os/week10-memory# chmod +x ~/praktikum-os/week10-memory/diagnosa-server.sh
root@UbuntuServer:~/praktikum-os/week10-memory# cd ~/praktikum-os/week10-memory
root@UbuntuServer:~/praktikum-os/week10-memory# bash diagnosa-server.sh
=== LAPORAN DIAGNOSA SERVER ===
Tue May  5 05:34:38 PM UTC 2026

--- Kondisi Memori ---
               total        used        free      shared  buff/cache   available
Mem:           3.7Gi       461Mi       2.0Gi       1.0Mi       1.5Gi       3.3Gi
Swap:          4.5Gi          0B       4.5Gi

--- Penggunaan Swap ---
NAME             TYPE SIZE USED PRIO
/swapfile        file   4G   0B   -2
/swapfile-week10 file 512M   0B   -3

--- 10 Proses Memori Tertinggi ---
USER         PID %CPU %MEM    VSZ   RSS TTY      STAT START   TIME COMMAND
root        1142  0.0  1.1 691508 44200 ?        Ssl  14:08   0:02 /usr/libexec/fwupd/fwupd
root         367  0.0  0.6 288988 27324 ?        SLsl 13:57   0:02 /sbin/multipathd -d -s
root         651  0.0  0.5 109684 23180 ?        Ssl  13:57   0:00 /usr/bin/python3 /usr/share/unattended-upgrades/unattended-upgrade-shutdown --wait-for-signal
root         312  0.0  0.4  66856 17368 ?        S<s  13:57   0:00 /usr/lib/systemd/systemd-journald
root         623  0.0  0.3 468972 13620 ?        Ssl  13:57   0:00 /usr/libexec/udisks2/udisksd
root           1  0.0  0.3  22216 13248 ?        Ss   13:57   0:02 /sbin/init splash noprompt noshell automatic-ubiquity
systemd+     462  0.0  0.3  21584 12996 ?        Ss   13:57   0:00 /usr/lib/systemd/systemd-resolved
root         681  0.0  0.3 392032 12912 ?        Ssl  13:57   0:00 /usr/sbin/ModemManager
fauzi        967  0.0  0.2  20100 11256 ?        Ss   14:00   0:00 /usr/lib/systemd/systemd --user
root        1037  0.0  0.2  14964 10652 ?        Ss   14:01   0:00 sshd: fauzi [priv]

--- Aktivitas Paging (5 sampel) ---
procs -----------memory---------- ---swap-- -----io---- -system-- -------cpu-------
 r  b   swpd   free   buff  cache   si   so    bi    bo   in   cs us sy id wa st gu
 1  0      0 2070124  35524 1577768    0    0    42   102  323    0  0  0 100  0  0  0
 0  0      0 2070124  35524 1577812    0    0     0     0  435  151  1  1 99  0  0  0
 0  0      0 2070124  35524 1577812    0    0     0     0  205   65  0  0 100  0  0  0
 0  0      0 2070124  35524 1577812    0    0     0     4  167   66  0  0 100  0  0  0
 0  0      0 2070124  35524 1577812    0    0     0     0  188   54  0  0 100  0  0  0

=== RINGKASAN ===
- Memori: normal
- Swap: tidak digunakan

Laporan disimpan ke: diagnosa-server-lambat.txt
```


**Analisis**
1. Jelaskan peran masing-masing fungsi: cek_memori, cek_swap, cek_proses,
cek_paging, dan ringkasan. Mengapa diagnosa dipecah menjadi fungsi
terpisah?  
Jawaban:  
cek_memori: Menampilkan kondisi RAM. Menghitung persentase memori tersedia  
cek_swap: Menampilkan swap aktif. Mengecek apakah swap sedang digunakan  
cek_proses: Menampilkan 10 proses dengan penggunaan memori terbesar  
cek_paging: Menjalankan vmstat 1 5. Melihat aktivitas si (swap in) dan so (swap out)  
ringkasan: Menyimpulkan kondisi RAM (normal/kritis) dan Swap (digunakan/tidak)  
Dipisah menjadi fungsi agar lebih modular, reusable, dan mudah dalam maintenance

2. Berdasarkan bagian RINGKASAN, apakah kondisi sistem normal atau kritis?
Jelaskan berdasarkan nilai threshold yang digunakan script.  
Jawaban:  
Available = 3.3 Gi dari 3.7 Gi sekitar 89%  
Threshold = 20%. Jauh di atas batas artinya NORMAL  
Swap used = 0B. Tidak ada tekanan memori  
Jadi, sistem dalam keadaan normal

3. Mengapa script menggunakan tee "$LAPORAN" bukan redirection biasa >
"$LAPORAN"? Apa keuntungannya?  
Jawaban:  
penggunaan tee dapat digunakan untuk output ke file + tampil di terminal, sedangakan > hanya ke file  
Keuntungan menggunakan tee adalh bisa lihat hasil langsung di layar sekaligus tersimpan ke file. cocok untuk monitoring/debugging

4. Dari output cek_paging, apakah ada aktivitas si atau so? Jika ada, apa
implikasinya terhadap performa server?  
Jawaban:  
so dan so tidak ada aktivitasnya. berarti tidak ada data yang dipindahkan. 
