# Signals in Linux 
 
## Overview 
Signals are a way of sending simple messages to processes. Most of these messages are already defined and can be found in `<linux/signal.h>`. However, signals can only be processed when the process is in user mode. If a signal has been sent to a process that is in kernel mode, it is dealt with immediately on returning to user mode. 
 
Every signal has a unique signal name, an abbreviation that begins with SIG (SIGINT for interrupt signal, for example). Each signal name is a macro which stands for a positive integer - the signal number for that kind of signal. Your programs should never make assumptions about the numeric code for a particular kind of signal, but rather refer to them always by the names defined. This is because the number for a given kind of signal can vary from system to system, but the meanings of the names are standardized and fairly uniform. 
 
Signals can be generated by the process itself, or they can be sent from one process to another. A variety of signals can be generated or delivered, and they have many uses for programmers. (To see a complete list of signals in the Linux environment, uses the command kill -l.) 

* There are total 64 signals in Linux, the list of all the signal can be sen by  
`$ kill –l`
 ```
 $ kill -l
 1) SIGHUP       2) SIGINT       3) SIGQUIT      4) SIGILL       5) SIGTRAP
 6) SIGABRT      7) SIGBUS       8) SIGFPE       9) SIGKILL     10) SIGUSR1
11) SIGSEGV     12) SIGUSR2     13) SIGPIPE     14) SIGALRM     15) SIGTERM
16) SIGSTKFLT   17) SIGCHLD     18) SIGCONT     19) SIGSTOP     20) SIGTSTP
21) SIGTTIN     22) SIGTTOU     23) SIGURG      24) SIGXCPU     25) SIGXFSZ
26) SIGVTALRM   27) SIGPROF     28) SIGWINCH    29) SIGIO       30) SIGPWR
31) SIGSYS      34) SIGRTMIN    35) SIGRTMIN+1  36) SIGRTMIN+2  37) SIGRTMIN+3
38) SIGRTMIN+4  39) SIGRTMIN+5  40) SIGRTMIN+6  41) SIGRTMIN+7  42) SIGRTMIN+8
43) SIGRTMIN+9  44) SIGRTMIN+10 45) SIGRTMIN+11 46) SIGRTMIN+12 47) SIGRTMIN+13
48) SIGRTMIN+14 49) SIGRTMIN+15 50) SIGRTMAX-14 51) SIGRTMAX-13 52) SIGRTMAX-12
53) SIGRTMAX-11 54) SIGRTMAX-10 55) SIGRTMAX-9  56) SIGRTMAX-8  57) SIGRTMAX-7
58) SIGRTMAX-6  59) SIGRTMAX-5  60) SIGRTMAX-4  61) SIGRTMAX-3  62) SIGRTMAX-2
63) SIGRTMAX-1  64) SIGRTMAX
 ```

## Few Important Signals with its descriptions: 

 | Signal | Value | Action | Comment|
 | :--:| :--:| :--:| :--|
 |SIGHUP|1|Term|Hangup detected on controlling terminal or death of controlling process|
 |sIGINT|2|Term|Interrupt from keyboard|
 |SIGQUIT|3|Core|Quit form keyboard|
 |SIGILL|4|Core|Illegal Instruction |
 |SIGABRT|6|Core|Abort signal form Abort(3)|
 |SIGFPE|8|Core|Floating point exception|
 |SIGKILL|9|Term|Kill signal|
 |SIGSEGV|11|Core|Invalid memory reference|
 |SIGPIPE|13|Term|Broken pipe: write to pipe with no readers|
 |SIGALRM|14|Term|Timer signal form alarm(2)|
 |SIGTERM|15|Term|Termination signal |
 |SIGUSR1|30,10,16|Term|User-defined signal-1|
 |SIGUSR2|31,12,17|Term|User-defined signal-2|
 |SIGCHLD|20,17,18|Ign|child stopped or terminated|
 |SIGCONT|19,18,25|cont|continue if stopped|
 |SIGSTOP|17,19,23|Stop|stop process|
 |SIGTSTP|18,20,24|Stop|stop typed at tty|
 |SIGTTIN|21,21,26|Stop|tty input for background process|
 |SIGTTOU|22,22,27|Stop|tty output for background process|

## Using most common signals

|value | usage|
| :--: | :--|
|1 |Reloading  process| 
|9|Killing  process|
|15|Terminating process|
|20|Stopping process |

## Working with signals 

* To kill the process completely  
    * First find out the process running in the system   
    $ `ps –u <user name>`  
    $ `kill <signal no> <process id>`  
```
$ ps -u
USER       PID %CPU %MEM    VSZ   RSS TTY      STAT START   TIME COMMAND
krishna+    60  0.9  0.0  15104  3628 tty1     S    15:22   0:00 -bash
krishna+    75  4.0  0.0  24152  5348 tty1     T    15:22   0:00 vi newfile
krishna+    76  0.0  0.0  15664  1848 tty1     R    15:22   0:00 ps -u

[1]+  Stopped                 vi newfile

$ kill -9 75
[1]+  Killed                  vi newfile
```

* To Terminate the process 
```
$ ps -u
USER       PID %CPU %MEM    VSZ   RSS TTY      STAT START   TIME COMMAND
krishna+    60  0.1  0.0  15104  3644 tty1     S    15:22   0:00 -bash
krishna+    78  1.0  0.0  12196   788 tty1     T    15:24   0:00 cat
krishna+    79  0.0  0.0  15664  1844 tty1     R    15:24   0:00 ps -u

[1]+  Stopped                 cat
$ kill -15 78
$ ps -u
USER       PID %CPU %MEM    VSZ   RSS TTY      STAT START   TIME COMMAND
krishna+    60  0.1  0.0  15104  3644 tty1     S    15:22   0:00 -bash
krishna+    80  0.0  0.0  15664  1848 tty1     R    15:25   0:00 ps -u
[1]+  Terminated              cat
```

* To stop the process
    * First login as a normal user and start a process   
    `su krishnaprasadkv`  
    `cat >testing.txt`  
    
    * open one more terminal and change to root   
    * Check its pid and kill it by using `20`.  
    `ps -u krishnaprasad `  
    ```
    root@DESKTOP-JDIP4H0:~# ps -u krishnaprasadkv
    PID TTY          TIME CMD
    8 tty1     00:00:02 bash
    301 tty1     00:00:00 crontab
    302 tty1     00:00:00 sh
    303 tty1     00:00:00 sensible-editor
    311 tty1     00:00:00 select-editor
    408 tty1     00:00:00 crontab
    409 tty1     00:00:00 sh
    410 tty1     00:00:00 sensible-editor
    418 tty1     00:00:00 editor
    1170 tty1     00:00:00 bash
    1181 tty1     00:00:00 cat
    1183 tty2     00:00:00 bash
    root@DESKTOP-JDIP4H0:~#
    ```  
    `root@DESKTOP-JDIP4H0:~# kill -20 1181`  
    
    * Change to user console and check its effect or not   
    ```
    cat > hello
    [1]+  Stopped                 cat > hello
    ```
    
    * Restart the process continue working type the below command in user console  
    `fg 1`  
    ```
    cat > hello
    ```

## Setting up the priority of a process
Linux can run a lot of processes at a time, which can slow down the speed of some high priority processes and result in poor performance.

To avoid this, you can tell your machine to prioritize processes as per your requirements.

This priority is called Niceness in Linux, and it has a value between -20 to 19. The lower the Niceness index, the higher would be a priority given to that task.

The default value of all the processes is 0.

### Working with renice

* To schedule a priority of a process before starting it   
    * To set a priority to a process before starting it, the syntax is   
    $ `nice –n <nice value range (-20 to 19)>  <command>`  
    $ `nice –n  5  cat > file.txt`

    * Log in to other terminal and check the nice value for the above command/ process.  
    $ `ps –elf`

* To change the nice value of any process while it is running  
    * To reschedule the nice value of existing process, first check the PID of that process by running $`ps –elf ` command.   
    * As from previous task we know the PID of cat command i.e. 1277   
    * Use the following command to renice the value of a cat command which is still running.  
    $ `renice <nice value (-20 to 19)> <PID>`    
    $ `renice 2 1277`  
    ```
    krishnaprasadkv@DESKTOP-JDIP4H0:~/test$ renice 2 1277
    1277 (process ID) old priority 0, new priority 2
    ```