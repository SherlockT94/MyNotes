# Linux
* [grep - search pattern](https://caspar.bgsu.edu/~courses/Stats/Labs/Handouts/grepsearch.htm)
# Windows
# Steganography
## Least Significant Bit (LSB)

    * One pixel contain 3 channels and 1 Byte per channel(0 - 255)
    * ![](https://www.boiteaklou.fr/assets/2018-08-12/byte_diagram.jpg)
    * The first bit on the left is the “heaviest” one since it’s the one that has the biggest influence on the value of the byte. Its weight is 128.
    * look at the bit on the very right. Its weight is 1 and it has a very minor impact on the value of the byte. In a way, this bit is the least significant bit of this byte. **Basically, unvisable!!!**
    * we can modify **3 bits per pixel(one bit per channel)** without it is noticeable.
    * **NOTE:** Make sure to hide your message inside a PNG file and not a JPEG or its lossy compression algorithm will overwrite your modifications!
    * we could use the zsteg to extract hide message.

## Reference
* [Steganography Tutorial: Least Significant Bit (LSB)](https://www.boiteaklou.fr/Steganography-Least-Significant-Bit.html)
* [CTF Tidbits: Part 1 — Steganography](https://medium.com/@FourOctets/ctf-tidbits-part-1-steganography-ea76cc526b40)
* [File signature List](https://www.garykessler.net/library/file_sigs.html)

# Zip

# Tools
* **file:** The file command determines the file type of a file. It reports the file type in human readable format (e.g. 'ASCII text') or MIME type (e.g. 'text/plain; charset=us-ascii'). As filenames in UNIX can be entirely independent of file type file can be a useful command to determine how to view or work with a file.
* **Strings:** finds and prints text strings embedded in all files strings filename. e.g. `strings filename | awk 'length($0)>15' | sort -u`
* **Hexeditor:** A hex editor, also called a binary file editor or byteeditor, is a type of program that allows a user to view and edit the raw and exact contents of files, that is, at the byte level. **Change image size!!!!!!**
* **binwalk:** Binwalk is a fast, easy to use tool for analyzing, reverse engineering, and extracting firmware images. Binwalk is a great tool for extracting hidden files from other files as well.
* **xxd:** is a Linux command that creates a hex dump of a given file or standard input. It can also convert a hex dump back to its original binary form. Like uuencode(1) and uudecode(1) it allows the transmission of binary data in a “mail-safe” ASCII representation, but has the advantage of decoding to standard output.
* **stegsolv:** A great GUI tool that covers a wide range of analysis, some of which is covered by the other tools mentioned above and a lot more including color profiles, planes, Color maps, strings.
* **Sonic visualizer:** Sonic Visualizer is a great tool to find hidden messages in audio files and a great way to work with audio files in general.
* **pngcheck**: Check for any corruption or anomalous sections pngcheck -v PNGs can contain a variety of data ‘chunks’ that are optional (non-critical) as far as rendering is concerned.
* **PCRT (PNG Check & Repair Tool)**: PCRT (PNG Check & Repair Tool) is a tool to help check if PNG image correct and try to auto fix the error. It's cross-platform, which can run on Windows, Linux and Mac OS.
* **exiftool:** Check out metadata of media files.
