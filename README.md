1.



Быстрый ответ
Разрежённый файл (англ. sparse file) — файл, в котором последовательности нулевых байтов заменены на информацию об этих последовательностях (список дыр). Дыра (англ. hole) — последовательность нулевых байт внутри файла, не записанная на диск.



2.



Нет, не могут, т.к. это просто ссылки на один и тот же inode - в нём и хранятся права доступа и имя владельца.




3.


Добавились диски sdb и sdc

vagrant@vagrant:~$ lsblk
NAME                 MAJ:MIN RM  SIZE RO TYPE MOUNTPOINT
sda                    8:0    0   64G  0 disk
├─sda1                 8:1    0  512M  0 part /boot/efi
├─sda2                 8:2    0    1K  0 part
└─sda5                 8:5    0 63.5G  0 part
  ├─vgvagrant-root   253:0    0 62.6G  0 lvm  /
  └─vgvagrant-swap_1 253:1    0  980M  0 lvm  [SWAP]
sdb                    8:16   0  2.5G  0 disk
sdc                    8:32   0  2.5G  0 disk




4.

vagrant@vagrant:~$ sudo fdisk /dev/sdb

Welcome to fdisk (util-linux 2.34).
Changes will remain in memory only, until you decide to write them.
Be careful before using the write command.

Device does not contain a recognized partition table.
Created a new DOS disklabel with disk identifier 0xd5243481.

Command (m for help): F
Unpartitioned space /dev/sdb: 2.51 GiB, 2683305984 bytes, 5240832 sectors
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes

Start     End Sectors  Size
 2048 5242879 5240832  2.5G

Command (m for help): n
Partition type
   p   primary (0 primary, 0 extended, 4 free)
   e   extended (container for logical partitions)
Select (default p): p
Partition number (1-4, default 1):
First sector (2048-5242879, default 2048):
Last sector, +/-sectors or +/-size{K,M,G,T,P} (2048-5242879, default 5242879): +2G

Created a new partition 1 of type 'Linux' and of size 2 GiB.

Command (m for help): n
Partition type
   p   primary (1 primary, 0 extended, 3 free)
   e   extended (container for logical partitions)
Select (default p): p
Partition number (2-4, default 2):
First sector (4196352-5242879, default 4196352):
Last sector, +/-sectors or +/-size{K,M,G,T,P} (4196352-5242879, default 5242879):

Created a new partition 2 of type 'Linux' and of size 511 MiB.

Command (m for help): w
The partition table has been altered.
Calling ioctl() to re-read partition table.



5.



vagrant@vagrant:~$ sudo sfdisk -d /dev/sdb > sdb.dump
vagrant@vagrant:~$ sudo sfdisk /dev/sdc < sdb.dump
Checking that no-one is using this disk right now ... OK

Disk /dev/sdc: 2.51 GiB, 2684354560 bytes, 5242880 sectors
Disk model: VBOX HARDDISK
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes

>>> Script header accepted.
>>> Script header accepted.
>>> Script header accepted.
>>> Script header accepted.
>>> Created a new DOS disklabel with disk identifier 0xd5243481.
/dev/sdc1: Created a new partition 1 of type 'Linux' and of size 2 GiB.
/dev/sdc2: Created a new partition 2 of type 'Linux' and of size 511 MiB.
/dev/sdc3: Done.

New situation:
Disklabel type: dos
Disk identifier: 0xd5243481

Device     Boot   Start     End Sectors  Size Id Type
/dev/sdc1          2048 4196351 4194304    2G 83 Linux
/dev/sdc2       4196352 5242879 1046528  511M 83 Linux

The partition table has been altered.
Calling ioctl() to re-read partition table.
Syncing disks.





6.



root@vagrant:~# mdadm --create /dev/md0 --level=1 --raid-devices=2 /dev/sd[bc]1
mdadm: Note: this array has metadata at the start and
    may not be suitable as a boot device.  If you plan to
    store '/boot' on this device please ensure that
    your boot-loader understands md/v1.x metadata, or use
    --metadata=0.90
Continue creating array? y
mdadm: Defaulting to version 1.2 metadata
mdadm: array /dev/md0 started.




7.



mdadm: Defaulting to version 1.2 metadata
mdadm: array /dev/md1 started.




8.



root@vagrant:~# pvcreate /dev/md0
  Physical volume "/dev/md0" successfully created.
root@vagrant:~# pvcreate /dev/md1
  Physical volume "/dev/md1" successfully created.

root@vagrant:~# pvs
  PV         VG        Fmt  Attr PSize    PFree
  /dev/md0             lvm2 ---    <2.00g   <2.00g
  /dev/md1             lvm2 ---  1018.00m 1018.00m
  /dev/sda5  vgvagrant lvm2 a--   <63.50g       0




9.


root@vagrant:~# vgcreate netology35 /dev/md0 /dev/md1
  Volume group "netology35" successfully created
root@vagrant:~# vgs
  VG         #PV #LV #SN Attr   VSize   VFree
  netology35   2   0   0 wz--n-  <2.99g <2.99g
  vgvagrant    1   2   0 wz--n- <63.50g     0



10.



root@vagrant:~# lvcreate -L 100m -n n35-10-lv netology35 /dev/md1
  Logical volume "n35-10-lv" created.
root@vagrant:~# lvs -o +devices
  LV        VG         Attr       LSize   Pool Origin Data%  Meta%  Move Log Cpy%Sync Convert Devices
  n35-10-lv netology35 -wi-a----- 100.00m                                                     /dev/md1(0)
  root      vgvagrant  -wi-ao---- <62.54g                                                     /dev/sda5(0)
  swap_1    vgvagrant  -wi-ao---- 980.00m                                                     /dev/sda5(16010)






11.


root@vagrant:~# mkfs.ext4 -L n35-11 -m 1 /dev/mapper/netology35-n35--10--lv
mke2fs 1.45.5 (07-Jan-2020)
Creating filesystem with 25600 4k blocks and 25600 inodes

Allocating group tables: done
Writing inode tables: done
Creating journal (1024 blocks): done
Writing superblocks and filesystem accounting information: done

root@vagrant:~# blkid | grep n35
/dev/mapper/netology35-n35--10--lv: LABEL="n35-11" UUID="08fcf9f4-ae46-4e5a-ab41-9b27bd5b68b3" TYPE="ext4"



12.



root@vagrant:~# mkdir /tmp/new
root@vagrant:~# mount /dev/mapper/netology35-n35--10--lv /tmp/new/
root@vagrant:~# mount | grep n35
/dev/mapper/netology35-n35--10--lv on /tmp/new type ext4 (rw,relatime,stripe=256)



13.


root@vagrant:~# cd /tmp/new/
root@vagrant:/tmp/new# wget https://mirror.yandex.ru/ubuntu/ls-lR.gz -O /tmp/new/test.gz
--2021-12-31 10:34:28--  https://mirror.yandex.ru/ubuntu/ls-lR.gz
Resolving mirror.yandex.ru (mirror.yandex.ru)... 213.180.204.183, 2a02:6b8::183
Connecting to mirror.yandex.ru (mirror.yandex.ru)|213.180.204.183|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 21503106 (21M) [application/octet-stream]
Saving to: ‘/tmp/new/test.gz’

/tmp/new/test.gz     100%[======================>]  20.51M  5.35MB/s    in 3.8s

2021-12-31 10:34:32 (5.35 MB/s) - ‘/tmp/new/test.gz’ saved [21503106/21503106]

root@vagrant:/tmp/new# ls -l
total 21016
drwx------ 2 root root    16384 Dec 31 10:31 lost+found
-rw-r--r-- 1 root root 21503106 Dec 31 05:31 test.gz
root@vagrant:/tmp/new# df -h /tmp/new/
Filesystem                          Size  Used Avail Use% Mounted on
/dev/mapper/netology35-n35--10--lv   93M   21M   70M  23% /tmp/new



14.



root@vagrant:/tmp/new# lsblk
NAME                         MAJ:MIN RM  SIZE RO TYPE  MOUNTPOINT
sda                            8:0    0   64G  0 disk
├─sda1                         8:1    0  512M  0 part  /boot/efi
├─sda2                         8:2    0    1K  0 part
└─sda5                         8:5    0 63.5G  0 part
  ├─vgvagrant-root           253:0    0 62.6G  0 lvm   /
  └─vgvagrant-swap_1         253:1    0  980M  0 lvm   [SWAP]
sdb                            8:16   0  2.5G  0 disk
├─sdb1                         8:17   0    2G  0 part
│ └─md0                        9:0    0    2G  0 raid1
└─sdb2                         8:18   0  511M  0 part
  └─md1                        9:1    0 1018M  0 raid0
    └─netology35-n35--10--lv 253:2    0  100M  0 lvm   /tmp/new
sdc                            8:32   0  2.5G  0 disk
├─sdc1                         8:33   0    2G  0 part
│ └─md0                        9:0    0    2G  0 raid1
└─sdc2                         8:34   0  511M  0 part
  └─md1                        9:1    0 1018M  0 raid0
    └─netology35-n35--10--lv 253:2    0  100M  0 lvm   /tmp/new



15.



root@vagrant:/tmp/new# gzip -t /tmp/new/test.gz
root@vagrant:/tmp/new# echo $?
0







16.


root@vagrant:/tmp/new# pvmove -n n35-10-lv /dev/md1 /dev/md0
  /dev/md1: Moved: 12.00%
  /dev/md1: Moved: 100.00%



17.



root@vagrant:/tmp/new# mdadm --fail /dev/md0 /dev/sdb1
mdadm: set /dev/sdb1 faulty in /dev/md0


18.



root@vagrant:/tmp/new# dmesg | grep md0 | tail -n 2
[ 2858.194026] md/raid1:md0: Disk failure on sdb1, disabling device.
               md/raid1:md0: Operation continuing on 1 devices.



19.


root@vagrant:/tmp/new# gzip -t /tmp/new/test.gz
root@vagrant:/tmp/new# echo $?
0


20.
Выполнено









