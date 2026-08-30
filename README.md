# OverTheWire's Bandit Challenge
This repo documents my progress through [OverTheWire's Bandit wargame](https://overthewire.org/wargames/bandit/) up to Level 22.
I have detailed the objective of each level, the commands I used and a brief description of each, as well as an explanation of the steps I used to reach the solution and what I learnt. 
## Level 0 -> 1
### Objective 
Log into the game using SSH. Given that the username and password are bandit0, host is bandit.labs.overthewire.org, and port is 2220. Read file 'readme' in home directory to obtain password. 
### Commands Used 
- `ssh` : Network protocol, allows secure remote login to a server 
- `ls` : Lists files and directories 
- `cat` : Concatenate, to read and output file data
### Solution 
```bash
ssh bandit0@bandit.labs.overthewire.org -p 2220
ls
cat readme
```
[Screenshot 1](screenshots/level%2001-1.png) | [Screenshot 2](screenshots/level%2001-2.png)
### Explanation 
`ssh` connected me to the port, and then I entered the given password bandit0. `ls` command listed all the files in the home directory - there was only one called readme. `cat` command displayed the contents of the file, inside which there was the password to Level 1. 

## Level 1 -> 2
### Objective 
Find password for Level 2 located in a file called - in home directory. 
### Commands Used 
- `ls`
- `cat`
### Solution 
```bash
ssh bandit1@bandit.labs.overthewire.org -p 2220
ls
cat ./-
```
[Screenshot of Terminal](screenshots/level12.png) 
### Explanation 
`cat -` didn't work because `-` is a special command. To bypass this, I had to specify the folder using `./`  to show that `-` is a file name. 

## Level 2 -> 3 
### Objective 
Find the password for the next level is stored in a file called --spaces in this filename-- located in the home directory
### Commands Used 
- `ls`
- `cat`
### Solution 
```bash
ssh bandit2@bandit.labs.overthewire.org -p 2220
ls
cat -- "--spaces in this filename--"
```
[Screenshot of Terminal](screenshots/level23.png)
### Explanation 
A file name with spaces needs to be put in quotes. However, because the filename starts with --, it is seen as passing an option. Starting with a -- is a signal that everything after -- is the filename. 

## Level 3 -> 4 
### Objective 
Find the password for the next level is stored in a hidden file in the inhere directory.
### Commands Used 
- `ls`
- `ls -a` : -a means "all", it shows all files  including hidden files 
- `cd` : Change directory, moves into a folder 
- `cat`
### Solution 
```bash
ssh bandit3@bandit.labs.overthewire.org -p 2220
ls
cd inhere
ls -a
cat ...Hiding-From-You
```
[Screenshot of Terminal](screenshots/level34.png)
### Explanation 
Files that start with . are hidden, and cannot be seen with `ls`. Therefore, `ls -a` has to be used to see *all* the files in the directory. 

## Level 4 -> 5 
### Objective
Find the password for the next level, which is stored in the only human-readable file in the inhere directory.
### Commands Used 
- `ls`
- `cd`
- `file` : Determines file type and format
- `cat`
### Solution 
```bash
ssh bandit4@bandit.labs.overthewire.org -p 2220
ls
cd inhere
ls
file ./*
cat ./-file07
```
[Screenshot of Terminal](screenshots/level45.png)
### Explanation 
`file ./*` shows the file types of all the files in the folder inhere. file07 is the only one said to be ASCII text, so catting (?? is that a word) into that gives the password. 

## Level 5 -> 6 
### Objective 
Find the password for Level 6, which is stored in a human readable, non-executable file which is 1033 bytes in the inhere directory. 
### Commands Used 
- `cd`
- `find` : Searches for files and directories based on conditions like name, size, type etc.
- `file`
- `cat`
### Solution 
```bash
ssh bandit5@bandit.labs.overthewire.org -p 2220
cd inhere
ls
find . -size 1033c
file ./maybehere07/.file2
cat ./maybehere07/.file2
```
[Screenshot of Terminal](screenshots/level56.png)

In this case, there were many ASCII text files so doing the `find` command by size first was more efficient. 

## Level 6 -> 7 
### Objective 
Find the password, stored anywhere on the server with the following properties: owned by user bandit7, owned by group bandit6 and 33 bytes in size. 
### Commands Used 
- `find`
- `cat`
### Solution 
```bash
ssh bandit6@bandit.labs.overthewire.org -p 2220 
find / -user bandit7 -group bandit6 -size 33c 2>/dev/null
cat /var/lib/dpkg/info/bandit7.password
```
[Screenshot of Terminal](screenshots/level67.png)
### Explanation 
Format for find command: 
`find <where to start> <condition1> <condition2> ...`

Where to start: 
- `.` : Is used when the file to be found is within your current folder or its subfolders 
- `/` : A general search of the entirety of the server

Conditions: 
- `find . -name "*.txt"` finds all files with the specified name. Supports wildcards. `-iname` can be used to ignore case sensitivity.
- `find . -type f` for regular files and `find . -type d` for directories. 
- `find . -size 1033c` for size. `+` and `-` before the value means "greater than"/"less than". No symbol means exact value. 
- `find / -user bandit7 -group bandit6` for owners.
  
`2>/dev/null` redirects stderr, the error channel (stream 2) to `/dev/null` which is a fake file that discards anything sent to it. 

## Level 7 -> 8 
### Objective 
Find the password for the next level which is stored in the file data.txt next to the word millionth
### Commands Used 
- `ls`
- `grep` : Searches for specific text patterns inside files
### Solution 
```bash
ssh bandit7@bandit.labs.overthewire.org -p 2220
ls
grep millionth data.txt
```
[Screenshot of Terminal](screenshots/level78.png)
### Explanation 
`grep` searches through text and prints out every line that matches the given pattern 
- `-i` : Ignores cases
- `-r` : Searches for the text in the current folder and subfolders
- `-n` : Shows the line number of the match
- `-c` : Counts the number of lines that match
- `-w` : Whole word
- `-l` : Lists the files that have matches

Note: `grep "^millionth"` means the line starts with millionth, and `grep "millionth$"` means the line ends with millionth.
## Level 8 -> 9 
### Objective 
Find the password for level 9 which is stored in the file data.txt and is the only line of text that occurs only once
### Commands Used 
- `sort` : Rearranges the text in ascending order
- `uniq` : Filters out *adjacent*, *consecutive* (very important - needs to be sorted) duplicate lines
- `-u` : Prints only the line that is completely unique
### Solution 
```bash
ls
sort data.txt | uniq -u
```
[Screenshot of Terminal](screenshots/level89.png)

Note: `|` feeds the newly sorted output into the next command. `>` can be used to feed the output into a new file
## Level 9 -> 10 
### Objective 
Find the password for the next level, which is stored in the file data.txt in one of the few human-readable strings, preceded by several ‘=’ characters.
### Commands Used
- `ls`
- `strings` : : Prints sequence of readable text characters 
- `grep`
### Solution 
```bash
ls
strings data.txt | grep "==="
```
[Screenshot of Terminal](screenshots/level910.png)
### Explanation 
I tried doing just `grep "===" data.txt` at first, but that didn't work because it read the binary data as well. `strings` was necessary to narrow it down to the human readable text characters first. 
## Level 10 -> 11 
### Objective 
Find the password for the next level stored in the file data.txt, which contains base64 encoded data.
### Commands Used 
- `ls`
- `base64 -d` : Decodes Base64 to human readable text
### Solution 
```bash
ls
base64 -d data.txt
```
[Screenshot of Terminal](screenshots/level1011.png)
### Explanation
Base64 is binary data represented as plain text using 64 ASCII characters. It can usually be identified by = or == padding at the end. `base64 -d` converts it to human readable text. 
## Level 11 -> 12 
### Objective 
Find the password for the next level, which is stored in the file data.txt, where all lowercase (a-z) and uppercase (A-Z) letters have been rotated by 13 positions
### Commands Used 
- `ls`
- `tr` : Translate, it takes whatever text you give and swaps out specified characters 
### Solution 
```bash
ls
tr 'A-Za-z' 'N-ZA-Mn-za-m' < data.txt
```
[Screenshot of Terminal](screenshots/level1112.png)
### Explanation 
`tr SET1 SET2 < inputfile.txt`
- Characters in SET1 get swapped out for the characters in corresponding positions in SET2
- `<` feeds the file's contents into `tr`
## Level 12 -> 13 
### Objective 
Find the password for the next level is stored in the file data.txt, which is a hexdump of a file that has been repeatedly compressed. 
### Commands Used 
- `ls`
- `mktemp -d` : Created a temporary directory with a random name that is guaranteed to be unique. `-d` specified that it should be a directory, otherwise `mktemp` creates a temporary file.
- `cp` : Copies file from one place to another. `~` is home directory, `.` is current directory.
- `xxd` : Hexdump, `-r` means reverse and converts hex text -> binary.
- `file`
- `mv` : Move, renames the file without changing its contents.
- `gunzip` : Decompresses .gz files 
- `bunzip2` : Decompresses .bz2 files 
- `tar -x` : Extracts tar file
- `cat`
### Solution 
```bash
ls
mktemp -d
cd /tmp/tmp.VQ4SrK2nnI
cp ~/data.txt .
xxd -r data.txt > data.bin
file data.bin #Returns gzip compressed data 
mv data.bin data.gz
gunzip data.gz
file data #Returns bzip2 compressed data
mv data data.bz2
bunzip2 data.bz2
file data #Returns gzip compressed data
mv data data.gz
gunzip data.gz
file data #Returns POSIX tar archive (GNU)
mv data data.tar
tar -xf data.tar
ls
file data5.bin #Returns POSIX tar archive (GNU)
tar -xf data5.bin
ls
file data6.bin #Returns bzip2 compressed data
mv data6.bin data6.bz2
bunzip2 data6.bz2
file data6 #Returns POSIX tar archive (GNU)
mv data6 data6.tar
tar -xf data6.tar
ls
file data8.bin #Returns gzip compressed data
mv data8.bin data8.gz
gunzip data8.gz
file data8
cat data8
```
[Screenshot 1](screenshots/level1213-1.png) | [Screenshot 2](screenshots/level1213-2.png) 
### Explanation 
- `.gz` and `.bz2` show that the file is compressed. So when decompressed, it loses that extension entirely and is not replaced by anything.
- tar is a bundle of files. It needs to be *extracted* from, which is why `-x` has to be used. `-f` specifies the file.
- `mv` can also be used to move files across different directories etc. 
## Level 13 -> 14 
### Objective 
Log into level 14 using a private SSH key. Find the password for the next level is stored in /etc/bandit_pass/bandit14
### Commands Used 
- `ls -a` : Lists all the files in long format ie with detailed information like permissions, owner, size and date
- `chmod` : Change mode, it changes permissions for owner, group and others represented as three digits
- `i` : Identity file, used to indicate the private key
- `echo` : Can be used to print text back to the screen or to a file
### Solution 
```bash
ssh bandit13@bandit.labs.overthewire.org -p 2220
ls
cat sshkey.private
exit
echo "-----BEGIN OPENSSH PRIVATE KEY-----
***
-----END OPENSSH PRIVATE KEY-----" > private.key
ls -la
chmod 600 private.key
ls -la
ssh bandit14@bandit.labs.overthewire.org -p 2220 -i private.key
cat /etc/bandit_pass/bandit14
```
[Screenshot 1](screenshots/level1314-1.png) | [Screenshot 2](screenshots/level1314-2.png) | [Screenshot 3](screenshots/level1314-3.png)

### Explanation 
`chmod 600` means that the owner can read and write, while group and others can do nothing
- 0 : `---` : No permissions
- 1 : `--x` : Execute only
- 2 : `-w-` : Write only
- 3 : `-wx` : Write and execute (3+1)
- 4 : `r--` : Read only
- 5 : `r-x` : Read and execute (4+1)
- 6 : `rw-` : Read and write (4+2)
- 7 : `rwx` : Full access ie read, write and execute (4+2+1)

<details> 
  <summary> oops </summary>
  I hated this level so much. I spent so much time on this task because I couldn't figure out why `ls -la` and `chmod` weren't working. I completely forgot that I was working remotely from Windows Terminal. I switched to Ubuntu after this.  
</details>

## Level 14 -> 15 
### Objective 
Find the password for the next level by submitting the password of the current level to port 30000 on localhost.
### Commands Used 
- `nc` : netcat, a tool for reading and writing data across network connections
### Solution 
```bash
ssh bandit14@bandit.labs.overthewire.org -p 2220 -i private.key
nc localhost 30000
aaWecNkG4FhxJQxz07uiwzVP6bJiYS65
```
[Screenshot of Terminal](screenshots/level1415.png)
## Level 15 -> 16 
### Objective 
Find the password for the next level by submitting the password of the current level to port 30001 on localhost using SSL/TLS encryption.
### Commands Used 
- `openssl s_client` : Diagnostic tool used to test, debug, and analyze SSL/TLS connections to remote servers
### Solution 
```bash
ssh bandit15@bandit.labs.overthewire.org -p 2220
openssl s_client -connect localhost:30001
pbLYuZtTg4MgaqfJx8jbA9gKKGqM68A7
```
[Screenshot 1](screenshots/level1516-1.png) | [Screenshot 2](screenshots/level1516-2.png)
### Explanation 
`s_client` does the same thing as `nc` but over an encrypted connection. Basic syntax: 
```bash
openssl s_client -connect hostname:port
```
