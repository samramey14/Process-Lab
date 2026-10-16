Disks, Filesystems and Mounting Lab 

Part 1.2 

Lsblk 

NAME  MAJ:MIN RM SIZE RO TYPE MOUNTPOINTS 

sda    8:0  0  10G 0 disk 

├─sda1  8:1  0 9.9G 0 part / 

├─sda14  8:14  0  3M 0 part 

└─sda15  8:15  0 124M 0 part /boot/efi 

sdb    8:16  0  10G 0 disk 

ls -l /dev/disk/by-id/google-data 

lrwxrwxrwx 1 root root 9 Sep 19 13:31 /dev/disk/by-id/google-data -> ../../sdb 

 

Part 1.4 

Block size-4KB Block count 5,120 Inode count=5,120 

 

Part 2.3 

Etc/fstab- PARTUUID=501de0c7-4c7d-45b6-bfb3-4242869d4f54 / ext4 rw,discard,errors=remount-ro,x-systemd.growfs 0 1 

PARTUUID=167fb6f9-528a-4eb2-9568-3b70f84d1c97 /boot/efi vfat defaults,umask=077 0 2 

UUID=d6926ffc-0a56-41e4-8e0b-4efb8259b77b /srv/data ext4 defaults,nofail 0 2 

 

Part 2.4 

findmnt --verify -0 parse errors, 2 errors, 1 warning 

mount –a- dmesg(1) may have more information after failed mount system call. 

 

Part 3.1 

Inode=5,120 link=5,125 

 

Part 3.4 

hard-link.txt= still works 

sym-link.txt= don’t work 

 

Part 3.6 

ln: failed to create hard link | No such file or directory 

ln: failed to access '/srv/data/subdir': No such file or directory 

drwxrwxrwt 11 root root 220 Sep 21 14:09 

 

Part 4.1 

mks inode count-120 

 

Part 4.2 

df -h /mnt/small 

Filesystem   Size Used Avail Use% Mounted on 

/dev/sda1    9.7G 2.8G 6.4G 31% / 

df -i /mnt/small 

Filesystem   Inodes IUsed IFree IUse% Mounted on 

/dev/sda1   647168 92519 554649  15% / 

 

Part 4.3 

ls -l /srv/data 

total 76 

du -sh /srv/data 

du: cannot read directory '/srv/data/run/sudo': Permission denied 

df -h /srv/data 

Filesystem   Size Used Avail Use% Mounted on 

/dev/sda1    9.7G 2.8G 6.4G 31% /srv/data 

 

Questions- 

1. /dev/disk/by-id/google-data is safer because it is a persistent, hardware specific symlink created by udev that always points to the same physical disk, whereas /dev/sdb is a volatile kernel name that can change between reboots or after hardware changes. 

 

2.It’s not sector 34 because that were root starts 

 

3. The reserved block count is for root 

 

4. The filesystem is slightly smaller than the 10 GB disk created because of the meta beta and reserved blocks. 

 

5. The partition knows about the metadata stored in the file system. Filesystem knows site-specific inform about /srv/data 

 

6. PARTUUID,ext4,PARTUUID,UID,unmask,remount. 0,2 controls the backup filesystems. 

   7.nofail tells the system to not report an error if the specified device or filesystem cannot be mounted. Without nofail the boot process will attempt to mount the missing device, fail, and stop booting. 

    

   8. daemon-reload still necessary because systemd does not automatically detect changes to unit files or mount configurations — it must be explicitly told to re‑read them. Systemd reads /etc/fstab at boot. 

    

   9. The boot will continue booting even if the no failed device is not present. If the no fail line is removed, the device will stop booting. 

      10. findmnt, ls, and systemctl --failed would say that there is no such device if the disk had been detached while the machine was down. 

       

      11. The difference occurs because st_blocks counts all 512‑byte blocks allocated to the file, including those that may be only partially filled, and it can also include metadata blocks or indirect blocks that store pointers rather than file data. 

       

      12. those bytes are the inode for the symlink. These don’t change the link count on notes.txt because the link is pointed to it, not attached to it. 

       

      13. When you run rm file.txt in Linux, rm does not erase the file data. Instead, it calls the unlink(2) system call, which removes one directory entry (name) from the filesystem. condition under which the data blocks would have been released is when all hard links to the inode are removed and no process has the file open. 

       

      14. A directory entry it is a fixed‑size data structure stored in a directory file that maps a filename to an inode number. 

       

      15.link count=5,130 

       

      16. The deletion is governed by the sticky bit special permission on the /tmp directory. The t in /tmp is the sticky bit indicator. /tmp is designed to be world‑writable so any user can create temporary files without special privileges. Without the sticky bit, a user could delete or overwrite other users’ files. 

       

      17. A loop device is a special type of block device in operating systems that represents a virtual or in memory storage area rather than a physical disk. A loop device fits into this model as part of the Harness layer, the execution environment and tools that run the work. 

       

      18. the filesystem is already using most of the available blocks, resize2fs cannot allocate the extra space needed for metadata. 

       

      19. The missing inodes are allocated to existing files, directories, and symbolic links on the filesystem. 

       

      20. FD is a File Descriptor Indicating how the file is being accessed by the process. NODE is the inode number of the file in the filesystem. 

       

      21. The “space” you saw is not actually gone from the filesystem — it’s been reclaimed by the kernel because the file link count dropped to zero. You must follow the service’s log rotation retention rules, the correct recovery model, and backup/shrink process to reclaim space. 

       

      Paragraph 

       

      You should first find large files and directories in the disk with du. Once you find a large file, check the content of the file to make sure it is a necessary file for the disk or system to run. If a large file is unnecessary for the disk to run, you can delete it with rm. You should also check the inode count because if the disk has run out of inodes, a new file can't be created. You can do this by going into a directory and running the command.ls-i. The deletion of unnecessary large files can help free up inodes. 