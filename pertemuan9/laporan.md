# Praktikum Sistem Operasi – Pertemuan 9  
##  1 Pemrograman Bash


> Nama   : Muhammad Fauzi Fadillah
>
> NIM    : 254107020085
>
> Kelas  : TI_1G


---

# Praktikum 7.1 Script Pertama: Laporan Sistem


### Langkah 1 - Buat workspace praktikum:

Command
```bash
mkdir -p ~/ praktikum - os / week09 /{ scripts , logs , data }
cd ~/ praktikum - os / week09 / scripts
```

Output:  

```bash
root@UbuntuServer:/home/fauzi# mkdir -p ~/praktikum-os/week09/{scripts,logs,data}
root@UbuntuServer:/home/fauzi# cd ~/praktikum-os/week09/scripts
root@UbuntuServer:~/praktikum-os/week09/scripts#
```

### Langkah 2 - Buat script dengan nano:

Command
```bash
nano laporan - sistem . sh
```

### Langkah 3 - Ketik isi berikut, simpan ( Ctrl+O Enter ), lalu keluar ( Ctrl+X ):

Command
```bash
#!/ bin/ bash
# Script : laporan - sistem .sh
echo " ================================ "
echo " LAPORAN SISTEM "
echo " ================================ "
echo " Tanggal : $( date '+%A, %d %B %Y ')"
echo "Jam : $( date '+%H:%M:%S ')"
echo " Hostname : $( hostname )"
echo " User : $( whoami )"
echo "CPU core : $( nproc )"
echo "RAM bebas : $( free -h | awk '/^ Mem/ { print $4 }')"
echo " Disk / : $(df -h / | awk 'NR ==2 { print $5 }')
terpakai "
echo " ================================ "
```

Output:  

```bash
  GNU nano 7.2                                                                                    laporan-sistem.sh
#!/bin/bash

# Script: laporan-sistem.sh

echo "==========================================="
echo "          LAPORAN SISTEM"
echo "==========================================="
echo "Tanggal   : $(date '+%A, %d %B %Y')"
echo "Jam       : $(date '+%H:%M:%S')"
echo "Hostname  : $(hostname)"
echo "User      : $(whoami)"
echo "CPU core  : $(nproc)"
echo "RAM bebas : $(free -h | awk '/^Mem/ {print $4}')"
echo "Disk /    : $(df -h / | awk 'NR==2 {print $5}')  terpakai"
echo "==========================================="







                                                                                                [ Read 16 lines ]
^G Help          ^O Write Out     ^W Where Is      ^K Cut           ^T Execute       ^C Location      M-U Undo         M-A Set Mark     M-] To Bracket   M-Q Previous     ^B Back          ^◂ Prev Word
^X Exit          ^R Read File     ^\ Replace       ^U Paste         ^J Justify       ^/ Go To Line    M-E Redo         M-6 Copy         ^Q Where Was     M-W Next         ^F Forward       ^▸ Next Word
```


### Langkah 4 - Beri izin dan jalankan:

Command
```bash
chmod + x laporan - sistem . sh
./ laporan - sistem . sh
```

Output:  

```bash
root@UbuntuServer:~/praktikum-os/week09/scripts# chmod +x laporan-sistem.sh
root@UbuntuServer:~/praktikum-os/week09/scripts# ./laporan-sistem.sh
===========================================
          LAPORAN SISTEM
===========================================
Tanggal   : Tuesday, 28 April 2026
Jam       : 14:57:03
Hostname  : UbuntuServer
User      : root
CPU core  : 2
RAM bebas : 3.2Gi
Disk /    : 33%  terpakai
===========================================
```

## Latihan 9.1  

Modifikasi laporan-sistem.sh agar menyimpan output ke file
laporan-YYYY-MM-DD.txt sekaligus menampilkannya di terminal. Petunjuk:
gunakan tee yang sudah dipelajari di bab sebelumnya.  


### Langkah 1  

Command
```bash
cd ~/praktikum-os/week09/scripts
nano laporan-sistem.sh
```

Output:  

```bash
  GNU nano 7.2                                                                                    laporan-sistem.sh
#!/bin/bash

# Script: laporan-sistem.sh

echo "==========================================="
echo "          LAPORAN SISTEM"
echo "==========================================="
echo "Tanggal   : $(date '+%A, %d %B %Y')"
echo "Jam       : $(date '+%H:%M:%S')"
echo "Hostname  : $(hostname)"
echo "User      : $(whoami)"
echo "CPU core  : $(nproc)"
echo "RAM bebas : $(free -h | awk '/^Mem/ {print $4}')"
echo "Disk /    : $(df -h / | awk 'NR==2 {print $5}')  terpakai"
echo "==========================================="







                                                                                                [ Read 16 lines ]
^G Help          ^O Write Out     ^W Where Is      ^K Cut           ^T Execute       ^C Location      M-U Undo         M-A Set Mark     M-] To Bracket   M-Q Previous     ^B Back          ^◂ Prev Word
^X Exit          ^R Read File     ^\ Replace       ^U Paste         ^J Justify       ^/ Go To Line    M-E Redo         M-6 Copy         ^Q Where Was     M-W Next         ^F Forward       ^▸ Next Word
```


### Langkah 2  

Command
```bash
TANGGAL=$(date '+%Y-%m-%d')
LOG_DIR="../logs"
FILE_OUTPUT="$LOG_DIR/laporan-$TANGGAL.txt"

mkdir -p "$LOG_DIR"

{
echo "==========================================="
echo "          LAPORAN SISTEM"
echo "==========================================="
echo "Tanggal   : $(date '+%A, %d %B %Y')"
echo "Jam       : $(date '+%H:%M:%S')"
echo "Hostname  : $(hostname)"
echo "User      : $(whoami)"
echo "CPU core  : $(nproc)"
echo "RAM bebas : $(free -h | awk '/^Mem/ {print $4}')"
echo "Disk /    : $(df -h / | awk 'NR==2 {print $5}')  terpakai"
echo "==========================================="
} | tee "$FILE_OUTPUT"

echo ""
echo "Laporan juga disimpan di: $FILE_OUTPUT"
```

Output:  

```bash
root@UbuntuServer:~/praktikum-os/week09/scripts# ./laporan-sistem.sh
===========================================
          LAPORAN SISTEM
===========================================
Tanggal   : Tuesday, 28 April 2026
Jam       : 15:02:07
Hostname  : UbuntuServer
User      : root
CPU core  : 2
RAM bebas : 3.2Gi
Disk /    : 33%  terpakai
===========================================

Laporan juga disimpan di: ../logs/laporan-2026-04-28.txt
```


---


# Praktikum 7.2 - Script Info Sistem dengan Argumen


### Langkah 1 - Buat script:  

Command
```bash
nano ~/praktikum-os/week09/scripts/info-sistem.sh
```

Output:  


### Langkah 2 - Ketik isi berikut:  

Command
```bash
#!/bin/bash
# Penggunaan : ./info-sistem.sh [nama - admin] [batas-disk-persen]

ADMIN=${1:-"Tidak dikenal"}
BATAS=${2:-80}
TANGGAL=$(date '+%F %T')
DISK_PERSEN=$(df / | awk 'NR==2 {print $5}' | tr -d '%')

echo "Admin     : $ADMIN "
echo "Tanggal : $TANGGAL "
echo "Disk /    : ${DISK_PERSEN}% terpakai "
echo "Batas     : ${BATAS}%"

if [ "$DISK_PERSEN" -gt "$BATAS" ]; then
        echo "STATUS    : PERINGATAN - disk melebihi batas !"
else
        SISA=$((BATAS - DISK_PERSEN))
        echo "STATUS    : Normal (sisa toleransi ${SISA}%)"
fi
```

### Langkah 3 - Simpan, beri izin, uji dengan berbagai kombinasi argumen:  

Command
```bash
chmod +x ~/praktikum-os/week09/scripts/info-sistem.sh
./info-sistem.sh
./info-sistem.sh "Dian" 50
./info-sistem.sh "Dian" 10
```

Output
```bash
root@UbuntuServer:~/praktikum-os/week09/scripts# chmod +x ~/praktikum-os/week09/scripts/info-sistem.sh
root@UbuntuServer:~/praktikum-os/week09/scripts# ./info-sistem.sh
Admin   : Tidak dikenal
Tanggal : 2026-04-28 15:25:14
Disk /  : 33% terpakai
Batas   : 80%
STATUS  : Normal (sisa toleransi 47%)
root@UbuntuServer:~/praktikum-os/week09/scripts# ./info-sistem.sh "Dian" 50
Admin   : Dian
Tanggal : 2026-04-28 15:25:27
Disk /  : 33% terpakai
Batas   : 50%
STATUS  : Normal (sisa toleransi 17%)
root@UbuntuServer:~/praktikum-os/week09/scripts# ./info-sistem.sh "Dian" 10
Admin   : Dian
Tanggal : 2026-04-28 15:25:32
Disk /  : 33% terpakai
Batas   : 10%
STATUS  : PERINGATAN - disk melebihi batas !
```

## Latihan 9.2  

Buat script kalkulator.sh yang menerima tiga argumen: <angka1>
<operator> <angka2> dengan operator +, -, *, atau /. Contoh:
./kalkulator.sh 20 + 5 menghasilkan 25. Gunakan case untuk memilih
operasi, dan validasi jika argumen tidak lengkap.


### Langkah 1  

Command
```bash
cd ~/praktikum-os/week09/scripts
```


### Langkah 2  

Command
```bash
nano kalkulator.sh
```


Input
```bash
#!/bin/bash
# Script: kalkulator.sh
# Penggunaan: ./kalkulator.sh <angka1> <operator> <angka2>
# Operator: +, -, *, /

# Validasi jumlah argumen
if [ $# -ne 3 ]; then
    echo "ERROR: Argumen tidak lengkap."
    echo "Penggunaan: $0 <angka1> <operator> <angka2>"
    echo "Operator: +, -, *, /"
    exit 1
fi

ANGKA1=$1
OPERATOR=$2
ANGKA2=$3

# Validasi bahwa angka1 dan angka2 adalah bilangan bulat (opsional, tapi baik)
if ! [[ "$ANGKA1" =~ ^-?[0-9]+$ ]] || ! [[ "$ANGKA2" =~ ^-?[0-9]+$ ]]; then
    echo "ERROR: Angka harus bilangan bulat."
    exit 1
fi

# Case untuk operator
case $OPERATOR in
    +)
        HASIL=$((ANGKA1 + ANGKA2))
        ;;
    -)
        HASIL=$((ANGKA1 - ANGKA2))
        ;;
    '*')   # Bintang perlu dikutip atau di-escape
        HASIL=$((ANGKA1 * ANGKA2))
        ;;
    /)
        if [ "$ANGKA2" -eq 0 ]; then
            echo "ERROR: Pembagian dengan nol tidak diperbolehkan."
            exit 1
        fi
        HASIL=$((ANGKA1 / ANGKA2))
        ;;
    *)
        echo "ERROR: Operator '$OPERATOR' tidak dikenal. Gunakan +, -, *, /"
        exit 1
        ;;
esac

echo "Hasil: $ANGKA1 $OPERATOR $ANGKA2 = $HASIL"
```

### Langkah 3  

Command
```bash
chmod +x kalkulator.sh
./kalkulator.sh 20 + 5
./kalkulator.sh 20 - 5
```


Output:
```bash
root@UbuntuServer:~/praktikum-os/week09/scripts# chmod +x kalkulator.sh
root@UbuntuServer:~/praktikum-os/week09/scripts# ./kalkulator.sh 20 + 5
Hasil: 20 + 5 = 25
root@UbuntuServer:~/praktikum-os/week09/scripts# ./kalkulator.sh 20 - 5
Hasil: 20 - 5 = 15
root@UbuntuServer:~/praktikum-os/week09/scripts# ./kalkulator.sh 20 '*' 5
Hasil: 20 * 5 = 100
root@UbuntuServer:~/praktikum-os/week09/scripts# ./kalkulator.sh 20 / 5
Hasil: 20 / 5 = 4
```


---


# Praktikum 7.3 - Script Grading dan Menu Interaktif  


### Langkah 1 - Buat script grading (menggunakan if dan for):

Command
```bash
nano ~/praktikum-os/week09/scripts/grading-batch.sh
```


### Langkah 2 - Ketik isi berikut:  

Command
```bash
#!/bin/bash
# Script: grading-batch.sh
# Proses daftar nilai mahasiswa

MAHASISWA=("Andi:92" "Budi:73" "Citra:55" "Deni:80" "Eka:45")

echo "=== HASIL GRADING ==="
for ENTRY in "${MAHASISWA[@]}"; do
    NAMA=$(echo "$ENTRY" | cut -d: -f1)
    NILAI=$(echo "$ENTRY" | cut -d: -f2)
    
    if [ "$NILAI" -ge 85 ]; then
        GRADE="A"
    elif [ "$NILAI" -ge 75 ]; then
        GRADE="B"
    elif [ "$NILAI" -ge 65 ]; then
        GRADE="C"
    elif [ "$NILAI" -ge 55 ]; then
        GRADE="D"
    else
        GRADE="E"
    fi
    
    printf "%-10s %3d %s\n" "$NAMA" "$NILAI" "$GRADE"
done
echo "====================="
```


### Langkah 3 - Simpan, beri izin, dan jalankan:  

Command
```bash
chmod +x ~/praktikum-os/week09/scripts/grading-batch.sh
./grading-batch.sh
```

Output
```bash
root@UbuntuServer:~/praktikum-os/week09/scripts# chmod +x ~/praktikum-os/week09/scripts/grading-batch.sh
root@UbuntuServer:~/praktikum-os/week09/scripts# ./grading-batch.sh
=== HASIL GRADING ===
Andi        92 A
Budi        73 C
Citra       55 D
Deni        80 B
Eka         45 E
=====================
```


### Langkah 4 - Buat script menu interaktif (while + case):  

Command
```bash
nano ~/praktikum-os/week09/scripts/menu-sistem.sh
```


### Langkah 5 - Ketik isi berikut:  

Command
```bash
#!/bin/bash
# Menu interaktif pemantauan sistem

while true; do
    echo ""
    echo "===== MENU MONITOR ====="
    echo "1) Info disk"
    echo "2) Info memori"
    echo "3) Proses teratas"
    echo "4) Keluar"
    echo -n "Pilih [1-4]: "
    read PILIHAN
    
    case $PILIHAN in
        1)
            df -h
            ;;
        2)
            free -h
            ;;
        3)
            ps aux --sort=-%cpu | head -6
            ;;
        4)
            echo "Sampai jumpa!"
            exit 0
            ;;
        *)
            echo "Pilihan tidak valid."
            ;;
    esac
done
```


### Langkah 6 - Beri izin dan jalankan, coba setiap opsi:  

Command
```bash
chmod +x ~/praktikum-os/week09/scripts/menu-sistem.sh
./menu-sistem.sh
```

Proses
```bash
root@UbuntuServer:~/praktikum-os/week09/scripts# nano ~/praktikum-os/week09/scripts/menu-sistem.sh
root@UbuntuServer:~/praktikum-os/week09/scripts# chmod +x ~/praktikum-os/week09/scripts/menu-sistem.sh
root@UbuntuServer:~/praktikum-os/week09/scripts# ./menu-sistem.sh

===== MENU MONITOR =====
1) Info disk
2) Info memori
3) Proses teratas
4) Keluar
Pilih [1-4]: 1
Filesystem      Size  Used Avail Use% Mounted on
tmpfs           382M  1.1M  381M   1% /run
/dev/sda2        25G  7.5G   16G  33% /
tmpfs           1.9G     0  1.9G   0% /dev/shm
tmpfs           5.0M     0  5.0M   0% /run/lock
tmpfs           382M   12K  382M   1% /run/user/1000

===== MENU MONITOR =====
1) Info disk
2) Info memori
3) Proses teratas
4) Keluar
Pilih [1-4]: 2
               total        used        free      shared  buff/cache   available
Mem:           3.7Gi       465Mi       3.1Gi       1.0Mi       346Mi       3.3Gi
Swap:          4.0Gi          0B       4.0Gi

===== MENU MONITOR =====
1) Info disk
2) Info memori
3) Proses teratas
4) Keluar
Pilih [1-4]: 3
USER         PID %CPU %MEM    VSZ   RSS TTY      STAT START   TIME COMMAND
root        1537  100  0.1  10884  4612 pts/2    R+   15:39   0:00 ps aux --sort=-%cpu
fauzi       1137  0.4  0.1  15128  7136 ?        S    14:51   0:11 sshd: fauzi@pts/1
root        1486  0.3  0.0      0     0 ?        I    15:30   0:01 [kworker/0:0-events]
root         145  0.2  0.0      0     0 ?        I    14:50   0:07 [kworker/0:2-events]
root        1386  0.1  1.1 617332 43968 ?        Ssl  15:19   0:01 /usr/libexec/fwupd/fwupd

===== MENU MONITOR =====
1) Info disk
2) Info memori
3) Proses teratas
4) Keluar
Pilih [1-4]: 4
Sampai jumpa!
```


## Latihan 9.3

Tambahkan ke script grading-batch.sh sebuah ringkasan di bagian bawah
yang menampilkan: jumlah mahasiswa per grade (A, B, C, D, E) menggunakan
perulangan for kedua yang mengiterasi array MAHASISWA.


### Langkah 1

Command:
```bash
nano ~/praktikum-os/week09/scripts/grading-batch.sh
```


### Langkah 2  

Input
```bash
#!/bin/bash
# Script: grading-batch.sh
# Proses daftar nilai mahasiswa + ringkasan per grade

MAHASISWA=("Andi:92" "Budi:73" "Citra:55" "Deni:80" "Eka:45")

echo "=== HASIL GRADING ==="

# Array untuk menyimpan grade setiap mahasiswa (opsional, bisa langsung hitung)
declare -A COUNTER
COUNTER[A]=0
COUNTER[B]=0
COUNTER[C]=0
COUNTER[D]=0
COUNTER[E]=0

for ENTRY in "${MAHASISWA[@]}"; do
    NAMA=$(echo "$ENTRY" | cut -d: -f1)
    NILAI=$(echo "$ENTRY" | cut -d: -f2)

    if [ "$NILAI" -ge 85 ]; then
        GRADE="A"
    elif [ "$NILAI" -ge 75 ]; then
        GRADE="B"
    elif [ "$NILAI" -ge 65 ]; then
        GRADE="C"
    elif [ "$NILAI" -ge 55 ]; then
        GRADE="D"
    else
        GRADE="E"
    fi

    printf "%-10s %3d %s\n" "$NAMA" "$NILAI" "$GRADE"

    # Increment counter grade
    COUNTER[$GRADE]=$((COUNTER[$GRADE] + 1))
done
echo "====================="
echo ""
echo "=== RINGKASAN PER GRADE ==="

# Perulangan for kedua untuk menampilkan ringkasan
for GRADE in A B C D E; do
    echo "Grade $GRADE : ${COUNTER[$GRADE]} mahasiswa"
done
echo "============================"
```


### Langkah 3  

Command
```bash
./grading-batch.sh
```

Output
```bash
root@UbuntuServer:~/praktikum-os/week09/scripts# ./grading-batch.sh
=== HASIL GRADING ===
Andi        92 A
Budi        73 C
Citra       55 D
Deni        80 B
Eka         45 E
=====================

=== RINGKASAN PER GRADE ===
Grade A : 1 mahasiswa
Grade B : 1 mahasiswa
Grade C : 1 mahasiswa
Grade D : 1 mahasiswa
Grade E : 1 mahasiswa
============================
```


---


# Praktikum 7.4 - Library Fungsi Validasi  


### Langkah 1 - Buat file library:

Command
```bash
nano ~/praktikum-os/week09/scripts/lib-validasi.sh
```


### Langkah 2 - Ketik isi berikut:

Command
```bash
#!/bin/bash
# lib-validasi.sh - Library fungsi validasi

adalah_angka() {
    [[ "$1" =~ ^[0-9]+$ ]]
}

error_exit() {
    echo "ERROR: $1" >&2
    exit 1
}

info() {
    echo "INFO: $1"
}

sukses() {
    echo "OK: $1"
}


konfirmasi() {
    local msg="$1"
    local jawaban
    echo -n "$msg [Y/N]: "
    read jawaban
    case "$jawaban" in
        [Yy]*) return 0 ;;
        *) return 1 ;;
    esac
}

# Cek apakah file bisa dibaca
file_bisa_dibaca() {
    [ -f "$1" ] && [ -r "$1" ]
}
```


### Langkah 3 - Buat script yang menggunakan library:  

Command
```bash
nano ~/praktikum-os/week09/scripts/pakai-library.sh
```


### Langkah 4 - Ketik isi berikut::  

Command
```bash
#!/bin/bash
# Muat library (seperti import di Java)
# Pastikan path ke lib-validasi.sh benar
source ~/praktikum-os/week09/scripts/lib-validasi.sh

# Validasi argumen
if [ $# -ne 2 ]; then
    error_exit "Penggunaan: $0 <angka> <path-file>"
fi

ANGKA=$1
FILE=$2

# Gunakan fungsi dari library
if adalah_angka "$ANGKA"; then
    sukses "Input '$ANGKA' adalah angka valid"
else
    error_exit "'$ANGKA' bukan angka"
fi

if file_bisa_dibaca "$FILE"; then
    sukses "File '$FILE' bisa dibaca"
    info "Jumlah baris: $(wc -l < "$FILE")"
else
    error_exit "File '$FILE' tidak ada atau tidak bisa dibaca"
fi
```


### Langkah 5 - Beri izin dan uji semua skenario:  

Command
```bash
chmod +x lib-validasi.sh pakai-library.sh
./pakai-library.sh 42 /etc/hostname
./pakai-library.sh abc /etc/hostname
./pakai-library.sh 42 /tidak-ada.txt
./pakai-library.sh 42
```

Output
```bash
root@UbuntuServer:~/praktikum-os/week09/scripts# chmod +x lib-validasi.sh pakai-library.sh
root@UbuntuServer:~/praktikum-os/week09/scripts# ./pakai-library.sh 42 /etc/hostname
OK: Input '42' adalah angka valid
OK: File '/etc/hostname' bisa dibaca
INFO: Jumlah baris: 1
root@UbuntuServer:~/praktikum-os/week09/scripts# ./pakai-library.sh abc /etc/hostname
ERROR: 'abc' bukan angka
root@UbuntuServer:~/praktikum-os/week09/scripts# ./pakai-library.sh 42 /tidak-ada.txt
OK: Input '42' adalah angka valid
ERROR: File '/tidak-ada.txt' tidak ada atau tidak bisa dibaca
root@UbuntuServer:~/praktikum-os/week09/scripts# ./pakai-library.sh 42
ERROR: Penggunaan: ./pakai-library.sh <angka> <path-file>
```


## Latihan 9.4

Tambahkan fungsi konfirmasi() ke lib-validasi.sh. Fungsi ini
menampilkan pertanyaan, membaca input Y/N dari user, mengembalikan
0 jika Y dan 1 jika N. Buat script demo yang memanggil fungsi ini sebelum
menghapus sebuah file.


### Langkah 1

Command
```bash
nano demo-konfirmasi.sh
```


### Langkah 2

Command
```bash
#!/bin/bash
# Script: demo-konfirmasi.sh
# Mendemonstrasikan fungsi konfirmasi sebelum menghapus file

# Muat library
source ~/praktikum-os/week09/scripts/lib-validasi.sh

# Cek argumen
if [ $# -ne 1 ]; then
    echo "Penggunaan: $0 <file-yang-akan-dihapus>"
    exit 1
fi

TARGET_FILE="$1"

# Cek apakah file ada
if [ ! -f "$TARGET_FILE" ]; then
    error_exit "File '$TARGET_FILE' tidak ditemukan."
fi

# Tampilkan info file
echo "File target: $TARGET_FILE"
echo "Ukuran: $(du -h "$TARGET_FILE" | cut -f1)  |  Baris: $(wc -l < "$TARGET_FILE")"

# Minta konfirmasi
if konfirmasi "Apakah Anda yakin ingin menghapus file '$TARGET_FILE'?"; then
    # Jika user menjawab Y (return 0)
    rm "$TARGET_FILE"
    sukses "File '$TARGET_FILE' telah dihapus."
else
    # Jika menjawab selain Y
    info "Penghapusan dibatalkan. File tetap aman."
fi
```


### Langkah 3

Command
```bash
chmod +x demo-konfirmasi.sh
echo "file contoh" > ~/praktikum-os/week09/data/contoh.txt
./demo-konfirmasi.sh ~/praktikum-os/week09/data/contoh.txt
```

Output
```bash
root@UbuntuServer:~/praktikum-os/week09/scripts# chmod +x demo-konfirmasi.sh
root@UbuntuServer:~/praktikum-os/week09/scripts# echo "file contoh" > ~/praktikum-os/week09/data/contoh.txt
root@UbuntuServer:~/praktikum-os/week09/scripts# ./demo-konfirmasi.sh ~/praktikum-os/week09/data/contoh.txt
File target: /root/praktikum-os/week09/data/contoh.txt
Ukuran: 4.0K  |  Baris: 1
Apakah Anda yakin ingin menghapus file '/root/praktikum-os/week09/data/contoh.txt'? [Y/N]: Y
OK: File '/root/praktikum-os/week09/data/contoh.txt' telah dihapus.
```


---


# Praktikum 7.5 -  Script Backup dengan Opsi  


### Langkah 1 - Buat script:

Command
```bash
nano ~/praktikum-os/week09/scripts/backup-data.sh
```


### Langkah 2 - Ketik isi berikut:

Command
```bash
#!/bin/bash
# Penggunaan : ./backup-data.sh [ -v] [-c] [-l logfile] <sumber> <tujuan>

VERBOSE= false
COMPRESS= false
LOG_FILE=""

while getopts "vcl:" OPSI; do
    case $OPSI in
        v) VERBOSE=true ;;
        c) COMPRESS=true ;;
        l) LOG_FILE="$OPTARG" ;;
        *) echo "Penggunaan: $0 [-v] [-c] [-l logfile] <sumber> <tujuan>"
           exit 1 ;;
    esac
done
shift $((OPTIND - 1))

SUMBER=$1
TUJUAN=$2

log() {
    local MSG="[$(date '+%T')] $1"
    echo "$MSG"
    [ -n "$LOG_FILE" ] && echo "$MSG" >> "$LOG_FILE"
}

[ -z "$SUMBER" ] || [ -z "$TUJUAN" ] && {
    echo " ERROR : sumber dan tujuan wajib diisi "; exit 1;
   }

[ ! -d "$SUMBER" ] && { log "ERROR: $SUMBER tidak ada"; exit 2; }

mkdir -p "$TUJUAN"
TANGGAL=$(date '+%F-%H%M%S')
[ "$VERBOSE" = true ] && log "Sumber : $SUMBER | Tujuan: $TUJUAN"

if [ "$COMPRESS" = true ]; then
    ARSIP="$TUJUAN/backup-$(basename "$SUMBER")-$TANGGAL.tar.gz"
    tar -czf "$ARSIP" -C "$(dirname "$SUMBER")" "$(basename "$SUMBER")"
    log "Arsip: $ARSIP ($(du -sh "$ARSIP" | cut -f1))"
else
    cp -r "$SUMBER" "$TUJUAN/backup-$(basename "$SUMBER")-$TANGGAL"
    log "Backup selesai."
fi
```


### Langkah 3 - Beri izin dan uji::

Command
```bash
chmod +x ~/praktikum-os/week09/scripts/backup-data.sh
cd ~/praktikum-os/week09/scripts

# Tanpa opsi
./backup-data.sh ~/praktikum-os/week09/data ~/praktikum-os/week09/logs

# Dengan verbose dan kompresi + log ke file
./backup-data.sh -v -c -l ../logs/backup.log \ ~/praktikum-os/week09/data ~/praktikum-os/week09/logs 

cat ../ logs / backup . log
```


Output
```bash
root@UbuntuServer:~/praktikum-os/week09/scripts# chmod +x ~/praktikum-os/week09/scripts/backup-data.sh
root@UbuntuServer:~/praktikum-os/week09/scripts# cd ~/praktikum-os/week09/scripts
root@UbuntuServer:~/praktikum-os/week09/scripts# ./backup-data.sh ~/praktikum-os/week09/data ~/praktikum-os/week09/logs
[16:21:47] Backup selesai.
root@UbuntuServer:~/praktikum-os/week09/scripts# ./backup-data.sh -v -c -l ../logs/backup.log ~/praktikum-os/week09/data ~/praktikum-os/week09/logs
[16:27:42] Sumber : /root/praktikum-os/week09/data | Tujuan: /root/praktikum-os/week09/logs
[16:27:42] Arsip: /root/praktikum-os/week09/logs/backup-data-2026-04-28-162742.tar.gz (4.0K)
root@UbuntuServer:~/praktikum-os/week09/scripts# cat ../logs/backup.log
[16:27:42] Sumber : /root/praktikum-os/week09/data | Tujuan: /root/praktikum-os/week09/logs
[16:27:42] Arsip: /root/praktikum-os/week09/logs/backup-data-2026-04-28-162742.tar.gz (4.0K)
```


---


# Praktikum 7.6 -  Debugging Script


### Langkah 1 - Buat script untuk dianalisis:

Command
```bash
nano ~/praktikum-os/week09/scripts/debug-latihan.sh
```


### Langkah 2 - Ketik isi berikut:

Command
```bash
#!/bin/bash
# Script: debug-latihan.sh
# Penggunaan: ./debug-latihan.sh <direktori> <batas-MB>

DIREKTORI=$1
BATAS=$2

if [ $# -ne 2 ]; then
    echo "Penggunaan: $0 <direktori> <batas-MB>"
    exit 1
fi

UKURAN=$(du -sm "$DIREKTORI" | cut -f1)
echo "Direktori : $DIREKTORI"
echo "Ukuran   : ${UKURAN} MB"
echo "Batas    : ${BATAS} MB"

if ["$UKURAN" -gt "$BATAS" ]; then
    echo "PERINGATAN: Ukuran melebihi batas!"
    echo "Kelebihan: $((UKURAN - BATAS)) MB"
else
    echo "Status: Normal (sisa: $((BATAS - UKURAN)) MB)"
fi
```


### Langkah 3 - Cek sintaks, lalu jalankan dengan tracing:

Command
```bash
chmod +x ~/praktikum-os/week09/scripts/debug-latihan.sh
bash -n debug-latihan.sh && echo "Sintaks OK"
bash -x debug-latihan.sh /etc 10
./debug-latihan.sh /var 50
./debug-latihan.sh

```

Output
```bash
root@UbuntuServer:~/praktikum-os/week09/scripts# chmod +x ~/praktikum-os/week09/scripts/debug-latihan.sh
root@UbuntuServer:~/praktikum-os/week09/scripts# bash -n debug-latihan.sh && echo "Sintaks OK"
Sintaks OK
root@UbuntuServer:~/praktikum-os/week09/scripts# bash -x debug-latihan.sh /etc 10
+ DIREKTORI=/etc
+ BATAS=10
+ '[' 2 -ne 2 ']'
++ du -sm /etc
++ cut -f1
+ UKURAN=7
+ echo 'Direktori : /etc'
Direktori : /etc
+ echo 'Ukuran   : 7 MB'
Ukuran   : 7 MB
+ echo 'Batas    : 10 MB'
Batas    : 10 MB
+ '[7' -gt 10 ']'
debug-latihan.sh: line 18: [7: command not found
+ echo 'Status: Normal (sisa: 3 MB)'
Status: Normal (sisa: 3 MB)
root@UbuntuServer:~/praktikum-os/week09/scripts# ./debug-latihan.sh /var 50
Direktori : /var
Ukuran   : 712 MB
Batas    : 50 MB
./debug-latihan.sh: line 18: [712: command not found
Status: Normal (sisa: -662 MB)
root@UbuntuServer:~/praktikum-os/week09/scripts# ./debug-latihan.sh
Penggunaan: ./debug-latihan.sh <direktori> <batas-MB>
```


## Latihan 9.5

Script debug-latihan.sh tidak menangani direktori yang tidak ada. Perbaiki dengan menambahkan:  
• set -e di baris kedua  
• Pengecekan -d "$DIREKTORI" sebelum memanggil du  
• Pesan error yang informatif jika direktori tidak ditemukan  
Uji dengan direktori yang tidak ada.


### Langkah 1

Command
```bash
nano debug-latihan.sh
```


### Langkah 2

Command
```bash
#!/bin/bash
set -euo pipefail   # -e: stop on error, -u: undefined var error, -o pipefail: pipeline error

# Script: debug-latihan.sh
# Penggunaan: ./debug-latihan.sh <direktori> <batas-MB>

# Validasi jumlah argumen
if [ $# -ne 2 ]; then
    echo "Penggunaan: $0 <direktori> <batas-MB>"
    exit 1
fi

DIREKTORI=$1
BATAS=$2

# Cek apakah direktori ada
if [ ! -d "$DIREKTORI" ]; then
    echo "ERROR: Direktori '$DIREKTORI' tidak ditemukan atau bukan direktori."
    exit 2
fi

# Dapatkan ukuran dalam MB
UKURAN=$(du -sm "$DIREKTORI" | cut -f1)

echo "Direktori : $DIREKTORI"
echo "Ukuran   : ${UKURAN} MB"
echo "Batas    : ${BATAS} MB"

# Perbandingan dengan spasi yang benar
if [ "$UKURAN" -gt "$BATAS" ]; then
    echo "PERINGATAN: Ukuran melebihi batas!"
    echo "Kelebihan: $((UKURAN - BATAS)) MB"
else
    echo "Status: Normal (sisa: $((BATAS - UKURAN)) MB)"
fi
```


### Langkah 3

Command
```bash
./debug-latihan.sh /etc 10
./debug-latihan.sh /tidak-ada 50
bash -x ./debug-latihan.sh /etc 10
```


Output
```bash
root@UbuntuServer:~/praktikum-os/week09/scripts# ./debug-latihan.sh /etc 10
Direktori : /etc
Ukuran   : 7 MB
Batas    : 10 MB
Status: Normal (sisa: 3 MB)
root@UbuntuServer:~/praktikum-os/week09/scripts# ./debug-latihan.sh /tidak-ada 50
ERROR: Direktori '/tidak-ada' tidak ditemukan atau bukan direktori.
root@UbuntuServer:~/praktikum-os/week09/scripts# bash -x ./debug-latihan.sh /etc 10
+ set -euo pipefail
+ '[' 2 -ne 2 ']'
+ DIREKTORI=/etc
+ BATAS=10
+ '[' '!' -d /etc ']'
++ du -sm /etc
++ cut -f1
+ UKURAN=7
+ echo 'Direktori : /etc'
Direktori : /etc
+ echo 'Ukuran   : 7 MB'
Ukuran   : 7 MB
+ echo 'Batas    : 10 MB'
Batas    : 10 MB
+ '[' 7 -gt 10 ']'
+ echo 'Status: Normal (sisa: 3 MB)'
Status: Normal (sisa: 3 MB)
```


---


# Tugas Praktikum

Kerjakan tugas di $HOME/praktikum-os/week09/. Script di scripts/, output di logs/.


## Tugas 1 Script Absensi Kelas

Konteks: instruktur mencatat kehadiran mahasiswa dari command line.


### Langkah 1  

Command
```bash
nano ~/praktikum-os/week09/scripts/absensi.sh
```



### Langkah 2  

Command
```bash
#!/bin/bash
# Script: absensi.sh
# Mencatat kehadiran mahasiswa

# Konfigurasi
LOG_DIR="../logs"
mkdir -p "$LOG_DIR"

# File log harian
TGL=$(date '+%Y-%m-%d')
ABSENSI_FILE="$LOG_DIR/absensi-$TGL.txt"

# Fungsi bantuan
usage() {
    echo "Penggunaan: $0 [OPTIONS] [NAMA] [STATUS]"
    echo ""
    echo "Catat kehadiran:"
    echo "  $0 \"Budi\" hadir"
    echo "  $0 \"Citra\" izin"
    echo "  $0 \"Dewi\" alpha"
    echo ""
    echo "Opsi:"
    echo "  -r      Tampilkan rekapitulasi (jumlah per status)"
    echo "  -h      Tampilkan bantuan ini"
    exit 0
}

# Fungsi rekapitulasi
rekap() {
    if [ ! -f "$ABSENSI_FILE" ]; then
        echo "Belum ada data absensi untuk hari ini."
        exit 0
    fi
    
    echo "=== REKAPITULASI ABSENSI $TGL ==="
    echo "File: $ABSENSI_FILE"
    echo ""
    
    # Inisialisasi counter
    HADIR=0
    IZIN=0
    ALPHA=0
    
    # Baca file dan hitung
    while IFS= read -r line; do
        case "$line" in
            *"- HADIR"*) ((HADIR++)) ;;
            *"- IZIN"*)  ((IZIN++)) ;;
            *"- ALPHA"*) ((ALPHA++)) ;;
        esac
    done < "$ABSENSI_FILE"
    
    echo "HADIR : $HADIR"
    echo "IZIN  : $IZIN"
    echo "ALPHA : $ALPHA"
    echo "TOTAL : $((HADIR + IZIN + ALPHA))"
    echo "================================"
}

# Fungsi catat kehadiran
catat() {
    local NAMA=$1
    local STATUS=$2
    
    # Validasi status
    if [[ "$STATUS" != "hadir" && "$STATUS" != "izin" && "$STATUS" != "alpha" ]]; then
        echo "ERROR: Status harus 'hadir', 'izin', atau 'alpha'"
        exit 1
    fi
    
    # Format waktu [HH:MM]
    JAM=$(date '+%H:%M')
    STATUS_UPPER=$(echo "$STATUS" | tr '[:lower:]' '[:upper:]')
    
    # Tulis ke file
    echo "[$JAM] $NAMA - $STATUS_UPPER" >> "$ABSENSI_FILE"
    echo "Dicatat: $NAMA - $STATUS_UPPER pada jam $JAM"
}

# Proses opsi dengan getopts
MODE="catat"
while getopts "rh" OPT; do
    case $OPT in
        r) MODE="rekap" ;;
        h) usage ;;
        *) usage ;;
    esac
done

# Geser argumen opsi
shift $((OPTIND - 1))

# Eksekusi berdasarkan mode
if [ "$MODE" = "rekap" ]; then
    rekap
else
    # Mode catat: butuh 2 argumen (NAMA STATUS)
    if [ $# -ne 2 ]; then
        echo "ERROR: Untuk mencatat, berikan NAMA dan STATUS."
        usage
    fi
    catat "$1" "$2"
fi
```


### Langkah 3  

Command
```bash
root@UbuntuServer:~/praktikum-os/week09/scripts# chmod +x absensi.sh
root@UbuntuServer:~/praktikum-os/week09/scripts# ./absensi.sh "Andi" hadir
./absensi.sh "Budi" hadir
./absensi.sh "Citra" izin
./absensi.sh "Dewi" alpha
./absensi.sh "Eka" hadir
Dicatat: Andi - HADIR pada jam 14:10
Dicatat: Budi - HADIR pada jam 14:10
Dicatat: Citra - IZIN pada jam 14:10
Dicatat: Dewi - ALPHA pada jam 14:10
Dicatat: Eka - HADIR pada jam 14:10
```

Output:  

```bash
root@UbuntuServer:/home/fauzi# mkdir -p ~/praktikum-os/week09/{scripts,logs,data}
root@UbuntuServer:/home/fauzi# cd ~/praktikum-os/week09/scripts
root@UbuntuServer:~/praktikum-os/week09/scripts#
```


### Langkah 4  

Command
```bash
./absensi.sh -r
./absensi.sh -h
cat ../logs/absensi-2026-04-26.txt
```

Output:  

```bash
root@UbuntuServer:~/praktikum-os/week09/scripts# ./absensi.sh -r
=== REKAPITULASI ABSENSI 2026-04-26 ===
File: ../logs/absensi-2026-04-26.txt

HADIR : 3
IZIN  : 1
ALPHA : 1
TOTAL : 5
================================
root@UbuntuServer:~/praktikum-os/week09/scripts# ./absensi.sh -h
Penggunaan: ./absensi.sh [OPTIONS] [NAMA] [STATUS]

Catat kehadiran:
  ./absensi.sh "Budi" hadir
  ./absensi.sh "Citra" izin
  ./absensi.sh "Dewi" alpha

Opsi:
  -r      Tampilkan rekapitulasi (jumlah per status)
  -h      Tampilkan bantuan ini
root@UbuntuServer:~/praktikum-os/week09/scripts# cat ../logs/absensi-2026-04-26.txt
[14:10] Andi - HADIR
[14:10] Budi - HADIR
[14:10] Citra - IZIN
[14:10] Dewi - ALPHA
[14:10] Eka - HADIR
```



## Tugas 2 Script Health Check Sistem

### Langkah 1  

Command
```bash
nano ~/praktikum-os/week09/scripts/healthcheck.sh
```


### Langkah 2  

Command
```bash
#!/bin/bash
# ================================================
# Script: healthcheck.sh
# Deskripsi: Memeriksa kesehatan sistem (CPU, memori, disk)
# Penggunaan: ./healthcheck.sh [-t batas] [-h]
# ================================================

set -euo pipefail

SCRIPT_NAME=$(basename "$0")
LOG_DIR="../logs"
mkdir -p "$LOG_DIR"

# Default batas disk (%)
BATAS_DISK=80

# File log harian
TGL=$(date '+%Y-%m-%d')
LOG_FILE="$LOG_DIR/healthcheck-$TGL.log"

usage() {
    echo "Penggunaan: $SCRIPT_NAME [-t batas] [-h]"
    echo "  -t batas   Atur batas peringatan disk (default 80%)"
    echo "  -h         Tampilkan bantuan"
    exit 0
}

log() {
    echo "[$(date '+%F %T')] $*"
}

error_exit() {
    echo "ERROR: $*" >&2
    exit 1
}

cleanup() {
    log "Script healthcheck selesai."
}
trap cleanup EXIT

# Proses opsi getopts
while getopts "t:h" opt; do
    case $opt in
        t)
            if [[ "$OPTARG" =~ ^[0-9]+$ ]] && [ "$OPTARG" -ge 0 ] && [ "$OPTARG" -le 100 ]; then
                BATAS_DISK="$OPTARG"
            else
                error_exit "Batas disk harus angka 0-100"
            fi
            ;;
        h) usage ;;
        *) usage ;;
    esac
done

shift $((OPTIND - 1))

# Mulai healthcheck
{
    echo "==========================================="
    echo "         HEALTH CHECK SISTEM"
    echo "==========================================="
    echo "Tanggal/Waktu : $(date '+%A, %d %B %Y %H:%M:%S')"
    echo "Hostname      : $(hostname)"
    echo "Uptime        : $(uptime -p 2>/dev/null || uptime | awk -F 'up ' '{print $2}' | awk -F ',' '{print $1}')"
    echo ""

    echo "--- CPU ---"
    # Load average dari uptime
    LOAD=$(uptime | awk -F 'load average:' '{print $2}' | xargs)
    echo "Load average : $LOAD"
    # CPU usage (sederhana, bisa pakai top -bn1)
    if command -v mpstat >/dev/null 2>&1; then
        CPU_IDLE=$(mpstat 1 1 | awk '/Average/ && /all/ {print $12}')
        CPU_USED=$(echo "100 - $CPU_IDLE" | bc)
        printf "CPU usage    : %.1f%%\n" "$CPU_USED"
    else
        echo "CPU usage    : (instal sysstat untuk detail)"
    fi
    echo ""

    echo "--- MEMORI ---"
    free -h | awk '/^Mem:/ {print "Total   : " $2; print "Terpakai: " $3; print "Bebas   : " $4}'
    echo ""

    echo "--- DISK (setiap filesystem) ---"
    df -h | grep -E '^/dev/' | while read -r line; do
        FILESYSTEM=$(echo "$line" | awk '{print $1}')
        UKURAN=$(echo "$line" | awk '{print $2}')
        TERPAKAI=$(echo "$line" | awk '{print $3}')
        AVAIL=$(echo "$line" | awk '{print $4}')
        PERSEN=$(echo "$line" | awk '{print $5}' | tr -d '%')
        MOUNT=$(echo "$line" | awk '{print $6}')
        
        printf "Filesystem: %s (%s)\n" "$FILESYSTEM" "$MOUNT"
        printf "  Ukuran : %s, Terpakai: %s (%s%%), Sisa: %s\n" "$UKURAN" "$TERPAKAI" "$PERSEN" "$AVAIL"
        
        if [ "$PERSEN" -gt "$BATAS_DISK" ]; then
            echo "PERINGATAN: Penggunaan disk melebihi batas $BATAS_DISK%"
        fi
        echo ""
    done

    echo "==========================================="
    echo "Log disimpan di: $LOG_FILE"
} | tee -a "$LOG_FILE"
```


### Langkah 3  

Command
```bash
chmod +x healthcheck.sh
./healthcheck.sh
```

Output:  

```bash
root@UbuntuServer:~/praktikum-os/week09/scripts# chmod +x healthcheck.sh
root@UbuntuServer:~/praktikum-os/week09/scripts# ./healthcheck.sh
===========================================
         HEALTH CHECK SISTEM
===========================================
Tanggal/Waktu : Sunday, 26 April 2026 14:14:10
Hostname      : UbuntuServer
Uptime        : up 1 hour, 47 minutes

--- CPU ---
Load average : 0.07, 0.03, 0.01
CPU usage    : 1.9%

--- MEMORI ---
Total   : 3.3Gi
Terpakai: 379Mi
Bebas   : 2.8Gi

--- DISK (setiap filesystem) ---
Filesystem: /dev/sda2 (/)
  Ukuran : 50G, Terpakai: 3.5G (8%), Sisa: 44G

===========================================
Log disimpan di: ../logs/healthcheck-2026-04-26.log
[2026-04-26 14:14:11] Script healthcheck selesai.
```


### Langkah 4  

Command
```bash
./healthcheck.sh -t 30
./healthcheck.sh -h
cat ../logs/healthcheck-2026-04-26.log
```

Output:  

```bash
root@UbuntuServer:~/praktikum-os/week09/scripts# ./healthcheck.sh -t 30
===========================================
         HEALTH CHECK SISTEM
===========================================
Tanggal/Waktu : Sunday, 26 April 2026 14:14:34
Hostname      : UbuntuServer
Uptime        : up 1 hour, 47 minutes

--- CPU ---
Load average : 0.05, 0.02, 0.00
CPU usage    : 2.2%

--- MEMORI ---
Total   : 3.3Gi
Terpakai: 380Mi
Bebas   : 2.8Gi

--- DISK (setiap filesystem) ---
Filesystem: /dev/sda2 (/)
  Ukuran : 50G, Terpakai: 3.5G (8%), Sisa: 44G

===========================================
Log disimpan di: ../logs/healthcheck-2026-04-26.log
[2026-04-26 14:14:35] Script healthcheck selesai.
root@UbuntuServer:~/praktikum-os/week09/scripts# ./healthcheck.sh -h
Penggunaan: healthcheck.sh [-t batas] [-h]
  -t batas   Atur batas peringatan disk (default 80%)
  -h         Tampilkan bantuan
[2026-04-26 14:14:54] Script healthcheck selesai.
root@UbuntuServer:~/praktikum-os/week09/scripts#
root@UbuntuServer:~/praktikum-os/week09/scripts# cat ../logs/healthcheck-2026-04-26.log
===========================================
         HEALTH CHECK SISTEM
===========================================
Tanggal/Waktu : Sunday, 26 April 2026 14:14:10
Hostname      : UbuntuServer
Uptime        : up 1 hour, 47 minutes

--- CPU ---
Load average : 0.07, 0.03, 0.01
CPU usage    : 1.9%

--- MEMORI ---
Total   : 3.3Gi
Terpakai: 379Mi
Bebas   : 2.8Gi

--- DISK (setiap filesystem) ---
Filesystem: /dev/sda2 (/)
  Ukuran : 50G, Terpakai: 3.5G (8%), Sisa: 44G

===========================================
Log disimpan di: ../logs/healthcheck-2026-04-26.log
===========================================
         HEALTH CHECK SISTEM
===========================================
Tanggal/Waktu : Sunday, 26 April 2026 14:14:34
Hostname      : UbuntuServer
Uptime        : up 1 hour, 47 minutes

--- CPU ---
Load average : 0.05, 0.02, 0.00
CPU usage    : 2.2%

--- MEMORI ---
Total   : 3.3Gi
Terpakai: 380Mi
Bebas   : 2.8Gi

--- DISK (setiap filesystem) ---
Filesystem: /dev/sda2 (/)
  Ukuran : 50G, Terpakai: 3.5G (8%), Sisa: 44G

===========================================
Log disimpan di: ../logs/healthcheck-2026-04-26.log
```
