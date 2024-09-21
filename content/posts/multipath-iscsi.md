---
title: "Multipath iscsi proxmox [id]"
date: 2024-09-20
draft: false

tags:
- bash
---
Konfigurasi ISCSI Nimble / Alletra (HPE) dan Promxox dengan Multipath
---

**1. Cek IQN pada isci proxmox**
```
cat /etc/iscsi/initiatorname.iscsi

```
akan tampil output seperti ini
```
## DO NOT EDIT OR REMOVE THIS FILE!
## If you remove this file, the iSCSI daemon will not start.
## If you change the InitiatorName, existing access control lists
## may reject this initiator.  The InitiatorName must be unique
## for each iSCSI initiator.  Do NOT duplicate iSCSI InitiatorNames.
InitiatorName=iqn.<REDACTED>-08.org.debian:01:<REDACTED>
```
lalu tambahkan data akses pada storage server yang akan digunakan
> disini saya mengguakan nimble/ alletra storage

>Penting untuk memulai semua koneksi ISCSI saat boot time, dengan cara setting pada `node.startup` menjadi `automatic` dan
default `node.session.timeo.replacement_timout` dari `120` menjadi `15` second

**2. konfigurasi data tersebut pada file `etc/iscsi/iscsid.conf`**
```
node.startup = automatic
node.session.timeo.replacement_timeout = 15

```
restart iscsi service pada proxmox
```
 systemctl restart iscsid.service

```

**3. jika sudah ditambahakan di server storage, saat nya di tambahkan pada server proxmox**

Pada **datacenter** -> pilih **storage** -> pilih **Add** -> pilih **ISCSI**

{{<image src="/assets/storage1.png" position="center"  >}}

Dalam Pop-up Add: iSCSI masukan :
- **ID** : Nama datastore iSCSI
- **Portal** : IP discovery iSCSI
- **Target** : LUN Target iSCSI
- **Unmark pada Use LUNs Directly**
- lalu **Add**
{{<image src="/assets/storage2.png" position="center"  >}}


{{<image src="/assets/storage3.png" position="center" >}}

**4. lalu check iscsi session dengan perintah :**
```
iscsiadm -m session

```
maka akan tampil output seperti ini:
```
tcp: [1] <REDACTED>.17:<REDACTED>,<REDACTED> iqn.2007-11.com.nimblestorage:<REDACTED> (non-flash)
tcp: [2] <REDACTED>.16:<REDACTED>,<REDACTED> iqn.2007-11.com.nimblestorage:<REDACTED> (non-flash)
tcp: [3] <REDACTED>.18:<REDACTED>,<REDACTED> iqn.2007-11.com.nimblestorage:<REDACTED> (non-flash)
tcp: [4] <REDACTED>.15:<REDACTED>,<REDACTED> iqn.2007-11.com.nimblestorage:<REDACTED> (non-flash)
```
jika tampil session lebih dari satu, maka sebelum melakukan pembuatan LVM pada disk, perlu dilakukan konfigurasi multipath pada Proxmox

# Active Multipath
**1. Install multipath tools**
```
apt-get update
apt-get install multipath-tools

```

>Rekomendasi dari Proxmox, agar kita menggunakan wwid (World Wide Identification) untuk mengidentifikasi disk, berikut perintah untuk mengetahui wwid pada disk

**2. Cek disk yang terkoneksi dengan iscsi dengan perintah** 
```
lsblk

```
akan tampil seperti berikut :
```
NAME               MAJ:MIN RM   SIZE RO TYPE MOUNTPOINTS
sda                  8:0    0 278.5G  0 disk
├─sda1               8:1    0  1007K  0 part
├─sda2               8:2    0     1G  0 part /boot/efi
└─sda3               8:3    0 277.5G  0 part
  ├─pve-swap       252:0    0    30G  0 lvm  [SWAP]
  ├─pve-root       252:1    0    50G  0 lvm  /
  ├─pve-data_tmeta 252:2    0   1.5G  0 lvm
  │ └─pve-data     252:4    0 144.5G  0 lvm
  └─pve-data_tdata 252:3    0 144.5G  0 lvm
    └─pve-data     252:4    0 144.5G  0 lvm
sdb                  8:16   0    10G  0 disk
sdc                  8:32   0    10G  0 disk
sdd                  8:48   0    10G  0 disk
sde                  8:64   0    10G  0 disk
sr0                 11:0    1  1024M  0 rom

```
terdapat 4 disk yang dengan volume yang sama, yang mana disk tersebut merupakan hasil data dari iscsi,
cara melakukan pengecekan apakah disk tersebut sama semua atau tidak, kita bisa melakukan pengecekan dengan wwid

```
/lib/udev/scsi_id -g -u -d /dev/sdb

```
akan tampil wwid dari disk :
```
2092<REDACTED>6585 #wwid disk iscsi saya

```
**3.  Kita tambahkan konfigurasi multipath pada server proxmox:**
```
vim /etc/multipath.conf

```
tambahkan code sebagai berikut:
```
defaults {
        polling_interval        2
        path_selector           "round-robin 0"
        path_grouping_policy    multibus
        uid_attribute           ID_SERIAL
        rr_min_io               100
        failback                immediate
        no_path_retry           queue
        user_friendly_names     yes
}

blacklist {
        wwid .*
}

blacklist_exceptions {
        wwid "2092<REDACTED>6585"
}
```
**4. WWID juga perlu di tambahkan di file `/etc/multipath/wwids` untuk menambahkan wwid pada file tersebut hanya perlu menjalankan perintah berikut:**
```
multipath -a 2092<REDACTED>6585

```
**5. Untuk mempermudah identifikasi path, kita bisa menggunakan alias yang di konfigurasi di `/etc/multipath/bindings` tambahkan script berikut pada file tsb**
```
mpatha 2092<REDACTED>6585

```


**6. Restart multipath tools untuk apply configurasi**
```
systemctl restart multipath-tools.service

```
cek konfigurasi apakah sudah sesuai atau belum dengan perintah

```
multipath -ll

```
outputnya akan tampil seperti berikut:
```
mpatha (2092<REDACTED>6585) dm-5 Nimble,Server
size=10G features='1 queue_if_no_path' hwhandler='1 alua' wp=rw
`-+- policy='round-robin 0' prio=50 status=active
  |- 11:0:0:170 sdb 8:16 active ready running
  |- 12:0:0:170 sdc 8:32 active ready running
  |- 13:0:0:170 sdd 8:48 active ready running
  `- 14:0:0:170 sde 8:64 active ready running

```
atau jika ingin lebih detil bisa menggunakan 
```
multipath -v3

```
# Create LVM storage over iSCSI
**1. Add new LVM**
 Pilih **datacenter** -> pilih **storage** -> pilih **Add** -> pilih **LVM**

{{<image src="/assets/lvm.png" position="center" >}}

akan muncul menu **Add: LVM** 
- **ID** : Nama / ID untuk LVM baru
- **Base storage** : pilih storage iSCSI yang baru
- **Base volume** : pilih LUN yang di set sebelumnya pada server Storage iSCSI
- **Volume Group** : Beri nama baru untuk volume group iSCSI
  
{{<image src="/assets/addlvm.png" position="center" >}}

Hasil Konfigurasi LVM over iSCSI 

{{<image src="/assets/doneadd.png" position="center" >}}


{{<image src="/assets/show.png" position="center" >}}

## Troubleshoot
#### Jika `multipath -ll` tidak menampilkan output
coba lakukan restart pada service ISCSI dan Multipath
```
 systemctl restart iscsid.service

```
```
systemctl restart multipath-tools

```