Bandit Wargame Notes

How to enter any level: ssh -p 2220 [banditX@bandit.labs.overthewire.org](mailto:banditX@bandit.labs.overthewire.org) replace X with the level number, enter the password when prompted

Level 0

Description: Log into the game using SSH on port 2220. Username and password are both bandit0.

Commands used: ssh connects to a remote machine securely ssh -p 2220 [bandit0@bandit.labs.overthewire.org](mailto:bandit0@bandit.labs.overthewire.org) -p specifies the port, default SSH port is 22 so we need to specify 2220

How to move on: once logged in go to the level 1 page to find out how to get the next password

---

Level 0 to 1

Description: The password is stored in a file called readme in the home directory.

Commands used: ls lists files in the current directory used to see what files exist in the home directory

cat prints the contents of a file to the terminal cat readme used to read the contents of the readme file

How to move on: ssh -p 2220 [bandit1@bandit.labs.overthewire.org](mailto:bandit1@bandit.labs.overthewire.org) password is the output of cat readme

---

Level 1 to 2

Description: The password is stored in a file called - in the home directory. The dash is a special character in Linux so you cannot just cat it directly.

Commands used: cat ./- the ./ tells the shell to look in the current directory without ./ the shell interprets - as stdin meaning it waits for keyboard input instead of reading the file cat ./-

How to move on: ssh -p 2220 [bandit2@bandit.labs.overthewire.org](mailto:bandit2@bandit.labs.overthewire.org)

---

Level 2 to 3

Description: The password is stored in a file called spaces in this filename.

Commands used: cat "spaces in this filename" wrapping the filename in quotes tells the shell to treat it as one argument without quotes the shell thinks each word is a separate file alternatively use backslashes: cat spaces\ in\ this\ filename

How to move on: ssh -p 2220 [bandit3@bandit.labs.overthewire.org](mailto:bandit3@bandit.labs.overthewire.org)

---

Level 3 to 4

Description: The password is stored in a hidden file inside the inhere directory. Hidden files in Linux start with a dot and are not shown by ls by default.

Commands used: cd inhere changes directory into the inhere folder

ls -la -l shows detailed information about files -a shows all files including hidden ones that start with a dot used to reveal the hidden file

cat .hiddenfilename reads the hidden file once you know its name

How to move on: ssh -p 2220 [bandit4@bandit.labs.overthewire.org](mailto:bandit4@bandit.labs.overthewire.org)

---

Level 4 to 5

Description: The password is stored in the only human readable file in the inhere directory. There are multiple files and only one contains readable text.

Commands used: cd inhere moves into the inhere directory

file ./-file00 or file ./* file command tells you what type of data is in a file ASCII text means human readable used to identify which file contains readable text instead of binary data

cat ./-file07 once you identify the human readable file you cat it

How to move on: ssh -p 2220 [bandit5@bandit.labs.overthewire.org](mailto:bandit5@bandit.labs.overthewire.org)

---

Level 5 to 6

Description: The password is somewhere in the inhere directory tree. It is human readable, 1033 bytes in size, and not executable.

Commands used: find . -type f -size 1033c -readable ! -executable find searches for files matching specific criteria -type f means regular file not a directory -size 1033c means exactly 1033 bytes, c stands for bytes -readable means we can read it ! -executable means it is not executable this narrows down thousands of files to exactly one

cat on the result once found

How to move on: ssh -p 2220 [bandit6@bandit.labs.overthewire.org](mailto:bandit6@bandit.labs.overthewire.org)

---

Level 6 to 7

Description: The password is stored somewhere on the server. It is owned by user bandit7, owned by group bandit6, and is 33 bytes in size.

Commands used: find / -user bandit7 -group bandit6 -size 33c 2>/dev/null / means search from the root of the entire filesystem -user bandit7 finds files owned by bandit7 -group bandit6 finds files belonging to group bandit6 -size 33c finds files exactly 33 bytes 2>/dev/null redirects error messages to nowhere so they dont clutter the output without 2>/dev/null you get thousands of permission denied errors

cat on the result once found

How to move on: ssh -p 2220 [bandit7@bandit.labs.overthewire.org](mailto:bandit7@bandit.labs.overthewire.org)

---

Level 7 to 8

Description: The password is stored in the file data.txt next to the word millionth.

Commands used: grep "millionth" data.txt grep searches for lines containing a specific pattern millionth is the keyword we are looking for returns only the line containing that word along with the password next to it

How to move on: ssh -p 2220 [bandit8@bandit.labs.overthewire.org](mailto:bandit8@bandit.labs.overthewire.org)

---

Level 8 to 9

Description: The password is stored in data.txt and is the only line that appears exactly once. All other lines are duplicates.

Commands used: sort data.txt | uniq -u sort organizes all lines alphabetically so duplicates are next to each other uniq -u filters the output and only shows lines that appear exactly once the pipe | passes the output of sort into uniq without sorting first uniq would not work correctly because it only compares adjacent lines

How to move on: ssh -p 2220 [bandit9@bandit.labs.overthewire.org](mailto:bandit9@bandit.labs.overthewire.org)

---

Level 9 to 10

Description: The password is stored in data.txt in one of the few human readable strings preceded by several = characters.

Commands used: strings data.txt | grep "===" strings extracts all human readable text from a binary file grep "===" filters for lines containing multiple = signs the password is on a line that starts with several equals signs

How to move on: ssh -p 2220 [bandit10@bandit.labs.overthewire.org](mailto:bandit10@bandit.labs.overthewire.org)

---

Level 10 to 11

Description: The password is stored in data.txt which contains base64 encoded data.

Commands used: base64 -d data.txt base64 is an encoding scheme that converts binary data into ASCII text -d flag means decode decodes the base64 content back to readable text which contains the password

How to move on: ssh -p 2220 [bandit11@bandit.labs.overthewire.org](mailto:bandit11@bandit.labs.overthewire.org)

---

Level 11 to 12

Description: The password is stored in data.txt where all letters have been rotated 13 positions (ROT13).

Commands used: cat data.txt | tr 'A-Za-z' 'N-ZA-Mn-za-m' tr translates characters from one set to another A-Za-z is the full alphabet both upper and lowercase N-ZA-Mn-za-m is the alphabet shifted 13 positions ROT13 means every letter is replaced by the letter 13 positions ahead of it applying it twice returns the original text

How to move on: ssh -p 2220 [bandit12@bandit.labs.overthewire.org](mailto:bandit12@bandit.labs.overthewire.org)

---

Level 12 to 13

Description: The password is stored in data.txt which is a hexdump of a file that has been repeatedly compressed.

Commands used: mkdir /tmp/mydir cd /tmp/mydir cp ~/data.txt . work in tmp because the home directory is not writable

xxd -r data.txt > data reverses the hexdump back into binary data

file data used repeatedly to identify what compression format the file currently is tells you whether it is gzip, bzip2, tar, etc

mv data data.gz then gzip -d data.gz mv data data.bz2 then bzip2 -d data.bz2 tar xf data.tar repeated decompression using the correct tool for each format you repeat this process multiple times until you reach a plain text file

cat on the final file to get the password

How to move on: ssh -p 2220 [bandit13@bandit.labs.overthewire.org](mailto:bandit13@bandit.labs.overthewire.org)

---

Level 13 to 14

Description: There is no password for this level. Instead there is a private SSH key that can be used to log into bandit14.

Commands used: ls reveals the file sshkey.private in the home directory

chmod 600 sshkey.private changes permissions so only the owner can read the key SSH refuses to use a key file that is too open permissions wise 600 means owner read and write, nobody else gets anything

ssh -i sshkey.private [bandit14@bandit.labs.overthewire.org](mailto:bandit14@bandit.labs.overthewire.org) -p 2220 -i specifies the identity file to use instead of a password logs into bandit14 using the key instead of typing a password

How to move on: you are already logged in as bandit14 after the ssh -i command

---

Level 14 to 15

Description: The password for the next level can be retrieved by submitting the current level password to port 30000 on localhost.

Commands used: cat /etc/bandit_pass/bandit14 reads the current level password which we need to submit

echo "password" | nc localhost 30000 nc is netcat, a tool for making network connections localhost means this same machine 30000 is the port the service is listening on the pipe sends the password as input to the service the service responds with the next password if correct

How to move on: ssh -p 2220 [bandit15@bandit.labs.overthewire.org](mailto:bandit15@bandit.labs.overthewire.org)

---

Level 15 to 16

Description: Same as the previous level but the connection must be made using SSL encryption.

Commands used: echo "password" | openssl s_client -connect localhost:30001 -quiet openssl s_client creates an SSL encrypted connection -connect specifies host and port -quiet suppresses the certificate information output the pipe sends the password through the encrypted connection

How to move on: ssh -p 2220 [bandit16@bandit.labs.overthewire.org](mailto:bandit16@bandit.labs.overthewire.org)

---

Level 16 to 17

Description: Submit the current password to a port between 31000 and 32000. First find which ports are open, then identify which one uses SSL, then submit to the correct one.

Commands used: nmap -sV -p 31000-32000 localhost nmap scans for open ports -sV detects what service and version is running on each port -p 31000-32000 limits the scan to that port range used to find which ports are open and which use SSL

openssl s_client -connect localhost:PORTNUMBER -quiet used to connect to each SSL port and test it the correct port responds with an RSA private key instead of echoing back

the output is an RSA private key which you save to a file and use to log into bandit17 save the key to ~/bandit17.key chmod 600 ~/bandit17.key ssh -i ~/bandit17.key [bandit17@bandit.labs.overthewire.org](mailto:bandit17@bandit.labs.overthewire.org) -p 2220

How to move on: use the SSH key to log in directly

---

Level 17 to 18

Description: There are two files in the home directory, passwords.old and passwords.new. The password is the only line that has changed between the two files.

Commands used: diff passwords.old passwords.new diff compares two files line by line and shows what is different lines with < are from the old file lines with > are from the new file the line that changed is the password for the next level

alternatively: grep -v -f passwords.old passwords.new -f uses passwords.old as a list of patterns to search for -v shows lines that do NOT match finds lines in passwords.new that do not exist in passwords.old

How to move on: ssh -p 2220 [bandit18@bandit.labs.overthewire.org](mailto:bandit18@bandit.labs.overthewire.org) note: logging in normally gives a Byebye message and disconnects you

---

Level 18 to 19

Description: The password is in a file called readme in the home directory but the .bashrc has been modified to log you out the moment you connect.

Commands used: ssh [bandit18@bandit.labs.overthewire.org](mailto:bandit18@bandit.labs.overthewire.org) -p 2220 cat readme appending a command at the end of the SSH command runs it directly on the server before .bashrc executes bypasses the logout trap entirely the password prints directly to your local terminal

How to move on: ssh -p 2220 [bandit19@bandit.labs.overthewire.org](mailto:bandit19@bandit.labs.overthewire.org)

---

Level 19 to 20

Description: There is a setuid binary in the home directory. It runs commands as bandit20. Use it to read bandit20s password file.

Commands used: ls -la reveals the setuid binary named bandit20-do the s in the permissions -rwsr-x--- shows setuid is set owned by bandit20 meaning it runs as bandit20 when executed

./bandit20-do whoami running it without arguments shows usage running it with whoami confirms it executes as bandit20

./bandit20-do cat /etc/bandit_pass/bandit20 uses the binary to read the password file as bandit20 we cannot read that file ourselves because we are bandit19 but since the binary runs as bandit20 it can read it for us

How to move on: ssh -p 2220 [bandit20@bandit.labs.overthewire.org](mailto:bandit20@bandit.labs.overthewire.org)

---

Level 20 to 21

Description: There is a setuid binary called suconnect that connects to a port you specify, reads a line, and compares it to bandit20s password. If it matches it sends back bandit21s password.

Commands used: need two terminal windows both logged into bandit20

terminal 1: echo "bandit20password" | nc -l -p 1050 nc -l -p 1050 sets up a listener on port 1050 the echo pipes the password into nc so it sends it automatically when something connects -l means listen mode -p specifies the port

terminal 2: ./suconnect 1050 connects suconnect to your listener on port 1050 suconnect receives the password, validates it, sends back bandit21s password that password appears in terminal 1

How to move on: ssh -p 2220 [bandit21@bandit.labs.overthewire.org](mailto:bandit21@bandit.labs.overthewire.org)

---

Level 21 to 22

Description: A program is running automatically via cron. Look in /etc/cron.d to find what is being executed.

Commands used: cat /etc/cron.d/cronjob_bandit22 reads the cron configuration file shows what script is running and when every minute it runs /usr/bin/cronjob_bandit22.sh as bandit22

cat /usr/bin/cronjob_bandit22.sh reads the actual script being run the script sets permissions on a file in /tmp and writes bandit22s password into it

cat /tmp/t7O6lds9S0RqQh9aMcz6ShpAoZKF7fgv reads the file where the password was written the filename comes directly from the script

How to move on: ssh -p 2220 [bandit22@bandit.labs.overthewire.org](mailto:bandit22@bandit.labs.overthewire.org)

---

Level 22 to 23

Description: Same pattern as the previous level but this time the script uses a hash to generate the filename.

Commands used: cat /etc/cron.d/cronjob_bandit23 finds the cron job for bandit23

cat /usr/bin/cronjob_bandit23.sh reads the script, it hashes the string "I am user bandit23" with md5sum and uses that as the filename

echo I am user bandit23 | md5sum | cut -d ' ' -f 1 replicates exactly what the script does echo generates the string md5sum hashes it cut -d ' ' -f 1 takes only the hash part without the trailing spaces and filename gives you the exact filename where the password is stored

cat /tmp/hashvalue reads the file using the hash you calculated

How to move on: ssh -p 2220 [bandit23@bandit.labs.overthewire.org](mailto:bandit23@bandit.labs.overthewire.org)

---

Level 23 to 24

Description: A cron job executes all scripts in /var/spool/bandit24/foo that are owned by bandit23. Write a script that copies bandit24s password somewhere readable.

Commands used: cat /etc/cron.d/cronjob_bandit24 find the cron job

cat /usr/bin/cronjob_bandit24.sh understand what it does it loops through all files in /var/spool/bandit24/foo executes any file owned by bandit23 deletes them after running

nano /tmp/myscript.sh write the script: #!/bin/bash cat /etc/bandit_pass/bandit24 > /tmp/password.txt chmod 777 /tmp/password.txt

chmod 777 /tmp/myscript.sh makes the script executable so the cron job can run it 777 means everyone can read write and execute

cp /tmp/myscript.sh /var/spool/bandit24/foo/ copies the script to where the cron job looks

wait up to 1 minute then: cat /tmp/password.txt

How to move on: ssh -p 2220 [bandit24@bandit.labs.overthewire.org](mailto:bandit24@bandit.labs.overthewire.org)

---

Level 24 to 25

Description: A daemon on port 30002 gives the bandit25 password if you send it bandit24s password plus the correct 4 digit pincode. You must brute force all 10000 combinations.

Commands used: nano /tmp/mybrute.sh write the script: #!/bin/bash for i in $(seq -w 0 9999); do echo "gb8KRRCsshuZXI0tUuR6ypOFjiZbf3G8 $i" done | nc localhost 30002 | grep -v "Wrong"

seq -w 0 9999 generates all numbers from 0000 to 9999 with leading zeros -w pads numbers to equal width so 1 becomes 0001 the for loop builds each line as password followed by pincode nc localhost 30002 sends all attempts through one connection grep -v "Wrong" filters out all the failed attempts and only shows the success line

chmod +x /tmp/mybrute.sh /tmp/mybrute.sh

How to move on: ssh -p 2220 [bandit25@bandit.labs.overthewire.org](mailto:bandit25@bandit.labs.overthewire.org)

---

Level 25 to 26

Description: Bandit26 does not use /bin/bash as its shell. It uses a custom program called showtext which runs more to display a file and then exits. You need to escape this.

Commands used: cat /etc/passwd | grep bandit26 reveals that bandit26s shell is /usr/bin/showtext

cat /usr/bin/showtext shows the script runs more on ~/text.txt then exits

the escape: make your terminal window very small so more cannot display the full file and stays open scp -P 2220 [bandit25@bandit.labs.overthewire.org](mailto:bandit25@bandit.labs.overthewire.org):~/bandit26.sshkey ~/bandit26.sshkey downloads the SSH key to your local Kali machine

chmod 600 ~/bandit26.sshkey with a tiny terminal window: ssh -i ~/bandit26.sshkey [bandit26@bandit.labs.overthewire.org](mailto:bandit26@bandit.labs.overthewire.org) -p 2220 more opens and stays at a percentage

press v opens vim from inside more

in vim: :set shell=/bin/bash sets the shell to bash

:shell drops you into a real bash shell as bandit26

cat /etc/bandit_pass/bandit26 gets the password

How to move on: stay in the bandit26 shell for the next level

---

Level 26 to 27

Description: There is a setuid binary in bandit26s home directory. Same concept as level 19.

Commands used: ls -la reveals bandit27-do with setuid set, owned by bandit27

./bandit27-do cat /etc/bandit_pass/bandit27 runs cat as bandit27 to read the password file

How to move on: ssh -p 2220 [bandit27@bandit.labs.overthewire.org](mailto:bandit27@bandit.labs.overthewire.org) connect from your local Kali terminal not from within the bandit server

---

Level 27 to 28

Description: There is a git repository. Clone it and find the password inside.

Commands used: git clone ssh://bandit27-[git@bandit.labs.overthewire.org](mailto:git@bandit.labs.overthewire.org):2220/home/bandit27-git/repo clones the repository over SSH using the custom port 2220 use bandit27s password when prompted

cd repo ls cat README the password is in the README file

How to move on: ssh -p 2220 [bandit28@bandit.labs.overthewire.org](mailto:bandit28@bandit.labs.overthewire.org)

---

Level 28 to 29

Description: Same git clone but the password in the README has been replaced with x characters. Check the commit history.

Commands used: git clone ssh://bandit28-[git@bandit.labs.overthewire.org](mailto:git@bandit.labs.overthewire.org):2220/home/bandit28-git/repo

cd repo cat README.md shows the password is redacted

git log shows the commit history two commits visible, the initial commit and a later one that redacted the password

git show commithashere shows exactly what changed in a specific commit looking at the initial commit reveals the password before it was hidden

How to move on: ssh -p 2220 [bandit29@bandit.labs.overthewire.org](mailto:bandit29@bandit.labs.overthewire.org)

---

Level 29 to 30

Description: Same git clone but the README says no passwords in production. The keyword is production meaning other branches might have the real password.

Commands used: git clone ssh://bandit29-[git@bandit.labs.overthewire.org](mailto:git@bandit.labs.overthewire.org):2220/home/bandit29-git/repo

cd repo git branch -a lists all branches including remote ones reveals branches like dev and sploits-dev alongside master

git checkout dev switches to the dev branch

cat README.md the dev branch has the real password that was never redacted

How to move on: ssh -p 2220 [bandit30@bandit.labs.overthewire.org](mailto:bandit30@bandit.labs.overthewire.org)

---

Level 30 to 31

Description: Same git clone but the README is empty and there is only one branch with one commit. The secret is in a git tag.

Commands used: git clone ssh://bandit30-[git@bandit.labs.overthewire.org](mailto:git@bandit.labs.overthewire.org):2220/home/bandit30-git/repo

cd repo cat README.md just an empty file message

git log only one commit, nothing useful in history

git branch -a only master branch

git tag lists all tags in the repository reveals a tag called secret

git show secret shows the content of the tag the password is stored directly in the tag

How to move on: ssh -p 2220 [bandit31@bandit.labs.overthewire.org](mailto:bandit31@bandit.labs.overthewire.org)

---

Level 31 to 32

Description: Push a file called key.txt containing the text May I come in? to the master branch of the remote repository.

Commands used: git clone ssh://bandit31-[git@bandit.labs.overthewire.org](mailto:git@bandit.labs.overthewire.org):2220/home/bandit31-git/repo

cd repo cat README.md tells you exactly what to do

cat .gitignore reveals that *.txt files are ignored by git

echo 'May I come in?' > key.txt creates the file with the required content

git add -f key.txt -f forces adding the file even though it is in .gitignore without -f git refuses to add it

git config --global user.email "[anything@anything.com](mailto:anything@anything.com)" git config --global user.name "anything" git requires identity before committing, can be anything

git commit -m "adding key.txt" commits the file locally

git push origin master pushes to the remote repository enter bandit31s password when prompted the server validates the file and prints the next password in the response

How to move on: ssh -p 2220 [bandit32@bandit.labs.overthewire.org](mailto:bandit32@bandit.labs.overthewire.org)

---

Level 32 to 33

Description: This is the uppercase shell. Everything you type gets converted to uppercase before being executed so normal commands do not work.

Commands used: $0 $0 is a special bash variable that holds the name of the current shell typing $0 is not affected by the uppercase conversion because it is a variable not a command running it spawns a new shell that is not restricted

once in the new shell: cat /etc/bandit_pass/bandit33 reads the final password

How to move on: there is no level 34 yet, you have completed Bandit
