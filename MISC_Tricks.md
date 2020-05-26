# Linux
## Basic Idea
1. Change dir to other user may find Interesting things.
2. go to /proc/<pid>/ to find something
    * maybe check cmdline to find what command is running & may find plaintext password
    * go to /proc/<PID>/ to list files and directories
     
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
    
3. check /etc/passwd to find how many user in the host & other info like shell they used
## Something I find about /proc directory

## Reference
[Exploring /proc File System in Linux](https://www.tecmint.com/exploring-proc-file-system-in-linux/): Basic about /proc path
### Tools & git repo
* [grep - search pattern](https://caspar.bgsu.edu/~courses/Stats/Labs/Handouts/grepsearch.htm)
* [jq](https://stedolan.github.io/jq/): a command line json processor(see hackthebox - Luke).
* [PEASS - Privilege Escalation Awesome Scripts SUITE](https://github.com/carlospolop/privilege-escalation-awesome-scripts-suite): privilege escalation tools for Windows and Linux/Unix.
### Articles 
* [Exploring /proc File System in Linux](https://www.tecmint.com/exploring-proc-file-system-in-linux/)
* [Discover the possibilities of the /proc directory](https://www.linux.com/news/discover-possibilities-proc-directory/)

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
