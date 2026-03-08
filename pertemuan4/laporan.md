# Praktikum Sistem Operasi – Pertemuan 4  
###  Operasi File dan Struktur Direktory

> Nama   : Muhammad Fauzi Fadillah
>
> NIM    : 254107020085
>
> Kelas  : TI_1G


---

# Tugas Pendahuluan

1. Apa yang dimaksud perintah-perintah direktory : pwd, cd, mkdir, rmdir.
>   Jawaban:  
    >   * ```pwd``` (Print Working Directory)  
    >       Perintah ```pwd``` digunakan untuk menampilkan direktori aktif saat ini.
    >   * ```cd``` (Change Directory)   
    >       Perintah ```cd``` digunakan untuk berpindah dari satu direktori ke direktori lain.
    >   * ```mkdir``` (Make Directory)  
    >       Perintah ```mkdir``` digunakan untuk membuat direktori baru.
    >   * ```rmdir``` (Remove Directory)  
    >       Perintah ```rmdir``` digunakan untuk menghapus direktori yang kosong.


2. Apa yang dimaksud perintah-perintah manipulasi file : cp, mv dan rm (sertakan 
format yang digunakan)
>   Jawaban:  
    >   * ```cp``` (Copy)  
    >       Perintah ```cp``` digunakan untuk menyalin file atau direktori.  
    >       Contoh Format:
    >       ```bash
    >       cp fileA.txt fileB.txt
    >       ```
    >       Format untuk Direktori:
    >       ```bash
    >       cp -r folder1 folder2
    >       ```

    >   * ```mv``` (Move)  
    >       Perintah ```mv``` digunakan untuk memindahkan atau mengganti nama file/direktori.  
    >       Contoh Format:
    >       ```bash
    >       mv file1.txt file2.txt      # rename
    >       mv file.txt /home/fauzi/    # pindah lokasi
    >       ```

    >   * ```rm``` (Remove)  
    >       Perintah ```rm``` digunakan untuk menghapus file atau direktori.   
    >       Contoh Format:
    >       ```bash
    >       rm fileA.txt
    >       ```
    >       Format untuk Direktori:
    >       ```bash
    >       rm -r folder1
    >       ```

3. Jelaskan perbedaan Symbolic link menggunakan hard link (direct) dan soft link
(indirect).
>   Jawaban:  
    >   * Hard Link (Direct Link)   
    >       * Mengarah langsung ke inode file asli.
    >       * Tidak bisa digunakan antar partisi berbeda.
    >       * Jika file asli dihapus, data masih bisa diakses melalui link.
    >       * Tidak bisa untuk direktori.
    >       Contoh Format:
    >       ```bash
    >       ln fileAsli fileLink
    >       ```
    >
    >   * Soft Link (Symbolic Link / Indirect Link)   
    >       * Mengarah ke nama file asli (seperti shortcut).
    >       * Bisa antar partisi berbeda.
    >       * Jika file asli dihapus, link menjadi rusak.
    >       * Bisa digunakan untuk direktori.  
    >
    >   Contoh Format:
    >   ```bash
    >   ln fileAsli fileLink
    >   ```

4. Tuliskan maksud perintah-perintah : file, find, which, locate dan grep
>   Jawaban:  
    >   * ```file```  
    >       Digunakan untuk mengetahui tipe suatu file.  
    >       Contoh Format:
    >       ```bash
    >       file program.c
    >       ```
    >   * ```find```  
    >       Digunakan untuk mencari file dalam struktur direktori.  
    >       Contoh Format:
    >       ```bash
    >       find /home -name "*.txt"
    >       ```
    >   * ```which```  
    >       Digunakan untuk mengetahui lokasi program/command dalam sistem.  
    >       Contoh Format:
    >       ```bash
    >       which ls
    >       ```
    >   * ```locate```  
    >       Digunakan untuk mengetahui lokasi program/command dalam sistem.  
    >       Contoh Format:
    >       ```bash
    >       locate nama_file
    >       ```
    >   * ```grep``` (Global Regular Expression Print)
    >       Digunakan untuk mencari teks tertentu dalam file.  
    >       Contoh Format:
    >       ```bash
    >       grep root /etc/passwd
    >       ```



# Latihan

1. Cobalah urutan perintah berikut:

```bash
$ cd
$ pwd
$ ls –al
$ cd .
$ pwd
$ cd ..
$ pwd
$ ls -al
$ cd ..
$ pwd
$ ls -al
$ cd /etc
$ ls –al | more
$ cat passwd
$ cd –
$ pwd
```

Output:  

```bash
root@UbuntuServer:/home/fauzi# cd
root@UbuntuServer:~# pwd
/root
root@UbuntuServer:~# ls -al
total 48
drwx------  6 root root 4096 Mar  8 09:41 .
drwxr-xr-x 23 root root 4096 Feb 24 10:19 ..
-rw-------  1 root root 3926 Mar  4 06:59 .bash_history
-rw-r--r--  1 root root 3106 Apr 22  2024 .bashrc
drwxr-xr-x  6 root root 4096 Feb 24 15:45 .config
-rw-------  1 root root   20 Mar  8 09:41 .lesshst
drwxr-xr-x  3 root root 4096 Feb 24 10:20 .local
drwxr-xr-x  3 root root 4096 Feb 24 13:21 praktikum-os
-rw-r--r--  1 root root  161 Apr 22  2024 .profile
drwx------  2 root root 4096 Feb 24 09:36 .ssh
-r-xr-xr-x  1 root root 7049 Feb 24 09:35 vboxpostinstall.sh
root@UbuntuServer:~# cd .
root@UbuntuServer:~# pwd
/root
root@UbuntuServer:~# cd ..
root@UbuntuServer:/# pwd
/
root@UbuntuServer:/# ls -al
total 4194396
drwxr-xr-x  23 root root       4096 Feb 24 10:19 .
drwxr-xr-x  23 root root       4096 Feb 24 10:19 ..
lrwxrwxrwx   1 root root          7 Apr 22  2024 bin -> usr/bin
drwxr-xr-x   2 root root       4096 Feb 26  2024 bin.usr-is-merged
drwxr-xr-x   3 root root       4096 Feb 24 09:54 boot
dr-xr-xr-x   2 root root       4096 Feb 24 09:12 cdrom
drwxr-xr-x  19 root root       4040 Mar  8 09:40 dev
drwxr-xr-x 112 root root       4096 Feb 24 16:02 etc
drwxr-xr-x   3 root root       4096 Feb 24 09:36 home
lrwxrwxrwx   1 root root          7 Apr 22  2024 lib -> usr/lib
lrwxrwxrwx   1 root root          9 Apr 22  2024 lib64 -> usr/lib64
drwxr-xr-x   2 root root       4096 Feb 26  2024 lib.usr-is-merged
drwx------   2 root root      16384 Feb 24 09:13 lost+found
drwxr-xr-x   2 root root       4096 Aug  5  2025 media
drwxr-xr-x   2 root root       4096 Aug  5  2025 mnt
drwxr-xr-x   2 root root       4096 Aug  5  2025 opt
dr-xr-xr-x 182 root root          0 Mar  8 09:40 proc
drwx------   6 root root       4096 Mar  8 09:41 root
drwxr-xr-x  28 root root        860 Mar  8 10:24 run
lrwxrwxrwx   1 root root          8 Apr 22  2024 sbin -> usr/sbin
drwxr-xr-x   2 root root       4096 Dec 11  2024 sbin.usr-is-merged
drwxr-xr-x   2 root root       4096 Feb 24 09:36 snap
drwxr-xr-x   2 root root       4096 Aug  5  2025 srv
-rw-------   1 root root 4294967296 Feb 24 10:19 swapfile
dr-xr-xr-x  13 root root          0 Mar  8 09:40 sys
drwxrwxrwt  14 root root       4096 Mar  8 09:43 tmp
drwxr-xr-x  12 root root       4096 Aug  5  2025 usr
drwxr-xr-x  13 root root       4096 Feb 24 09:35 var
root@UbuntuServer:/# cd ..
root@UbuntuServer:/# pwd
/
root@UbuntuServer:/# cd /etc
root@UbuntuServer:/etc# ls -al | more
total 960
drwxr-xr-x 112 root root       4096 Feb 24 16:02 .
drwxr-xr-x  23 root root       4096 Feb 24 10:19 ..
-rw-r--r--   1 root root       3444 Jul  5  2023 adduser.conf
drwxr-xr-x   2 root root       4096 Feb 24 09:58 alternatives
drwxr-xr-x   2 root root       4096 Feb 24 09:53 apparmor
drwxr-xr-x   9 root root      12288 Feb 24 09:53 apparmor.d
drwxr-xr-x   3 root root       4096 Aug  5  2025 apport
drwxr-xr-x   9 root root       4096 Feb 24 09:13 apt
-rw-r--r--   1 root root       2319 Mar 31  2024 bash.bashrc
-rw-r--r--   1 root root         45 Aug  5  2025 bash_completion
drwxr-xr-x   2 root root       4096 Aug  5  2025 bash_completion.d
-rw-r--r--   1 root root        367 Aug  2  2022 bindresvport.blacklist
drwxr-xr-x   2 root root       4096 Jul  2  2025 binfmt.d
drwxr-xr-x   2 root root       4096 Aug  5  2025 byobu
drwxr-xr-x   3 root root       4096 Aug  5  2025 ca-certificates
-rw-r--r--   1 root root       6288 Aug  5  2025 ca-certificates.conf
drwxr-xr-x   5 root root       4096 Feb 24 09:53 cloud
drwxr-xr-x   2 root root       4096 Feb 24 09:14 console-setup
drwx------   2 root root       4096 Jul  2  2025 credstore
drwx------   2 root root       4096 Jul  2  2025 credstore.encrypted
drwxr-xr-x   2 root root       4096 Aug  5  2025 cron.d
drwxr-xr-x   2 root root       4096 Feb 24 09:34 cron.daily
drwxr-xr-x   2 root root       4096 Aug  5  2025 cron.hourly
drwxr-xr-x   2 root root       4096 Aug  5  2025 cron.monthly
-rw-r--r--   1 root root       1136 Aug  5  2025 crontab
drwxr-xr-x   2 root root       4096 Aug  5  2025 cron.weekly
drwxr-xr-x   2 root root       4096 Aug  5  2025 cron.yearly
drwxr-xr-x   2 root root       4096 Aug  5  2025 cryptsetup-initramfs
-rw-r--r--   1 root root         54 Aug  5  2025 crypttab
drwxr-xr-x   4 root root       4096 Aug  5  2025 dbus-1
-rw-r--r--   1 root root       2967 Apr 12  2024 debconf.conf
-rw-r--r--   1 root root         11 Apr 22  2024 debian_version
drwxr-xr-x   3 root root       4096 Feb 24 16:02 default
-rw-r--r--   1 root root       1706 Jul  5  2023 deluser.conf
drwxr-xr-x   2 root root       4096 Aug  5  2025 depmod.d
drwxr-xr-x   3 root root       4096 Aug  5  2025 dhcp
-rw-r--r--   1 root root       1429 May  7  2024 dhcpcd.conf
drwxr-xr-x   4 root root       4096 Feb 24 09:34 dpkg
-rw-r--r--   1 root root        685 Apr  8  2024 e2scrub.conf
-rw-r--r--   1 root root        106 Aug  5  2025 environment
-rw-r--r--   1 root root       1853 Oct 17  2022 ethertypes
drwxr-xr-x   4 root root       4096 Feb 24 09:33 fonts
-rw-r--r--   1 root root        473 Feb 24 10:21 fstab
-rw-r--r--   1 root root        694 Apr  8  2024 fuse.conf
drwxr-xr-x   4 root root       4096 Feb 24 09:53 fwupd
-rw-r--r--   1 root root       2584 Jan 31  2024 gai.conf
drwxr-xr-x   4 root root       4096 Feb 24 09:58 ghostscript
drwxr-xr-x   2 root root       4096 Feb 24 09:35 gnutls
drwxr-xr-x   2 root root       4096 Aug  5  2025 groff
-rw-r--r--   1 root root        775 Feb 24 09:36 group
-rw-r--r--   1 root root        770 Feb 24 09:36 group-
drwxr-xr-x   2 root root       4096 Feb 24 09:53 grub.d
root@UbuntuServer:/etc# cat passwd
root:x:0:0:root:/root:/bin/bash
daemon:x:1:1:daemon:/usr/sbin:/usr/sbin/nologin
bin:x:2:2:bin:/bin:/usr/sbin/nologin
sys:x:3:3:sys:/dev:/usr/sbin/nologin
sync:x:4:65534:sync:/bin:/bin/sync
games:x:5:60:games:/usr/games:/usr/sbin/nologin
man:x:6:12:man:/var/cache/man:/usr/sbin/nologin
lp:x:7:7:lp:/var/spool/lpd:/usr/sbin/nologin
mail:x:8:8:mail:/var/mail:/usr/sbin/nologin
news:x:9:9:news:/var/spool/news:/usr/sbin/nologin
uucp:x:10:10:uucp:/var/spool/uucp:/usr/sbin/nologin
proxy:x:13:13:proxy:/bin:/usr/sbin/nologin
www-data:x:33:33:www-data:/var/www:/usr/sbin/nologin
backup:x:34:34:backup:/var/backups:/usr/sbin/nologin
list:x:38:38:Mailing List Manager:/var/list:/usr/sbin/nologin
irc:x:39:39:ircd:/run/ircd:/usr/sbin/nologin
_apt:x:42:65534::/nonexistent:/usr/sbin/nologin
nobody:x:65534:65534:nobody:/nonexistent:/usr/sbin/nologin
systemd-network:x:998:998:systemd Network Management:/:/usr/sbin/nologin
systemd-timesync:x:997:997:systemd Time Synchronization:/:/usr/sbin/nologin
dhcpcd:x:100:65534:DHCP Client Daemon,,,:/usr/lib/dhcpcd:/bin/false
messagebus:x:101:102::/nonexistent:/usr/sbin/nologin
systemd-resolve:x:992:992:systemd Resolver:/:/usr/sbin/nologin
pollinate:x:102:1::/var/cache/pollinate:/bin/false
polkitd:x:991:991:User for polkitd:/:/usr/sbin/nologin
syslog:x:103:104::/nonexistent:/usr/sbin/nologin
uuidd:x:104:105::/run/uuidd:/usr/sbin/nologin
tcpdump:x:105:107::/nonexistent:/usr/sbin/nologin
tss:x:106:108:TPM software stack,,,:/var/lib/tpm:/bin/false
landscape:x:107:109::/var/lib/landscape:/usr/sbin/nologin
fwupd-refresh:x:989:989:Firmware update daemon:/var/lib/fwupd:/usr/sbin/nologin
usbmux:x:108:46:usbmux daemon,,,:/var/lib/usbmux:/usr/sbin/nologin
fauzi:x:1000:1000:fauzi:/home/fauzi:/bin/bash
sshd:x:109:65534::/run/sshd:/usr/sbin/nologin
root@UbuntuServer:/etc# cd -
/
root@UbuntuServer:/# pwd
/
```


2. Lanjutkan penelusuran pohon pada sistem file menggunakan cd, ls, pwd dan cat. Telusuri direktory /bin, /usr/bin, /sbin, /tmp dan /boot.  

* /bin
```bash
root@UbuntuServer:/# cd /bin
root@UbuntuServer:/bin# ls
'['                                   display-im6.q16           jq                                 pamfind                     pnmcolormap           rnano                   systemd-cat
 411toppm                             distro-info               jsondiff                           pamfix                      pnmcomp               routel                  systemd-cgls
 aa-enabled                           dmesg                     jsonpatch                          pamfixtrunc                 pnmconvol             rpcgen                  systemd-cgtop
 aa-exec                              dnsdomainname             json-patch-jsondiff                pamflip                     pnmcrop               rrsync                  systemd-confext
 aa-features-abi                      domainname                jsonpointer                        pamfunc                     pnmcut                rsync                   systemd-creds
 acpidbg                              do-release-upgrade        json_pp                            pamgauss                    pnmdepth              rsync-ssl               systemd-cryptenroll
 add-apt-repository                   dpkg                      jsonschema                         pamgetcolor                 pnmenlarge            rtla                    systemd-cryptsetup
 addpart                              dpkg-deb                  JxrDecApp                          pamgradient                 pnmfile               rtstat                  systemd-delta
 animate                              dpkg-divert               JxrEncApp                          pamhomography               pnmflip               runcon                  systemd-detect-virt
 animate-im6                          dpkg-maintscript-helper   kbdinfo                            pamhue                      pnmgamma              run-one                 systemd-escape
 animate-im6.q16                      dpkg-query                kbd_mode                           pamlevels                   pnmhisteq             run-one-constantly      systemd-firstboot
 anytopnm                             dpkg-realpath             kbxutil                            pamlookup                   pnmhistmap            run-one-until-failure   systemd-hwdb
 apport-bug                           dpkg-split                keep-one-running                   pammasksharpen              pnmindex              run-one-until-success   systemd-id128
 apport-cli                           dpkg-statoverride         kernel-install                     pammixinterlace             pnminterp             run-parts               systemd-inhibit
 apport-collect                       dpkg-trigger              kill                               pammixmulti                 pnminterp-gen         run-this-one            systemd-machine-id-setup
 apport-unpack                        du                        killall                            pammosaicknit               pnminvert             rview                   systemd-mount
 appstreamcli                         dumpkeys                  kmod                               pamoil                      pnmmargin             rvim                    systemd-notify
 apropos                              dvipdf                    kmodsign                           pampaintspill               pnmmercator           sadf                    systemd-path
 apt                                  eatmydata                 landscape-sysinfo                  pamperspective              pnmmontage            sar                     systemd-repart
 apt-add-repository                   ec2metadata               last                               pampick                     pnmnlfilt             sar.sysstat             systemd-run
 apt-cache                            echo                      lastb                              pampop9                     pnmnoraw              savelog                 systemd-socket-activate
 apt-cdrom                            ed                        lastlog                            pamrecolor                  pnmnorm               sbattach                systemd-stdio-bridge
 apt-config                           editor                    lcf                                pamrestack                  pnmpad                sbigtopgm               systemd-sysext
 apt-extracttemplates                 egrep                     ldd                                pamrgbatopng                pnmpaste              sbkeysync               systemd-sysusers
 apt-ftparchive                       eject                     ld.so                              pamrubber                   pnmpsnr               sbsiglist               systemd-tmpfiles
 apt-get                              enc2xs                    leaftoppm                          pamscale                    pnmquant              sbsign                  systemd-tty-ask-password-agent
 apt-key                              encguess                  less                               pamseq                      pnmquantall           sbvarsign               systemd-umount
 apt-mark                             env                       lessecho                           pamshadedrelief             pnmremap              sbverify                tabs
 apt-sortpkgs                         envsubst                  lessfile                           pamsharpmap                 pnmrotate             scalar                  tac
 arch                                 eps2eps                   lesskey                            pamsharpness                pnmscale              scandeps                tail
 asciitopgm                           eqn                       lesspipe                           pamshuffle                  pnmscalefixed         scp                     tapestat
 atktopbm                             escp2topbm                lexgrog                            pamsistoaglyph              pnmshear              screen                  tar
 automat-visualize3                   ex                        libnetcfg                          pamslice                    pnmsmooth             screendump              taskset
 avstopam                             expand                    libsixel-config                    pamsplit                    pnmsplit              script                  tbl
 awk                                  expiry                    link                               pamstack                    pnmstitch             scriptlive              tclsh
 b2sum                                expr                      linux32                            pamstereogram               pnmtile               scriptreplay            tclsh8.6
 base32                               eyuvtoppm                 linux64                            pamstretch                  pnmtoddif             scsi_logging_level      tcpdump
 base64                               factor                    linux-boot-prober                  pamstretch-gen              pnmtofiasco           scsi_mandat             tee
 basename                             faillog                   linux-check-removal                pamsumm                     pnmtofits             scsi_readcap            telnet
 basenc                               fallocate                 linux-update-symlinks              pamsummcol                  pnmtojbig             scsi_ready              tempfile
 bash                                 false                     linux-version                      pamtable                    pnmtojpeg             scsi_satl               test
 bashbug                              fc-cache                  lispmtopgm                         pamthreshold                pnmtopalm             scsi_start              tgatoppm
 bc                                   fc-cat                    ln                                 pamtilt                     pnmtopclxl            scsi_stop               thinkjettopbm
 bdftopcf                             fc-conflist               lnstat                             pamtoavs                    pnmtoplainpnm         scsi_temperature        tic
 bdftruncate                          fc-list                   loadkeys                           pamtodjvurle                pnmtopng              sdiff                   tifftopnm
 bioradtopgm                          fc-match                  loadunimap                         pamtofits                   pnmtopnm              sed                     time
 bmptopnm                             fc-pattern                locale                             pamtogif                    pnmtops               select-editor           timedatectl
 bmptoppm                             fc-query                  locale-check                       pamtohdiff                  pnmtorast             sensible-browser        timeout
 boltctl                              fc-scan                   localectl                          pamtohtmltbl                pnmtorle              sensible-editor         tkconch3
 bpftrace                             fc-validate               localedef                          pamtojpeg2k                 pnmtosgi              sensible-pager          tload
 bpftrace-aotrt                       fgconsole                 logger                             pamtompfont                 pnmtosir              sensible-terminal       tmux
 brushtopbm                           fgrep                     login                              pamtooctaveimg              pnmtotiff             seq                     tnftp
 btop                                 fiascotopnm               loginctl                           pamtopam                    pnmtotiffcmyk         setarch                 toe
 btrfs                                figlet                    logname                            pamtopdbimg                 pnmtoxwd              setfont                 toilet
 btrfsck                              figlet-toilet             look                               pamtopfm                    pod2html              setkeycodes             top
 btrfs-convert                        file                      lowntfs-3g                         pamtopng                    pod2man               setleds                 touch
 btrfs-find-root                      finalrd                   ls                                 pamtopnm                    pod2text              setlogcons              tput
 btrfs-image                          find                      lsattr                             pamtoqoi                    pod2usage             setmetamode             tr
 btrfs-map-logical                    findmnt                   lsblk                              pamtosrf                    podchecker            setpci                  trace-cmd
 btrfs-select-super                   fitstopnm                 lsb_release                        pamtosvg                    pollinate             setpriv                 tracepath
 btrfstune                            flock                     lscpu                              pamtotga                    pphs                  setsid                  trial3
 busctl                               fmt                       lshw                               pamtotiff                   ppm3d                 setterm                 troff
 busybox                              fold                      lsinitramfs                        pamtouil                    ppmbrighten           setupcon                true
 byobu                                fonttosfnt                lsipc                              pamtowinicon                ppmchange             sftp                    truncate
 byobu-config                         free                      lslocks                            pamtoxvmini                 ppmcie                sg                      tset
 byobu-ctrl-a                         fstopgm                   lslogins                           pamtris                     ppmcolormask          sg_bg_ctl               tsort
 byobu-disable                        ftp                       lsmem                              pamundice                   ppmcolors             sg_compare_and_write    tty
 byobu-disable-prompt                 fuser                     lsmod                              pamunlookup                 ppmdcfont             sg_copy_results         turbostat
 byobu-enable                         fusermount                lsns                               pamvalidate                 ppmddumpfont          sg_dd                   twist3
 byobu-enable-prompt                  fusermount3               lsof                               pamwipeout                  ppmdim                sg_decode_sense         twistd3
 byobu-export                         fwupdmgr                  lspci                              pamx                        ppmdist               sg_emc_trespass         tzselect
 byobu-janitor                        fwupdtool                 lspgpot                            paperconf                   ppmdither             sg_format               ua
 byobu-keybindings                    g3topbm                   lspower                            partx                       ppmdmkfont            sg_get_config           ubuntu-advantage
 byobu-launch                         gapplication              lsusb                              passwd                      ppmdraw               sg_get_elem_status      ubuntu-bug
 byobu-launcher                       gawk                      lzcat                              paste                       ppmfade               sg_get_lba_status       ubuntu-distro-info
 byobu-launcher-install               gawkbug                   lzcmp                              pastebinit                  ppmflash              sg_ident                ubuntu-drivers
 byobu-launcher-uninstall             gdbus                     lzdiff                             patch                       ppmforge              sginfo                  ubuntu-security-status
 byobu-layout                         gdk-pixbuf-csource        lzegrep                            pathchk                     ppmglobe              sg_inq                  ucf
 byobu-prompt                         gdk-pixbuf-pixdata        lzfgrep                            pbget                       ppmhist               sgitopnm                ucfq
 byobu-quiet                          gdk-pixbuf-thumbnailer    lzgrep                             pbmclean                    ppmlabel              sg_logs                 ucfr
 byobu-reconnect-sockets              gemtopbm                  lzless                             pbmlife                     ppmmake               sg_luns                 uclampset
 byobu-screen                         gemtopnm                  lzma                               pbmmake                     ppmmix                sg_map                  ucs2any
 byobu-select-backend                 gencat                    lzmainfo                           pbmmask                     ppmnorm               sg_map26                udevadm
 byobu-select-profile                 geqn                      lzmore                             pbmminkowski                ppmntsc               sgm_dd                  udisksctl
 byobu-select-session                 getconf                   macptopbm                          pbmnoise                    ppmpat                sg_modes                ul
 byobu-shell                          getent                    mailmail3                          pbmpage                     ppmquant              sg_opcodes              umount
 byobu-silent                         getkeycodes               man                                pbmpscale                   ppmquantall           sgp_dd                  uname
 byobu-status                         getopt                    mandb                              pbmreduce                   ppmrainbow            sg_persist              unattended-upgrade
 byobu-status-detail                  gettext                   manifest                           pbmtext                     ppmrelief             sg_prevent              unattended-upgrades
 byobu-tmux                           gettext.sh                manpath                            pbmtextps                   ppmrough              sg_raw                  uncompress
 byobu-ugraph                         ghostscript               man-recode                         pbmto10x                    ppmshadow             sg_rbuf                 unexpand
 byobu-ulevel                         giftopnm                  mapscrn                            pbmto4425                   ppmshift              sg_rdac                 unicode_start
 cacaclock                            ginstall-info             markdown-it                        pbmtoascii                  ppmspread             sg_read                 unicode_stop
 cacademo                             gio                       mawk                               pbmtoatk                    ppmtoacad             sg_read_attr            uniq
 cacafire                             gio-querymodules          mbimcli                            pbmtobbnbg                  ppmtoapplevol         sg_read_block_limits    unlink
 cacaplay                             git                       mbim-network                       pbmtocis                    ppmtoarbtxt           sg_read_buffer          unlzma
 cacaserver                           git-receive-pack          mcookie                            pbmtocmuwm                  ppmtoascii            sg_readcap              unminimize
 cacaview                             git-shell                 md5sum                             pbmtodjvurle                ppmtobmp              sg_read_long            unmkinitramfs
 cameratopam                          git-upload-archive        md5sum.textutils                   pbmtoepsi                   ppmtoeyuv             sg_reassign             unshare
 captoinfo                            git-upload-pack           mdatopbm                           pbmtoepson                  ppmtogif              sg_referrals            unsquashfs
 cat                                  glib-compile-schemas      mdig                               pbmtoescp2                  ppmtoicr              sg_rep_pip              unxz
 catman                               gouldtoppm                memhog                             pbmtog3                     ppmtoilbm             sg_rep_zones            unzstd
 cftp3                                gpasswd                   memusage                           pbmtogem                    ppmtojpeg             sg_requests             update-alternatives
 chafa                                gpg                       memusagestat                       pbmtogo                     ppmtoleaf             sg_reset                update-mime-database
 chage                                gpg-agent                 mesg                               pbmtoibm23xx                ppmtolj               sg_reset_wp             upower
 chardet                              gpgconf                   mgrtopbm                           pbmtoicon                   ppmtomap              sg_rmsn                 uptime
 chardetect                           gpg-connect-agent         migratepages                       pbmtolj                     ppmtomitsu            sg_rtpg                 usb-devices
 chattr                               gpgparsemail              migrate-pubring-from-classic-gpg   pbmtoln03                   ppmtompeg             sg_safte                usbhid-dump
 chcon                                gpgsm                     migspeed                           pbmtolps                    ppmtoneo              sg_sanitize             usbip
 chfn                                 gpgsplit                  mkdir                              pbmtomacp                   ppmtopcx              sg_sat_identify         usbipd
 chgrp                                gpgtar                    mkfifo                             pbmtomatrixorbital          ppmtopgm              sg_sat_phy_event        usbreset
 chmod                                gpgv                      mkfontdir                          pbmtomda                    ppmtopi1              sg_sat_read_gplog       users
 choom                                gpg-wks-client            mkfontscale                        pbmtomgr                    ppmtopict             sg_sat_set_features     utmpdump
 chown                                gpic                      mk_modmap                          pbmtomrf                    ppmtopj               sg_scan                 uuidgen
 chrt                                 gpu-manager               mknod                              pbmtonokia                  ppmtopjxl             sg_seek                 uuidparse
 chsh                                 grep                      mksquashfs                         pbmtopgm                    ppmtoppm              sg_senddiag             varlinkctl
 chvt                                 gresource                 mktemp                             pbmtopi3                    ppmtopuzz             sg_ses                  vcs-run
 cifsiostat                           groff                     mmcli                              pbmtopk                     ppmtorgb3             sg_ses_microcode        vdir
 cistopbm                             grog                      mogrify                            pbmtoplot                   ppmtosixel            sg_start                VGAuthService
 ckbcomp                              grops                     mogrify-im6                        pbmtoppa                    ppmtospu              sg_stpg                 vi
 ckeygen3                             grotty                    mogrify-im6.q16                    pbmtopsg3                   ppmtoterm             sg_stream_ctl           view
 cksum                                groups                    montage                            pbmtoptx                    ppmtotga              sg_sync                 vigpg
 clear                                growpart                  montage-im6                        pbmtosunicon                ppmtouil              sg_test_rwbuf           vim
 clear_console                        grub-editenv              montage-im6.q16                    pbmtowbmp                   ppmtowinicon          sg_timestamp            vim.basic
 cloud-id                             grub-file                 more                               pbmtox10bm                  ppmtoxpm              sg_turs                 vimdiff
 cloud-init                           grub-fstest               mount                              pbmtoxbm                    ppmtoyuv              sg_unmap                vim.tiny
 cloud-init-per                       grub-glue-efi             mountpoint                         pbmtoybm                    ppmtoyuvsplit         sg_verify               vimtutor
 cmp                                  grub-kbdcomp              mpstat                             pbmtozinc                   ppmtv                 sg_vpd                  vmhgfs-fuse
 cmuwmtopbm                           grub-menulst2cfg          mrftopbm                           pbmupc                      ppmwheel              sg_write_buffer         vmstat
 codepage                             grub-mkfont               mt                                 pbput                       pr                    sg_write_long           vm-support
 col                                  grub-mkimage              mt-gnu                             pbputs                      preconv               sg_write_same           vmtoolsd
 col1                                 grub-mklayout             mtr                                pc1toppm                    printafm              sg_write_verify         vmware-alias-import
 col2                                 grub-mknetdir             mtrace                             pcdindex                    printenv              sg_write_x              vmware-checkvm
 col3                                 grub-mkpasswd-pbkdf2      mtr-packet                         pcdovtoppm                  printf                sg_wr_mode              vmware-hgfsclient
 col4                                 grub-mkrelpath            mtvtoppm                           pcxtoppm                    prlimit               sg_xcopy                vmware-namespace-cmd
 col5                                 grub-mkrescue             mv                                 pdb3                        pro                   sg_zone                 vmware-rpctool
 col6                                 grub-mkstandalone         namei                              pdb3.12                     prove                 sh                      vmware-toolbox-cmd
 col7                                 grub-mount                nano                               pdbimgtopam                 prtstat               sha1sum                 vmware-vgauth-cmd
 col8                                 grub-ntldr-img            nawk                               pdf2dsc                     ps                    sha224sum               vmware-vmblock-fuse
 col9                                 grub-render-label         nc                                 pdf2ps                      ps2ascii              sha256sum               vmware-xferlogs
 colcrt                               grub-script-check         nc.openbsd                         peekfd                      ps2epsi               sha384sum               w
 colrm                                grub-syslinux2cfg         neofetch                           perf                        ps2pdf                sha512sum               w3m
 column                               gs                        neotoppm                           perl                        ps2pdf12              shasum                  w3mman
 comm                                 gsbj                      neqn                               perl5.38.2                  ps2pdf13              showconsolefont         wall
 compare                              gsdj                      netaddr                            perl5.38-x86_64-linux-gnu   ps2pdf14              showkey                 watch
 compare-im6                          gsdj500                   netcat                             perlbug                     ps2pdfwr              shred                   watchgnupg
 compare-im6.q16                      gsettings                 networkctl                         perldoc                     ps2ps                 shuf                    wbmptopbm
 composite                            gslj                      networkd-dispatcher                perlivp                     ps2ps2                sirtopnm                wc
 composite-im6                        gslp                      newgrp                             perlthanks                  ps2txt                sixel2png               wdctl
 composite-im6.q16                    gsnd                      NF                                 pf2afm                      psfaddtable           skill                   wget
 conch3                               gtbl                      ngettext                           pfbtopfa                    psfgettable           slabtop                 whatis
 conjure                              gunzip                    nice                               pfmtopam                    psfstriptable         sldtoppm                whereis
 conjure-im6                          gzexe                     nisdomainname                      pgmabel                     psfxtable             sleep                   which
 conjure-im6.q16                      gzip                      nl                                 pgmbentley                  psidtopgm             slogin                  which.debianutils
 convert                              h2ph                      nohup                              pgmcrater                   pslog                 snap                    whiptail
 convert-im6                          h2xs                      nproc                              pgmdeshadow                 pstopnm               snapctl                 who
 convert-im6.q16                      hardlink                  nroff                              pgmedge                     pstree                snapfuse                whoami
 corelist                             hd                        nsenter                            pgmenhance                  pstree.x11            snice                   wifi-status
 cp                                   hdifftopam                nslookup                           pgmhist                     ptar                  soelim                  winicontopam
 cpan                                 head                      nstat                              pgmkernel                   ptardiff              sort                    winicontoppm
 cpan5.38-x86_64-linux-gnu            helpztags                 nsupdate                           pgmmake                     ptargrep              sos                     write
 cpio                                 hexdump                   ntfs-3g                            pgmmedian                   ptx                   sos-collector           www-browser
 cpupower                             hipstopgm                 ntfs-3g.probe                      pgmminkowski                purge-old-kernels     sosreport               X11
 c_rehash                             host                      ntfscat                            pgmmorphconv                pwd                   sotruss                 x86_64
 crontab                              hostid                    ntfscluster                        pgmnoise                    pwdx                  spctoppm                x86_energy_perf_policy
 csplit                               hostname                  ntfscmp                            pgmnorm                     py3clean              splain                  xargs
 ctail                                hostnamectl               ntfsdecrypt                        pgmoil                      py3compile            split                   xauth
 ctstat                               hpcdtoppm                 ntfsfallocate                      pgmramp                     py3versions           splitfont               xbmtopbm
 curl                                 htop                      ntfsfix                            pgmslice                    pybabel               spottopgm               xdg-user-dir
 cut                                  hwe-support-status        ntfsinfo                           pgmtexture                  pybabel-python3       sprof                   xdg-user-dirs-update
 cvtsudoers                           i386                      ntfsls                             pgmtofs                     pydoc3                sputoppm                ximtoppm
 dash                                 icontopbm                 ntfsmove                           pgmtolispm                  pydoc3.12             sqfscat                 xpmtoppm
 date                                 iconv                     ntfsrecover                        pgmtopbm                    pygettext3            sqfstar                 xsubpp
 dbus-cleanup-sockets                 id                        ntfssecaudit                       pgmtopgm                    pygettext3.12         srftopam                xvminitoppm
 dbus-daemon                          identify                  ntfstruncate                       pgmtoppm                    pygmentize            ss                      xwdtopnm
 dbus-monitor                         identify-im6              ntfsusermap                        pgmtosbig                   pyhtmlizer3           ssh                     xxd
 dbus-run-session                     identify-im6.q16          ntfswipe                           pgmtost4                    pyserial-miniterm     ssh-add                 xz
 dbus-send                            ilbmtoppm                 numactl                            pgrep                       pyserial-ports        ssh-agent               xzcat
 dbus-update-activation-environment   imagetops                 numastat                           pi1toppm                    python3               ssh-argv0               xzcmp
 dbus-uuidgen                         img2sixel                 numfmt                             pi3topbm                    python3.12            ssh-copy-id             xzdiff
 dbxtool                              img2txt                   nvidia-detector                    pic                         pzstd                 ssh-import-id           xzegrep
 dd                                   imgtoppm                  od                                 pico                        qmicli                ssh-import-id-gh        xzfgrep
 ddbugtopbm                           import                    oem-getlogs                        piconv                      qmi-firmware-update   ssh-import-id-lp        xzgrep
 deallocvt                            import-im6                on_ac_power                        pidof                       qmi-network           ssh-keygen              xzless
 debconf                              import-im6.q16            openssl                            pidstat                     qoitopam              ssh-keyscan             xzmore
 debconf-apt-progress                 inetutils-telnet          openvt                             pidwait                     qrttoppm              st4topgm                ybmtopbm
 debconf-communicate                  info                      os-prober                          pinentry                    quirks-handler        stat                    yes
 debconf-copydb                       infobrowser               pager                              pinentry-curses             rasttopnm             static-sh               ypdomainname
 debconf-escape                       infocmp                   palmtopnm                          ping                        rawtopgm              stdbuf                  yuvsplittoppm
 debconf-set-selections               infotocap                 pamaddnoise                        ping4                       rawtoppm              strace                  yuvtoppm
 debconf-show                         infotopam                 pamaltsat                          ping6                       rbash                 strace-log-merge        yuy2topam
 debian-distro-info                   install                   pamarith                           pinky                       rdma                  stream                  zcat
 deb-systemd-helper                   install-info              pambackground                      pjtoppm                     readlink              stream-im6              zcmp
 deb-systemd-invoke                   instmodsh                 pambayer                           pkaction                    realpath              stream-im6.q16          zdiff
 delpart                              ionice                    pambrighten                        pkcheck                     red                   streamzip               zdump
 delv                                 iostat                    pamcat                             pkcon                       rename.ul             stty                    zegrep
 df                                   ip                        pamchannel                         pkill                       renice                su                      zeisstopnm
 dh_bash-completion                   ipcmk                     pamcomp                            pkmon                       rescan-scsi-bus.sh    sudo                    zfgrep
 dh_installxmlcatalogs                ipcrm                     pamcrater                          pktopbm                     reset                 sudoedit                zforce
 diff                                 ipcs                      pamcut                             pkttyagent                  resizecons            sudoreplay              zgrep
 diff3                                iptables-xml              pamdeinterlace                     pl2pm                       resizepart            sum                     zipdetails
 dig                                  ischroot                  pamdepth                           pldd                        resolvectl            sunicontopnm            zless
 dir                                  iscsiadm                  pamdice                            plymouth                    rev                   svgtopam                zmore
 dircolors                            jbigtopnm                 pamditherbw                        pmap                        rgb3toppm             sync                    znew
 dirmngr                              join                      pamedge                            pngtopam                    rgrep                 systemctl               zstd
 dirmngr-client                       journalctl                pamendian                          pngtopnm                    rlatopam              systemd                 zstdcat
 dirname                              jp2a                      pamenlarge                         pnmalias                    rletopnm              systemd-ac-power        zstdgrep
 display                              jpeg2ktopam               pamexec                            pnmarith                    rm                    systemd-analyze         zstdless
 display-im6                          jpegtopnm                 pamfile                            pnmcat                      rmdir                 systemd-ask-password    zstdmt
```

* /usr/bin
```bash
root@UbuntuServer:/# cd /usr/bin/
root@UbuntuServer:/usr/bin# pwd
/usr/bin
root@UbuntuServer:/usr/bin# ls -l | more
total 134152
-rwxr-xr-x 1 root root       55744 Jun 22  2025 [
-rwxr-xr-x 1 root root       14640 Mar 31  2024 411toppm
-rwxr-xr-x 1 root root       18744 Aug 15  2025 aa-enabled
-rwxr-xr-x 1 root root       18744 Aug 15  2025 aa-exec
-rwxr-xr-x 1 root root       18736 Aug 15  2025 aa-features-abi
-rwxr-xr-x 1 root root        1622 Jan 13 13:56 acpidbg
-rwxr-xr-x 1 root root       16422 Jul  2  2025 add-apt-repository
-rwxr-xr-x 1 root root       14720 Sep 16 00:08 addpart
lrwxrwxrwx 1 root root          25 Mar 31  2024 animate -> /etc/alternatives/animate
lrwxrwxrwx 1 root root          29 Mar 31  2024 animate-im6 -> /etc/alternatives/animate-im6
-rwxr-xr-x 1 root root       14656 Mar 31  2024 animate-im6.q16
-rwxr-xr-x 1 root root       12556 Mar 31  2024 anytopnm
-rwxr-xr-x 1 root root        2322 Apr 18  2024 apport-bug
-rwxr-xr-x 1 root root       13625 Jul  8  2025 apport-cli
lrwxrwxrwx 1 root root          10 Jul  8  2025 apport-collect -> apport-bug
-rwxr-xr-x 1 root root        3790 Jul  8  2025 apport-unpack
-rwxr-xr-x 1 root root      141544 Apr  8  2024 appstreamcli
lrwxrwxrwx 1 root root           6 Aug  5  2025 apropos -> whatis
-rwxr-xr-x 1 root root       18824 Oct 22  2024 apt
lrwxrwxrwx 1 root root          18 Jul  2  2025 apt-add-repository -> add-apt-repository
-rwxr-xr-x 1 root root       88544 Oct 22  2024 apt-cache
-rwxr-xr-x 1 root root       27104 Oct 22  2024 apt-cdrom
-rwxr-xr-x 1 root root       31120 Oct 22  2024 apt-config
-rwxr-xr-x 1 root root       23008 Aug  5  2025 apt-extracttemplates
-rwxr-xr-x 1 root root      227816 Aug  5  2025 apt-ftparchive
-rwxr-xr-x 1 root root       51680 Oct 22  2024 apt-get
-rwxr-xr-x 1 root root       28664 Oct 22  2024 apt-key
-rwxr-xr-x 1 root root       55776 Oct 22  2024 apt-mark
-rwxr-xr-x 1 root root       51608 Aug  5  2025 apt-sortpkgs
-rwxr-xr-x 1 root root       35336 Jun 22  2025 arch
-rwxr-xr-x 1 root root       14640 Mar 31  2024 asciitopgm
-rwxr-xr-x 1 root root       14640 Mar 31  2024 atktopbm
-rwxr-xr-x 1 root root         984 Aug  5  2025 automat-visualize3
-rwxr-xr-x 1 root root       14640 Mar 31  2024 avstopam
lrwxrwxrwx 1 root root          21 Apr  8  2024 awk -> /etc/alternatives/awk
-rwxr-xr-x 1 root root       55816 Jun 22  2025 b2sum
-rwxr-xr-x 1 root root       39432 Jun 22  2025 base32
-rwxr-xr-x 1 root root       39432 Jun 22  2025 base64
-rwxr-xr-x 1 root root       35336 Jun 22  2025 basename
-rwxr-xr-x 1 root root       47624 Jun 22  2025 basenc
-rwxr-xr-x 1 root root     1446024 Mar 31  2024 bash
-rwxr-xr-x 1 root root        6988 Mar 31  2024 bashbug
-rwxr-xr-x 1 root root       93000 Aug  5  2025 bc
-rwxr-xr-x 1 root root       43272 Apr  8  2024 bdftopcf
-rwxr-xr-x 1 root root       14488 Apr  8  2024 bdftruncate
-rwxr-xr-x 1 root root       14640 Mar 31  2024 bioradtopgm
-rwxr-xr-x 1 root root       26928 Mar 31  2024 bmptopnm
lrwxrwxrwx 1 root root           8 Mar 31  2024 bmptoppm -> bmptopnm
-rwxr-xr-x 1 root root      125792 Aug  5  2025 boltctl
-rwxr-xr-x 1 root root     2250576 Feb  5  2025 bpftrace
-rwxr-xr-x 1 root root     1009016 Feb  5  2025 bpftrace-aotrt
-rwxr-xr-x 1 root root       14640 Mar 31  2024 brushtopbm
```

* /sbin
```bash
root@UbuntuServer:/# cd /sbin
root@UbuntuServer:/sbin# pwd
/sbin
root@UbuntuServer:/sbin# ls -l | more
total 33140
-rwxr-xr-x 1 root root     39680 Aug 15  2025 aa-load
-rwxr-xr-x 1 root root      3225 Aug 15  2025 aa-remove-unknown
-rwxr-xr-x 1 root root     40000 Aug 15  2025 aa-status
-rwxr-xr-x 1 root root       137 Apr 12  2024 aa-teardown
-rwxr-xr-x 1 root root     14904 Aug  5  2025 accessdb
-rwxr-xr-x 1 root root      3075 Jan  5 22:01 addgnupghome
lrwxrwxrwx 1 root root         7 Jul  5  2023 addgroup -> adduser
-rwxr-xr-x 1 root root      1053 Mar 31  2024 add-shell
-rwxr-xr-x 1 root root     55191 Jul  5  2023 adduser
-rwxr-xr-x 1 root root     60992 Sep 16 00:08 agetty
-rwxr-xr-x 1 root root   1629848 Aug 15  2025 apparmor_parser
lrwxrwxrwx 1 root root         9 Aug 15  2025 apparmor_status -> aa-status
-rwxr-xr-x 1 root root      2217 Jan  5 22:01 applygnupgdefaults
-rwxr-xr-x 1 root root     36862 Apr 16  2024 argdist-bpfcc
-rwxr-xr-x 1 root root     26960 Jul 10  2025 arpd
lrwxrwxrwx 1 root root        27 Aug  5  2025 arptables -> /etc/alternatives/arptables
lrwxrwxrwx 1 root root        17 Aug  5  2025 arptables-nft -> xtables-nft-multi
lrwxrwxrwx 1 root root        17 Aug  5  2025 arptables-nft-restore -> xtables-nft-multi
lrwxrwxrwx 1 root root        17 Aug  5  2025 arptables-nft-save -> xtables-nft-multi
lrwxrwxrwx 1 root root        35 Aug  5  2025 arptables-restore -> /etc/alternatives/arptables-restore
lrwxrwxrwx 1 root root        32 Aug  5  2025 arptables-save -> /etc/alternatives/arptables-save
-rwxr-xr-x 1 root root     35144 Apr 28  2024 badblocks
-rwxr-xr-x 1 root root      2380 Apr 16  2024 bashreadline-bpfcc
-rwxr-xr-x 1 root root       698 Mar  7  2024 bashreadline.bt
-rwxr-xr-x 1 root root     14648 Apr  8  2024 bcache-super-show
-rwxr-xr-x 1 root root     16346 Apr 16  2024 bindsnoop-bpfcc
-rwxr-xr-x 1 root root     11365 Apr 16  2024 biolatency-bpfcc
-rwxr-xr-x 1 root root       681 Mar  7  2024 biolatency.bt
-rwxr-xr-x 1 root root       664 Mar  7  2024 biolatency-kp.bt
-rwxr-xr-x 1 root root     10248 Apr 16  2024 biolatpcts-bpfcc
-rwxr-xr-x 1 root root      3957 Apr 16  2024 biopattern-bpfcc
-rwxr-xr-x 1 root root     27856 Aug  5  2025 biosdecode
-rwxr-xr-x 1 root root     10833 Apr 16  2024 biosnoop-bpfcc
-rwxr-xr-x 1 root root      1148 Mar  7  2024 biosnoop.bt
-rwxr-xr-x 1 root root       915 Mar  7  2024 biostacks.bt
-rwxr-xr-x 1 root root      9567 Apr 16  2024 biotop-bpfcc
-rwxr-xr-x 1 root root      1166 Apr 16  2024 bitesize-bpfcc
-rwxr-xr-x 1 root root       567 Mar  7  2024 bitesize.bt
-rwxr-xr-x 1 root root     16351 Nov 27  2024 blkdeactivate
-rwxr-xr-x 1 root root     22912 Sep 16 00:08 blkdiscard
-rwxr-xr-x 1 root root     55720 Sep 16 00:08 blkid
-rwxr-xr-x 1 root root     35200 Sep 16 00:08 blkzone
-rwxr-xr-x 1 root root     35200 Sep 16 00:08 blockdev
-rwxr-xr-x 1 root root      2601 Apr 16  2024 bpflist-bpfcc
-rwxr-xr-x 1 root root      1622 Jan 13 13:56 bpftool
-rwxr-xr-x 1 root root    111096 Jul 10  2025 bridge
-rwxr-xr-x 1 root root      6627 Apr 16  2024 btrfsdist-bpfcc
-rwxr-xr-x 1 root root      9985 Apr 16  2024 btrfsslower-bpfcc
lrwxrwxrwx 1 root root        11 Jul  1  2024 cache_check -> pdata_tools
lrwxrwxrwx 1 root root        11 Jul  1  2024 cache_dump -> pdata_tools
lrwxrwxrwx 1 root root        11 Jul  1  2024 cache_metadata_size -> pdata_tools
lrwxrwxrwx 1 root root        11 Jul  1  2024 cache_repair -> pdata_tools
```

* /tmp
```bash
root@UbuntuServer:/# cd /tmp/
root@UbuntuServer:/tmp# pwd
/tmp
root@UbuntuServer:/tmp# ls -l | more
total 32
drwx------ 2 root root 4096 Mar  8 09:40 snap-private-tmp
drwx------ 3 root root 4096 Mar  8 09:43 systemd-private-8e2adb585c014011afb6858f69caff11-fwupd.service-7RsiQX
drwx------ 3 root root 4096 Mar  8 09:40 systemd-private-8e2adb585c014011afb6858f69caff11-ModemManager.service-d9WdOY
drwx------ 3 root root 4096 Mar  8 09:40 systemd-private-8e2adb585c014011afb6858f69caff11-polkit.service-eQap6A
drwx------ 3 root root 4096 Mar  8 09:40 systemd-private-8e2adb585c014011afb6858f69caff11-systemd-logind.service-LYaP0I
drwx------ 3 root root 4096 Mar  8 09:40 systemd-private-8e2adb585c014011afb6858f69caff11-systemd-resolved.service-oW3ShW
drwx------ 3 root root 4096 Mar  8 09:40 systemd-private-8e2adb585c014011afb6858f69caff11-systemd-timesyncd.service-TSgYBN
drwx------ 3 root root 4096 Mar  8 09:43 systemd-private-8e2adb585c014011afb6858f69caff11-upower.service-oYCqzc
```

* /boot
```bash
root@UbuntuServer:/# cd /boot/
root@UbuntuServer:/boot# pwd
/boot
root@UbuntuServer:/boot# ls -l | more
total 96788
-rw-r--r-- 1 root root   287599 Jan 13 13:56 config-6.8.0-100-generic
drwxr-xr-x 5 root root     4096 Feb 24 09:33 grub
lrwxrwxrwx 1 root root       28 Feb 24 09:33 initrd.img -> initrd.img-6.8.0-100-generic
-rw-r--r-- 1 root root 74658699 Feb 24 09:54 initrd.img-6.8.0-100-generic
lrwxrwxrwx 1 root root       28 Feb 24 09:33 initrd.img.old -> initrd.img-6.8.0-100-generic
-rw------- 1 root root  9120274 Jan 13 13:56 System.map-6.8.0-100-generic
lrwxrwxrwx 1 root root       25 Feb 24 09:33 vmlinuz -> vmlinuz-6.8.0-100-generic
-rw------- 1 root root 15030664 Jan 13 14:42 vmlinuz-6.8.0-100-generic
lrwxrwxrwx 1 root root       25 Feb 24 09:33 vmlinuz.old -> vmlinuz-6.8.0-100-generic
```


3. Telusuri direktory /dev. Identifikasi perangkat yang tersedia. Identifikasi tty (termninal) Anda (ketik who am i); siapa pemilih tty Anda (gunakan ls –l).  
```bash
root@UbuntuServer:/# cd /dev/
root@UbuntuServer:/dev# ls
autofs         cpu_dma_latency  hidraw0    loop1         mem     random  snapshot  tty11  tty20  tty3   tty39  tty48  tty57  tty9       ttyS16  ttyS25  ttyS6        vboxguest  vcsa1  vcsu4
block          cuse             hpet       loop2         mqueue  rfkill  snd       tty12  tty21  tty30  tty4   tty49  tty58  ttyprintk  ttyS17  ttyS26  ttyS7        vboxuser   vcsa2  vcsu5
bsg            disk             hugepages  loop3         net     rtc     sr0       tty13  tty22  tty31  tty40  tty5   tty59  ttyS0      ttyS18  ttyS27  ttyS8        vcs        vcsa3  vcsu6
btrfs-control  dma_heap         hwrng      loop4         null    rtc0    stderr    tty14  tty23  tty32  tty41  tty50  tty6   ttyS1      ttyS19  ttyS28  ttyS9        vcs1       vcsa4  vfio
bus            dri              i2c-0      loop5         nvram   sda     stdin     tty15  tty24  tty33  tty42  tty51  tty60  ttyS10     ttyS2   ttyS29  udmabuf      vcs2       vcsa5  vga_arbiter
cdrom          ecryptfs         initctl    loop6         port    sda1    stdout    tty16  tty25  tty34  tty43  tty52  tty61  ttyS11     ttyS20  ttyS3   uhid         vcs3       vcsa6  vhci
char           fb0              input      loop7         ppp     sda2    tty       tty17  tty26  tty35  tty44  tty53  tty62  ttyS12     ttyS21  ttyS30  uinput       vcs4       vcsu   vhost-net
console        fd               kmsg       loop-control  psaux   sg0     tty0      tty18  tty27  tty36  tty45  tty54  tty63  ttyS13     ttyS22  ttyS31  urandom      vcs5       vcsu1  vhost-vsock
core           full             log        mapper        ptmx    sg1     tty1      tty19  tty28  tty37  tty46  tty55  tty7   ttyS14     ttyS23  ttyS4   userfaultfd  vcs6       vcsu2  zero
cpu            fuse             loop0      mcelog        pts     shm     tty10     tty2   tty29  tty38  tty47  tty56  tty8   ttyS15     ttyS24  ttyS5   userio       vcsa       vcsu3  zfs
root@UbuntuServer:/dev# who am i
fauzi    pts/2        2026-03-08 09:42 (192.168.88.139)
root@UbuntuServer:/dev# ls -l /dev/tty1
crw------- 1 fauzi tty 4, 1 Mar  8 09:41 /dev/tty1
```


4. Telusuri derectory /proc. Tampilkan isi file interrupts, devices, cpuinfo, meminfo dan uptime menggunakan perintah cat. Dapatkah Anda melihat mengapa directory /proc disebut pseudo -filesystem yang memungkinkan akses ke struktur data kernel ?  

Command:
```bash
cd /proc
cat cpuinfo
cat meminfo
cat uptime
```

Output:
```bash
root@UbuntuServer:/# cd /proc/
root@UbuntuServer:/proc# cat cpuinfo
processor       : 0
vendor_id       : GenuineIntel
cpu family      : 6
model           : 140
model name      : 11th Gen Intel(R) Core(TM) i5-1135G7 @ 2.40GHz
stepping        : 1
microcode       : 0xffffffff
cpu MHz         : 2419.208
cache size      : 8192 KB
physical id     : 0
siblings        : 2
core id         : 0
cpu cores       : 2
apicid          : 0
initial apicid  : 0
fpu             : yes
fpu_exception   : yes
cpuid level     : 22
wp              : yes
flags           : fpu vme de pse tsc msr pae mce cx8 apic sep mtrr pge mca cmov pat pse36 clflush mmx fxsr sse sse2 ht syscall nx rdtscp lm constant_tsc rep_good nopl xtopology nonstop_tsc cpuid tsc_known_freq pni pclmulqdq ssse3 fma cx16 pcid sse4_1 sse4_2 movbe popcnt aes xsave avx f16c rdrand hypervisor lahf_lm abm 3dnowprefetch ibrs_enhanced fsgsbase bmi1 avx2 bmi2 invpcid rdseed adx clflushopt sha_ni arat md_clear flush_l1d arch_capabilities
bugs            : spectre_v1 spectre_v2 spec_store_bypass swapgs retbleed eibrs_pbrsb gds bhi its its_native_only
bogomips        : 4838.41
clflush size    : 64
cache_alignment : 64
address sizes   : 39 bits physical, 48 bits virtual
power management:

processor       : 1
vendor_id       : GenuineIntel
cpu family      : 6
model           : 140
model name      : 11th Gen Intel(R) Core(TM) i5-1135G7 @ 2.40GHz
stepping        : 1
microcode       : 0xffffffff
cpu MHz         : 2419.208
cache size      : 8192 KB
physical id     : 0
siblings        : 2
core id         : 1
cpu cores       : 2
apicid          : 1
initial apicid  : 1
fpu             : yes
fpu_exception   : yes
cpuid level     : 22
wp              : yes
flags           : fpu vme de pse tsc msr pae mce cx8 apic sep mtrr pge mca cmov pat pse36 clflush mmx fxsr sse sse2 ht syscall nx rdtscp lm constant_tsc rep_good nopl xtopology nonstop_tsc cpuid tsc_known_freq pni pclmulqdq ssse3 fma cx16 pcid sse4_1 sse4_2 movbe popcnt aes xsave avx f16c rdrand hypervisor lahf_lm abm 3dnowprefetch ibrs_enhanced fsgsbase bmi1 avx2 bmi2 invpcid rdseed adx clflushopt sha_ni arat md_clear flush_l1d arch_capabilities
bugs            : spectre_v1 spectre_v2 spec_store_bypass swapgs retbleed eibrs_pbrsb gds bhi its its_native_only
bogomips        : 4838.41
clflush size    : 64
cache_alignment : 64
address sizes   : 39 bits physical, 48 bits virtual
power management:

root@UbuntuServer:/proc# cat meminfo
MemTotal:        3911656 kB
MemFree:         2849264 kB
MemAvailable:    3409404 kB
Buffers:           26368 kB
Cached:           724760 kB
SwapCached:            0 kB
Active:           287648 kB
Inactive:         558604 kB
Active(anon):     104996 kB
Inactive(anon):        0 kB
Active(file):     182652 kB
Inactive(file):   558604 kB
Unevictable:       27316 kB
Mlocked:           27316 kB
SwapTotal:       4194300 kB
SwapFree:        4194300 kB
Zswap:                 0 kB
Zswapped:              0 kB
Dirty:              1688 kB
Writeback:             0 kB
AnonPages:        122484 kB
Mapped:           146708 kB
Shmem:              1080 kB
KReclaimable:      42204 kB
Slab:              89692 kB
SReclaimable:      42204 kB
SUnreclaim:        47488 kB
KernelStack:        2496 kB
PageTables:         3888 kB
SecPageTables:         0 kB
NFS_Unstable:          0 kB
Bounce:                0 kB
WritebackTmp:          0 kB
CommitLimit:     6150128 kB
Committed_AS:     630192 kB
VmallocTotal:   34359738367 kB
VmallocUsed:       21436 kB
VmallocChunk:          0 kB
Percpu:             1024 kB
HardwareCorrupted:     0 kB
AnonHugePages:         0 kB
ShmemHugePages:        0 kB
ShmemPmdMapped:        0 kB
FileHugePages:         0 kB
FilePmdMapped:         0 kB
Unaccepted:            0 kB
HugePages_Total:       0
HugePages_Free:        0
HugePages_Rsvd:        0
HugePages_Surp:        0
Hugepagesize:       2048 kB
Hugetlb:               0 kB
DirectMap4k:      108480 kB
DirectMap2M:     3987456 kB
root@UbuntuServer:/proc# cat uptime
10113.76 19967.58
```

Alasan kenapa disebut pseudo-filesystem, karena tidak benar-benar menyimpan file di disk, dibuat langsung oleh kernel, dan isinya data real-time sistem

5. Ubahlah direktory home ke user lain secara langsung menggunakan cd ~username.
```bash
root@UbuntuServer:/# cd ~fauzi
root@UbuntuServer:/home/fauzi#
```

6. Ubah kembali ke direktory home Anda.
```bash
root@UbuntuServer:/home/fauzi# cd
root@UbuntuServer:~#
```

7. Buat subdirektory work dan play.
```bash
root@UbuntuServer:~# mkdir work play
root@UbuntuServer:~# ls
play  praktikum-os  vboxpostinstall.sh  work
root@UbuntuServer:~#
```

8. Hapus subdirektory work
```bash
root@UbuntuServer:~# rmdir work
root@UbuntuServer:~# ls
play  praktikum-os  vboxpostinstall.sh
root@UbuntuServer:~#
```

9. Copy file /etc/passwd ke direktory home Anda.
```bash
root@UbuntuServer:~# cp /etc/passwd ~
root@UbuntuServer:~#
```

10. Pindahkan ke subirectory play.
```bash
root@UbuntuServer:~# mv passwd play/
root@UbuntuServer:~#
```

11. Ubahlah ke subdirektory play dan buat symbolic link dengan nama terminal yang menunjuk ke perangkat tty. Apa yang terjadi jika melakukan hard link ke perangkat tty ?  
```bash
root@UbuntuServer:~# cd play
root@UbuntuServer:~/play# ln -s /dev/tty terminal
root@UbuntuServer:~/play# ls -l terminal
lrwxrwxrwx 1 root root 8 Mar  8 12:51 terminal -> /dev/tty
root@UbuntuServer:~/play# ln -s /dev/pts/0 terminal
ln: failed to create symbolic link 'terminal': File exists
root@UbuntuServer:~/play#
```

Biasanya gagal, karena hard link tidak bisa dibuat lintas filesystem/partisi, sedangkan home dan /dev umumnya berada pada filesystem yang berbeda. Pada sistem modern, bisa juga gagal karena pembatasan keamanan.

12. Buatlah file bernama hello.txt yang berisi kata ”hello word”. Dapatkah Anda gunakan ”cp” menggunakan ”terminal” sebagai file asal untuk menghasilkan efek yang sama ?  
```bash
root@UbuntuServer:~/play# echo "hello word" > hello.txt
root@UbuntuServer:~/play# cp terminal hello2.txt
hello world
root@UbuntuServer:~/play# cat hello.txt hello2.txt
hello word
hello world
root@UbuntuServer:~/play#
```

Apakah bisa memakai cp dengan terminal sebagai file asal untuk efek yang sama?
Bisa secara konsep, karena terminal menunjuk ke device tty, dan di Linux device diperlakukan sebagai file.

13. Copy hello.txt ke terminal. Apa yang terjadi ?
```bash
root@UbuntuServer:~/play# cp hello.txt terminal
hello word
root@UbuntuServer:~/play#
```

14. Masih direktory home, copy keseluruhan direktory play ke direktory bernama work
menggunakan symbolic link.
```bash
root@UbuntuServer:~/play# cd ~
root@UbuntuServer:~# ln -s ~/play ~/work
root@UbuntuServer:~# ls -l
total 16
drwxr-xr-x 2 root root 4096 Mar  8 12:58 play
drwxr-xr-x 3 root root 4096 Feb 24 13:21 praktikum-os
-r-xr-xr-x 1 root root 7049 Feb 24 09:35 vboxpostinstall.sh
lrwxrwxrwx 1 root root   10 Mar  8 13:41 work -> /root/play
root@UbuntuServer:~#
```

15. Hapus direktory work dan isinya dengan satu perintah
```bash
root@UbuntuServer:~# rm -rf work
root@UbuntuServer:~# ls -l
total 16
drwxr-xr-x 2 root root 4096 Mar  8 12:58 play
drwxr-xr-x 3 root root 4096 Feb 24 13:21 praktikum-os
-r-xr-xr-x 1 root root 7049 Feb 24 09:35 vboxpostinstall.sh
root@UbuntuServer:~#
```

# Laporan Resmi
1. Analisa hasil percobaan yang Anda lakukan.  
a. Analisa setiap hasil tampilannya.  
b. Pada Percobaan 1 point 3 buatlah pohon dari struktur file dan direktori  
c. Bila terdapat pesan error, jelaskan penyebabnya.

2. Kerjakan latihan diatas dan analisa hasil tampilannya.  

3. Berikan kesimpulan dari praktikum ini.  

> Jawaban:  
> Error yang muncul:  
> * rmdir B error karena direktori tidak kosong.  
> * cd /<user/C error karena path salah.  
> * Hard link ke device bisa gagal karena batasan sistem file.
>
> Percobaan 1 poin 3
> ```bash
> mkdir A B C A/D A/E B/F A/D/A
> ```
>
> Strukturnya:
> ```bash
> .
> ├── A
> │   ├── D
> │   │   └── A
> │   └── E
> ├── B
> │   └── F
> └── C
> ```
>
> Kesimpulan
>
> 1. Linux memakai struktur direktori hierarkis dari root ```/```
> 1. Semua perangkat dianggap sebagai file
> 1. File memiliki tipe dan permission
> 1. Hard link dan soft link memiliki perbedaan penting
> 1. ```/proc``` adalah pseudo filesystem
> 1. Manipulasi file dan direktori dilakukan dengan cp, mv, rm, mkdir, dll