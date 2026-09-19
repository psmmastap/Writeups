```
Level 0
SSH into bandit0
-command used: ssh bandit0@bandit.labs.overthewire.org -p 2220
password: bandit0
```

```
Level 1
Find README which contains password for bandit1 and SSH on port 2220 into user bandit1

-readme was in the home directory and I just needed to cat the file to read the contents
-Password: ZjLjTmM6FvvyRnrb2rfNWOZOTa6ip5If
```

```
Level 2
Open a file called '-' to find the password for bandit2

-At first I tried "cat -" but that didn't work because it was expecting a tag so after some research I learned that I can also cat files by starting the filename with "./"
-command used for answer: cat ./- 
-Password: 263JGJPfgU6LtdEvgfWU1XP5yac29mFx
```

```
Level 3
Open a file called "--spaces in this filename--" to find the password for bandit3

-I initially tried the './' trick that I learned previously but it alone did not work. After a couple of minutes trying things I used quotes after './' over the entire name of the file and it worked
-command used for answer: cat ./"--spaces in this filename--"
-Password: MNk8KNH3Usiio41PRUEoDFPqfxLPlSmx
```

```
Level 4
The password for the next level is stored in a hidden file in the inhere directory.

-After moving to the 'inhere' directory and trying ls it showed that there was nothing in there. I then used du to confirm that there was a file inside with the du valuing to 8. Just using find with no tags revealed to me that there was a file named ./...Hiding-From-You which held the password for the next level
-Password: 2WmrDFRmJIq3IPxneAaMGhap0pFhF3NJ
```

```
Level 5
The password for the next level is stored in the only human-readable file in the inhere directory

-I am not sure if this is the intended way to do this but inside the inhere directory there were 10 files named "-file00" ... "-file09" and I used "file -f '-file__" which sent an output like this

\010KU\256\265\201\273\001\011g\364\237\335: cannot open `\010KU\256\265\201\273\001\011g\364\237\335' (No such file or directory)
\272\361jD\\3319\372hx:                      cannot open `\272\361jD\\3319\372hx' (No such file or directory)

-Until I did it for "-file07" which gave me an output of 

4oQYVPkxZOOEOO5pTW81FB8j8lxXGUQw: cannot open `4oQYVPkxZOOEOO5pTW81FB8j8lxXGUQw' (No such file or directory)

-Which that looked like the password to me so I tried cat on the file and it let me read it.
-Password: 4oQYVPkxZOOEOO5pTW81FB8j8lxXGUQw

--I went back to try cat on the other files and the output is something like this
d%�h�U��N?CN�qy�▒������B��g4V�
--so I guess I could have just tried cat on all of them until it worked
---After trying file with no tags it revealed to me that all the other files contain data while file 7 contains ASCII text so thats what was going on with cat
```

```
Level 6
Instructions: The password for the next level is stored in a file somewhere under the **inhere** directory and has all of the following properties:

- human-readable
- 1033 bytes in size
- not executable
  
-At first I was lost because my previous methods just revolved around me testing every file I found and luckily finding the answer but when I opened this level there were so many files and directories that it wouldve taken so long for that to work. I then did some research to see if find could search for files based on attributes like readablility, file size, and executability and it turned successful
-Command I found: find -type f -readable -size 1033c ! -executable
-type -f searches for regular files and the size can be adjusted for many different attributes but 'c' means bytes

-Password: HWasnPhtq9AVKe0dmk45nxy20cvUa6EG
```

```
Level 7
Instructions: The password for the next level is stored **somewhere on the server** and has all of the following properties:

- owned by user bandit7
- owned by group bandit6
- 33 bytes in size
  
-I struggled on this for quite a while. At first I tried ls but nothing showed up so intuitively I thought of using find like I did in the last level to find things that I couldnt see throughout all the directories that were hidden. After a little bit of research I found out that find can also search based on ownership by a user and group using the tags -user and -group.
-I then used the command "find -type f -user bandit7 -group bandit6" but it didn't work so I was confused until I did "find / -type f ..." which gave me a lot of output that was mostly stuff that I didnt have permission to search.
-I scanned through this entire output and found the correct file that had the password inside of it but then I thought about what if I tried that and there were thousands or even more files that I would have to search through. That would be way to tedious so I did some research and found the tag "2>/dev/nul" cut out all the files that I wasnt able to search which then led my command to give me only the correct answer

-Command used: find / -type f -user bandit7 -group bandit6 -size 33c 2>/dev/null
-Password: morbNTDkSW6jIlUc0ymOdMaLnOlFVAaj
```

```
Level 8
Instructions: The password for the next level is stored in the file **data.txt** next to the word **millionth**

-Ok so finding the file was very easy but I used cat to read in it and there are an absurd amount of lines in it so I am going to have to find a different way to search for the word "millionth"
-After looking at the grep manual I saw that the -F option can search through a file looking for a specific string so I tried
-"grep -F "millionth" data.txt"
-And it seemed to work giving me the output of "millionth       dfwvzFQi4mU0wfNbFOe9RoWskMLg7eEc"

Password: dfwvzFQi4mU0wfNbFOe9RoWskMLg7eEc
```

```
Level 9
Instructions: The password for the next level is stored in the file **data.txt** and is the only line of text that occurs only once

-After looking at the file it would be to large for me to count and find the line of code that only occurs once so my first thought is for me to find a way to parse through the txt file and if it recognizes the same line then whatever tool will not print that out. I am not sure what tool could do that but I will do some research
-grep, sort, uniq, strings, base64, tr, tar, gzip, bzip2, xxd. These are the commands that the website said I may want to use in order for me to solve this so I will look at the manuals to see if anything stands out
-In the uniq manual i found this
	-u, --unique
              only print unique lines
-I tried to run "uniq -u data.txt" and it did not work. I might be doing something wrong.
-OK so I learned something about uniq, it can only find repeated lines if they are already sorted so initially I tried "sort data.txt | uniq -u data.txt" but then that came out wrong. I then realized that even though I am piping the sorted txt file into uniq I am screwing myself up by then restating data.txt so I omited the last data.txt and I got the password
-Command used: sort data.txt | uniq -u

Password: 4CKMh1JI91bUIZZPXDqGanal4xvAg0JM
```

```
Level 10
Instructions: The password for the next level is stored in the file **data.txt** in one of the few human-readable strings, preceded by several ‘=’ characters.

-Alright so I looked at the file and found the password but I want to find a better way to truly find what I am looking for
-I think that I found a better way, I used the strings command to disregard all of the nonreadable text and I was left with lines of 4 or more of normal text. Then I piped that output into a grep looking for "====" and this is the output I got

========== the
2========== password
========== is
========== FGUW5ilLVJrxX9kMYMmlN4MgbpfMiqey

-I am not sure if this is the intended way to find the answer but I will roll with it
-Command used: strings data.txt | grep "======="
Password: FGUW5ilLVJrxX9kMYMmlN4MgbpfMiqey
```

```
Level 11
Instructions: The password for the next level is stored in the file **data.txt**, which contains base64 encoded data

-Just from reading the instructions I will need to use the base64 command to decrypt the password. Now I am very inexperienced and im not sure if there are certain tags that I need for decryption
-Ok that was really easy, all I had to do was pipe the data.txt into a decryption and the answer was right there
-Command used: cat data.txt | base64 -d
Password: dtR173fZKb0RRsDFSGsg2RWnpNVj3qRr
```

```
Level 12
Instructions: The password for the next level is stored in the file **data.txt**, where all lowercase (a-z) and uppercase (A-Z) letters have been rotated by 13 positions

-Same logic as in level 11 except of base64 I need to decrypt the txt by using a tool to shift each letter 13 positions
-Inside data.txt Gur cnffjbeq vf 7k16JArUVv5LxVuJfsSVdbbtaHGlw9D4
-For encryptions I use cyberchef which most of the time has the answer and is able to decrypt it well and on the website there is an encryption method called Rot13 in which letter are simply shifted as given in this problem. After putting the output into cyberchef it decrypted it to
-Decryption: The password is 7x16WNeHIi5YkIhWsfFIqoognUTyj9Q4
Password: 7x16WNeHIi5YkIhWsfFIqoognUTyj9Q4
```

```
Level 13
Instructions: The password for the next level is stored in the file **data.txt**, which is a hexdump of a file that has been repeatedly compressed. For this level it may be useful to create a directory under /tmp in which you can work. Use mkdir with a hard to guess directory name. Or better, use the command “mktemp -d”. Then copy the datafile using cp, and rename it using mv (read the manpages!)

-To skip the boring part I copied the data.txt into the tmp directory and now I am trying to solve how to decypher the hexdump inside the file
-I am struggling to find an answer but I did see that the xxd command has stuff to do with hexdump encoding and decoding. Also by looking at the manual for it I figured out that the output that I am seeing was first encoded with this type of format
"% xxd -l 120 -c 12 xxd.1"
-Now I am trying to figure out how to reverse it but just using the -r tag returns back a bunch of unreadable code. I tried to put the output into cyberchef but it doesnt seem to be working due to the amount of times the data was encoded

For reference this is the encrypted code I am looking at
00000000: 1f8b 0808 00da cf69 0203 6461 7461 322e  .......i..data2.
00000010: 6269 6e00 0136 02c9 fd42 5a68 3931 4159  bin..6...BZh91AY
00000020: 2653 5978 ae89 f600 001c 7fff db7d bfef  &SYx.........}..
00000030: 8ff7 f7ff ffc2 ffcd 7cbd 2ee4 dff9 aff3  ........|.......
00000040: ef7b d577 e9f7 adbd dfbb fbff b001 3b30  .{.w..........;0
00000050: 63a0 6806 8326 400d 0031 0000 00d1 a313  c.h..&@..1......
00000060: 2000 001a 034d 1a19 0000 0032 0681 89a6   ....M.....2....
.
.
.
-After looking through the man files for xxd I found that the actual ASCII that was encrypted is all based on the last column so my thought process now is that I need to use xxd on the third column in order to decrypt it
-My hypothesis did not work so I will sleep on it and try again tomorrow, if need be I will google how xxd works and how hexdump can be decoded

-I took a couple of days off and when I came back I found this usage line example
xxd -r [-s [-]offset] [-c cols] [-ps] [infile [outfile]]
-Now what I am planning to do is use the format that I found earlier in the manual with this formatting to see if -r will work and give me readable output
-I think I need to find a tag that skips over the unreadable output

-I looked up what the format of the hexdump is and the first column (00000000) is the offset by byte position in the file, the second is the hex payload grouped by 2-byte (18 bit) chunks for readablility, and the third column is obviously the ASCII code. On top of that that the first byte (1f8b) represents that the outer file is a Gzip compressed file. Another thing that I found was that the 9th and 10th byte (42 5a68 39) it translates directly to ASCII as BZh9 which is the header for a Bzip2 file so that means the file I am trying to decrypt was a bzip archive that was then again compressed by gzip. So now with this information I think that I need to do the reverse

-WOW. That took so long to figure out but once I knew what to do it became so much easier. 
-So first I needed to use xxd to reverse the hexdump into a compressed file type and by using "file" you can find out what it is compressed by whether that be gzip, bzip2, or tar and so this file was compressed around 8 times by different formats. First I needed to use gunzip which gave me an unreadable file that was then compressed by bzip2 and then just unzipping these files over and over again finally gave me the answer

*Important*
Commands used: gunzip, bunzip2, tar -xvf (x=extract, v=verbose, f=file), mv (whenever the file extension didnt match for gunzip I needed to use mv to change it)
Final output: final.txt: The password is FO5dwFsc0cbaIiH0h8J2eUks2vdTDwAn
Password: FO5dwFsc0cbaIiH0h8J2eUks2vdTDwAn
```

```
Level 14 (Bandit13 for ssh)
Instructions: The password for the next level is stored in **/etc/bandit_pass/bandit14 and can only be read by user bandit14**. For this level, you don’t get the next password, but you get a private SSH key that can be used to log into the next level. Look at the commands that logged you into previous bandit levels, and find out how to use the key for this level.  
If you need help with this level: a hint file can be found in the home directory.  
Make sure to read the error messages as they are informative.


```