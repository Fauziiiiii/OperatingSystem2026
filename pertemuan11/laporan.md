# Praktikum Sistem Operasi – Pertemuan 11  
##  1 Manajemen File & User/Group 


> Nama   : Muhammad Fauzi Fadillah
>
> NIM    : 254107020085
>
> Kelas  : TI_1G


---

# Praktikum 9.1 — Permissions 


### Langkah 1: Buat direktori kerja dan dua file uji.

Command:

```bash
mkdir ~/lab-permissions && cd ~/lab-permissions
echo "data rahasia" > secret.txt
echo '#!/bin/bash' > myscript.sh
echo 'echo Hello' >> myscript.sh
ls -la
```

Output:  

```bash
root@UbuntuServer:/home/fauzi# mkdir -p ~/praktikum-os/week11/lab-permissions
root@UbuntuServer:/home/fauzi# cd ~/praktikum-os/week11/lab-permissions
root@UbuntuServer:~/praktikum-os/week11/lab-permissions# echo "data rahasia" > secret.txt
echo '#!/bin/bash' > myscript.sh
echo 'echo Hello' >> myscript.sh
ls -la
total 16
drwxr-xr-x 2 root root 4096 May 12 09:52 .
drwxr-xr-x 3 root root 4096 May 12 09:52 ..
-rw-r--r-- 1 root root   23 May 12 09:52 myscript.sh
-rw-r--r-- 1 root root   13 May 12 09:52 secret.txt
```


### Langkah 2: Jadikan secret.txt privat hanya untuk owner.

Command:

```bash
chmod 600 secret.txt
ls -l secret.txt
```

Output:  

```bash
root@UbuntuServer:~/praktikum-os/week11/lab-permissions# chmod 600 secret.txt
root@UbuntuServer:~/praktikum-os/week11/lab-permissions# ls -l secret.txt
-rw------- 1 root root 13 May 12 09:52 secret.txt
```


### Langkah  3: Jadikan myscript.sh dapat dijalankan.

Command:

```bash
chmod 755 myscript.sh
ls -l myscript.sh
./myscript.sh
```

Output:  

```bash
root@UbuntuServer:~/praktikum-os/week11/lab-permissions# chmod 755 myscript.sh
root@UbuntuServer:~/praktikum-os/week11/lab-permissions# ls -l myscript.sh
-rwxr-xr-x 1 root root 23 May 12 09:52 myscript.sh
root@UbuntuServer:~/praktikum-os/week11/lab-permissions# ./myscript.sh
Hello
root@UbuntuServer:~/praktikum-os/week11/lab-permissions#
```


### Langkah 4: Buat direktori bersama dan amati efek SGID sederhana.

Command:

```bash
mkdir shared-dir
chmod g+s shared-dir
ls -ld shared-dir
```

Output:  

```bash
root@UbuntuServer:~/praktikum-os/week11/lab-permissions# mkdir shared-dir
root@UbuntuServer:~/praktikum-os/week11/lab-permissions# chmod g+s shared-dir
root@UbuntuServer:~/praktikum-os/week11/lab-permissions# ls -ld shared-dir
drwxr-sr-x 2 root root 4096 May 12 09:56 shared-dir
```


### Langkah 5: Uji efek umask pada file baru.

Command:

```bash
umask
umask 027
touch testfile-027
ls -l testfile-027
```

Output:  

```bash
root@UbuntuServer:~/praktikum-os/week11/lab-permissions# umask
0022
root@UbuntuServer:~/praktikum-os/week11/lab-permissions# umask 027
root@UbuntuServer:~/praktikum-os/week11/lab-permissions# touch testfile-027
root@UbuntuServer:~/praktikum-os/week11/lab-permissions# ls -l testfile-027
-rw-r----- 1 root root 0 May 12 10:01 testfile-027
```



## Analisis
1. Mengapa secret.txt tidak dapat dibaca oleh group dan others setelah chmod 600?  
Jawaban  
Karena group dan others bernilai 0, maka mereka tidak memiliki hak:  
membaca (r)  
menulis (w)  
menjalankan (x)  
Akibatnya hanya owner (root) yang dapat mengakses isi secret.txt.  

2. Apa perbedaan arti 600 dan 755 terhadap file yang diuji?  
Jawaban  
Permission 600:  
Owner → read & write  
Group → tidak ada akses  
Others → tidak ada akses  
Permission 755:  
Owner: read, write, execute  
Group → read & execute  
Others → read & execute

3. Setelah umask 027, permission apa yang dihasilkan untuk file baru, dan mengapa bukan 777?  
Jawaban  
Menghasilkan permission 640 (rw-r--). Karena file baru maksimal memiliki permission dasar 666 karena file biasa tidak otomatis executable. Kemudian dikurangi oleh nilai umask.  
Mengapa bukan 777, karena akan memberikan full akses kepada semua user, dan akan membahayakan untuk sistem


---


# Praktikum 9.2 — ACL 


### Langkah 1: Siapkan file dan lihat permission standar tanpa ACL tambahan.

Command:

```bash
mkdir -p ~/praktikum-os/week11/lab-acl
cd ~/praktikum-os/week11/lab-acl
echo "Data petting" > confidential.txt
chmod 640 confidential.txt
ls -l confidential.txt
getfacl confidential.txt
```

Output:  

```bash
root@UbuntuServer:~/praktikum-os/week11/lab-permissions# mkdir -p ~/praktikum-os/week11/lab-acl
root@UbuntuServer:~/praktikum-os/week11/lab-permissions# cd ~/praktikum-os/week11/lab-acl
root@UbuntuServer:~/praktikum-os/week11/lab-acl# echo "Data penting" > confidential.txt
root@UbuntuServer:~/praktikum-os/week11/lab-acl# chmod 640 confidential.txt
root@UbuntuServer:~/praktikum-os/week11/lab-acl# ls -l confidential.txt
-rw-r----- 1 root root 13 May 12 10:14 confidential.txt
root@UbuntuServer:~/praktikum-os/week11/lab-acl# getfacl confidential.txt
Command 'getfacl' not found, but can be installed with:
apt install acl
root@UbuntuServer:~/praktikum-os/week11/lab-acl# apt install acl
root@UbuntuServer:~/praktikum-os/week11/lab-acl# getfacl confidential.txt
# file: confidential.txt
# owner: root
# group: root
user::rw-
group::r--
other::---
```

### Langkah 2: Beri akses baca ke satu user tertentu tanpa mengubah owner atau group.

Command:

```bash
setfacl -m u:root:r confidential.txt
ls -l confidential.txt
getfacl confidential.txt
```

Output:  

```bash
root@UbuntuServer:~/praktikum-os/week11/lab-acl# setfacl -m u:root:r confidential.txt
root@UbuntuServer:~/praktikum-os/week11/lab-acl# ls -l confidential.txt
-rw-r-----+ 1 root root 13 May 12 10:14 confidential.txt
root@UbuntuServer:~/praktikum-os/week11/lab-acl# getfacl confidential.txt
# file: confidential.txt
# owner: root
# group: root
user::rw-
user:root:r--
group::r--
mask::r--
other::---
```


### Langkah 3: Buat direktori bersama yang mewariskan ACL ke file baru

Command:

```bash
mkdir shared
setfacl -d -m u:root:rwx shared
setfacl -d -m u:www-data:r-x shared
getfacl shared

touch shared/inherited.txt
getfacl shared/inherited.txt
```

Output:  

```bash
root@UbuntuServer:~/praktikum-os/week11/lab-acl# mkdir shared
root@UbuntuServer:~/praktikum-os/week11/lab-acl# setfacl -d -m u:root:rwx shared
root@UbuntuServer:~/praktikum-os/week11/lab-acl# setfacl -d -m u:www-data:r-x shared
root@UbuntuServer:~/praktikum-os/week11/lab-acl# getfacl shared
# file: shared
# owner: root
# group: root
user::rwx
group::r-x
other::---
default:user::rwx
default:user:root:rwx
default:user:www-data:r-x
default:group::r-x
default:mask::rwx
default:other::---

root@UbuntuServer:~/praktikum-os/week11/lab-acl# touch shared/inherited.txt
root@UbuntuServer:~/praktikum-os/week11/lab-acl# getfacl shared/inherited.txt
# file: shared/inherited.txt
# owner: root
# group: root
user::rw-
user:root:rwx                   #effective:rw-
user:www-data:r-x               #effective:r--
group::r-x                      #effective:r--
mask::rw-
other::---
```


---


# Praktikum 9.3A — Membuat dan Mengelola User 


### Langkah 1: buat dua user


Command:

```bash
sudo useradd -m -s /bin/bash userA
sudo useradd -m -s /bin/bash userB
sudo passwd userA
sudo passwd userB
```

Output:  

```bash
root@UbuntuServer:~/praktikum-os/week11/lab-acl# sudo useradd -m -s /bin/bash userA
root@UbuntuServer:~/praktikum-os/week11/lab-acl# sudo useradd -m -s /bin/bash userB
root@UbuntuServer:~/praktikum-os/week11/lab-acl# sudo passwd userA
New password:
Retype new password:
passwd: password updated successfully
root@UbuntuServer:~/praktikum-os/week11/lab-acl# sudo passwd userB
New password:
Retype new password:
passwd: password updated successfully
```


### Langkah 2: verifikasi

Command:

```bash
id userA
getent passwd userA
```

Output:  

```bash
root@UbuntuServer:~/praktikum-os/week11/lab-acl# id userA
uid=1001(userA) gid=1001(userA) groups=1001(userA)
root@UbuntuServer:~/praktikum-os/week11/lab-acl# getent passwd userA
userA:x:1001:1001::/home/userA:/bin/bash
```


### Langkah 3: modifikasi shell userA

Command:

```bash
sudo usermod -s /bin/zsh userA
getent passwd userA
```

Output:  

```bash
root@UbuntuServer:~/praktikum-os/week11/lab-acl# sudo usermod -s /bin/zsh userA
usermod: Warning: missing or non-executable shell '/bin/zsh'
root@UbuntuServer:~/praktikum-os/week11/lab-acl# getent passwd userA
userA:x:1001:1001::/home/userA:/bin/zsh
```


### Langkah 4: lock dan unlock userB

Command:

```bash
sudo usermod -L userB
sudo passwd -S userB
sudo usermod -U userB
sudo passwd -S userB
```

Output:  

```bash
root@UbuntuServer:~/praktikum-os/week11/lab-acl# sudo usermod -L userB
root@UbuntuServer:~/praktikum-os/week11/lab-acl# sudo passwd -S userB
userB L 2026-05-12 0 99999 7 -1
root@UbuntuServer:~/praktikum-os/week11/lab-acl# sudo usermod -U userB
root@UbuntuServer:~/praktikum-os/week11/lab-acl# sudo passwd -S userB
userB P 2026-05-12 0 99999 7 -1
```


---


# Praktikum 9.3B — Group Management 


### Langkah 1: buat dua group


Command:

```bash
sudo groupadd labgroup
sudo groupadd readonly-group
```


### Langkah 2: tambahkan userA ke kedua group


Command:

```bash
sudo usermod -aG labgroup,readonly-group userA
```


### Langkah 3: tambahkan userB hanya ke readonly - group


Command:

```bash
sudo usermod -aG readonly-group userB
```


### Langkah 4: verifikasi


Command:

```bash
id userA
id userB
getent group labgroup
getent group readonly-group
```

Output:  

```bash
root@UbuntuServer:~/praktikum-os/week11/lab-acl# id userA
uid=1001(userA) gid=1001(userA) groups=1001(userA),1003(labgroup),1004(readonly-group)
root@UbuntuServer:~/praktikum-os/week11/lab-acl# id userB
uid=1002(userB) gid=1002(userB) groups=1002(userB),1004(readonly-group)
root@UbuntuServer:~/praktikum-os/week11/lab-acl# getent group labgroup
labgroup:x:1003:userA
root@UbuntuServer:~/praktikum-os/week11/lab-acl# getent group readonly-group
readonly-group:x:1004:userA,userB
```


---


# Praktikum 9.3C — Password Aging Policy 


### Langkah 1: set aging policy untuk userA


Command:

```bash
sudo chage -M 60 -W 7 -m 1 userA
sudo chage -l userA
```


Output:

```bash
root@UbuntuServer:~/praktikum-os/week11/lab-acl# sudo chage -M 60 -W 7 -m 1 userA
root@UbuntuServer:~/praktikum-os/week11/lab-acl# sudo chage -l userA
Last password change                                    : May 12, 2026
Password expires                                        : Jul 11, 2026
Password inactive                                       : never
Account expires                                         : never
Minimum number of days between password change          : 1
Maximum number of days between password change          : 60
Number of days of warning before password expires       : 7
```


### Langkah 2: paksa userA ganti password saat login pertama


Command:

```bash
sudo chage -d 0 userA
```


### Langkah 3: kunci password userB


Command:

```bash
sudo passwd -l userB
sudo passwd -S userB
```


Output:

```bash
root@UbuntuServer:~/praktikum-os/week11/lab-acl# sudo passwd -l userB
passwd: password changed.
root@UbuntuServer:~/praktikum-os/week11/lab-acl# sudo passwd -S userB
userB L 2026-05-12 0 99999 7 -1
```


### Langkah 4:  unlock kembali


Command:

```bash
sudo passwd -u userB
sudo passwd -S userB
```


Output:

```bash
root@UbuntuServer:~/praktikum-os/week11/lab-acl# sudo passwd -u userB
passwd: password changed.
root@UbuntuServer:~/praktikum-os/week11/lab-acl# sudo passwd -S userB
userB P 2026-05-12 0 99999 7 -1
```


---


# Praktikum 9.4 — Konfigurasi sudo


### Langkah 1: Buat file konfigurasi sudo khusus untuk userA.


Command:

```bash
sudo visudo -f /etc/sudoers.d/lab-userA

userA ALL=(root) NOPASSWD: /usr/bin/apt update, /usr/bin/apt upgrade
userA ALL=(root) /bin/systemctl status *
```



### Langkah 2: Verifikasi aturan yang aktif dan uji hasilnya.


Command:

```bash
sudo -l -U userA
sudo grep "userA" /var/log/auth.log | tail -10
```


Output:

```bash
root@UbuntuServer:~/praktikum-os/week11/lab-acl# sudo -l -U userA
Matching Defaults entries for userA on UbuntuServer:
    env_reset, mail_badpass, secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin, use_pty

User userA may run the following commands on UbuntuServer:
    (root) NOPASSWD: /usr/bin/apt update, /usr/bin/apt upgrade
    (root) /bin/systemctl status *
root@UbuntuServer:~/praktikum-os/week11/lab-acl# sudo grep "userA" /var/log/auth.log | tail -10
2026-05-12T10:40:06.152886+00:00 UbuntuServer usermod[25211]: add 'userA' to group 'readonly-group'
2026-05-12T10:40:06.152942+00:00 UbuntuServer usermod[25211]: add 'userA' to shadow group 'labgroup'
2026-05-12T10:40:06.152979+00:00 UbuntuServer usermod[25211]: add 'userA' to shadow group 'readonly-group'
2026-05-12T11:01:26.942740+00:00 UbuntuServer sudo:     root : TTY=pts/2 ; PWD=/root/praktikum-os/week11/lab-acl ; USER=root ; COMMAND=/usr/bin/chage -M 60 -W 7 -m 1 userA
2026-05-12T11:01:26.966173+00:00 UbuntuServer chage[25255]: changed password expiry for userA
2026-05-12T11:01:35.383282+00:00 UbuntuServer sudo:     root : TTY=pts/2 ; PWD=/root/praktikum-os/week11/lab-acl ; USER=root ; COMMAND=/usr/bin/chage -l userA
2026-05-12T11:02:50.926377+00:00 UbuntuServer sudo:     root : TTY=pts/2 ; PWD=/root/praktikum-os/week11/lab-acl ; USER=root ; COMMAND=/usr/bin/chage -d 0 userA
2026-05-12T11:02:50.949188+00:00 UbuntuServer chage[25264]: changed password expiry for userA
2026-05-12T11:06:54.666939+00:00 UbuntuServer sudo:     root : TTY=pts/2 ; PWD=/root/praktikum-os/week11/lab-acl ; USER=root ; COMMAND=/usr/sbin/visudo -f /etc/sudoers.d/lab-userA
2026-05-12T11:08:10.034956+00:00 UbuntuServer sudo:     root : TTY=pts/2 ; PWD=/root/praktikum-os/week11/lab-acl ; USER=root ; COMMAND=/usr/bin/grep userA /var/log/auth.log
```


---


# Praktikum 9.5 — Disk Quota


### Langkah 1: Buat image filesystem kecil dan mount dengan opsi quota.


Command:

```bash
sudo dd if=/dev/zero of=/tmp/quota-test.img bs=1M count=100
sudo mkfs.ext4 /tmp/quota-test.img
sudo mkdir -p /mnt/quota-test
sudo mount -o loop,usrquota,grpquota /tmp/quota-test.img /mnt/quota-test
```


Output:

```bash
root@UbuntuServer:~/praktikum-os/week11/lab-acl# sudo dd if=/dev/zero of=/tmp/quota-test.img bs=1M count=100
100+0 records in
100+0 records out
104857600 bytes (105 MB, 100 MiB) copied, 0.151329 s, 693 MB/s
root@UbuntuServer:~/praktikum-os/week11/lab-acl# sudo mkfs.ext4 /tmp/quota-test.img
mke2fs 1.47.0 (5-Feb-2023)
Discarding device blocks: done
Creating filesystem with 25600 4k blocks and 25600 inodes

Allocating group tables: done
Writing inode tables: done
Creating journal (1024 blocks): done
Writing superblocks and filesystem accounting information: done

root@UbuntuServer:~/praktikum-os/week11/lab-acl# sudo mkdir -p /mnt/quota-test
root@UbuntuServer:~/praktikum-os/week11/lab-acl# sudo mount -o loop,usrquota,grpquota /tmp/quota-test.img /mnt/quota-test
root@UbuntuServer:~/praktikum-os/week11/lab-acl#
```


### Langkah 2: Buat database quota dan aktifkan enforcement.


Command:

```bash
sudo quotacheck -cug /mnt/quota-test
sudo quotaon -v /mnt/quota-test
sudo repquota /mnt/quota-test
```


Output:

```bash
root@UbuntuServer:~/praktikum-os/week11# sudo quotacheck -cug /mnt/quota-test
root@UbuntuServer:~/praktikum-os/week11# sudo quotaon -v /mnt/quota-test
quotaon: Your kernel probably supports ext4 quota feature but you are using external quota files. Please switch your filesystem to use ext4 quota feature as external quota files on ext4 are deprecated.
/dev/loop0 [/mnt/quota-test]: group quotas turned on
/dev/loop0 [/mnt/quota-test]: user quotas turned on
root@UbuntuServer:~/praktikum-os/week11# sudo repquota /mnt/quota-test
*** Report for user quotas on device /dev/loop0
Block grace time: 7days; Inode grace time: 7days
                        Block limits                File limits
User            used    soft    hard  grace    used  soft  hard  grace
----------------------------------------------------------------------
root      --      20       0       0              2     0     0


```


### Langkah 3: Tetapkan quota untuk user uji dan amati hasilnya.


Command:

```bash
sudo edquota -u userA
sudo repquota /mnt/quota-test
```


### Langkah 4: Bersihkan lingkungan uji setelah selesai.


Command:

```bash
sudo quotaoff /mnt/quota-test
sudo umount /mnt/quota-test
sudo rm /tmp/quota-test.img
```


---


#  Latihan 9.A — Audit dan Kolaborasi


### Langkah 1: Temukan file SUID aktif dengan find / -perm -4000 -type f 2>/dev/null, lalu jelaskan tiga file yang Anda kenali beserta alasannya.


Command:

```bash
find / -perm -4000 -type f 2>/dev/null
```


Output:

```bash
root@UbuntuServer:~/praktikum-os/week11# find / -perm -4000 -type f 2>/dev/null
/usr/lib/polkit-1/polkit-agent-helper-1
/usr/lib/dbus-1.0/dbus-daemon-launch-helper
/usr/lib/openssh/ssh-keysign
/usr/bin/gpasswd
/usr/bin/sudo
/usr/bin/newgrp
/usr/bin/mount
/usr/bin/chfn
/usr/bin/chsh
/usr/bin/umount
/usr/bin/su
/usr/bin/fusermount3
/usr/bin/passwd
```


### Langkah 2: Cari direktori world-writable dan tentukan mana yang valid dan mana yang berisiko


Command:

```bash
find / -type d -perm -002 2>/dev/null | head -10
```


Output:

```bash
root@UbuntuServer:~/praktikum-os/week11# find / -type d -perm -002 2>/dev/null | head -10
/run/screen
/run/lock
/var/tmp
/var/tmp/systemd-private-fe443632be84465298a5692ebf7f2dd1-systemd-logind.service-USxoJe/tmp
/var/tmp/systemd-private-fe443632be84465298a5692ebf7f2dd1-upower.service-tHWtEd/tmp
/var/tmp/systemd-private-fe443632be84465298a5692ebf7f2dd1-systemd-timesyncd.service-iJ6jgR/tmp
/var/tmp/systemd-private-fe443632be84465298a5692ebf7f2dd1-systemd-resolved.service-rgGw64/tmp
/var/tmp/systemd-private-fe443632be84465298a5692ebf7f2dd1-ModemManager.service-RPLMPr/tmp
/var/tmp/systemd-private-fe443632be84465298a5692ebf7f2dd1-polkit.service-Sq7mBH/tmp
/var/crash
```


### Langkah 3: Rancang konfigurasi permission standar dan ACL untuk direktori proyek /srv/webapp/ agar group webapp-team dapat menulis, user deploy hanya membaca, dan file baru selalu mewarisi group proyek.


Command:

```bash
sudo groupadd webapp-team
sudo useradd -m -s /bin/bash deploy
sudo passwd deploy
sudo mkdir -p /srv/webapp
sudo chown root:webapp-team /srv/webapp
sudo chmod 2770 /srv/webapp
sudo setfacl -m u:deploy:r-x /srv/webapp
sudo setfacl -d -m g:webapp-team:rwx /srv/webapp
sudo setfacl -d -m u:deploy:r-x /srv/webapp

ls -ld /srv/webapp
getfacl /srv/webapp
```


Output:

```bash
root@UbuntuServer:~/praktikum-os/week11# sudo groupadd webapp-team
root@UbuntuServer:~/praktikum-os/week11# sudo useradd -m -s /bin/bash deploy
root@UbuntuServer:~/praktikum-os/week11# sudo passwd deploy
New password:
Retype new password:
passwd: password updated successfully
root@UbuntuServer:~/praktikum-os/week11# sudo mkdir -p /srv/webapp
root@UbuntuServer:~/praktikum-os/week11# sudo chown root:webapp-team /srv/webapp
root@UbuntuServer:~/praktikum-os/week11# sudo chmod 2770 /srv/webapp
root@UbuntuServer:~/praktikum-os/week11# sudo setfacl -m u:deploy:r-x /srv/webapp
root@UbuntuServer:~/praktikum-os/week11# sudo setfacl -d -m g:webapp-team:rwx /srv/webapp
root@UbuntuServer:~/praktikum-os/week11# sudo setfacl -d -m u:deploy:r-x /srv/webapp
root@UbuntuServer:~/praktikum-os/week11# ls -ld /srv/webapp
drwxrws---+ 2 root webapp-team 4096 May 12 11:54 /srv/webapp
root@UbuntuServer:~/praktikum-os/week11# getfacl /srv/webapp
getfacl: Removing leading '/' from absolute path names
# file: srv/webapp
# owner: root
# group: webapp-team
# flags: -s-
user::rwx
user:deploy:r-x
group::rwx
mask::rwx
other::---
default:user::rwx
default:user:deploy:r-x
default:group::rwx
default:group:webapp-team:rwx
default:mask::rwx
default:other::---
```



#  Latihan 9.B — Kebijakan Akun dan Quota


### Langkah 1


Command:

```bash
sudo useradd -m -s /bin/bash intern
sudo passwd intern
```


### Langkah 2


Command:

```bash
sudo usermod -aG labgroup intern
id intern
```


Output:

```bash
root@UbuntuServer:~/praktikum-os/week11# sudo usermod -aG labgroup intern
root@UbuntuServer:~/praktikum-os/week11# id intern
uid=1004(intern) gid=1007(intern) groups=1007(intern),1003(labgroup)
```


### Langkah 3


Command:

```bash
sudo chage -M 45 -W 7 intern
sudo chage -l intern
```


Output:

```bash
root@UbuntuServer:~/praktikum-os/week11# sudo chage -M 45 -W 7 intern
root@UbuntuServer:~/praktikum-os/week11# sudo chage -l intern
Last password change                                    : May 12, 2026
Password expires                                        : Jun 26, 2026
Password inactive                                       : never
Account expires                                         : never
Minimum number of days between password change          : 0
Maximum number of days between password change          : 45
Number of days of warning before password expires       : 7
```


### Langkah 4


Command:

```bash
sudo visudo -f /etc/sudoers.d/intern  
  GNU nano 7.2                                                                                /etc/sudoers.d/intern.tmp *
intern ALL=(root) NOPASSWD: /bin/systemctl status *  

sudo -l -U intern
```


Output:

```bash
root@UbuntuServer:~/praktikum-os/week11# sudo visudo -f /etc/sudoers.d/intern
root@UbuntuServer:~/praktikum-os/week11# sudo -l -U intern
Matching Defaults entries for intern on UbuntuServer:
    env_reset, mail_badpass, secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin, use_pty

User intern may run the following commands on UbuntuServer:
    (root) NOPASSWD: /bin/systemctl status *
```


### Langkah 5


Command:

```bash
sudo setquota -u intern 102400 204800 500 1000 /home
sudo repquota -v /home | grep intern
```



### Langkah 6


Command:

```bash
getent passwd intern

groups intern

sudo chage -l intern
sudo -l -U intern
sudo quota -u intern
```


Output:

```bash
root@UbuntuServer:~/praktikum-os/week11# getent passwd intern
intern:x:1004:1007::/home/intern:/bin/bash
root@UbuntuServer:~/praktikum-os/week11# groups intern
intern : intern labgroup
root@UbuntuServer:~/praktikum-os/week11# sudo chage -l intern
Last password change                                    : May 12, 2026
Password expires                                        : Jun 26, 2026
Password inactive                                       : never
Account expires                                         : never
Minimum number of days between password change          : 0
Maximum number of days between password change          : 45
Number of days of warning before password expires       : 7
root@UbuntuServer:~/praktikum-os/week11# sudo -l -U intern
Matching Defaults entries for intern on UbuntuServer:
    env_reset, mail_badpass, secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin, use_pty

User intern may run the following commands on UbuntuServer:
    (root) NOPASSWD: /bin/systemctl status *
root@UbuntuServer:~/praktikum-os/week11# sudo quota -u intern
root@UbuntuServer:~/praktikum-os/week11#
```
