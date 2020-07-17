---
title: 'SSH keys'
published: true
---

<p>    
    <b>ssh</b> is a quick way to log on and access the file system of your server. From the bash shell on <a href="https://redmouse.xyz/install-lamp-on-windows-10/">WSL</a>, I would use this command.</p>

<p style="font-family:Courier; color:white; background-color:black;">

$ ssh  jimmy@redmouse.xyz

</p>

<p>The remote server will prompt you for the password. You can skip the password step by installing key-based SSH. ( Note:you will still be able to log on using a password on devices without a key pair).</p>


<p style="font-family:Courier; color:white; background-color:black;">


jimmy@WSLBASH:~$ ssh-keygen -t rsa<br>
Generating public/private rsa key pair.<br>
Enter file in which to save the key (/home/jimmy/.ssh/id_rsa):<br>
  

</p>
<p>
Enter - accept as default</p>

<p style="font-family:Courier; color:white; background-color:black;">

Enter passphrase (empty for no passphrase):<br>
Enter same passphrase again:<br>

</p>

<p>Enter - leave the passphrase as blank</p>


<p style="font-family:Courier; color:white; background-color:black;">

Your identification has been saved in /home/jimmy/.ssh/id_rsa.<br>
Your public key has been saved in /home/jimmy/.ssh/id_rsa.pub.<br>
The key fingerprint is:<br>
SHA256:j....lgOkE jimmy@WSLBASH<br>
The key's randomart image is:<br>
+---[RSA 2048]----+<br>
|   .=++..+ oE    |<br>
...................<br>
|              + =|<br>
|               = |<br>
+----[SHA256]-----+<br>
jimmy@WSLBASH:~$<br>

</p>

<p>Your output should look something like this.</p>

<p>Now you have a private and public key saved in ~/.ssh/
Copy the public key to your server.</p>


<p style="font-family:Courier; color:white; background-color:black;">
ssh-copy-id -i ~/.ssh/id_rsa.pub yourserver.com
</p>
<p>
Now you should be able to log in using ssh without having to enter a password. No password means you can start using other useful bash commands such as scp and rsync.</p>


<p>You can simplify ssh login by creating a configuration file</p>

<p style="font-family:Courier; color:white; background-color:black;">
sudo nano ~/.ssh/config
</p>
<p>
Enter these commands for user jimmy</p>


<p style="font-family:Courier; color:white; background-color:black;">Host yourserver
HostName yourserver.com<br>
Port 22<br>
User jimmy<br>
IdentityFile  ~/.ssh/id_rsa<br>
</p>
<p>You can now ssh into your serve</p>
r with

<p style="font-family:Courier; color:white; background-color:black;">ssh yourserver</p>


<p>Once you have this setup you can harden ssh by removing root login and  preventing ssh login by password. This makes a ssh brute force attack much more difficult.</p>