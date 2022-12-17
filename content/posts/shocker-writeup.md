

---
author: "FBueller"
title: "[HTB] Nibbles Writeup"
date: "2019-03-11"
description: "Sample article showcasing basic Markdown syntax and formatting for HTML elements."
FRtags: ["markdown", "css", "html", "themes"]
FRcategories: ["themes", "syntax"]
FRseries: ["Themes Guide"]
aliases: ["migrate-from-jekyl"]
ShowToc: true
TocOpen: true
weight: 2
---
###### reconancance 
- I decided to use nmap to do a port scan on the target machine 
- I will be using the following arguments "nmap -sCV -v3 $ip"
- "-sCV", the "C" will have nmap run its default script. The "V" argument is used to probe for service version information 
![[Screen Shot 2022-12-12 at 10.04.31 PM.png]]
```bash
set ip "10.10.10.75"
nmap -sCV -v3 at $ip -oG nibblescan.txt
```
- The "-v3" argument is to set the verbosity level to 3. All this will do is increase the amount of output displayed during the scan
###### nmap scan output:
```bash
╰─ nmap -sCV -v3 $ip -oG nible
Starting Nmap 7.93 ( https://nmap.org ) at 2022-12-12 21:57 PST
NSE: Loaded 155 scripts for scanning.
NSE: Script Pre-scanning.
NSE: Starting runlevel 1 (of 3) scan.
Initiating NSE at 21:57
Completed NSE at 21:57, 0.00s elapsed
NSE: Starting runlevel 2 (of 3) scan.
Initiating NSE at 21:57
Completed NSE at 21:57, 0.00s elapsed
NSE: Starting runlevel 3 (of 3) scan.
Initiating NSE at 21:57
Completed NSE at 21:57, 0.00s elapsed
Initiating Ping Scan at 21:57
Scanning 10.10.10.75 [2 ports]
Completed Ping Scan at 21:57, 0.08s elapsed (1 total hosts)
Initiating Connect Scan at 21:57
Scanning nibbleblog (10.10.10.75) [1000 ports]
Discovered open port 80/tcp on 10.10.10.75
Discovered open port 22/tcp on 10.10.10.75
Completed Connect Scan at 21:58, 17.82s elapsed (1000 total ports)
Initiating Service scan at 21:58
Scanning 2 services on nibbleblog (10.10.10.75)
Completed Service scan at 21:58, 6.17s elapsed (2 services on 1 host)
NSE: Script scanning 10.10.10.75.
NSE: Starting runlevel 1 (of 3) scan.
Initiating NSE at 21:58
Completed NSE at 21:58, 2.45s elapsed
NSE: Starting runlevel 2 (of 3) scan.
Initiating NSE at 21:58
Completed NSE at 21:58, 0.34s elapsed
NSE: Starting runlevel 3 (of 3) scan.
Initiating NSE at 21:58
Completed NSE at 21:58, 0.00s elapsed
Nmap scan report for nibbleblog (10.10.10.75)
Host is up, received syn-ack (0.078s latency).
Scanned at 2022-12-12 21:57:52 PST for 27s
Not shown: 998 closed tcp ports (conn-refused)
PORT   STATE SERVICE REASON  VERSION
22/tcp open  ssh     syn-ack OpenSSH 7.2p2 Ubuntu 4ubuntu2.2 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey:
|   2048 c4f8ade8f80477decf150d630a187e49 (RSA)
| ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQD8ArTOHWzqhwcyAZWc2CmxfLmVVTwfLZf0zhCBREGCpS2WC3NhAKQ2zefCHCU8XTC8hY9ta5ocU+p7S52OGHlaG7HuA5Xlnihl1INNsMX7gpNcfQEYnyby+hjHWPLo4++fAyO/lB8NammyA13MzvJy8pxvB9gmCJhVPaFzG5yX6Ly8OIsvVDk+qVa5eLCIua1E7WGACUlmkEGljDvzOaBdogMQZ8TGBTqNZbShnFH1WsUxBtJNRtYfeeGjztKTQqqj4WD5atU8dqV/iwmTylpE7wdHZ+38ckuYL9dmUPLh4Li2ZgdY6XniVOBGthY5a2uJ2OFp2xe1WS9KvbYjJ/tH
|   256 228fb197bf0f1708fc7e2c8fe9773a48 (ECDSA)
| ecdsa-sha2-nistp256 AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTYAAABBBPiFJd2F35NPKIQxKMHrgPzVzoNHOJtTtM+zlwVfxzvcXPFFuQrOL7X6Mi9YQF9QRVJpwtmV9KAtWltmk3qm4oc=
|   256 e6ac27a3b5a9f1123c34a55d5beb3de9 (ED25519)
|_ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIC/RjKhT/2YPlCgFQLx+gOXhC6W3A3raTzjlXQMT8Msk
80/tcp open  http    syn-ack Apache httpd 2.4.18 ((Ubuntu))
|_http-title: Site doesn't have a title (text/html).
|_http-server-header: Apache/2.4.18 (Ubuntu)
| http-methods:
|_  Supported Methods: GET HEAD POST OPTIONS
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

NSE: Script Post-scanning.
NSE: Starting runlevel 1 (of 3) scan.
Initiating NSE at 21:58
Completed NSE at 21:58, 0.00s elapsed
NSE: Starting runlevel 2 (of 3) scan.
Initiating NSE at 21:58
Completed NSE at 21:58, 0.00s elapsed
NSE: Starting runlevel 3 (of 3) scan.
Initiating NSE at 21:58
Completed NSE at 21:58, 0.00s elapsed
Read data files from: /usr/local/bin/../share/nmap
Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 27.69 seconds
```
##### breakdown
- looking out the output from the nmap scan we can see that there are two known ports that are open (port 22, 80)
- you can also see what version of apache that is being ran (apache 2.14.18) 
- it can also be observed that it is running on ubuntu linux
- after visting the web page that we found (port 80 being open let us know something was at http://<the_ip_address>:80)![[Screen Shot 2022-12-12 at 10.13.10 PM.png]]
###### Directory bruteforcing
- we can now find any hidden directorries did not get exposed during the nmap scan using a directory bruteforceing tool
- the tool I will be using for this is called dirbister 
