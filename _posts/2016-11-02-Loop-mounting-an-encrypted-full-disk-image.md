Open:

    fdisk -l -u imagename.img

    kpartx -a -v imagename.img

    cryptsetup luksOpen /dev/mapper/loop0pN LUKSNAME

    vgscan

    vgchange -a y VGNAME

    mount /dev/mapper/VGNAME-LVNAME MOUNTPOINT

Close:

    umount MOUNTPOINT

    vgchange -a n VGNAME

    cryptsetup luksClose LUKSNAME

    dmsetup info
    dmsetup remove /dev/mapper/loop0p1
    dmsetup remove /dev/mapper/loop0pN…

    losetup -a
    losetup -d /dev/loop0

 

source: http://www.blaicher.com/2013/01/accessing-an-encrypted-full-disc-image-lukslvm/

