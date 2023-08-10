# Lian_Yu

[Link to room](https://tryhackme.com/room/lianyu)

## Task 1 Find the Flags

### 1. Deploy the VM and Start the Enumeration.

> No answers needed.

Let's start with some basic enumeration. First, I visited the IP address on the web browser. It was a simple webpage with a nice ocean view.

![image](https://github.com/ivanloh1216/TryHackMe/assets/58873811/9010ff8c-e144-464c-bc08-7ac4dddfb6f5)

Then, I did a Nmap scan and found a few open ports, ports 21, 22, 80, and 111. 

![image](https://github.com/ivanloh1216/TryHackMe/assets/58873811/c4312825-ae73-4d87-8470-beb7ee32e991)

Next, I did a Gobuster enumeration and found a result. `/island/`

![image](https://github.com/ivanloh1216/TryHackMe/assets/58873811/3e9409c1-a310-4db8-a979-f8c6ee3b6938)

Going to the page on a browser gets the below output. 

![image](https://github.com/ivanloh1216/TryHackMe/assets/58873811/e3f7ad54-ca6c-4543-a722-98d1fab29b11)

No information can be found on the website, however by checking the page source, I found a word in white color (hidden when viewed in the browser). 

![image](https://github.com/ivanloh1216/TryHackMe/assets/58873811/c87447a2-2835-4731-8dda-8ad5b132757b)

I am not sure what to do with the word `vigilante` at the moment, let's continue with some further enumeration first. Let's perform one more Gobuster enumeration on the new directory.

![image](https://github.com/ivanloh1216/TryHackMe/assets/58873811/6d57a6e6-2fb3-4c86-bcce-9873ec725fb7)

We got a new subdirectory, `/island/2100`. Let's visit it.

![image](https://github.com/ivanloh1216/TryHackMe/assets/58873811/edd5a872-293a-452c-a3a4-ceb1c31f304c)

The website shows not much information. Let's check the page source. 

![image](https://github.com/ivanloh1216/TryHackMe/assets/58873811/8330fc2c-1a27-4cdc-ae21-05099bb1524f)

According to the page source, we can perform further enumeration with the new subdirectory. The keyword in the commented line suggested that we should check for a `.ticket` extension.
Let's perform one more enumeration using the following Gobuster command.
`gobuster dir -u 10.10.185.103/island/2100/ -w /usr/share/wordlists/dirbuster/directory-list-2.3-small.txt -x .ticket`

![image](https://github.com/ivanloh1216/TryHackMe/assets/58873811/287bcfed-f2d1-4a5d-a802-1629cfb911c9)

Great, we found a new directory. By visiting the website, we get the below output.

![image](https://github.com/ivanloh1216/TryHackMe/assets/58873811/f9fe1121-8744-404d-bb22-b48ea7475469)

After checking the hint on the THM room, I found that the string is an encoded base. Hence, let's put the string into [CyberChef](https://gchq.github.io/CyberChef/)

Using `base58`, we get something that appears to be a password. 

![image](https://github.com/ivanloh1216/TryHackMe/assets/58873811/378aff96-5004-40c0-aec8-ec047852a859)

Linking back to the information we enumerated in previous steps, we now have a pair of username and password.
`vigilante:!#th3h00d`

There are two ports in which we could try to use these credentials, namely `FTP` and `SSH`. Checking on the questions asked in the THM room, I first try to log in to the FTP and managed to get in with this credentials.

![image](https://github.com/ivanloh1216/TryHackMe/assets/58873811/65419dfd-3071-4f79-97b5-0f69fefcbbf6)

Let's list out the files we have using `ls -all`. 

![image](https://github.com/ivanloh1216/TryHackMe/assets/58873811/1a73b629-7eb8-42f7-8a23-c9ba80545208)

We have 3 images. Let's download them and we will analyze them later. For now, I decided to browse around the FTP server to see what other information we can get.

![image](https://github.com/ivanloh1216/TryHackMe/assets/58873811/9a651ec4-3935-4ad8-ba4c-8b5504fc3316)

I found out that there is another user `slade`. However, we cannot change to that directory. 

### 2. What is the Web Directory you found?

Refer to the information enumerated in the previous steps and the question requirements. 

> 2100

### 3. What is the file name you found?

Refer to the information enumerated in the previous steps.

> green_arrow.ticket

### 4. What is the FTP Password?

Refer to the information enumerated in the previous steps.

> !#th3h00d

### 5. What is the file name with SSH password?

Remember we downloaded 3 images from the FTP server earlier? Let's analyze them now. 

#### Leave_me_alone.png

This image file seems corrupted. Let's open it in a hex editor.

![image](https://github.com/ivanloh1216/TryHackMe/assets/58873811/d3967762-9f93-41da-ad6b-ba92cd5b3bd6)

Indeed the file signature header is wrong. Let's correct it.

![image](https://github.com/ivanloh1216/TryHackMe/assets/58873811/10e1343d-1457-40a8-9ba8-81ab95b9ed55)

Now we can open it successfully.

![Leave_me_alone](https://github.com/ivanloh1216/TryHackMe/assets/58873811/590e316e-a387-45cb-ad9c-172841917145)

Looking at this picture, `password` seems to be a password.

#### Queen's_Gambit.png 

![Queen's_Gambit](https://github.com/ivanloh1216/TryHackMe/assets/58873811/2fd1335a-fcaa-4c69-98f2-791e126aff8d)

`Binwalk` and `exiftool` cannot find anything.  

![image](https://github.com/ivanloh1216/TryHackMe/assets/58873811/a9232bf5-ae2d-4b0a-babe-229815f58b1c)

Let's skip this picture for now.

#### aa.jpg

This is the only `.jpg` file so I am thinking we should be able to get something from it using `steghide`. Let's use `steghide` to try and extract any hidden information from it. 
Type in the command `steghide extract -sf aa.jpg` and provide `password` as the password. 

![image](https://github.com/ivanloh1216/TryHackMe/assets/58873811/2a33d2ca-4f2d-4da0-9c2e-7c049ccb17d2)

Yes, we extracted some hidden files from it. Let's check the file contents.

![image](https://github.com/ivanloh1216/TryHackMe/assets/58873811/45638d79-f355-452a-90c1-4c09a4c25fe9)

From the `shado` file, we can find a string that should be the SSH password for the other user - `slade`. 

Hence, the answer to this question is:

> shado

### 6. user.txt

Log in to the SSH server using the credentials we found in the previous steps. `slade:M3tahuman` 

![image](https://github.com/ivanloh1216/TryHackMe/assets/58873811/5714c8a6-0df9-449c-a0f7-04d6f7c671a2)

Then, list out the files in the directory. We will see one file which is `user.txt`. Let's find the flag in this file. 

![image](https://github.com/ivanloh1216/TryHackMe/assets/58873811/626119ab-7eec-4a19-978a-1dd8f2d55932)

> THM{P30P7E_K33P_53CRET5__C0MPUT3R5_D0N'T}

### 7. root.txt

Next, let's see what this user `slade` can perform with his account. Type in `sudo -l`.

![image](https://github.com/ivanloh1216/TryHackMe/assets/58873811/e03f0d15-cb9f-424c-9507-9f092d4035ae)

So `slade` can perform `sudo /usr/bin/pkexec`, but what does the command do?

![image](https://github.com/ivanloh1216/TryHackMe/assets/58873811/741d0cf0-9570-4408-b2b1-15c9430547a8)

Apparently `slade` can execute a command as another user. That should help us to obtain the root flag.
Let's go ahead and type the command `sudo /usr/bin/pkexec ls /root` to see if the flag is found in the root directory.

![image](https://github.com/ivanloh1216/TryHackMe/assets/58873811/a2051ca1-c43f-4d18-a00e-4fced19c0bcd)

Yes, the final flag is there. Let's get the flag.

![image](https://github.com/ivanloh1216/TryHackMe/assets/58873811/6e96accb-4623-4e65-9425-5fe8a7bfedc3)

> THM{MY_W0RD_I5_MY_B0ND_IF_I_ACC3PT_YOUR_CONTRACT_THEN_IT_WILL_BE_COMPL3TED_OR_I'LL_BE_D34D}
