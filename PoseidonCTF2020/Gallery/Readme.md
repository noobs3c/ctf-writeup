# Gallery 

**Category:** Forensics

**Description:** I have an old damaged disk , it contains a lot of my images , I use it as a gallery actually , in it , I had a secret can you get it back for me ? [Link to the file](https://mega.nz/file/WahAzABZ#7G3oM4uiG2rESutwZcNwaklLzEkVf5ex3R0P6qSVEdg)




## Solution:

The link gives us an EWF file.
```console
noobsec@labtop:~/Télechargements# file challenge.E01 
challenge.E01: EWF/Expert Witness/EnCase image file format
```

I searched Google about mounting EWF files and found ewf-tool


```console
noobsec@labtop:~/Télechargements# mkdir rawimage
noobsec@labtop:~/Télechargements# ewfmount challenge.E01 rawimage/
ewfmount 20140807

noobsec@labtop:~/Télechargements# mkdir mountpoint
noobsec@labtop:~/Télechargements# mount ./rawimage/ewf1 ./mountpoint -o ro,loop,show_sys_files,streams_interace=windows 
noobsec@labtop:~/Télechargements# cd mountpoint/
noobsec@labtop:~/Télechargements/mountpoint# ls -la
total 3032
drwxrwxrwx 1 root root    4096 Jul 27 01:57  .
drwxr-xr-x 4 root root    4096 Aug 11 01:28  ..
-rwxrwxrwx 1 root root    2560 Jul 27 01:37 '$AttrDef'
-rwxrwxrwx 1 root root       0 Jul 27 01:37 '$BadClus'
-rwxrwxrwx 1 root root     576 Jul 27 01:37 '$Bitmap'
-rwxrwxrwx 1 root root    8192 Jul 27 01:37 '$Boot'
drwxrwxrwx 1 root root       0 Jul 27 01:37 '$Extend'
-rwxrwxrwx 1 root root 2097152 Jul 27 01:37 '$LogFile'
-rwxrwxrwx 1 root root    4096 Jul 27 01:37 '$MFTMirr'
drwxrwxrwx 1 root root       0 Jul 27 01:38 '$RECYCLE.BIN'
---------- 1 root root       0 Jul 27 01:37 '$Secure'
-rwxrwxrwx 1 root root  131072 Jul 27 01:37 '$UpCase'
-rwxrwxrwx 1 root root       0 Jul 27 01:37 '$Volume'
-rwxrwxrwx 1 root root  332475 Jul 27 01:53  1200px-Blason_étoile_du_sahel.svg.png
-rwxrwxrwx 1 root root   86910 Jul 27 01:52  1200px-Flag_commune_Sousse.svg.png
-rwxrwxrwx 1 root root   15165 Jul 27 01:54  156-1568990_trident-trident-vector-png-transparent-png.png
-rwxrwxrwx 1 root root    9112 Jul 27 01:52 'download (1).png'
-rwxrwxrwx 1 root root   11128 Jul 27 01:52 'download (2).png'
-rwxrwxrwx 1 root root   10038 Jul 27 01:52  download.png
-rwxrwxrwx 1 root root  195761 Jul 27 01:52  Poseidon.png
drwxrwxrwx 1 root root       0 Jul 27 01:38  Steghide
drwxrwxrwx 1 root root       0 Jul 27 01:37 'System Volume Information'
-rwxrwxrwx 1 root root   26894 Jul 27 01:54  trident-trident-png-clip-art.png
-rwxrwxrwx 1 root root   39386 Jul 27 01:53 'unnamed (1).png'
-rwxrwxrwx 1 root root   89751 Jul 27 01:53  unnamed.png
-rwxrwxrwx 1 root root    8340 Jul 27 01:48  Wallpaper_HD_19756487Ef4.jpg
```

As we can see there's a directory called `Steghide` it gives a hint about using the tool. Steghide extracts hidden image data form image files with a password and works only on jpg file formats. So I apply it on `Wallpaper_HD_19756487Ef4.jpg`

```console
noobsec@labtop:~/Télechargements# steghide extract -sf Wallpaper_HD_19756487Ef4.jpg 
Enter passphrase: 
steghide: could not extract any data with that passphrase!
```
As you can see we need a password and it must be hiddden somewhere (that what i thought first). I check all images with different steganography techniques like LSB check and all but found nothing. After looking into the directories I found some files in the $RECYCLE.BIN.

```console
noobsec@labtop:~/Télechargements/mountpoint/$RECYCLE.BIN/S-1-5-21-1731612336-1848057521-3450626154-1001# ls -la
total 22
drwxrwxrwx 1 root root     0 Jul 27 01:57  .
drwxrwxrwx 1 root root     0 Jul 27 01:38  ..
-rwxrwxrwx 1 root root    60 Jul 27 01:57 '$IJXVQDX.txt'
-rwxrwxrwx 1 root root    58 Jul 27 01:57 '$IK1ODPJ.txt'
-rwxrwxrwx 1 root root     0 Jul 27 01:56 '$RJXVQDX.txt'
-rwxrwxrwx 1 root root 17982 Jul 27 01:44 '$RK1ODPJ.txt'
-rwxrwxrwx 1 root root   129 Jul 27 01:38  desktop.ini
```
###### Remarks: you can do this without ewf-tool on windows by using [OsForensics](https://www.osforensics.com)

The largest file contains some ascii , and at this step i was stuck like 2 hours then a friend told me to try `$RK1ODPJ.txt` as a wordlist to crack the passphrase.
```
zQn5(bX6V!q+r/kZtFW8fTN&@2
EZ5TvP>Cr)_jeR3y*BaGV[xz@s
%}CaZKWDvSP![wLf25nkqJ{^Ms
fGWzq?>{cbJLVd4^3p(Re!s5S}
VGR54uPvsj!y=*]}6X@w{r/>Zz
Lf@s{JP&=3j6[x$X-c/EuGb7gK
mKDy4.a6rUj59pe&^{%?8C!/Zq
YE=bJ?HPkBL}qrmMQ+&*CTfZ3V
[_mj5BhzKvU2.kVDn&H^s/fN{7
z)$y@]cagShdbk+XY&7E%4rB2H
```
So I used [stegcracker](https://github.com/Paradoxis/StegCracker) to bruteforce the password on the jpg file with this wordlist.

```console
noobsec@labtop:~/Télechargements# stegcracker Wallpaper_HD_19756487Ef4.jpg worlist.txt 
StegCracker 2.0.9 - (https://github.com/Paradoxis/StegCracker)
Copyright (c) 2020 - Luke Paris (Paradoxis)

Counting lines in wordlist..
Attacking file 'Wallpaper_HD_19756487Ef4.jpg' with wordlist 'worlist.txt'..
Successfully cracked file with password: fs6-K*Qa!qeG5Jv.URBx8)]Zu%
Tried 174 passwords
Your file has been written to: Wallpaper_HD_19756487Ef4.jpg.out
```
### Success ! now cat the file and we have the flag in plaintext 

## Flag:`Poseidon{uR3_4_G00D_AN4Ly5t}`
