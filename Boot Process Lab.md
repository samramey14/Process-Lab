Boot Process Lab 

The Four Boot Process phases 

1.Firmware- 2026-09-13T13:46:41.856359+00:00 csc171-lab1-sramey kernel: VFS: Disk quotas dquot_6.6.0 

2.Bootloader- 2026-09-13T13:46:41.856300+00:00 csc171-lab1-sramey systemd[1]: Mounting proc-sys-fs-binfmt_misc.mount - Arbitrary Executable File Formats File System... 

3.Kernel- 2026-09-13T13:46:41.856262+00:00 csc171-lab1-sramey kernel: pci 0000:00:01.3: [8086:7113] type 00 class 0x068000 conventional PCI endpoint 

4.PID-1- 2026-09-13T18:43:51.873390+00:00 csc171-lab1-sramey systemd[1]: Started session-6.scope - Session 6 of User samramey14 

Questions 

1.1.754881 

2.console=tty0 

3.The symlink points to a process that runs until the system shutdown. 

4. A nitramfs needs to exist because the kernel needs a working environment before it can mount the real root filesystem. If the kernel tried to mount the root filesystem directly There could be many problems like missing drivers, boot failure, the kernel would need to be rebuilt for every storage setup and there are no tools to prepare the file system. 

5. one unit that appears near the top of blame but is not on the critical chain is google-cloud-ops-agent.service. Optimizing this service will not make the boot go faster because it runs in the backgrounds and collects logs. 

   6.There are 24 services running when doing nothing. System1—failed don’t report anything. 

   The coloured dot: nginx.service - A high performance web server and a reverse proxy server 

   Loaded: path- loaded (/usr/lib/systemd/system/nginx.service;) 

   Loaded:enabled state- (enabled; preset: enabled) 

   Active:state- active (running) since Sat 2026-09-12 14:36:35 UTC; 4h 9min ago 

   Main PID- 767 (nginx) 

   CGroup:tree- /system.slice/nginx.service 

   The last lines- 767 "nginx: master process /usr/sbin/nginx -g daemon on; master_process on;" 

   ​       ├─768 "nginx: worker process" 

   ​       └─769 "nginx: worker process" 

   7. Restart and reload has a time and content difference. 
   8. the targets WantedBy= names is multi-user.target. the command that systemctl reload nginx run is usr/sbin/nginx -g 'daemon on; master_process on;' -s reload 
   9. systemctl enable does not directly modify the file systems, instead create symbolic links to the configuration. This can tell a service to start automatically during a boot. 
   10. systemctl status nginx does not report an error because nginx is disabled. The one word in the status output that explains why the site is down is inactive 
   11. journalctl -u nginx -b shows no entries because it is the boot log of the current boot. journalctl -u nginx -b -1 Is the log of the previous which is why it shows more entries. You wouldn't have been able to see the boot log of a previous boot if you skipped step 4.1. 

   12.Enable is the right command rather than start because enable active the service at the boot while start active the service automatically. 

   systemd-analyze timings 

   1.965s(kernel) + 13.418s(userspace) =15.383s(total) 

   Paragraph 

   The first thing you should check if a server came back from a reboot with a service missing is what the status of the service is. This can show if a service is disabled or enabled. The second you should check is if the service is enabled. A disabled service won't reset after a boot. The third thing you could should check is the boot log to see if there are any startup boot errors. This can show you if there are missing files or if the network or database isn't ready Which may prevent a surface from restarting from a boot. 