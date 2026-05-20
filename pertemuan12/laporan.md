# Praktikum Sistem Operasi – Pertemuan 12  
##  1  Manajemen Service


> Nama   : Muhammad Fauzi Fadillah
>
> NIM    : 254107020085
>
> Kelas  : TI_1G


---

# Praktek 10.1: Amati Layanan Aktif Saat Boot


### Langkah 1: Lihat semua layanan yang sedang berjalan

Command:

```bash
systemctl list-units --type=service --state=running
```

Output:  

```bash
root@UbuntuServer:~/praktikum-os/week12# systemctl list-units --type=service --state=running
  UNIT                        LOAD   ACTIVE SUB     DESCRIPTION
  cron.service                loaded active running Regular background program processing daemon
  dbus.service                loaded active running D-Bus System Message Bus
  getty@tty1.service          loaded active running Getty on tty1
  ModemManager.service        loaded active running Modem Manager
  multipathd.service          loaded active running Device-Mapper Multipath Device Controller
  polkit.service              loaded active running Authorization Manager
  rsyslog.service             loaded active running System Logging Service
  ssh.service                 loaded active running OpenBSD Secure Shell server
  systemd-journald.service    loaded active running Journal Service
  systemd-logind.service      loaded active running User Login Management
  systemd-networkd.service    loaded active running Network Configuration
  systemd-resolved.service    loaded active running Network Name Resolution
  systemd-timesyncd.service   loaded active running Network Time Synchronization
  systemd-udevd.service       loaded active running Rule-based Manager for Device Events and Files
  udisks2.service             loaded active running Disk Manager
  unattended-upgrades.service loaded active running Unattended Upgrades Shutdown
  user@1000.service           loaded active running User Manager for UID 1000

Legend: LOAD   → Reflects whether the unit definition was properly loaded.
        ACTIVE → The high-level unit activation state, i.e. generalization of SUB.
        SUB    → The low-level unit activation state, values depend on unit type.

17 loaded units listed.
```


### Langkah 2: Lihat semua unit service yang ada (aktif maupun tidak)

Command:

```bash
systemctl list-unit-files --type=service | head -30
```

Output:  

```bash
root@UbuntuServer:~/praktikum-os/week12# systemctl list-unit-files --type=service | head -30
UNIT FILE                                    STATE           PRESET
apparmor.service                             enabled         enabled
apport-autoreport.service                    static          -
apport-coredump-hook@.service                static          -
apport-forward@.service                      static          -
apport.service                               enabled         enabled
apt-daily-upgrade.service                    static          -
apt-daily.service                            static          -
apt-news.service                             static          -
autovt@.service                              alias           -
blk-availability.service                     enabled         enabled
bolt.service                                 static          -
cloud-config.service                         enabled         enabled
cloud-final.service                          enabled         enabled
cloud-init-hotplugd.service                  static          -
cloud-init-local.service                     enabled         enabled
cloud-init.service                           enabled         enabled
console-getty.service                        disabled        disabled
console-setup.service                        enabled         enabled
container-getty@.service                     static          -
cron.service                                 enabled         enabled
cryptdisks-early.service                     masked          enabled
cryptdisks.service                           masked          enabled
dbus-org.freedesktop.hostname1.service       alias           -
dbus-org.freedesktop.locale1.service         alias           -
dbus-org.freedesktop.login1.service          alias           -
dbus-org.freedesktop.ModemManager1.service   alias           -
dbus-org.freedesktop.resolve1.service        alias           -
dbus-org.freedesktop.thermald.service        alias           -
dbus-org.freedesktop.timedate1.service       alias           -
```


### Langkah 3:  Analisis waktu boot dan temukan layanan paling lambat.

Command:

```bash
systemd-analyze
systemd-analyze blame | head -15
```

Output:  

```bash
root@UbuntuServer:~/praktikum-os/week12# systemd-analyze
Startup finished in 9.963s (kernel) + 4.625s (userspace) = 14.588s
graphical.target reached after 4.587s in userspace.
root@UbuntuServer:~/praktikum-os/week12# systemd-analyze blame | head -15
40.770s apt-daily-upgrade.service
22.472s fwupd-refresh.service
 3.075s dev-sda2.device
 1.937s snapd.seeded.service
 1.832s snapd.service
 1.800s systemd-networkd-wait-online.service
 1.179s apparmor.service
 1.139s dpkg-db-backup.service
  976ms systemd-networkd.service
  967ms polkit.service
  961ms udisks2.service
  733ms grub-common.service
  687ms rsyslog.service
  549ms systemd-resolved.service
  543ms logrotate.service
```


---


# Praktek 10.2: Kelola Layanan SSH


### Langkah 1: Periksa status SSH secara menyeluruh.

Command:

```bash
systemctl status ssh
systemctl is-active ssh
systemctl is-enabled ssh
```

Output:

```bash
root@UbuntuServer:~/praktikum-os/week12/chapter10-services# systemctl status ssh
● ssh.service - OpenBSD Secure Shell server
     Loaded: loaded (/usr/lib/systemd/system/ssh.service; enabled; preset: enabled)
     Active: active (running) since Wed 2026-05-20 14:40:41 UTC; 41min ago
TriggeredBy: ● ssh.socket
       Docs: man:sshd(8)
             man:sshd_config(5)
   Main PID: 895 (sshd)
      Tasks: 1 (limit: 4486)
     Memory: 4.1M (peak: 5.3M)
        CPU: 62ms
     CGroup: /system.slice/ssh.service
             └─895 "sshd: /usr/sbin/sshd -D [listener] 0 of 10-100 startups"

May 20 14:40:41 UbuntuServer systemd[1]: Starting ssh.service - OpenBSD Secure Shell server...
May 20 14:40:41 UbuntuServer sshd[895]: Server listening on 0.0.0.0 port 22.
May 20 14:40:41 UbuntuServer sshd[895]: Server listening on :: port 22.
May 20 14:40:41 UbuntuServer systemd[1]: Started ssh.service - OpenBSD Secure Shell server.
May 20 14:42:13 UbuntuServer sshd[1112]: Accepted password for fauzi from 192.168.88.49 port 55537 ssh2
May 20 14:42:13 UbuntuServer sshd[1112]: pam_unix(sshd:session): session opened for user fauzi(uid=1000) by fauzi(uid=0)
root@UbuntuServer:~/praktikum-os/week12/chapter10-services# systemctl is-active ssh
active
root@UbuntuServer:~/praktikum-os/week12/chapter10-services# systemctl is-enabled ssh
enabled
```


### Langkah 2: Lakukan restart dan pantau perubahannya.

Command:

```bash
sudo systemctl restart ssh
systemctl status ssh
```

Output:

```bash
root@UbuntuServer:~/praktikum-os/week12/chapter10-services# sudo systemctl restart ssh
root@UbuntuServer:~/praktikum-os/week12/chapter10-services# systemctl status ssh
● ssh.service - OpenBSD Secure Shell server
     Loaded: loaded (/usr/lib/systemd/system/ssh.service; enabled; preset: enabled)
     Active: active (running) since Wed 2026-05-20 15:24:00 UTC; 3s ago
TriggeredBy: ● ssh.socket
       Docs: man:sshd(8)
             man:sshd_config(5)
    Process: 2340 ExecStartPre=/usr/sbin/sshd -t (code=exited, status=0/SUCCESS)
   Main PID: 2341 (sshd)
      Tasks: 1 (limit: 4486)
     Memory: 1.2M (peak: 1.5M)
        CPU: 23ms
     CGroup: /system.slice/ssh.service
             └─2341 "sshd: /usr/sbin/sshd -D [listener] 0 of 10-100 startups"

May 20 15:24:00 UbuntuServer systemd[1]: Starting ssh.service - OpenBSD Secure Shell server...
May 20 15:24:00 UbuntuServer sshd[2341]: Server listening on 0.0.0.0 port 22.
May 20 15:24:00 UbuntuServer sshd[2341]: Server listening on :: port 22.
May 20 15:24:00 UbuntuServer systemd[1]: Started ssh.service - OpenBSD Secure Shell server.
```


### Langkah 3: Lihat dependensi SSH.

Command:

```bash
systemctl list-dependencies ssh
```

Output:

```bash
root@UbuntuServer:~/praktikum-os/week12/chapter10-services# systemctl list-dependencies ssh
ssh.service
● ├─-.mount
● ├─ssh.socket
● ├─system.slice
● └─sysinit.target
●   ├─apparmor.service
●   ├─blk-availability.service
●   ├─dev-hugepages.mount
●   ├─dev-mqueue.mount
●   ├─finalrd.service
●   ├─keyboard-setup.service
●   ├─kmod-static-nodes.service
○   ├─ldconfig.service
●   ├─lvm2-lvmpolld.socket
●   ├─lvm2-monitor.service
●   ├─multipathd.service
○   ├─open-iscsi.service
●   ├─plymouth-read-write.service
●   ├─plymouth-start.service
●   ├─proc-sys-fs-binfmt_misc.automount
●   ├─setvtrgb.service
●   ├─sys-fs-fuse-connections.mount
●   ├─sys-kernel-config.mount
●   ├─sys-kernel-debug.mount
●   ├─sys-kernel-tracing.mount
○   ├─systemd-ask-password-console.path
●   ├─systemd-binfmt.service
○   ├─systemd-firstboot.service
○   ├─systemd-hwdb-update.service
○   ├─systemd-journal-catalog-update.service
●   ├─systemd-journal-flush.service
●   ├─systemd-journald.service
○   ├─systemd-machine-id-commit.service
●   ├─systemd-modules-load.service
○   ├─systemd-pcrmachine.service
○   ├─systemd-pcrphase-sysinit.service
○   ├─systemd-pcrphase.service
○   ├─systemd-pstore.service
●   ├─systemd-random-seed.service
○   ├─systemd-repart.service
●   ├─systemd-resolved.service
●   ├─systemd-sysctl.service
○   ├─systemd-sysusers.service
●   ├─systemd-timesyncd.service
●   ├─systemd-tmpfiles-setup-dev-early.service
●   ├─systemd-tmpfiles-setup-dev.service
●   ├─systemd-tmpfiles-setup.service
○   ├─systemd-tpm2-setup-early.service
○   ├─systemd-tpm2-setup.service
●   ├─systemd-udev-trigger.service
●   ├─systemd-udevd.service
○   ├─systemd-update-done.service
●   ├─systemd-update-utmp.service
```


### Langkah 4: Cek semua unit yang gagal di sistem.

Command:

```bash
systemctl --failed
```

Output:

```bash
root@UbuntuServer:~/praktikum-os/week12/chapter10-services# systemctl --failed
  UNIT LOAD ACTIVE SUB DESCRIPTION

0 loaded units listed.
```


---


# Praktek 10.3: Buat Layanan Sederhana dari Skrip Bash


### Langkah 1: Siapkan konten yang akan dilayani.

Command:

```bash
cd ~/lab-os/chapter10-services
mkdir -p situs-demo
nano situs-demo/index.html

<!DOCTYPE html>
<html>
<head>
    <title>Demo Web Server</title>
    <meta charset="utf-8">
</head>
<body>
    <h1>Halo dari layanan systemd kustom!</h1>
    <p>Layanan ini dibuat pada praktek Bab 10.</p>
    <hr>
    <p><small>Server berjalan sebagai service systemd</small></p>
</body>
</html>
```

Output:

```bash
root@UbuntuServer:~/praktikum-os/week12/chapter10-services# mkdir -p situs-demo
root@UbuntuServer:~/praktikum-os/week12/chapter10-services# nano situs-demo/index.html
root@UbuntuServer:~/praktikum-os/week12/chapter10-services#
```


### Langkah 2: Buat skrip wrapper untuk server HTTP

Command:

```bash
nano ~/praktikum-os/week12/chapter10-services/jalankan-server.sh

#!/bin/bash
# Skrip wrapper untuk server HTTP Python

DIREKTORI="$HOME/praktikum-os/week12/chapter10-services/situs-demo"
PORT=9090

echo "Memulai server di port $PORT..."
echo "Melayani direktori: $DIREKTORI"

# Jalankan server HTTP Python
exec python3 -m http.server $PORT --directory "$DIREKTORI"

chmod +x ~/praktikum-os/week12/chapter10-services/jalankan-server.sh
```

Output:

```bash
root@UbuntuServer:~/praktikum-os/week12/chapter10-services# mkdir -p situs-demo
root@UbuntuServer:~/praktikum-os/week12/chapter10-services# nano situs-demo/index.html
root@UbuntuServer:~/praktikum-os/week12/chapter10-services# nano jalankan-service.sh
```


### Langkah 3: Buat berkas unit systemd untuk layanan ini.

Command:

```bash
nano ~/praktikum-os/week12/chapter10-services/demo-web.service

[Unit]
Description=Demo Web Server Praktek Bab 10
After=network.target

[Service]
Type=simple
User=root
WorkingDirectory=/root/praktikum-os/week12/chapter10-services/situs-demo
ExecStart=/usr/bin/python3 -m http.server 9090
Restart=on-failure
RestartSec=3s

[Install]
WantedBy=multi-user.target

sudo cp ~/praktikum-os/week12/chapter10-services/demo-web.service /etc/systemd/system/demo-web.service
sudo systemctl daemon-reload
```

Output:

```bash
root@UbuntuServer:~/praktikum-os/week12/chapter10-services# nano demo-web.service
root@UbuntuServer:~/praktikum-os/week12/chapter10-services# sudo cp ~/praktikum-os/week12/chapter10-services/demo-web.service /etc/systemd/system/demo-web.service
root@UbuntuServer:~/praktikum-os/week12/chapter10-services# sudo systemctl daemon-reload
```


### Langkah 4: Jalankan layanan dan verifikasi.

Command:

```bash
sudo systemctl start demo-web
systemctl status demo-web
curl http://localhost:9090
```

Output:

```bash
root@UbuntuServer:~/praktikum-os/week12/chapter10-services# sudo systemctl daemon-reload
sudo systemctl restart demo-web
sudo systemctl status demo-web
● demo-web.service - Demo Web Server Praktek Bab 10
     Loaded: loaded (/etc/systemd/system/demo-web.service; disabled; preset: enabled)
     Active: active (running) since Wed 2026-05-20 16:08:08 UTC; 34ms ago
   Main PID: 3323 (python3)
      Tasks: 1 (limit: 4486)
     Memory: 2.8M (peak: 3.1M)
        CPU: 13ms
     CGroup: /system.slice/demo-web.service
             └─3323 /usr/bin/python3 -m http.server 9090

May 20 16:08:08 UbuntuServer systemd[1]: Started demo-web.service - Demo Web Server Praktek Bab 10.
root@UbuntuServer:~/praktikum-os/week12/chapter10-services# curl http://localhost:9090
<!DOCTYPE html>
<html>
<head>
    <title>Demo Web Server</title>
    <meta charset="utf-8">
</head>
<body>
    <h1>Halo dari layanan systemd kustom!</h1>
    <p>Layanan ini dibuat pada praktek Bab 10.</p>
    <hr>
    <p><small>Server berjalan sebagai service systemd</small></p>
</body>
</html>
```


### Langkah 5: Uji fitur restart otomatis


Command:

```bash
# lihat PID proses saat ini
systemctl status demo-web | grep "Main PID"

# hentikan proses secara paksa ( simulasi crash )
sudo kill -9 $(systemctl show demo-web --property=MainPID --value)

# tunggu beberapa detik lalu cek -- systemd harus menghidupkannya kembali
sleep 5
systemctl status demo-web
# PID akan berubah karena proses baru dijalankan
```

Output:

```bash
root@UbuntuServer:~/praktikum-os/week12/chapter10-services# systemctl status demo-web | grep "Main PID"
   Main PID: 3323 (python3)
root@UbuntuServer:~/praktikum-os/week12/chapter10-services# sudo kill -9 $(systemctl show demo-web --property=MainPID --value)
root@UbuntuServer:~/praktikum-os/week12/chapter10-services# sleep 5
root@UbuntuServer:~/praktikum-os/week12/chapter10-services# systemctl status demo-web
● demo-web.service - Demo Web Server Praktek Bab 10
     Loaded: loaded (/etc/systemd/system/demo-web.service; disabled; preset: enabled)
     Active: active (running) since Wed 2026-05-20 16:11:45 UTC; 34s ago
   Main PID: 3647 (python3)
      Tasks: 1 (limit: 4486)
     Memory: 9.3M (peak: 9.7M)
        CPU: 48ms
     CGroup: /system.slice/demo-web.service
             └─3647 /usr/bin/python3 -m http.server 9090

May 20 16:11:45 UbuntuServer systemd[1]: demo-web.service: Scheduled restart job, restart counter is at 1.
May 20 16:11:45 UbuntuServer systemd[1]: Started demo-web.service - Demo Web Server Praktek Bab 10.
```


### Langkah 6: Bersihkan layanan uji setelah selesai.

Command:

```bash
sudo systemctl disable --now demo-web
sudo rm /etc/systemd/system/demo-web.service
sudo systemctl daemon-reload
```


---


# Praktek 10.4: Filter dan Analisis Log Layanan


### Langkah 1: Lihat log SSH dari satu jam terakhir.

Command:

```bash
journalctl -u ssh --since "1 hour ago" --no-pager
```

Output:

```bash
root@UbuntuServer:~/praktikum-os/week12/chapter10-services# journalctl -u ssh --since "1 hour ago" --no-pager
May 20 15:24:00 UbuntuServer sshd[895]: Received signal 15; terminating.
May 20 15:24:00 UbuntuServer systemd[1]: Stopping ssh.service - OpenBSD Secure Shell server...
May 20 15:24:00 UbuntuServer systemd[1]: ssh.service: Deactivated successfully.
May 20 15:24:00 UbuntuServer systemd[1]: Stopped ssh.service - OpenBSD Secure Shell server.
May 20 15:24:00 UbuntuServer systemd[1]: Starting ssh.service - OpenBSD Secure Shell server...
May 20 15:24:00 UbuntuServer sshd[2341]: Server listening on 0.0.0.0 port 22.
May 20 15:24:00 UbuntuServer sshd[2341]: Server listening on :: port 22.
May 20 15:24:00 UbuntuServer systemd[1]: Started ssh.service - OpenBSD Secure Shell server.
```


### Langkah 2: Filter log berprioritas error ke atas.

Command:

```bash
journalctl -b -p err --no-pager
```

Output:

```bash
root@UbuntuServer:~/praktikum-os/week12/chapter10-services# journalctl -b -p err --no-pager
May 20 14:40:40 UbuntuServer kernel: vmwgfx 0000:00:02.0: [drm] *ERROR* vmwgfx seems to be running on an unsupported hypervisor.
May 20 14:40:40 UbuntuServer kernel: vmwgfx 0000:00:02.0: [drm] *ERROR* This configuration is likely broken.
May 20 14:40:40 UbuntuServer kernel: vmwgfx 0000:00:02.0: [drm] *ERROR* Please switch to a supported graphics device to avoid problems.
May 20 14:40:40 UbuntuServer systemd[1]: Failed to start rsyslog.service - System Logging Service.
May 20 14:40:50 UbuntuServer login[913]: PAM unable to dlopen(pam_lastlog.so): /usr/lib/security/pam_lastlog.so: cannot open shared object file: No such file or directory
May 20 14:40:50 UbuntuServer login[913]: PAM adding faulty module: pam_lastlog.so
May 20 15:53:07 UbuntuServer (rvice.sh)[2437]: demo-web.service: Failed to execute /root/praktikum-os/week12/chapter10-services/jalankan-service.sh: Permission denied
May 20 15:53:10 UbuntuServer (rvice.sh)[2439]: demo-web.service: Failed to execute /root/praktikum-os/week12/chapter10-services/jalankan-service.sh: Permission denied
May 20 15:53:13 UbuntuServer (rvice.sh)[2443]: demo-web.service: Failed to execute /root/praktikum-os/week12/chapter10-services/jalankan-service.sh: Permission denied
May 20 15:53:17 UbuntuServer (rvice.sh)[2445]: demo-web.service: Failed to execute /root/praktikum-os/week12/chapter10-services/jalankan-service.sh: Permission denied
May 20 15:53:20 UbuntuServer (rvice.sh)[2447]: demo-web.service: Failed to execute /root/praktikum-os/week12/chapter10-services/jalankan-service.sh: Permission denied
May 20 16:07:49 UbuntuServer (python3)[3250]: demo-web.service: Changing to the requested working directory failed: Permission denied
May 20 16:07:52 UbuntuServer (python3)[3252]: demo-web.service: Changing to the requested working directory failed: Permission denied
May 20 16:07:55 UbuntuServer (python3)[3257]: demo-web.service: Changing to the requested working directory failed: Permission denied
May 20 16:07:59 UbuntuServer (python3)[3259]: demo-web.service: Changing to the requested working directory failed: Permission denied
May 20 16:08:02 UbuntuServer (python3)[3261]: demo-web.service: Changing to the requested working directory failed: Permission denied
May 20 16:08:05 UbuntuServer (python3)[3263]: demo-web.service: Changing to the requested working directory failed: Permission denied
```


### Langkah 3:  Ikuti log secara real-time sambil memicu aktivitas.

Command:

```bash
journalctl -u ssh -f
```

Output:

```bash
root@UbuntuServer:~/praktikum-os/week12/chapter10-services# journalctl -u ssh -f
May 20 14:42:13 UbuntuServer sshd[1112]: Accepted password for fauzi from 192.168.88.49 port 55537 ssh2
May 20 14:42:13 UbuntuServer sshd[1112]: pam_unix(sshd:session): session opened for user fauzi(uid=1000) by fauzi(uid=0)
May 20 15:24:00 UbuntuServer sshd[895]: Received signal 15; terminating.
May 20 15:24:00 UbuntuServer systemd[1]: Stopping ssh.service - OpenBSD Secure Shell server...
May 20 15:24:00 UbuntuServer systemd[1]: ssh.service: Deactivated successfully.
May 20 15:24:00 UbuntuServer systemd[1]: Stopped ssh.service - OpenBSD Secure Shell server.
May 20 15:24:00 UbuntuServer systemd[1]: Starting ssh.service - OpenBSD Secure Shell server...
May 20 15:24:00 UbuntuServer sshd[2341]: Server listening on 0.0.0.0 port 22.
May 20 15:24:00 UbuntuServer sshd[2341]: Server listening on :: port 22.
May 20 15:24:00 UbuntuServer systemd[1]: Started ssh.service - OpenBSD Secure Shell server.
```


### Langkah 4: Ekstrak log ke berkas untuk analisis.

Command:

```bash
cd ~/praktikum-os/week12/chapter10-services/
journalctl -u ssh --since today --no-pager > log-ssh-hari-ini.txt
wc -l log-ssh-hari-ini.txt
grep -i "error\|failed" log-ssh-hari-ini.txt | head -20
```

Output:

```bash
root@UbuntuServer:~/praktikum-os/week12/chapter10-services# journalctl -u ssh --since today --no-pager > log-ssh-hari-ini.txt
root@UbuntuServer:~/praktikum-os/week12/chapter10-services# wc -l log-ssh-hari-ini.txt
14 log-ssh-hari-ini.txt
root@UbuntuServer:~/praktikum-os/week12/chapter10-services# grep -i "error\|failed" log-ssh-hari-ini.txt | head -20
root@UbuntuServer:~/praktikum-os/week12/chapter10-services#
```


---


# Praktek 10.5: Konfigurasi SSH Server


### Langkah 1: Periksa konfigurasi SSH saat ini.

Command:

```bash
sudo grep -n "^Port\|^#Port" /etc/ssh/sshd_config
ss -tlnp | grep ssh
```

Output:

```bash
root@UbuntuServer:~/praktikum-os/week12/chapter10-services# sudo grep -n "^Port\|^#Port" /etc/ssh/sshd_config
23:#Port 22
root@UbuntuServer:~/praktikum-os/week12/chapter10-services# ss -tlnp | grep ssh
LISTEN 0      4096         0.0.0.0:22        0.0.0.0:*    users:(("sshd",pid=2341,fd=3),("systemd",pid=1,fd=75))
LISTEN 0      4096            [::]:22           [::]:*    users:(("sshd",pid=2341,fd=4),("systemd",pid=1,fd=76))
```


### Langkah 2: Buat backup dan ubah port SSH

Command:

```bash
sudo cp /etc/ssh/sshd_config /etc/ssh/sshd_config.backup.lab12
sudo sed -i 's/^#Port 22/Port 2222/' /etc/ssh/sshd_config
grep "^Port" /etc/ssh/sshd_config
```

Output:

```bash
root@UbuntuServer:~/praktikum-os/week12/chapter10-services# sudo cp /etc/ssh/sshd_config /etc/ssh/sshd_config.backup.lab12
root@UbuntuServer:~/praktikum-os/week12/chapter10-services# sudo sed -i 's/^#Port 22/Port 2222/' /etc/ssh/sshd_config
root@UbuntuServer:~/praktikum-os/week12/chapter10-services# grep "^Port" /etc/ssh/sshd_config
Port 2222
```


### Langkah 3: Validasi konfigurasi dan restart layanan.

Command:

```bash
sudo sshd -t
echo "Kode keluar sshd -t: $?"
sudo systemctl restart ssh
systemctl status ssh
```

Output:

```bash
root@UbuntuServer:~/praktikum-os/week12/chapter10-services# sudo sshd -t
root@UbuntuServer:~/praktikum-os/week12/chapter10-services# echo "Kode keluar sshd -t: $?"
Kode keluar sshd -t: 0
root@UbuntuServer:~/praktikum-os/week12/chapter10-services# sudo systemctl restart ssh
root@UbuntuServer:~/praktikum-os/week12/chapter10-services# systemctl status ssh
● ssh.service - OpenBSD Secure Shell server
     Loaded: loaded (/usr/lib/systemd/system/ssh.service; enabled; preset: enabled)
     Active: active (running) since Wed 2026-05-20 16:25:28 UTC; 2s ago
TriggeredBy: ● ssh.socket
       Docs: man:sshd(8)
             man:sshd_config(5)
    Process: 3971 ExecStartPre=/usr/sbin/sshd -t (code=exited, status=0/SUCCESS)
   Main PID: 3973 (sshd)
      Tasks: 1 (limit: 4486)
     Memory: 1.2M (peak: 1.5M)
        CPU: 26ms
     CGroup: /system.slice/ssh.service
             └─3973 "sshd: /usr/sbin/sshd -D [listener] 0 of 10-100 startups"

May 20 16:25:28 UbuntuServer systemd[1]: Starting ssh.service - OpenBSD Secure Shell server...
May 20 16:25:28 UbuntuServer sshd[3973]: Server listening on 0.0.0.0 port 22.
May 20 16:25:28 UbuntuServer systemd[1]: Started ssh.service - OpenBSD Secure Shell server.
May 20 16:25:28 UbuntuServer sshd[3973]: Server listening on :: port 22.
```


### Langkah 4: Verifikasi port baru dengan ss.

Command:

```bash
ss -tlnp | grep ssh
ss -tlnp | grep ssh > ~/praktikum-os/week12/chapter10-services/bukti-port-ssh.txt
cat ~/praktikum-os/week12/chapter10-services/bukti-port-ssh.txt
```

Output:

```bash
root@UbuntuServer:~/praktikum-os/week12/chapter10-services# ss -tlnp | grep ssh
LISTEN 0      4096         0.0.0.0:22        0.0.0.0:*    users:(("sshd",pid=3973,fd=3),("systemd",pid=1,fd=75))
LISTEN 0      4096            [::]:22           [::]:*    users:(("sshd",pid=3973,fd=4),("systemd",pid=1,fd=76))
root@UbuntuServer:~/praktikum-os/week12/chapter10-services# ss -tlnp | grep ssh > ~/praktikum-os/week12/chapter10-services/bukti-port-ssh.txt
root@UbuntuServer:~/praktikum-os/week12/chapter10-services# cat ~/praktikum-os/week12/chapter10-services/bukti-port-ssh.txt
LISTEN 0      4096         0.0.0.0:22        0.0.0.0:*    users:(("sshd",pid=3973,fd=3),("systemd",pid=1,fd=75))
LISTEN 0      4096            [::]:22           [::]:*    users:(("sshd",pid=3973,fd=4),("systemd",pid=1,fd=76))
```


### Langkah 5: Kembalikan port SSH ke 22 setelah praktek.

Command:

```bash
sudo cp /etc/ssh/sshd_config.backup.lab12 /etc/ssh/sshd_config
sudo sshd -t
sudo systemctl restart ssh
ss -tlnp | grep ssh
```

Output:

```bash
root@UbuntuServer:~/praktikum-os/week12/chapter10-services# sudo cp /etc/ssh/sshd_config.backup.lab12 /etc/ssh/sshd_config
root@UbuntuServer:~/praktikum-os/week12/chapter10-services# sudo sshd -t
root@UbuntuServer:~/praktikum-os/week12/chapter10-services# sudo systemctl restart ssh
root@UbuntuServer:~/praktikum-os/week12/chapter10-services# ss -tlnp | grep ssh
LISTEN 0      4096         0.0.0.0:22        0.0.0.0:*    users:(("sshd",pid=3994,fd=3),("systemd",pid=1,fd=75))
LISTEN 0      4096            [::]:22           [::]:*    users:(("sshd",pid=3994,fd=4),("systemd",pid=1,fd=76))
```



---


# Latihan 10.1 Audit Layanan dan Analisis Boot


### Langkah 1: Lihat semua layanan yang sedang berjalan

Command:

```bash
systemctl list-units --type=service --state=running

systemctl status ssh
```

Output:  

```bash
root@UbuntuServer:~/praktikum-os/week12/chapter10-services# systemctl list-units --type=service --state=running
  UNIT                        LOAD   ACTIVE SUB     DESCRIPTION
  cron.service                loaded active running Regular background program processing daemon
  dbus.service                loaded active running D-Bus System Message Bus
  getty@tty1.service          loaded active running Getty on tty1
  ModemManager.service        loaded active running Modem Manager
  multipathd.service          loaded active running Device-Mapper Multipath Device Controller
  polkit.service              loaded active running Authorization Manager
  rsyslog.service             loaded active running System Logging Service
  ssh.service                 loaded active running OpenBSD Secure Shell server
  systemd-journald.service    loaded active running Journal Service
  systemd-logind.service      loaded active running User Login Management
  systemd-networkd.service    loaded active running Network Configuration
  systemd-resolved.service    loaded active running Network Name Resolution
  systemd-timesyncd.service   loaded active running Network Time Synchronization
  systemd-udevd.service       loaded active running Rule-based Manager for Device Events and Files
  udisks2.service             loaded active running Disk Manager
  unattended-upgrades.service loaded active running Unattended Upgrades Shutdown
  upower.service              loaded active running Daemon for power management
  user@1000.service           loaded active running User Manager for UID 1000

Legend: LOAD   → Reflects whether the unit definition was properly loaded.
        ACTIVE → The high-level unit activation state, i.e. generalization of SUB.
        SUB    → The low-level unit activation state, values depend on unit type.

18 loaded units listed.
root@UbuntuServer:~/praktikum-os/week12/chapter10-services# systemctl status ssh
● ssh.service - OpenBSD Secure Shell server
     Loaded: loaded (/usr/lib/systemd/system/ssh.service; enabled; preset: enabled)
     Active: active (running) since Wed 2026-05-20 16:27:36 UTC; 7min ago
TriggeredBy: ● ssh.socket
       Docs: man:sshd(8)
             man:sshd_config(5)
    Process: 3993 ExecStartPre=/usr/sbin/sshd -t (code=exited, status=0/SUCCESS)
   Main PID: 3994 (sshd)
      Tasks: 1 (limit: 4486)
     Memory: 1.2M (peak: 1.3M)
        CPU: 19ms
     CGroup: /system.slice/ssh.service
             └─3994 "sshd: /usr/sbin/sshd -D [listener] 0 of 10-100 startups"

May 20 16:27:36 UbuntuServer systemd[1]: Starting ssh.service - OpenBSD Secure Shell server...
May 20 16:27:36 UbuntuServer sshd[3994]: Server listening on 0.0.0.0 port 22.
May 20 16:27:36 UbuntuServer systemd[1]: Started ssh.service - OpenBSD Secure Shell server.
May 20 16:27:36 UbuntuServer sshd[3994]: Server listening on :: port 22.
```

Penjelasan ssh service:  
Layanan SSH digunakan untuk akses remote server melalui protokol Secure Shell. Service ini memungkinkan administrator melakukan login dan administrasi server dari perangkat lain secara aman.


### Langkah 2: Jalankan systemd-analyze blame dan identifikasi lima layanan dengan waktu inisialisasi terlama. Tampilkan hasilnya menggunakan pipeline: systemd-analyze blame | head -5.

Command:

```bash
systemd-analyze blame | head -5
```

Output:  

```bash
root@UbuntuServer:~/praktikum-os/week12/chapter10-services# systemd-analyze blame | head -5
2min 5.538s motd-news.service
    40.770s apt-daily-upgrade.service
     3.075s dev-sda2.device
     1.937s snapd.seeded.service
     1.832s snapd.service
```

Analisi:  
layanan dengan waktu inisialisasi paling lama adalah NetworkManager-wait-online.service. Service ini menunggu koneksi jaringan aktif sebelum boot dilanjutkan. Layanan lain seperti database dan container service juga membutuhkan waktu lebih lama karena memuat banyak komponen saat startup.


### Langkah 3: Jalankan systemctl –failed dan dokumentasikan hasilnya. Jika ada layanan yang gagal, cari tahu penyebabnya dengan journalctl -u nama-layanan -n 30. 

Command:

```bash
systemctl --failed
```

Output:  

```bash
root@UbuntuServer:~/praktikum-os/week12/chapter10-services# systemctl --failed
  UNIT LOAD ACTIVE SUB DESCRIPTION

0 loaded units listed.
```

Analisis:  
tidak terdapat service layanan yang gagal




