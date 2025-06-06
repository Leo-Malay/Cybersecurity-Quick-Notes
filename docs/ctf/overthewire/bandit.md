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
- Level 11: ``
- Level 12: ``
- Level 13: ``
- Level 14: ``
- Level 15: ``
- Level 16: ``
- Level 17: ``
- Level 18: ``
- Level 19: ``
- Level 20: ``
- Level 21: ``
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
