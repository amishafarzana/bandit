# OverTheWire's Bandit Challenge
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

SSH connected me to the port, and then I entered the given password bandit0. ls command listed all the files in the home directory - there was only one called readme. cat command displayed the contents of the file, inside which there was the password to Level 1. 
### Password 
<details> 
  <summary> Spoiler </summary> 
  6y2kwnwK6grgvwvpvLaa2T1cpFEKOhNR
</details>

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

`cat -` didn't work because - is a special command. To bypass this, I had to specify the folder using `./`  to show that - is a file name. 
### Password 
<details>
  <summary> Spoiler </summary>
  PK8fYLZg2hnHSz83plBL1iEPKdD3QToB
</details>

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

A file name with spaces needs to be put in quotes. However, because the filename starts with --, it is seen as passing an option. Starting with a -- is a signal that everything after -- is the filename. 
### Password 
<details>
  <summary> Spoiler </summary>
  7ZZ2LFrykP2zEyvBl4m3clcL7tGYJPME
</details>

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

Files that start with . are hidden, and cannot be seen with `ls`. Therefore, `ls -a` has to be used to see *all* the files in the directory. 
### Password 
<details> 
  <summary> Spoiler </summary>
  xzTXq1rDJQVVAzdv5cHq1TQytTWufAMq
</details>

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

`file ./*` shows the file types of all the files in the folder inhere. file07 is the only one said to be ASCII text, so catting (?? is that a word) into that gives the password. 
### Password 
<details>
  <summary> Spoiler </summary>
  6C7h9GD8M6ai5nr7wo1RonrzFjj9yIrG
</details>
