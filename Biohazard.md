
# Biohazard

[link to room](https://tryhackme.com/room/biohazard)

## Task 1: Introduction

```
sudo nmap -sC -sV 10.10.3.151          
Starting Nmap 7.94 ( https://nmap.org ) at 2023-08-01 00:06 EDT
Nmap scan report for 10.10.3.151
Host is up (0.20s latency).
Not shown: 996 closed tcp ports (reset)
PORT     STATE    SERVICE    VERSION
21/tcp   open     ftp        vsftpd 3.0.3
22/tcp   open     ssh        OpenSSH 7.6p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 c9:03:aa:aa:ea:a9:f1:f4:09:79:c0:47:41:16:f1:9b (RSA)
|   256 2e:1d:83:11:65:03:b4:78:e9:6d:94:d1:3b:db:f4:d6 (ECDSA)
|_  256 91:3d:e4:4f:ab:aa:e2:9e:44:af:d3:57:86:70:bc:39 (ED25519)
80/tcp   open     http       Apache httpd 2.4.29 ((Ubuntu))
|_http-title: Beginning of the end
|_http-server-header: Apache/2.4.29 (Ubuntu)
1352/tcp filtered lotusnotes
Service Info: OSs: Unix, Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 370.20 seconds

```
1. How many open ports?

> Refer to the nmap results.

2. What is the team name in operation?

> Check the home page of the website.

## Task 2: The Mansion

1. What is the emblem flag?

By visiting the http://<Machine's IP Address>/, we can see that there is a link to the mansion. The link will lead to http://<Machine's IP Address>/mansionmain/.
By inspecting the page source, we will see "<!-- It is in the /diningRoom/ -->" in the html codes. 

```
<!doctype html>
        <head>
                <title>Main hall</title>
                <h1 align="center">Main hall</h1>
        </head>

        <body>
        <img alt="mainhall" src="../images/Mainhall12.jpg" style="display: block;margin-left: auto;margin-right: auto; width: 50%;"/>

        <p>The team reach the mansion safe and sound. However, it appear that Chris is missing</p>
	<p>Jill try to open the door but stopped by Weasker</p>
        <p>Suddenly, a gunshot can be heard in the nearby room. Weaker order Jill to make an investigate on the gunshot. Where is the room?</p>
	<!-- It is in the /diningRoom/ -->
        </body>

</html>
```
Then, navigate to http://<Machine's IP Address>/diningRoom/.
The website mentioned that there's an emblem on the wall, followed by an embedded link. Click on the link and you will get the emblem flag. 

Then, inspect the page source again and you will find "<!-- SG93IGFib3V0IHRoZSAvdGVhUm9vbS8= -->". This is an encoded text. 

```
<html>
        <head>
                <title>Dining room</title>
                <h1 align="center">Dining room</h1>
        </head>

        <body>
        	<img alt="diningroom" src="../images/maxresdefault.jpg" style="display: block;margin-left: auto;margin-right: auto; width: 50%;"/>

        	<p>After reaching the room, Jill and Barry started their investigation</p>
		<p>Blood stein can be found near the fireplace. Hope it is not belong to Chris.</p>
		<p>After a short investigation with barry, Jill can't find any empty shell. Maybe another room?</p>
		<!-- SG93IGFib3V0IHRoZSAvdGVhUm9vbS8= -->
        </body>

	There is an emblem slot on the wall, put the emblem?			<form action="emblem_slot.php" method="POST">
			<input type="text" name="emblem_slot" col="100" placeholder="Input flag"><br>
			<input type="submit" value="submit">
			</form>
	
</html>

```

Hence, we will use [CyberChef](https://gchq.github.io/CyberChef/) to decode it. Once decoded, you will find the link to the next page.

2. What is the lock pick flag?

According to the link decoded from the earlier step, you should be on the Tea Room page. (http://<Machine's IP Address>/teaRoom/)


Read the content of the website and you will find an embedded link for the Lockpick. Click on it to get the lock pick flag. 

Then, follow the instruction on the website and you should be on the next page, Art room. (http://<Machine's IP Address>/artRoom/)
There is another embedded link, which will bring you to a map. (http://<Machine's IP Address>/artRoom/MansionMap.html)
By following the links in the map in order, we shall proceed to find the remaining flags. 
