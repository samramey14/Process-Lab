Part 5.1 

ls -l /srv/lab06/notice.txt- -rw-rw-r-- 1 alice alice 18 Nov 4 14:25 /srv/lab06/notice.txt 

stat -c '%a %A %U:%G %n' /srv/lab06/notice.txt- 664 -rw-rw-r-- alice:alice /srv/lab06/notice.txt 

stat -c '%u:%g' /srv/lab06/notice.txt- 1001:1005 

1.420 420 400 rw-=6 r--=4. A new file group gets automatically. made when a new file joins. 

Part 5.2 

sudo -u alice cat /srv/lab06/notice.txt- Release is Friday 

sudo -u bob  cat /srv/lab06/notice.txt- cat: /srv/lab06/notice.txt: Permission denied 

sudo -u carol cat /srv/lab06/notice.txt- Release is Friday 

2.Bob cannot read this file because the command sets everything in the group digit to 0. Which means he can't read, write, or execute. Coral and Alice Can read this file because the commend set them to read it. 

3. Root is a privileged user account (UID 0) that can bypass most file permission checks. Root still gets checked for execute because it's up to the user if a file should be executed. 

Part 5.3a 

sudo -u carol ls -l /srv/lab06/vault- total 8 

-rw-r--r-- 1 alice alice 3 Nov 4 14:42 answer.txt 

-rw-r--r-- 1 alice alice 9 Nov 4 14:42 plan.tx 

sudo -u carol cat /srv/lab06/vault/plan.txt 

the plan 

sudo -u carol ls /srv/lab06/vault 

ls: cannot open directory '/srv/lab06/vault': Permission denied 

sudo -u carol cat /srv/lab06/vault/plan.txt 

the plan 

sudo -u carol ls /srv/lab06/vault 

answer.txt plan.txt 

sudo -u carol ls -l /srv/lab06/vault 

ls: cannot access '/srv/lab06/vault/plan.txt': Permission denied 

ls: cannot access '/srv/lab06/vault/answer.txt': Permission denied 

total 0 

-????????? ? ? ? ?      ? answer.txt 

-????????? ? ? ? ?      ? Plan.txt 

sudo -u carol cat /srv/lab06/vault/plan.txt 

cat: /srv/lab06/vault/plan.txt: Permission denied 

4. 

Mode ls                 the Names  open a file by name  Why 

755  -rw-r--r—1             alice        Yes              Owner 

​     -rw-r--r-- 1 

711  ls: cannot open directory   carol      No              No access 

744   answer.txt plan.txt      carol       No               No access 

 

Part 5.3b 

sudo -u carol rm /srv/lab06/vault/plan.txt 

rm: cannot remove '/srv/lab06/vault/plan.txt': Permission denied 

sudo -u carol rm -f /srv/lab06/vault/answer.txt 

5.rm is a user‑space utility that checks and operates only on the files it’s told to delete, while the Linux kernel enforces broader, system‑wide rules for all file operations, including deletion. 

Part 5.4 

sudo -u bob namei -l /home/alice/project/report.txt 

f: /home/alice/project/report.txt 

drwxr-xr-x root root / 

drwxr-xr-x root root home 

drwx------ alice alice alice 

​            project - Permission denied 

sudo    namei -l /home/alice/project/report.txt 

f: /home/alice/project/report.txt 

drwxr-xr-x root root / 

drwxr-xr-x root root home 

drwx------ alice alice alice 

drwxrwxr-x alice devs project 

-rw-rw-r-- alice devs report.tx 

6. Bob can't see the file because Alice has an x on the field. A way to fix this is to keep share directories out of a home directory. 

Part 5.5 

sudo find /srv/lab06/tree -exec stat -c '%a %A %n' {} + 

775 drwxrwxr-x /srv/lab06/tree 

775 drwxrwxr-x /srv/lab06/tree/docs 

664 -rw-rw-r-- /srv/lab06/tree/docs/README 

755 -rwxr-xr-x /srv/lab06/tree/run.sh 

775 drwxrwxr-x /srv/lab06/tree/src 

664 -rw-rw-r-- /srv/lab06/tree/src/main.c 

sudo find /srv/lab06/tree -exec stat -c '%a %A %n' {} + 

644 drw-r--r-- /srv/lab06/tree 

644 drw-r--r-- /srv/lab06/tree/docs 

644 -rw-r--r-- /srv/lab06/tree/docs/README 

644 -rw-r--r-- /srv/lab06/tree/run.sh 

644 drw-r--r-- /srv/lab06/tree/src 

644 -rw-r--r-- /srv/lab06/tree/src/main.c 

sudo find /srv/lab06/tree -exec stat -c '%a %A %n' {} + 

750 drwxr-x--- /srv/lab06/tree 

750 drwxr-x--- /srv/lab06/tree/docs 

640 -rw-r----- /srv/lab06/tree/docs/README 

640 -rw-r----- /srv/lab06/tree/run.sh 

750 drwxr-x--- /srv/lab06/tree/src 

640 -rw-r----- /srv/lab06/tree/src/main.c 

7. 644 Meet the directories unstable because nobody could execute them. X repaired /srv/lab06/tree, /srv/lab06/tree/docs and /srv/lab06/tree/src. A symbolic command to take away group write is g-. 

Part 5.6 

sudo -u alice /srv/lab06/tree/run.sh 

sudo: /srv/lab06/tree/run.sh: command not found 

 sudo -u alice chgrp sudo /srv/lab06/notice.txt 

chgrp: changing group of '/srv/lab06/notice.txt': Operation not permitted 

8. sudo -u alice /srv/lab06/tree/run.sh don't work because you must be in a root console to run run.sh. sudo -u alice chgrp sudo /srv/lab06/notice.txt don't work because you can't change a group of a directory. directory unless you're root. 

Part 5.7 

umask 

0002 

su - alice -c umask 

0002 

sudo -u alice sh -c umask 

0002  

9. Each setting is subtracted from 777 for a directory and 666 for a file. u000 is a sigh for the null character. 
10. Unmesk Default setting. Is 777 for directroys and 666 for files. The file set the permissions and subtracts it from the unmasked value. On a system where every user shares one primary group users the default permissions are the same and set for everybody reducing the risk of accidental privilege escalation from file ownership changes. 

Part 6.1a 

sudo find / -xdev -perm -4000 -type f | sort 

/usr/bin/chfn 

/usr/bin/chsh 

/usr/bin/gpasswd 

/usr/bin/mount 

/usr/bin/newgrp 

/usr/bin/passwd 

/usr/bin/su 

/usr/bin/sudo 

/usr/bin/umount 

/usr/lib/dbus-1.0/dbus-daemon-launch-helper 

/usr/lib/openssh/ssh-keysign 

/usr/lib/polkit-1/polkit-agent-helper-1 

/usr/sbin/exim4 

sudo find / -xdev -perm -4000 -type f | wc -l 

13 

sudo -u carol /usr/bin/id 

uid=1003(carol) gid=1007(carol) groups=1007(carol) 

sudo -u carol /srv/lab06/id-setuid 

uid=1003(carol) gid=1007(carol) euid=0(root) groups=1007(carol) 

11.The field that the second output has that the first does not is euid. Uid is who carol is and she is acting as the euid. If an auditor finds a setuid bibary in /srv their conclusion is high risk. 

Part 6.1b 

ls -l /srv/lab06/id-setuid 

-rwSr--r-- 1 root root 51656 Nov 4 16:04 /srv/lab06/id-setuid 

ls -l /srv/lab06/id-script 

-rwsr-xr-x 1 root root 27 Nov 4 16:11 /srv/lab06/id-script 

sudo -u carol /srv/lab06/id-script 

uid=1003(carol) gid=1007(carol) groups=1007(carol) 

ls -l /mnt/nosuid/id-setuid 

-rwsr-xr-x 1 root root 51656 Nov 4 16:12 /mnt/nosuid/id-setuid 

sudo -u carol /mnt/nosuid/id-setuid 

uid=1003(carol) gid=1007(carol) groups=1007(carol) 

12. In Capital S and the script, the bit was set. the nosuid mount the bit was honored. The script is the kernel’s deliberate policy. Use nosuid on all writable, shared, or public-accessible filesystems. 

Part 6.2 

sudo ls -l /srv/lab06/shared 

total 4 

-rw-rw-r-- 1 alice alice  0 Nov 4 16:18 one 

drwxrwxr-x 2 alice alice 4096 Nov 4 16:18 sub-one 

sudo ls -l /srv/lab06/shared 

total 8 

-rw-rw-r-- 1 alice alice  0 Nov 4 16:18 one 

drwxrwxr-x 2 alice alice 4096 Nov 4 16:18 sub-one 

drwxrwsr-x 2 alice devs 4096 Nov 4 16:19 sub-two 

-rw-r--r-- 1 alice devs   0 Nov 4 16:20 three 

-rw-rw-r-- 1 alice devs   0 Nov 4 16:19 two 

One that worked- two 

13. The thing that stopped bob from entering these two directories was that he didn't have the execute permission. If a team member creates a file in a directory with unmask 077 the kernel will create that file with mode 600. The Directory can enforce read and write but not execute. 

Part 6.3 

stat -c '%a %A %n' /srv/lab06/shared 

3770 drwxrws--T /srv/lab06/shared  

sudo -u bob rm -f /srv/lab06/shared/two 

rm: cannot remove '/srv/lab06/shared/two': Operation not permitted 

sudo -u bob mv /srv/lab06/shared/two /srv/lab06/shared/renamed 

mv: cannot move '/srv/lab06/shared/two' to '/srv/lab06/shared/renamed': Operation not permitted 

14. Operation not permited bypasses mormal mode-bit. The kernel can still block the operation becuse of mandatory access control. the four-digit octal mode of the directory now, is 760. 

Part 7.1 

sudo grep '^root:' /etc/shadow | cut -d: -f1-2 

root 

sudo sshd -T | grep -E '^permitrootlogin' 

permitrootlogin no 

su: Authentication failure 

15.sudo That's the only way into root because root doesn't have a password in this VM.  

Part 7.2 

Defaults    env_reset 

Defaults    mail_badpass 

Defaults    secure_path="/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin" 

Defaults    use_pty 

root  ALL=(ALL:ALL) ALL 

%sudo  ALL=(ALL:ALL) ALL 

@includedir /etc/sudoers.d 

id -nG 

samramey14 adm dip video plugdev google-sudoers docker lxd 

16. Samramey 14 gogle-sudoers adm docker. The administrator group gave me this rule and I am put in this automatically because I am the owner of this VM. 
17. Goggle accunt, Goggle Pasword, SSh, VM 

Part 7.3 

su: Authentication failure 

su: Authentication failure 

 

User alice is not allowed to run sudo on csc171-lab1-sramey. 

18. The first refused was authentication becuse alice is not a super user or root. The secnod one is an authorization failure because alice is not allow in the shadow file. 

Part 7.4 

su - alice -c 'sudo systemctl restart ssh && echo "restarted"' 

Password:  

su: Authentication failure 

su - alice -c 'sudo systemctl stop ssh' 

Password:  

su: Authentication failure 

su - alice -c 'sudo systemctl restart ssh.service' 

Password:  

su: Authentication failure 

su - alice -c 'sudo -l' 

Password:  

su: Authentication failure 

19. The first sudo command required authentication; the second was allowed without a password because sudo keeps an authentication token for a limited time, so no prompt is needed until that token expires. the rule so that devs may restart *or* reload ssh is confurgre a new ssh key. 

Part 7.5 

sudo visudo -c -f /tmp/broken 

/tmp/broken:1:57: syntax error 

%devs ALL=(root) NOPASSWD /usr/bin/systemctl restart ssh 

sudo visudo -c -f /tmp/devs.conf 

/tmp/devs.conf: parsed OK 

sudo -l -U alice 

User alice is not allowed to run sudo on csc171-lab1-sramey 

20.The missing character is that there is no password. If a Linux system has no root password, you can reset it without reinstalling the OS.Code’s Dev Containers extension only recognizes specific, documented configuration file names — devs.conf is not one of them, so it is ignored. 

21. Both systemctl restart ssh and Sudo id ask for a passworld. In a team of five administrators all using su - with a shared root password, removing one member leaves the system with a single point of failure for privileged access. The situation requires immediate and deliberate changes to prevent unauthorized or unsafe access. Red hat uses wheel instead of sudo. 

Part 7.7 

sudo journalctl _COMM=sudo --since "-30min" --no-pager | grep -E 'alice' 

Nov 04 18:22:44 csc171-lab1-sramey sudo[660865]: samramey14 : TTY=pts/0 ; PWD=/home/samramey14 ; USER=root ; COMMAND=/usr/sbin/adduser alice sudo 

Nov 04 18:23:11 csc171-lab1-sramey sudo[660888]: samramey14 : TTY=pts/0 ; PWD=/home/samramey14 ; USER=root ; COMMAND=/usr/sbin/deluser alice sudo 

22. The fields of a sudo log line are the timestamp showing when the command was executed. The hostname shows what system runs the command. Program name showing it is a sudo command. User shows who invoked sudo shows who requested the command. Terminal shows the terminal session. Working directory shows the directory in which the command was issued. Command executed records the command run. 

Part 8.1 

ls -l /srv/lab06/report.txt- before chmod 600 

-rw-r-----+ 1 alice devs 11 Nov 4 18:36 /srv/lab06/report.txt 

getfacl -p /srv/lab06/report.txt- before chmod 600 

\# file: /srv/lab06/report.txt 

\# owner: alice 

\# group: devs 

user::rw- 

user:carol:r-- 

group::r-- 

mask::r-- 

other::--- 

sudo -u carol cat /srv/lab06/report.txt- before chmod 600 

Q3 numbers 

ls -l /srv/lab06/report.txt-after chmod 600 

-rw-------+ 1 alice devs 11 Nov 4 18:36 /srv/lab06/report.txt 

getfacl -p /srv/lab06/report.txt- after chmod 600 

\# file: /srv/lab06/report.txt 

\# owner: alice 

\# group: devs 

user::rw- 

user:carol:r--         #effective:--- 

group::r--           #effective:--- 

mask::--- 

other::--- 

sudo -u carol cat /srv/lab06/report.txt- after cmod 600 

cat: /srv/lab06/report.txt: Permission denied 

23. IPOSIX ACLs are checked after the standard owner/group/other mode bits. If the file’s ACL contains a named user entry for “carol” with permission to read/write that entry will still apply even after you run chmod 600. The effective annotation means that it is working. 

Part 8.2 

getfacl -p /srv/lab06/shared 

\# file: /srv/lab06/shared 

\# owner: root 

\# group: devs 

\# flags: -st 

user::rwx 

user:carol:r-x 

group::rwx 

mask::rwx 

other::--- 

default:user::rwx 

default:user:carol:r-x 

default:group::rwx 

defaultunmask::rwx 

default:other::--- 

sudo getfacl -p /srv/lab06/shared/four 

\# file: /srv/lab06/shared/four 

\# owner: alice 

\# group: devs 

user::rw- 

user:carol:r-x         #effective:r-- 

group::rwx           #effective:rw- 

mask::rw- 

other::--- 

24. Carol can't read two because she is not the owner or part of its group, and the other bits don't have read permissions. ACL can do things that the combination of the setgid bit and umask cannot because ACLs can impose named-user and named-group permissions and specific permission for new files/directories, independent of the creator’s primary group or system umask. 

Part 8.3 

aa-status summary 

1 processes have profiles defined. 

1 processes are in enforce mode. 

  /usr/sbin/haveged (555)  

0 processes are in complain mode. 

0 processes are in prompt mode. 

0 processes are in kill mode. 

0 processes are unconfined but have a profile defined. 

0 processes are in mixed mode. 

25. Discretionary means the owner of the file or group can decide what to do with it. Sudo is confined to this VM. 
26. A normal user can't read a root file unless they are an administrator or run sudo. aa-status shows the report. 

Part 9.1 

grep -E '^(Uid|Gid|Groups):' /proc/self/status 

Uid:  1000  1000  1000  1000 

Gid:  1003  1003  1003  1003 

Groups: 4 30 44 46 1000 1001 1002 1003 

sudo -u alice grep -E '^(Uid|Gid|Groups):' /proc/self/status 

Uid:  1001  1001  1001  1001 

Gid:  1005  1005  1005  1005 

Groups: 1004 1005 

sudo     grep -E '^(Uid|Gid|Groups):' /proc/self/status 

Uid:  0    0    0    0 

Gid:  0    0    0    0 

Groups: 0  

ps -o pid,ruid,euid,suid,fuid,user,comm -p $$ 

  PID RUID EUID SUID FUID USER   COMMAND 

660409 1000 1000 1000 1000 samrame+ bash 

27. The four Uid columns are grep,-E, ^(Uid|Gid|Groups):' /proc/self/status. /proc.self/status holds carol's and –E hold root. 
28. Processes carry the identity of the owner in the group that is assigned to them. 

Part 9.2 

ps -o pid,ruid,euid,user,comm -p $pid 

  PID RUID EUID USER   COMMAND 

662027 1002 1002 bob   sleep 

sudo -u alice kill $pid 

kill: (662027): Operation not permitted 

29. The kernel compares the user ID of the owner who started the process to the user who is killing the process. If the user matches the owner of the process, they can kill that process. If they don't they cannot kill the process. 

Part 9.3 

grep -E '^(passwd|group):' /etc/nsswitch.conf 

passwd:     files 

group:     files 

grep -E '^(passwd|group):' /etc/nsswitch.conf 

passwd:     files systemd 

group:     files systemd 

sudo journalctl -u lab06-dyn --no-pager -o cat 

Started lab06-dyn.service - [systemd-run] /usr/bin/sh -c "id; ls -ldn /var/lib/lab06-dyn/; exec sleep 600". 

uid=65513(lab06-dyn) gid=65513(lab06-dyn) groups=65513(lab06-dyn) 

drwxr-xr-x 2 65513 65513 4096 Nov 4 19:16 /var/lib/lab06-dyn/ 

ps -o pid,user,uid,comm -p $pid 

  PID USER    UID COMMAND 

662199 lab06-d+ 65513 sleep 

getent passwd lab06-dyn 

lab06-dynx:65513:65513:Dynamic User:/:/usr/sbin/nologin 

getent passwd lab06-dyn || echo "no such user" 

no such user  

sudo ls -ldn /var/lib/private/lab06-dyn 

drwxr-xr-x 2 65534 65534 4096 Nov 4 19:16 /var/lib/private/lab06-dyn 

 getent passwd 65534 

nobodyx:65534:65534:nobody:/nonexistent:/usr/sbin/nologin 

30. A DynamicUser= yes gains the ability to have a temporary, sandboxed user environment and multiple built‑in security protections, but it gives up persistent file ownership and some shared access to system resources.65534 is nobody a special, unprivileged system account designed to represent the least‑privileged identity in the system. 

Paragraph  

A file can be readable by everyone and still unreadable by one person because of the file's permissions. If a file group permission does not allow to read, then a person on the group can't read the file. A person who is not in the group and is not the owner is put into the other category and if the other permissions allow read, then that person can read the file. Sometimes the web server process runs under a different User ID (UID). This can make a file readable to everyone in the permission bits but still inaccessible to the web server process because it’s not the file’s owner and not in its group. 