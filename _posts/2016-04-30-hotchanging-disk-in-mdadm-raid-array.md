---
layout: post
title: hot changing disk in mdadm raid array
---

to hot change a disk in an mdadm array, the disk to remove is sdb, the disk to add is sdc:

HAVE A BACKUP OF YOUR DATA
really,
backup your data

Verify the configuration of the array:

    #cat /proc/mdstat

Clone the partition table on the news disk:
using sgdisk (install gdisk if you don't have it) backup the partition table of the source disk:

    sgdisk -b part.table /dev/sdb

the restore it on the destination disk, the -G switch is to generate new GUIDS for the partition since sgdisk also clones the GUIDS:

    sgdisk -l part.table -G /dev/sdc

verify that the partitions are ok.

Modify the RAID array:

remove the "old" disk:

    mdadm --manage /dev/md0 --fail /dev/sdb1
    mdadm --manage /dev/md0 --remove /dev/sdb1

add the "new" disk to the array:

    mdadm --manage /dev/md0 --add /dev/sdc1

Monitor the rebuild of the array:

    #cat /proc/mdstat
    [CUT]
    [==>..................]  recovery = 14% (nnnn/nnnnnnnn) finish=50.6min speed=57001K/sec
    [CUT]

Wait for the rebuild.
Patiently.
