Part 1.1

Id- uid=1000(samramey14) gid=1003(samramey14) groups=1003(samramey14),4(adm),30(dip),44(video),46(plugdev),1000(google-sudoers),1001(docker),1002(lxd)

ps -o pid,uid,user,cmd -p $$

PID UID USER CMD

299567 1000 samrame+ -bash

grep -E '^(Uid|Gid|Groups):' /proc/$$/status

Uid: 1000 1000 1000 1000

Gid: 1003 1003 1003 1003

Groups: 4 30 44 46 1000 1001 1002 1003

1.Id get the names from a Registry configuration. This tells you if the authoritative answer is a local or DNS.

2.cown did not refuse UID 4242 because the file’s metadata will be valid until a user with that UID is created or the UID is reassigned.

Part 1.3

cat: /etc/shadow: Permission denied

Sudo –l-(ALL : ALL) NOPASSWD: ALL

3.the authentication used is PAM (Pluggable Authentication Modules) to verify your credentials and the authorization is /etc/sudoers and any drop-in files in /etc/sudoers.d/. This all deponed on ssh

4

-1.authentication failure because it a password that does not connect with the site 

-2.authorization failure because an ordinary user should not be allowed to edit a configuration file

 -3. authentication failure because the business should remove the ssh key of a past employee because a future employee can log in using that key.

 -4.authorization failure because a web server should not read /etc/shadow because that where the hashed passwords are located

5.The attacker will have to get through both the browser and SSH because the attacker will need to get to the Google Cloud browser to get to the virtual machine and then SSH into the machine.

Part 2.1

ls -l /etc/passwd /etc/shadow /etc/group-

-rw-r--r-- 1 root root 835 Oct 14 18:48 /etc/group -rw-r--r-- 1 root root 1437 Aug 27 21:58 /etc/passwd -rw-r----- 1 root shadow 740 Aug 27 21:58 /etc/shadow

wc -l /etc/passwd- 28 /etc/passwd

6.The GECOS felid contains /home/samramey14:/bin/bash. I don't have to run adduser because this felid is run autodically with a new user.

Part 2.2

UID 0-1 system accounts-24

People-1, samramey14x:1000:1003::/home/samramey14:/bin/bash

7.1 account has UID 0. Checking for the name root would miss root because you won't have the right Privileges to access root.

8.The shell that every system account has is /bin/nologin. /bin/nologin prints a message Saying that an account is not available. /bin/nologin is important because it doesn't let you interact with the shell until you have a password.

9.My password felid is 3!. I think this means that I could log in to my VM using this password.

10.The shadow value starts records when the eclipse happens.

11.The one name listed in id –nG that is not in grep is my user account samramey14. This is not listed here because it is a primary user while the rest are supplementary.

12.The account that lets me use sudo is admin. I got added automatically because I am the owner of this machine.

Part 2.5

passwd: files

group: files

shadow: files

13.line counts may differ because ls include hidden files.

Part 3.1

adduser: info: Adding new user `alice' to supplemental / extra groups `users' ...

info: Adding user `alice' to group `users' ...

getent: alicex:1004:1004:,,,:/home/alice:/bin/bash

Id: uid=1004(alice) gid=1004(alice) groups=1004(alice),100(users)

14.Adduser selected the UID and put the new user in a group. The hash algorithm is MD5

Part 3.2

getent passwd bob- bobx:1005:1005::/home/bob:/bin/sh

sudo grep '^bob:' /etc/shadow- bob:!:20751:0:99999:7:::

ls -d /home/bob- ls: cannot access '/home/bob': No such file or directory

15.Bob don't have a password or a directory.The useradd command that would have made a bob comparable to alice issudo useradd -m -s /bin/bash -G sudo bob.

Part 3.3

file /usr/sbin/useradd /usr/sbin/adduser- /usr/sbin/useradd: ELF 64-bit LSB pie executable, x86-64, version 1 (SYSV), dynamically linked, interpreter /lib64/ld-linux-x86-64.so.2, BuildID[sha1]=e23ba367672b68097740464c4d6570fb3294539b, for GNU/Linux 3.2.0, stripped

/usr/sbin/adduser: Perl script text executable

head -1 /usr/sbin/adduser- #! /usr/bin/perl -T

dpkg -S /usr/sbin/useradd /usr/sbin/adduser

passwd: /usr/sbin/useradd

adduser: /usr/sbin/adduser

Adduser programs

1 execve("/bin/find"

1 execve("/bin/gpasswd"

8 execve("/bin/logger"

1 execve("/bin/sh"

1 execve("/sbin/groupadd"

1 execve("/sbin/useradd"

1 execve("/sbin/usermod"

1 execve("/usr/sbin/adduser"

Useradd-331924 execve("/sbin/useradd", ["/sbin/useradd", "-d", "/home/dave", "-g", "1006", "-s", "/bin/bash", "-u", "1006", "dave"], 0x556444c88810 /* 20 vars */) = 0

16.Adduser leaves out the password and UID that useradd puts back.

17.Debian will ask for extra details and close the home directory. The script should use useradd -m alice.

Part 3.4

useradd –D

GROUP=100

GROUPS=

HOME=/home

INACTIVE=-1

EXPIRE=

SHELL=/bin/sh

SKEL=/etc/skel

USRSKEL=/usr/etc/skel

CREATE_MAIL_SPOOL=no

LOG_INIT=yes

Login.defs

efs

HOME_MODE 0700

UID_MIN 1000

UID_MAX 60000

ENCRYPT_METHOD YESCRYPT

USERGROUPS_ENAB yes

18.Bob's shell is in SHELL=/bin/sh. Bob doesn't have a home directory because there is no home mode line. Alice has /bin/bash because the settings have not been configured, and this is the default configuration.

getent passwd erin- erinx:1007:1007:Erin Park:/home/erin:/bin/bash

id frank- uid=2001(frank) gid=2100(projects) groups=2100(projects),100(users)

getent passwd frank- frankx:2001:2100:Frank Osei:/srv/homes/frank:/bin/bash

sudo chage -l frank

Last password change : Oct 25, 2026

Password expires : never

Password inactive : never

Account expires : Jan 23, 2027

Minimum number of days between password change : 0

Maximum number of days between password change : 99999

Number of days of warning before password expires : 7

19.frank get no group named frank because the account does not have the right permissions.

Part 3.6

Loops: 1- lab06u1x:2002:2002:Lab 06 student:/home/lab06u1:/bin/bash

lab06u2x:2003:2003:Lab 06 student:/home/lab06u2:/bin/bash

2- lab06u1x:2002:2002:Lab 06 student:/home/lab06u1:/bin/bash

lab06u2x:2003:2003:Lab 06 student:/home/lab06u2:/bin/bash

getent passwd lab06u9- lab06u9x:2003:2003:Lab 06 student, renamed:/home/lab06u9:/bin/bash

20 Two reasons a script should call useradd rather than adduser is for portability and for non‑interactive, fully controlled execution.

21 Dedian helper Shadow-suite tool

adduser NAME /etc/shadow

adduser –system NAME useradd

addgroup NAME groupadd

adduser NAME GROUP usermod -aG

deluser --remove-home NAME net user username /delete

delgroup NAME net user username /delete

22.The last command failed because we are not the Alice user, so we cannot open her home directory.

Part 3.8

Id bob before usermod- uid=1005(bob) gid=1005(bob) groups=1005(bob),100(users),1008(devs)

Id bon after usermod- uid=1005(bob) gid=1005(bob) groups=1005(bob),1008(devs)

23.-G without a –a would just the groups the user is in. Runing sudo usermod -G devs $USER on your account adds a primary group to your account. Two commands that add groups without a risk are groupadd and addgroup.

Part 3.9

id alice- uid=1004(alice) gid=1004(alice) groups=1004(alice),100(users),1008(devs),1009(ops)

grep Groups /proc/$pid/status- Groups: 100 1004 1008

sudo -u alice id -nG - alice users devs ops

24.The database shows everything that Alice is in. The running process shows everything that Alice is doing in that process. The new process shows what a new process gets when it starts up.

Part 3.10

sudo grep '^alice:' /etc/shadow- alice:!$y$j9T$3S/O2jMedbLvbOrFmMagz0$M7Es/fkt98tLYQSwL9EwNJtAikAgmHdM2GV.VvrS6t2:20751:0:99999:7:::

25.Su refused the password because I don't have the right permissions, and this is a security feature. sudo –u alice worked because it is the right permission to make the user authentication into the account.

ls -l /srv/lab06 before deleting Bob

total 4

-rw-rw-r-- 1 bob bob 18 Oct 26 13:30 report.txt

ls -l /srv/lab06 after deleting Bob

total 4

-rw-rw-r-- 1 1005 1005 18 Oct 26 13:30 report.txt

ls -l /srv/lab06 after creating Carol

total 4

-rw-rw-r-- 1 carol carol 18 Oct 26 13:30 report.txt

26.Carol can read and write a file she never seen because the inode of that file stores metadata about that file, independent of the file’s name or the user who created it.

27.Su failed because the password felids are different. Sudo -u myapp can run the command as is because sudo is the superuser commend that runs a commend with all privileges. Runing sudo account instead of root is better because it lends more flexibility and is less destructive.

Part 4.1

common-auth

auth [success=1 default=ignore] pam_unix.so nullok

auth requisite pam_deny.so

auth required pam_permit.so

auth optional pam_cap.so

common-account

account [success=1 new_authtok_reqd=done default=ignore] pam_unix.so

account requisite pam_deny.so

account required pam_permit.so

28.The Pam_permit decides the outcome of the authentication.

29.pam_nologin.so uses the auth module. This module type asks for the user Identity.

Part 4.2

Frist login-Warning: Permanently added 'localhost' (ED25519) to the list of known hosts.

alice

uid=1004(alice) gid=1004(alice) groups=1004(alice),100(users),1008(devs),1009(ops)

logged in from ::1 36452 22

30.The client set up a Private ssh key that the server checked against its private key. An attacker can't steal this key because it is highly encrypted.

Part 4.3

The Client error is a permission error.

Oct 26 15:32:52 csc171-lab1-sramey sshd-session[366219]: Authentication refused: bad ownership or modes for directory /home/alice

31.Ssh cares about changing the permissions because that means other users can write and read your encrypted key. This also means that they could change your key so that it doesn't work anymore.

Part 4.4

Lock password

Client output-alice

Expire the account

Client output-Your account has expired; please contact your system administrator.

Connection closed by ::1 port 22

journal line tail: option used in invalid context -- 5

-bash: 200~journalctl: command not found

Take away shell

Client output- Your account has expired; please contact your system administrator.

Connection closed by ::1 port 22

32.a

**Method** |**Did the key authenticate?**| **Was a session started?** |**What stopped it** 

passwd –l            Yes 

usermod -e 1                                                             Yes 

usermod -s /usr/sbin/nologin                                                                              Yes

32.b I would use what stopped it method. The method people reach for first is that a session started is wrong because a session has already started.

Part 4.5

-bash: as_alice: command not found

33.The PAM auth stage refused the login because if somebody else can. Read or write this Account the ssh key can't go through.

Part 4.6a

sshd -T output

usepam yes

permitrootlogin no

pubkeyauthentication yes

passwordauthentication no

strictmodes yes

authorizedkeysfile .ssh/authorized_keys .ssh/authorized_keys2

34.Locking Alice password does not stop her key because the password and key are different settings. So, you'll have to lock both settings if you don't want Alice to get into her account.

Part 4.6b

Error message-ssh: Could not resolve hostname external_ip: Name or service not known

Part 4.7

Fingerprint listings

google-guest-agent-manager.service loaded active running Google Compute Engine Guest Agent Plugin Manager

google-guest-compat-manager.service loaded active running Google Compute Engine Guest Compat Manager

35.Gcloud sends a challenge, The user sights in with a private key and gcloud verifies with the public key.

36.The line records that gcloud accepts a public key from the user. The fields of a sudo that would let an auditor reconstruct who did what is PWD and COMMAND.

Paragraph

One of the most important parts of Linux system administration is knowing the difference between authentication and authorization. Authentication is when the server verifies who you are at login. An example of authentication in this lab is when I would have to make Alice log in using her ssh key. Authorization is when the server checks that you have the right permissions to run a certain command or read a certain file. An example of authorization from this lab is when I used sudo to read the etc/shadow file.