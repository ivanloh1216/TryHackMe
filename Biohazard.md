# Biohazard

[Link to room](https://tryhackme.com/room/biohazard)

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
### 1. How many open ports?

> Refer to the nmap results.

### 2. What is the team name in operation?

> Check the home page of the website.

## Task 2: The Mansion

### 1. What is the emblem flag?

By visiting the `http://<Machine's IP Address>/`, we can see that there is a link to the mansion. The link will lead to `http://<Machine's IP Address>/mansionmain/`.
By inspecting the page source, we will see the instruction to go to the next room.

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
Then, navigate to `http://<Machine's IP Address>/diningRoom/`.
The website mentioned that there's an emblem on the wall, followed by an embedded link. Click on the link and you will get the emblem flag. 

![image](https://github.com/ivanloh1216/TryHackMe/assets/58873811/dae48857-e0f0-4679-bbac-9b27647e40f2)

We refresh the Dining Room page and try to enter the emblem flag into the input field, but it shows "Nothing happens". So we will come back to this later.

![image](https://github.com/ivanloh1216/TryHackMe/assets/58873811/c5971bb7-f5dc-4078-9bd8-023886214c78)

Then, inspect the page source again and you will find an encoded text. 

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

### 2. What is the lock pick flag?

According to the link decoded from the earlier step, you should be on the Tea Room page. `http://<Machine's IP Address>/teaRoom/`

![image](https://github.com/ivanloh1216/TryHackMe/assets/58873811/c6e66d57-3c79-440d-ae8f-da89b121c48d)

Read the content of the website and you will find an embedded link for the Lockpick. Click on it to get the lock pick flag. 

Then, follow the instruction on the website and you should be on the next page, Art room. `http://<Machine's IP Address>/artRoom/`

![image](https://github.com/ivanloh1216/TryHackMe/assets/58873811/7561f652-8589-4c23-b2b3-b737ab84c9da)

There is another embedded link, which will bring you to a map. `http://<Machine's IP Address>/artRoom/MansionMap.html`
By following the directories in the map in order, we shall proceed to find the remaining flags. 

![image](https://github.com/ivanloh1216/TryHackMe/assets/58873811/958716b2-e0c3-4bd9-9160-dff8d880b953)

### 3. What is the music sheet flag?

By following the directories on the map in order, we should be in the Bar Room. Hence, navigate to `http://<Machine's IP Address>/barRoom/`.

![image](https://github.com/ivanloh1216/TryHackMe/assets/58873811/1fd3e2c0-abbf-41dc-8b83-734a7669ba17)

We will be able to find an input field to unlock the door, hence we enter the lock pick flag found earlier.
This will unlock the next page, which is the Bar Room. 

![image](https://github.com/ivanloh1216/TryHackMe/assets/58873811/489c220d-69bd-47a1-ae4c-ef45db0058fa)

Notice that there is another embedded link, click on it and it will open another page with the following information.

```
Look like a music note
NV2XG2LDL5ZWQZLFOR5TGNRSMQ3TEZDFMFTDMNLGGVRGIYZWGNSGCZLDMU3GCMLGGY3TMZL5
```

This is another encoded text hence we will use [CyberChef](https://gchq.github.io/CyberChef/) to decode it. Once decoded, you will see the music sheet flag.

### 4. What is the gold emblem flag?

Continuing from the previous step, we shall enter the music sheet flag into the input field. This will open up another page. 

![image](https://github.com/ivanloh1216/TryHackMe/assets/58873811/2d9a14d6-ed82-4e72-9daa-bc4f06529ece)

Click on the embedded link to obtain the gold emblem flag.

![image](https://github.com/ivanloh1216/TryHackMe/assets/58873811/ebaba4fe-adba-49fb-8349-a2ece126d0e7)

### 5. What is the shield key flag? 

Continuing from the previous step, we are instructed to refresh the previous page. Hence, we do as instructed and shall see the following input field.

![image](https://github.com/ivanloh1216/TryHackMe/assets/58873811/d8a6cf8e-befa-4b35-b1c2-a2341cded9db)

Take note that the website asked us to enter the emblem flag, hence we should enter the very first flag we obtained and not the golden emblem flag.
By entering the emblem flag as required, we will see another page loaded with the name "Rebecca". We do not know what to do with this at the moment 
so keep this information for now. 

Remember in the earlier steps, we were unable to enter the correct input in the Dining Room? Let's navigate back to the Dining Room page since we have
found another emblem now, which is the `Golden Emblem`. Then, enter the `Golden Emblem` flag and you will see `klfvg ks r wimgnd biz mpuiui ulg fiemok tqod. Xii jvmc tbkg ks tempgf tyi_hvgct_jljinf_kvc` on the next page. This is again an encrypted text hence we will use [CyberChef](https://gchq.github.io/CyberChef/) to decrypt it. This time, we select the Vigenere Decoder.

![image](https://github.com/ivanloh1216/TryHackMe/assets/58873811/f34eeac3-a49e-4754-9395-08a1cc01b2c9)

The hint we obtained here shows where to find the shield key flag. Hence, go to `http://<Machine's IP Address>/diningRoom/the_great_shield_key.html` 
and we will get the shield key flag. 

### 6. What is the blue gem flag?

According to the Map earlier, we should be in the Dining Room 2F now. Hence, navigate to `http://<Machine's IP Address>/diningRoom2F/`.
We shall see that there is no information on the website itself. Hence, as usual, let's inspect the page source.

```
<html>
        <head>
                <title>Dining room 2F</title>
                <h1 align="center">Dining room 2F</h1>
        </head>

        <body>
        <img alt="dining room 2F" src="../images/Vlcsnap-2015-01-26-08h54m37s183.png" style="display: block;margin-left: auto;margin-right: auto; width: 50%;"/>

	<p>Once Jill reach the room, she saw a tall status with a shiining blue gem on top of it. However, she can't reach it</p>
	<!-- Lbh trg gur oyhr trz ol chfuvat gur fgnghf gb gur ybjre sybbe. Gur trz vf ba gur qvavatEbbz svefg sybbe. Ivfvg fnccuver.ugzy -->       
        </body>

</html>
```

Notice there is yet another encrypted message commented in the HTML codes. Using [CyberChef](https://gchq.github.io/CyberChef/), we obtained the following information. 

![image](https://github.com/ivanloh1216/TryHackMe/assets/58873811/9d217bb4-b433-4d18-b8da-0b6e84b379f5)

Hence, navigate to `http://<Machine's IP Address>/diningRoom/sapphire.html` to find the blue gem flag. 

### 7. What is the FTP username and FTP password?

We should be on the Tiger status room now, hence navigate to `http://<Machine's IP Address>/tigerStatusRoom/`. 

![image](https://github.com/ivanloh1216/TryHackMe/assets/58873811/0b7cbe41-6109-49df-9abb-ae5a29639540)

Enter the blue gem flag we obtained in the previous step. We will get the following information. 

```
crest 1:
S0pXRkVVS0pKQkxIVVdTWUpFM0VTUlk9
Hint 1: Crest 1 has been encoded twice
Hint 2: Crest 1 contanis 14 letters
Note: You need to collect all 4 crests, combine and decode to reavel another path
The combination should be crest 1 + crest 2 + crest 3 + crest 4. Also, the combination is a type of encoded base and you need to decode it
```

Keep this for now, let's navigate to the next page, `http://<Machine's IP Address>/galleryRoom/`.

![image](https://github.com/ivanloh1216/TryHackMe/assets/58873811/d29ac127-b451-4fe7-bb82-8f9bee53ae32)

Click on the embedded link and you will find this.

```
crest 2:
GVFWK5KHK5WTGTCILE4DKY3DNN4GQQRTM5AVCTKE
Hint 1: Crest 2 has been encoded twice
Hint 2: Crest 2 contanis 18 letters
Note: You need to collect all 4 crests, combine and decode to reavel another path
The combination should be crest 1 + crest 2 + crest 3 + crest 4. Also, the combination is a type of encoded base and you need to decode it
```

Again, let's keep this information for now. Let's navigate to the next page, `http://<Machine's IP Address>/studyRoom/`.

![image](https://github.com/ivanloh1216/TryHackMe/assets/58873811/528aa676-417b-4d6c-8c43-7db7e9f3a3e5)

We haven't found any information about helmet, so we skip this for now.

Then, navigate to the next page, `http://<Machine's IP Address>/armorRoom/`.

![image](https://github.com/ivanloh1216/TryHackMe/assets/58873811/8118433c-e3c0-44ca-a7ee-8719d95dff54)

Enter the shield flag obtained in the previous steps, and we will see this page. 

![image](https://github.com/ivanloh1216/TryHackMe/assets/58873811/f672f219-1eab-4657-a11e-726a8a83e532)

Click on the embedded link and we will find this. 

```
crest 3:
MDAxMTAxMTAgMDAxMTAwMTEgMDAxMDAwMDAgMDAxMTAwMTEgMDAxMTAwMTEgMDAxMDAwMDAgMDAxMTAxMDAgMDExMDAxMDAgMDAxMDAwMDAgMDAxMTAwMTEgMDAxMTAxMTAgMDAxMDAwMDAgMDAxMTAxMDAgMDAxMTEwMDEgMDAxMDAwMDAgMDAxMTAxMDAgMDAxMTEwMDAgMDAxMDAwMDAgMDAxMTAxMTAgMDExMDAwMTEgMDAxMDAwMDAgMDAxMTAxMTEgMDAxMTAxMTAgMDAxMDAwMDAgMDAxMTAxMTAgMDAxMTAxMDAgMDAxMDAwMDAgMDAxMTAxMDEgMDAxMTAxMTAgMDAxMDAwMDAgMDAxMTAwMTEgMDAxMTEwMDEgMDAxMDAwMDAgMDAxMTAxMTAgMDExMDAwMDEgMDAxMDAwMDAgMDAxMTAxMDEgMDAxMTEwMDEgMDAxMDAwMDAgMDAxMTAxMDEgMDAxMTAxMTEgMDAxMDAwMDAgMDAxMTAwMTEgMDAxMTAxMDEgMDAxMDAwMDAgMDAxMTAwMTEgMDAxMTAwMDAgMDAxMDAwMDAgMDAxMTAxMDEgMDAxMTEwMDAgMDAxMDAwMDAgMDAxMTAwMTEgMDAxMTAwMTAgMDAxMDAwMDAgMDAxMTAxMTAgMDAxMTEwMDA=
Hint 1: Crest 3 has been encoded three times
Hint 2: Crest 3 contanis 19 letters
Note: You need to collect all 4 crests, combine and decode to reavel another path
The combination should be crest 1 + crest 2 + crest 3 + crest 4. Also, the combination is a type of encoded base and you need to decode it

```

Again, keep it for now, let's navigate to the final page on the map, `http://<Machine's IP Address>/armorRoom/`. 

![image](https://github.com/ivanloh1216/TryHackMe/assets/58873811/63e3a4e1-bd5b-4cba-b101-ae73a129529a)

Enter the shield flag obtained in the previous steps, and we will see this page.

![image](https://github.com/ivanloh1216/TryHackMe/assets/58873811/5e613a5d-310b-43d1-9636-889a40a0732d)

Click on the embedded link and we will find this.

```
crest 4:
gSUERauVpvKzRpyPpuYz66JDmRTbJubaoArM6CAQsnVwte6zF9J4GGYyun3k5qM9ma4s
Hint 1: Crest 2 has been encoded twice
Hint 2: Crest 2 contanis 17 characters
Note: You need to collect all 4 crests, combine and decode to reavel another path
The combination should be crest 1 + crest 2 + crest 3 + crest 4. Also, the combination is a type of encoded base and you need to decode it

```

Now that we have all 4 crests, let's try to string them together.

1. Crest 1: `S0pXRkVVS0pKQkxIVVdTWUpFM0VTUlk9`, decode this twice to get `RlRQIHVzZXI6IG`.
2. Crest 2: `GVFWK5KHK5WTGTCILE4DKY3DNN4GQQRTM5AVCTKE`, decode this twice to get `h1bnRlciwgRlRQIHBh`.
3. Crest 3: `MDAxMTAxMTAgMDAxMTAwMTEgMDAxMDAwMDAgMDAxMTAwMTEgMDAxMTAwMTEgMDAxMDAwMDAgMDAxMTAxMDAgMDExMDAxMDAgMDAxMDAwMDAgMDAxMTAwMTEgMDAxMTAxMTAgMDAxMDAwMDAgMDAxMTAxMDAgMDAxMTEwMDEgMDAxMDAwMDAgMDAxMTAxMDAgMDAxMTEwMDAgMDAxMDAwMDAgMDAxMTAxMTAgMDExMDAwMTEgMDAxMDAwMDAgMDAxMTAxMTEgMDAxMTAxMTAgMDAxMDAwMDAgMDAxMTAxMTAgMDAxMTAxMDAgMDAxMDAwMDAgMDAxMTAxMDEgMDAxMTAxMTAgMDAxMDAwMDAgMDAxMTAwMTEgMDAxMTEwMDEgMDAxMDAwMDAgMDAxMTAxMTAgMDExMDAwMDEgMDAxMDAwMDAgMDAxMTAxMDEgMDAxMTEwMDEgMDAxMDAwMDAgMDAxMTAxMDEgMDAxMTAxMTEgMDAxMDAwMDAgMDAxMTAwMTEgMDAxMTAxMDEgMDAxMDAwMDAgMDAxMTAwMTEgMDAxMTAwMDAgMDAxMDAwMDAgMDAxMTAxMDEgMDAxMTEwMDAgMDAxMDAwMDAgMDAxMTAwMTEgMDAxMTAwMTAgMDAxMDAwMDAgMDAxMTAxMTAgMDAxMTEwMDA=`, decode this thrice to get `c3M6IHlvdV9jYW50X2h`. 
4. Crest 4: `gSUERauVpvKzRpyPpuYz66JDmRTbJubaoArM6CAQsnVwte6zF9J4GGYyun3k5qM9ma4s`, decode this twice to get `pZGVfZm9yZXZlcg==`.

Finally, concatenate all the decoded text to become `RlRQIHVzZXI6IGh1bnRlciwgRlRQIHBhc3M6IHlvdV9jYW50X2hpZGVfZm9yZXZlcg==`. Decode that once and we will get the FTP username and FTP password. 

## Task 3: The guard house

### 1. Where is the hidden directory mentioned by Barry?

Now that we got the credentials to access the FTP server, let's find out what we have in the FTP server.

![image](https://github.com/ivanloh1216/TryHackMe/assets/58873811/1c05eb5b-0c53-4a52-b6e4-6b340644a2ba)

Using the command `ls`, we can see that we have got some files that can be downloaded here. Hence, go ahead and download all the files using the `get` followed by the filename for each file.

#### Contents of `important.txt`

```
Jill,

I think the helmet key is inside the text file, but I have no clue on decrypting stuff. Also, I come across a /h*****_c***et/ door but it was locked.

From,
Barry
```
Hence, we can find the hidden directory in the `important.txt`.

### 2. Password for the encrypted file?

We have another 3 jpg images which are important. 

![001-key](https://github.com/ivanloh1216/TryHackMe/assets/58873811/36b955c5-f9dd-4e0c-a147-f61af244c503)
![002-key](https://github.com/ivanloh1216/TryHackMe/assets/58873811/13c647b2-9e07-4680-91c4-5e71c6ada207)
![003-key](https://github.com/ivanloh1216/TryHackMe/assets/58873811/f1234abb-290a-4c8b-90c6-4c44758ca3f8)

1. For the first key, let's use `steghide` to obtain the secret information embedded in this image. 
![image](https://github.com/ivanloh1216/TryHackMe/assets/58873811/36800575-8650-41a9-90c3-5658d9a86047)

We obtained a partial key here, `cGxhbnQ0Ml9jYW`, keep it for later. 

2. For the second key, let's use `exiftool` to check for the important message in this image.
![image](https://github.com/ivanloh1216/TryHackMe/assets/58873811/4ca53b3c-b884-440c-98b9-2da9bfc96da8)

We obtained another partial key here, `5fYmVfZGVzdHJveV9`, keep it for later. 

3. For the third key, let's use `binwalk` to extract any important information in this image.
![image](https://github.com/ivanloh1216/TryHackMe/assets/58873811/e5036b75-0470-41fa-b732-65fbd31117c7)

We obtained the final part of the key, `3aXRoX3Zqb2x0`. 

Again, let's concatenate all three strings, `cGxhbnQ0Ml9jYW5fYmVfZGVzdHJveV93aXRoX3Zqb2x0`, and decode them in [CyberChef](https://gchq.github.io/CyberChef/). 

Hence, we will get the password for the encrypted file. 

### 3. What is the helmet key flag?

Remember we downloaded another file from the FTP server? The filename is `helmet_key.txt.gpg`. This is an encrypted file using `gpg`. Let's decrypt it to see the information inside. Type in the command `gpg --output message.txt --decrypt helmet_key.txt.gpg`. This will output the information of the encryption file into `message.txt`. 

![image](https://github.com/ivanloh1216/TryHackMe/assets/58873811/be68f39e-a2bf-4dd8-a043-e8dd47809c7e)

Open the `message.txt` and we will find the helmet key flag. 

## Task 4: The Revisit

### 1. What is the SSH login username?

Navigate to the secret directory. 

![image](https://github.com/ivanloh1216/TryHackMe/assets/58873811/2e8f8bc9-c437-4604-b28b-4c44ed99fe5e)

Enter the helmet flag we obtained. 

![image](https://github.com/ivanloh1216/TryHackMe/assets/58873811/ea89e624-5e55-43e8-8021-8b3143e2ddca)

We will find two embedded links here. Click each of the links to find out what is inside.

#### First link

```
wpbwbxr wpkzg pltwnhro, txrks_xfqsxrd_bvv_fy_rvmexa_ajk 
```
This is another ciphertext. Use [Vigenere Decoder](https://www.dcode.fr/vigenere-cipher) to decrypt it. We shall have an important piece of information. We do not know what to do with it at this point so let's save it for later.

#### Second link

```
SSH password: T_v****_r****
```
This is the SSH password.

Remember we have one more website that we did not message to get information from it earlier? Navigate to `http//<Machine's IP Address>/studyRoom/`.
Now that we have the helmet flag, let's input the helmet flag and see what we have.

![image](https://github.com/ivanloh1216/TryHackMe/assets/58873811/4c6faa91-df86-48e7-8c0e-b18e7ecc5dfb)

We will get another website with an embedded link. By clicking on the link, we will download a zipped file. Inside the zipped file, we will have the information for the SSH login username. 

## 2. What is the SSH login password?

> Refer to the previous step.

### 3. Who the STARS bravo team leader?

![image](https://github.com/ivanloh1216/TryHackMe/assets/58873811/84469982-af41-402f-800b-db37609b2c31)

> Find this on the website.

## Task 5: Underground laboratory

### 1. Where you found Chris? 

Now that we have the SSH credentials, let's log in to the SSH server.

![image](https://github.com/ivanloh1216/TryHackMe/assets/58873811/5650cff6-6925-4736-9f54-873c30d4ac60)

Initially when we perform `ls`, there is nothing shown. Hence, it suggests that there might be hidden files or directories. Then, perform `ls -al` to list down the files. One file that stood out is the `.jailcell` file. Let's navigate to the file. We will see that there is a `chris.txt` file. Let's go ahead and see what is inside.

![image](https://github.com/ivanloh1216/TryHackMe/assets/58873811/90ebd083-841e-4735-80e4-10b79784dff0)

Here we can find out where Chris is and who the traitor is. 

### 2. Who is the traitor?

> Refer to the previous step.

### 3. The login password for the traitor?

> Refer to the decrypted information in earlier steps.

### 4. The name of the ultimate form?

Now that we have weasker's SSH credentials, let's log in to the SSH using his credentials.

![image](https://github.com/ivanloh1216/TryHackMe/assets/58873811/12a9f4c0-1eb2-4499-ae24-bbb11bcf35dd)

By listing down the files in the directories, we can see that there is a `weasker_note.txt`. Let's see what is inside.

![image](https://github.com/ivanloh1216/TryHackMe/assets/58873811/febaa6b7-ae83-4979-acbf-10d72c49fc45)

Here we will have the information for the name of the ultimate form.

### 5. The root flag?

Let's try to go to the root directory. 

![image](https://github.com/ivanloh1216/TryHackMe/assets/58873811/8b685b4c-dcd0-42e8-913c-98f0c971473e)

We can see that there is a `root` folder. However, `weasker` does not have the permission to open that folder. Hence, we will try to change to `root` account using `sudo su`. 

![image](https://github.com/ivanloh1216/TryHackMe/assets/58873811/96d5fa46-9b45-48b0-ad12-2c8867745a13)

After entering weasker's password, we successfully upgrade ourselves to a root user. We can then open the `root` folder. In the `root` folder, we can see there is a `root.txt` file. Let's try to see if we can find the root flag inside by using `cat root.txt`. Finally, the root flag is shown. 



