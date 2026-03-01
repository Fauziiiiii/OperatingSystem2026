# Praktikum Sistem Operasi – Pertemuan 3  
###  Dasar Input/Output (I/O)

> Nama   : Muhammad Fauzi Fadillah
>
> NIM    : 254107020085
>
> Kelas  : TI_1G


---


# 1.11 Latihan

### Praktikum 3.1
#### Buatlah script yang:

1. Menampilkan daftar 10 file terbesar di direktori /var/log/
2. Menyimpan hasilnya ke file large-logs.txt
3. Menampilkan output juga di terminal menggunakan tee
4. Menangani error dengan redirect ke error.log

- **Command**
    ```bash
    find /var/log -type f -exec ls -lh {} \; 2> error.log | \sort -k5 -rh | \head -10 | \tee large-logs.txt
    ```

- **Output**
    ```
    -rw-r-----+ 1 root systemd-journal 8.0M Mar  1 17:14 /var/log/journal/13d6a38cd371455c84fe63df28c439af/user-1000.journal
    -rw-r-----+ 1 root systemd-journal 8.0M Mar  1 17:14 /var/log/journal/13d6a38cd371455c84fe63df28c439af/system.journal
    -rw-r-----+ 1 root systemd-journal 8.0M Mar  1 17:08 /var/log/journal/13d6a38cd371455c84fe63df28c439af/user-1000@1a57f95da8234c6e85ac1b203afad359-0000000000002742-00064bf6d4cf4513.journal
    -rw-r-----+ 1 root systemd-journal 8.0M Mar  1 17:08 /var/log/journal/13d6a38cd371455c84fe63df28c439af/system@9bd2f02d5d9141fcb444699c9237319b-0000000000002930-00064bf980cc2776.journal
    -rw-r-----+ 1 root systemd-journal 8.0M Mar  1 17:05 /var/log/journal/13d6a38cd371455c84fe63df28c439af/system@00064bf980d0aba0-377acc45fc3fc56f.journal~
    -rw-r-----+ 1 root systemd-journal 8.0M Mar  1 16:10 /var/log/journal/13d6a38cd371455c84fe63df28c439af/system@00064bf8d2dee88b-d363cd9ae2a7ce30.journal~
    -rw-r-----+ 1 root systemd-journal 8.0M Mar  1 13:56 /var/log/journal/13d6a38cd371455c84fe63df28c439af/user-1000@138c8cd95cf04f0497ade3447bb0d4b7-0000000000002287-00064b9f9be08dd6.journal
    -rw-r-----+ 1 root systemd-journal 8.0M Mar  1 13:56 /var/log/journal/13d6a38cd371455c84fe63df28c439af/system@1a57f95da8234c6e85ac1b203afad359-00000000000023c3-00064bf6d1f85aec.journal
    -rw-r-----+ 1 root systemd-journal 8.0M Mar  1 13:55 /var/log/journal/13d6a38cd371455c84fe63df28c439af/system@138c8cd95cf04f0497ade3447bb0d4b7-0000000000001ef7-00064b9f1406281e.journal
    -rw-r-----+ 1 root systemd-journal 8.0M Feb 25 05:13 /var/log/journal/13d6a38cd371455c84fe63df28c439af/user-1000@00064b9f9be08f63-1dedb53803092ec8.journal~
    ```


---


### Praktikum 3.2
#### Buat pipeline yang:

1. Membaca /etc/passwd
2. Mengekstrak username (kolom pertama)
3. Mengurutkan alfabetis
4. Menyimpan ke file sorted-users.txt
Hint: Gunakan cut, sort, dan operator redirect.

- **Perintah**
    ```bash
    cut -d: -f1 /etc/passwd | sort > sorted-users.txt
    ```

- **Cek Hasil**
    ```bash
    cat sorted-users.txt
    ```

- **Output**
    ```
    _apt
    backup
    bin
    daemon
    dhcpcd
    fauzi
    fwupd-refresh
    games
    irc
    landscape
    list
    lp
    mail
    man
    messagebus
    news
    nobody
    polkitd
    pollinate
    proxy
    root
    sshd
    sync
    sys
    syslog
    systemd-network
    systemd-resolve
    systemd-timesync
    tcpdump
    tss
    usbmux
    uucp
    uuidd
    www-data
    ```


---


### Praktikum 3.3
#### Tulis script monitoring yang:

1. Mencatat penggunaan CPU dan memory setiap 5 detik
2. Menyimpan log dengan timestamp
3. Berjalan selama 1 menit (12 iterasi)
4. Output ditampilkan di terminal DAN disimpan ke file

- **Perintah**
    ```bash
    nano monitor.sh
    ```

- **Isi dengan:**
    ```bash
    LOGFILE="monitor.log"

    for i in {1..12}
    do
        echo "===== $(date) =====" | tee -a $LOGFILE
        top -bn1 | grep "Cpu(s)" | tee -a $LOGFILE
        free -h | tee -a $LOGFILE
        echo "" | tee -a $LOGFILE
        sleep 5
    done
    ```

- **Cek Hasil**
    ```bash
    chmod +x monitor.sh
    ./monitor.sh
    ```

- **Output**
    ```
    ===== Sun Mar  1 07:13:31 PM UTC 2026 =====
    %Cpu(s):  0.0 us,  0.0 sy,  0.0 ni, 90.5 id,  0.0 wa,  0.0 hi,  9.5 si,  0.0 st
                total        used        free      shared  buff/cache   available
    Mem:           3.7Gi       441Mi       3.2Gi       1.0Mi       267Mi       3.3Gi
    Swap:          4.0Gi          0B       4.0Gi

    ===== Sun Mar  1 07:13:36 PM UTC 2026 =====
    %Cpu(s):  0.0 us,  0.0 sy,  0.0 ni, 90.0 id,  0.0 wa,  0.0 hi, 10.0 si,  0.0 st
                total        used        free      shared  buff/cache   available
    Mem:           3.7Gi       441Mi       3.2Gi       1.0Mi       267Mi       3.3Gi
    Swap:          4.0Gi          0B       4.0Gi

    ===== Sun Mar  1 07:13:42 PM UTC 2026 =====
    %Cpu(s):  4.5 us,  0.0 sy,  0.0 ni, 86.4 id,  0.0 wa,  0.0 hi,  9.1 si,  0.0 st
                total        used        free      shared  buff/cache   available
    Mem:           3.7Gi       441Mi       3.2Gi       1.0Mi       267Mi       3.3Gi
    Swap:          4.0Gi          0B       4.0Gi

    ===== Sun Mar  1 07:13:47 PM UTC 2026 =====
    %Cpu(s):  0.0 us,  0.0 sy,  0.0 ni,100.0 id,  0.0 wa,  0.0 hi,  0.0 si,  0.0 st
                total        used        free      shared  buff/cache   available
    Mem:           3.7Gi       441Mi       3.2Gi       1.0Mi       267Mi       3.3Gi
    Swap:          4.0Gi          0B       4.0Gi

    ===== Sun Mar  1 07:13:52 PM UTC 2026 =====
    %Cpu(s):  0.0 us,  5.0 sy,  0.0 ni, 95.0 id,  0.0 wa,  0.0 hi,  0.0 si,  0.0 st
                total        used        free      shared  buff/cache   available
    Mem:           3.7Gi       441Mi       3.2Gi       1.0Mi       267Mi       3.3Gi
    Swap:          4.0Gi          0B       4.0Gi
    ```


---


### Praktikum 3.4
#### Buat perintah yang:

1. Mencari semua file .conf di sistem
2. Membuang pesan "Permission denied"
3. Menghitung jumlah file yang ditemukan
4. Menyimpan daftar path lengkap ke file

- **Perintah**
    ```bash
    find / -type f -name "*.conf" 2>/dev/null | tee conf-files.txt | wc -l
    ```

- **Output**
    ```
    349
    ```


---


### Praktikum 3.5
#### Implementasikan script backup yang:

1. Menggunakan tar untuk backup direktori
2. Menampilkan progress dengan tee
3. Mencatat stdout ke backup-success.log
4. Mencatat stderr ke backup-error.log
5. Menambahkan timestamp di setiap log entry

- **Perintah**
    ```bash
    nano backup.sh
    ```

- **Isi dengan:**
    ```bash
    SOURCE="/home"
    BACKUP_FILE="backup-$(date +%Y%m%d-%H%M%S).tar.gz"

    echo "===== $(date) Backup dimulai =====" | tee -a backup-success.log

    tar -czvf $BACKUP_FILE $SOURCE \
        2> >(while read line; do echo "$(date): $line"; done >> backup-error.log) \
        | while read line; do echo "$(date): $line"; done | tee -a backup-success.log

    echo "===== $(date) Backup selesai =====" | tee -a backup-success.log
    ```

- **Cek Hasil**
    ```bash
    chmod +x monitor.sh
    ./monitor.sh
    ```

- **Output**
    ```
    ===== Sun Mar  1 07:24:45 PM UTC 2026 Backup dimulai =====
    Sun Mar  1 07:24:45 PM UTC 2026: /home/
    Sun Mar  1 07:24:45 PM UTC 2026: /home/fauzi/
    Sun Mar  1 07:24:45 PM UTC 2026: /home/fauzi/large-logs.txt
    Sun Mar  1 07:24:45 PM UTC 2026: /home/fauzi/backup.sh
    Sun Mar  1 07:24:45 PM UTC 2026: /home/fauzi/.ssh/
    Sun Mar  1 07:24:45 PM UTC 2026: /home/fauzi/.ssh/authorized_keys
    Sun Mar  1 07:24:45 PM UTC 2026: /home/fauzi/.bash_logout
    Sun Mar  1 07:24:45 PM UTC 2026: /home/fauzi/sorted-users.txt
    Sun Mar  1 07:24:45 PM UTC 2026: /home/fauzi/backup-error.log
    Sun Mar  1 07:24:45 PM UTC 2026: /home/fauzi/error.log
    Sun Mar  1 07:24:45 PM UTC 2026: /home/fauzi/backup-success.log
    Sun Mar  1 07:24:45 PM UTC 2026: /home/fauzi/.sudo_as_admin_successful
    Sun Mar  1 07:24:45 PM UTC 2026: /home/fauzi/backup-20260301-192445.tar.gz
    Sun Mar  1 07:24:45 PM UTC 2026: /home/fauzi/conf-files.txt
    Sun Mar  1 07:24:45 PM UTC 2026: /home/fauzi/monitor.log
    Sun Mar  1 07:24:45 PM UTC 2026: /home/fauzi/.cache/
    Sun Mar  1 07:24:45 PM UTC 2026: /home/fauzi/.cache/motd.legal-displayed
    Sun Mar  1 07:24:45 PM UTC 2026: /home/fauzi/.bashrc
    Sun Mar  1 07:24:45 PM UTC 2026: /home/fauzi/monitor.sh
    Sun Mar  1 07:24:45 PM UTC 2026: /home/fauzi/.profile
    Sun Mar  1 07:24:45 PM UTC 2026: /home/fauzi/.bash_history
    ===== Sun Mar  1 07:24:45 PM UTC 2026 Backup selesai =====
    ```