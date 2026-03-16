#OverTheWire: Bandit — Writeup

## Level 0
Initial SSH connection:
```bash
ssh bandit0@bandit.labs.overthewire.org -p 2220
```
---
## Level 0 → 1
```bash
ls
cat readme
```
Password: `ZjLjTmM6FvvyRnrb2rfNWOZOTa6ip5If`
---
## Level 1 → 2
The file's name is `-`, so we have to specify the exact route:
```bash
cat /home/bandit1/-
```
Password: `263JGJPfgU6LtdEvgfWU1XP5yac29mFx`
---
## Level 2 → 3
The file has spaces in its name, so we use quotes:
```bash
cat "./-spaces in this filename--"
```
Password: `MNk8KNH3Usiio41PRUEoDFPqfxLPlSmx`
---
## Level 3 → 4
The file is hidden 
```bash
cd inhere
ls -la
cat "...Hiding-From-You"
```
Password: `2WmrDFRmJIq3IPxneAaMGhap0pFhF3NJ`
---
## Level 4 → 5
```bash
cd inhere
file ./*
cat ./-file07
```
Password: `4oQYVPkxZOOEOO5pTW81FB8j8lxXGUQw`
---
## Level 5 → 6
We are looking for a very specific file:
```bash
cd inhere
find . -type f -size 1033c
```
Password: `HWasnPhtq9AVKe0dmk45nxy20cvUa6EG`
---
## Level 6 → 7
Since it says the password is somwhere on the server, not in that directory, I did:
```bash
find / -type f -size 33c -user bandit7 -group bandit6 2>/dev/null
```
file founded: `/var/lib/dpkg/info/bandit7.password`
```bash
cat /var/lib/dpkg/info/bandit7.password
```
Password: `morbNTDkSW6jIlUc0ymOdMaLnOlFVAaj`
---
## Level 7 → 8
The flag is in `data.txt` next to the word `millionth`.
```bash
cat data.txt | grep "millionth"
```
>  `cat data.txt grep "millionth"` bad idea, you have to use the pipe `|`.
**Password: `dfwvzFQi4mU0wfNbFOe9RoWskMLg7eEc`**
---
## Level 8 → 9
```bash
sort data.txt | uniq -u
```
Password: `4CKMh1JI91bUIZZPXDqGanal4xvAg0JM`
---
Writeup in progress
