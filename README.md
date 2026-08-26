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
SSH connected me to the port, and then I entered the given password bandit0. ls command listed all the files in the home directory - there was only one called readme. cat command displayed the contents of the file, inside which there was the password to Level 1. 
### Password 
<details> 
  <summary> Spoiler </summary> 
  6y2kwnwK6grgvwvpvLaa2T1cpFEKOhNR
</details>
