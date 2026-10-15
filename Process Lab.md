Process Lab

Part 3.4

system uptime:0:00.20

number of users:1

three load averages:7796,5704,3572

percentage of CPU idle time: 14.3

used memory: 0.3

total memory:0.6

1.A ps shows 2 columns while ps aux shows 2928 columns.

2.A ? in the TTY column means the process is not attached to any controlling terminal

3.6928

4.Ss means the process is in interruptible sleep and is the session leader.

5./sbin/init

6.R means the process is running, and S means the process is asleep.

7.otelopscol

8.The three load average numbers represent the average number of processes waiting for CPU or I/O over the last 1,5 and 15 minutes.

9.Every 3 seconds

10.Job number and process ID

11.Ctrl+C immediately terminates a running process, while Ctrl+Z suspends it

12.the process is paused

13.Stopped

14.Changed from stopped to running

15.many jobs at once

16.when you run the kill command without specifying a signal, it defult sends the SIGTERM signal (signal number 15) to the target process

17.Kill terminate a process without claning it up

18.Stop cannot be blocked or ignored while TSTP can be blocked and ignored

19.killall terminate all process while kill terminate one process

20.a file named nohup.out to direct the command output

21.run a process without turning on the system

22.Niceness values range from 20 to +19. a higher niceness value means the process gets less CPU time and is considered less important

23.Only root can change a niceness value

24.There is no single niceness value that guarantees a process will run only when the CPU is completely idle. Niceness is a relative CPU priority hint, not an absolute “run only if idle” flag.