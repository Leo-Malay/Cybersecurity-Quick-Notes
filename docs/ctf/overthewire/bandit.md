# Bandit - Beginner

## Level 0

**Goal:** Login to the ssh server `bandit.labs.overthewire.org` on port `2220`. The username is `bandit0` and password is `bandit0`

To solve this, I used the following command to connect to the ssh server with given username, password and port.

```bash
ssh bandit0@bandit.labs.overthewire.org -p 2220
```

Upon logging in, Found the `readme` file in the home directory of user which contained the password the the level 1.

## Level 1

**Goal:** The password for the next level is stored in a file called `-` located in the home directory

This one is pretty simple. To open this file we make use of the `cat` commad, however, it is required to enter the whole path to the `-` file. Using the following command we get the password to level 2.

```bash
cat ./-
```

## Level 2

**Goal:** The password for the next level is stored in a file called spaces in this filename located in the home directory

I found out 2 ways of solving this question. First, you can simple direct linux to print all the files. Second, escape the space character using backslash. Thus we have out password for level 3.

```bash
cat ./*
cat ./spaces\ in\ this\ filename
```

## Level 3

**Goal:** The password for the next level is stored in a hidden file in the inhere directory.

On logging in, I found that in the home directory there is another directory named `inhere` and suprisingly it's empty!. Thus I tried the `ll` commad to show all the hidden files in the folder and there it was, the file name was `..Hiding-From-You`. And on to level 4

```bash
ll
cat ./...Hiding-From-You
```

## Level 4

**Goal:** The password for the next level is stored in the only human-readable file in the inhere directory.

Interesting...I tried printing all the file however, they all of them contained binary code except one. Well I found the password but it was not the right way... Thus after reading the level description. I used the `man` command to know what the suggested commands do. Among them was `file` command which had the description `determine file type`. Bingo I used the file command and passed all the files using `./*` and led me to the file with human readable ASCII text and there I found the password for level 5.

```bash
man file
cd inhere
file ./*
cat ./-file07
```

## Level 5

**Goal:** The password for the next level is stored in a file somewhere under the inhere directory and has all of the following properties:

- human-readable
- 1033 bytes in size
- not executable

The i`inhere` directory contained multiple directories. This took me some time to process. I had to read a lot of `man` page for `find` command. After reading for some time, I eventually figured out and found the password.

```bash
find -size 1033c    # not b but c for character in bytes.
ll ./maybehere07/.file2     # To verify the size and permission
cat ./maybehere07/.file2
```

## Level 6

**Goal:** The password for the next level is stored somewhere on the server and has all of the following properties:

- owned by user bandit7
- owned by group bandit6
- 33 bytes in size

Well again read the `man` pages for `find` and constructed the following command which gave a lot on nonsence but had the one file path with password to next level.

```bash
find ../../ -size 33c -user bandit7 -group bandit6
cat ../../var/lib/dpkg/info/bandit7.password
```

## Level 7

**Goal:** The password for the next level is stored in the file data.txt next to the word `millionth`

To see how many words are actually there I used the `wc -w` command. Then I tried to print all with `cat` and `grep` combo and it game me the password to the next level.

```bash
cat data.txt | grep "millionth"
```

## Level 8

**Goal:** The password for the next level is stored in the file data.txt and is the only line of text that occurs only once.

Intresting... I used `whatis` command for `sort` and `uniq` given in the suggested command and almost figured out the answer. What `sort` does is, it sorts all the lines in the files in A-Z formart. What `uniq` does is, it check the adjacent lines and return the unique line. Thus for the soultion, I have to `sort` and then `uniq`. Bonus: We can use both `u` and `c` flag.

```bash
sort data.txt | uniq -u     # Better.. only one line output
sort data.txt | uniq -c     # Okay Okay.. many lines output
```

## Level 9

**Goal:** The password for the next level is stored in the file data.txt in one of the few human-readable strings, preceded by several ‘=’ characters.

From the given suggested commands, I used `whatis` commad on `strings` and it informed me that it print the sequences of printable characters in files. We I got the solution as now I can do the `grep` on the output.

```bash
strings data.txt | grep "=="
```

## Level 10

**Goal:** The password for the next level is stored in the file data.txt, which contains base64 encoded data

Given the data.txt file is base64 encoded and we need to decode it to obtain the password to the next level. Well first I checked what is the purpose of `base64` command given as a suggestion. Then I went to the `man` page found my answer.

```bash
base64 -d data.txt
```

## Level 11

**Goal:** The password for the next level is stored in the file data.txt, where all lowercase (a-z) and uppercase (A-Z) letters have been rotated by 13 positions

Well Interestingly, its llike a ceasar cipher where the key is 13. Given the suggested commands, I chose to do `whatis` on the unfamiliar command `tr` and found out it helps in translation of charaters. Well what I can now do is I can replact A with N and so on...

```bash
cat data.txt | tr [a-z] [n-za-m] | tr [A-Z] [N-ZA-M]
```

The above is my initial solution but is there a better way?? After some trial and error, I found the compact version which is not that pretty but I guess good enough...

```bash
cat data.txt | tr [a-zA-Z] [n-za-mN-ZA-M]
```

## Level 12

**Goal:** TThe password for the next level is stored in the file data.txt, which is a hexdump of a file that has been repeatedly compressed. For this level it may be useful to create a directory under /tmp in which you can work. Use mkdir with a hard to guess directory name. Or better, use the command “mktemp -d”. Then copy the datafile using cp, and rename it using mv

First things first create a tmp directory, copy the file there and move there. Now as stated it is hexdeumped thus we need to reverse it. Reviewing the suggested commands I found `xxd` and used `whatis` command on it and reading the `man` page I reversed it. Now to check what kind on compress it it we use the `file` command. Now I need to decompress it. Again the suggested commands, I came to know about `gzip` which is used for compression and expansion of files.. Thus reading the man page and performed the `gunzip` command. Now again I used `file` command to check and it said we need to `bzip2`. Thus I did `whatis` and `man` on `bzip2`. After that It was again `gunzip`. Then It was `tar` just see the command trail.

```bash
mktemp -d
cp data.txt /tmp/tmp.COBhrxOjPS/data.txt
cd /tmp/tmp.COBhrxOjPS
xxd -r data.txt temp.txt
file temp.txt
cp temp.txt tmp1.z
gunzip tmp1.z
file tmp1
mv tmp1 tmp1.bz
bunzip2 tmp1.bz
file tmp1
mv temp.txt tmp1.z
gunzip tmp1.z
file tmp1
mv tmp1 tmp1.tar
tar -xf tmp1.tar
file data5.bin
mv data5.bin tmp2.tar
tar -xf tmp2.tar
file data6.bin
rm tmp1.tar tmp2.tar
file data6.bin
mv data6.bin tmp1.bz
bunzip tmp1.bz
bunzip2 tmp1.bz
file tmp1
mv tmp1 tmp1.tar
tar -xf tmp1.tar
file data8.bin
rm tmp1.tar
mv data8.bin tmp1.z
gunzip tmp1.z
file tmp1
```

## Level 13

**Goal:** The password for the next level is stored in /etc/bandit_pass/bandit14 and can only be read by user bandit14. For this level, you don’t get the next password, but you get a private SSH key that can be used to log into the next level.

I have the private key which I can use to login to account bandit14. But I had to look at the `man` page of `ssh` on how to supply the private key as file in the command.

```bash
ssh bandit14@bandit.labs.overthewire.org -p 2220 -i sshkey.private
cat /etc/bandit_pass/bandit14
```

## Level 14

**Goal:** The password for the next level can be retrieved by submitting the password of the current level to port 30000 on localhost.

Okay what i unserdatnd is if i do a post request to the localhost:30000 with the current password I'll get the password to the next level. I tried to pipe the test string to the given address and port using the netcat `nc` command which I figured out after looking at's `man` pages. Upon submitting I got the reply "Wrong Answer". Thus it confirmed that I just need to replace the string "Hello" with the current password.

```bash
echo "Hello" | nc localhost 30000
```

## Level 15

**Goal:** The password for the next level can be retrieved by submitting the password of the current level to port 30001 on localhost using SSL/TLS encryption.

The provided hint suggessted to read "CONNECTED COMMANDS" but where?? I went through all the man pages of suggested command and found `openssl s_client`. Now using this command we can get the password.

```bash
openssl s_client -connect localhost:30001   # Open Connection and paste the password.
```

## Level 16

**Goal:** The credentials for the next level can be retrieved by submitting the password of the current level to a port on localhost in the range 31000 to 32000. First find out which of these ports have a server listening on them. Then find out which of those speak SSL/TLS and which don’t. There is only 1 server that will give the next credentials, the others will simply send back to you whatever you send to it.

To find out which ports are listening, I used `nmap` to scan quickly. It reutrn 5 ports which I tried connecting to all of them and the server with port 31760 is the correct server where password will be extracted. However I faced problem of KeyUpdate Interactive problem. Which I solved using the `-quiet` flag at the end of function. However I received the private key!!

```bash
openssl s_client -connect localhost:31790 -quiet
```

## Level 17

**Goal:** There are 2 files in the homedirectory: passwords.old and passwords.new. The password for the next level is in passwords.new and is the only line that has been changed between passwords.old and passwords.new

Basically what we need to do is find the difference between both the files. Easy Pizy we can do ths using the `diff` commad.

```bash
diff passwords.old passwords.new
```

## Level 18

**Goal:** The password for the next level is stored in a file readme in the homedirectory. Unfortunately, someone has modified .bashrc to log you out when you log in with SSH.

Well Took a lot of time searching the `ssh` man pages eventually had to do a quick google search on how to open ssh with other shell like sh and got the answer.

```bash
ssh bandit18@bandit.labs.overthewire.org -p 2220 -t sh
cat readme
```

## Level 19

**Goal:** To gain access to the next level, you should use the setuid binary in the homedirectory. Execute it without arguments to find out how to use it. The password for this level can be found in the usual place (/etc/bandit_pass), after you have used the setuid binary.

The given binary file allows us to run our command with uid of bandit20 like a privilege to change to bandit20 user and run the command. Thus getting the answer is really easy. If you know this concept of uid, suid, guid then it's really easy.

```bash
./bandit20-do cat /etc/bandit_pass/bandit20
```

## Level 20

**Goal:** There is a setuid binary in the homedirectory that does the following: it makes a connection to localhost on the port you specify as a commandline argument. It then reads a line of text from the connection and compares it to the password in the previous level (bandit20). If the password is correct, it will transmit the password for the next level (bandit21).

Took some to understand what it does but yes did it without any help. So what we need to do is start a server and server the password of currentlevel. To server the password we will start the `nc` in listen mode and server the password.

```bash
echo "0qXahG8ZjOVMN9Ghs7iOWsCfZyXOUbYO" | nc -l 2323 &  # Run in backgound
./suconnect 2323
```

## Level 21

**Goal:**

## Level 22

**Goal:**

## Level 23

**Goal:**

## Level 24

**Goal:**

## Level 25

**Goal:**

## Level 26

**Goal:**

## Level 27

**Goal:**

## Level 28

**Goal:**

## Level 29

**Goal:**

## Level 30

**Goal:**

## Level 31

**Goal:**

## Level 32

**Goal:**

## Level 33

**Goal:**

## Level 34

**Goal:**

## Passwords

- Level 1: `ZjLjTmM6FvvyRnrb2rfNWOZOTa6ip5If`
- Level 2: `263JGJPfgU6LtdEvgfWU1XP5yac29mFx`
- Level 3: `MNk8KNH3Usiio41PRUEoDFPqfxLPlSmx`
- Level 4: `2WmrDFRmJIq3IPxneAaMGhap0pFhF3NJ`
- Level 5: `4oQYVPkxZOOEOO5pTW81FB8j8lxXGUQw`
- Level 6: `HWasnPhtq9AVKe0dmk45nxy20cvUa6EG`
- Level 7: `morbNTDkSW6jIlUc0ymOdMaLnOlFVAaj`
- Level 8: `dfwvzFQi4mU0wfNbFOe9RoWskMLg7eEc`
- Level 9: `4CKMh1JI91bUIZZPXDqGanal4xvAg0JM`
- Level 10: `FGUW5ilLVJrxX9kMYMmlN4MgbpfMiqey`
- Level 11: `dtR173fZKb0RRsDFSGsg2RWnpNVj3qRr`
- Level 12: `7x16WNeHIi5YkIhWsfFIqoognUTyj9Q4`
- Level 13: `FO5dwFsc0cbaIiH0h8J2eUks2vdTDwAn`
- Level 14: `MU4VWeTyJk8ROof1qqmcBPaLh7lDCPvS`
- Level 15: `8xCjnmgoKbGLhHFAZlGE5Tmu4M2tKJQo`
- Level 16: `kSkvUpMQ7lBYyCM4GBPvCvT1BfWRy0Dx`
- Level 17:

```text
-----BEGIN RSA PRIVATE KEY-----
MIIEogIBAAKCAQEAvmOkuifmMg6HL2YPIOjon6iWfbp7c3jx34YkYWqUH57SUdyJ
imZzeyGC0gtZPGujUSxiJSWI/oTqexh+cAMTSMlOJf7+BrJObArnxd9Y7YT2bRPQ
Ja6Lzb558YW3FZl87ORiO+rW4LCDCNd2lUvLE/GL2GWyuKN0K5iCd5TbtJzEkQTu
DSt2mcNn4rhAL+JFr56o4T6z8WWAW18BR6yGrMq7Q/kALHYW3OekePQAzL0VUYbW
JGTi65CxbCnzc/w4+mqQyvmzpWtMAzJTzAzQxNbkR2MBGySxDLrjg0LWN6sK7wNX
x0YVztz/zbIkPjfkU1jHS+9EbVNj+D1XFOJuaQIDAQABAoIBABagpxpM1aoLWfvD
KHcj10nqcoBc4oE11aFYQwik7xfW+24pRNuDE6SFthOar69jp5RlLwD1NhPx3iBl
J9nOM8OJ0VToum43UOS8YxF8WwhXriYGnc1sskbwpXOUDc9uX4+UESzH22P29ovd
d8WErY0gPxun8pbJLmxkAtWNhpMvfe0050vk9TL5wqbu9AlbssgTcCXkMQnPw9nC
YNN6DDP2lbcBrvgT9YCNL6C+ZKufD52yOQ9qOkwFTEQpjtF4uNtJom+asvlpmS8A
vLY9r60wYSvmZhNqBUrj7lyCtXMIu1kkd4w7F77k+DjHoAXyxcUp1DGL51sOmama
+TOWWgECgYEA8JtPxP0GRJ+IQkX262jM3dEIkza8ky5moIwUqYdsx0NxHgRRhORT
8c8hAuRBb2G82so8vUHk/fur85OEfc9TncnCY2crpoqsghifKLxrLgtT+qDpfZnx
SatLdt8GfQ85yA7hnWWJ2MxF3NaeSDm75Lsm+tBbAiyc9P2jGRNtMSkCgYEAypHd
HCctNi/FwjulhttFx/rHYKhLidZDFYeiE/v45bN4yFm8x7R/b0iE7KaszX+Exdvt
SghaTdcG0Knyw1bpJVyusavPzpaJMjdJ6tcFhVAbAjm7enCIvGCSx+X3l5SiWg0A
R57hJglezIiVjv3aGwHwvlZvtszK6zV6oXFAu0ECgYAbjo46T4hyP5tJi93V5HDi
Ttiek7xRVxUl+iU7rWkGAXFpMLFteQEsRr7PJ/lemmEY5eTDAFMLy9FL2m9oQWCg
R8VdwSk8r9FGLS+9aKcV5PI/WEKlwgXinB3OhYimtiG2Cg5JCqIZFHxD6MjEGOiu
L8ktHMPvodBwNsSBULpG0QKBgBAplTfC1HOnWiMGOU3KPwYWt0O6CdTkmJOmL8Ni
blh9elyZ9FsGxsgtRBXRsqXuz7wtsQAgLHxbdLq/ZJQ7YfzOKU4ZxEnabvXnvWkU
YOdjHdSOoKvDQNWu6ucyLRAWFuISeXw9a/9p7ftpxm0TSgyvmfLF2MIAEwyzRqaM
77pBAoGAMmjmIJdjp+Ez8duyn3ieo36yrttF5NSsJLAbxFpdlc1gvtGCWW+9Cq0b
dxviW8+TFVEBl1O4f7HVm6EpTscdDxU+bCXWkfjuRb7Dy9GOtt9JPsX8MBTakzh3
vBgsyi/sN3RqRBcGU40fOoZyfAMT8s1m/uYv52O6IgeuZ/ujbjY=
-----END RSA PRIVATE KEY-----
```

- Level 18: `x2gLTTjFwMOhQ8oWNbMN362QKxfRqGlO`
- Level 19: `cGWpMaKXVwDUNgPAVJbWYuGHVn9zl3j8`
- Level 20: `0qXahG8ZjOVMN9Ghs7iOWsCfZyXOUbYO`
- Level 21: `EeoULMCra2q0dSkYj561DX7s1CpBuOBt`
- Level 22: ``
- Level 23: ``
- Level 24: ``
- Level 25: ``
- Level 26: ``
- Level 27: ``
- Level 28: ``
- Level 29: ``
- Level 30: ``
- Level 31: ``
- Level 32: ``
- Level 33: ``
- Level 34: ``

```

```
