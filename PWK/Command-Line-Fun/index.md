# COMMAND LINE FUN
+ Introduction to Kali Linux most popular and common commands

# 1 - THE BASH ENVIRONMENT
+ Bash is a shell to run complex commands and preform different tasks

## 1.1 - ENVIRONMENT VARIABLES
+ A new environment is initialized when opening new bash treminal
+ environment data is stored on ENVIRONMENT VARIABLES

```bash
$PATH : directories which bash searches in for commands
echo $PATH

$USER : the current user
$HOME : the current users home directory
export b=1.1.1.1 # make new global environment variable
b=1.1.1.1 # make new environment variable but only accessible in current shell
env # Show all environment varialbes
```


## 1.2 - TAB COMPLETION
+ Bash auto complete commands with `TAB` Key

```bash
ls D<tab>
Downloads/ Desktop/ Documents/

ls De<tab>
Desktop/
```


## 1.3 - BASH HISTORY TRICKS
+ Bash store used commands in history file named .bash_history in user's home directory
+ 2 env variables contains history size
	+ `$HISTSIZE` which is number of commands stored in memory for the current session
	+ `HISTFILESIZE` which is how many commands kept in history file
	+ These can be saved in bash history file
+ arrow key up moves backward in history
+ arrow ket down moves forward in history
+ `CTRL + R` searchs in commands and fins suitable one

```bash
history # Show history commands
!<number> # Run command with the number associated
!! # Execute last command in out terminal session (like arrow key up and enter)
```

# 2 - PIPING AND REDIRECTION
+ Every program has 3 data streams connected to it
	+ stdin : 0
	+ stdout : 1
	+ stderr : 2
+ We can connect streams between programs by piping using pipe operation `|`

## 2.1 - REDIRECTING TO A NEW FILE
```bash
echo abcd > "file name" # Replace <file name> content with abcd, If the file does not exist it will be created
```

## 2.2 -  REDIRECTING TO AN EXISTING FILE
```bash
echo abcd >> "file name" # Append content to the existing file
```

## 2.3 -  REDIRECTING FROM A FILE
```bash
wc -m < filename.txt # Counts characters in a file
```


## 2.4 - REDIRECTING STDERR
```bash
ls ./non_existing_file 2>error.txt # Redirect standard error of command to error.txt
```

## 2.5 - PIPING
```bash
cat filename.txt | wc -c # Read file and pipes it to wc which counts characters of the file
cat filename.txt | wc -c > count.txt 
```


# 3 - TEXT SEARCHING AND MANIPULATION
+ File and Text Handling with (grep, sed, awk, cut) advanced usage requires good understanding of regex

## 3.1 - GREP
+ Search for text i output and print matches

```bash
ls -la /usr/bin | grep zip # Print lines which hash zip in it
grep -i # Case insensitive
grep -r # Search Recursively
```

## 3.2 - SED
+ Text editing on a stream of text

```bash
echo "Try hard" | sed 's/hard/harder' # Replace harder with harder s means replace
"Try harder"
```

## 3.3 - CUT
+ Extract section of text and print it

```bash
echo 'a,b,c,d' | cut -d ',' -f 2 # Split text by ',' and prints second element
b

cut -d ":" -f 1 /etc/passwd
"usernames in /etc/passwd"
```

## 3.4 - AWK
+ A programming language designed for text proccessing and data extraction
+ cut only can accept one character field seperator but awk is more flexible

```bash
echo "aaaa::bbbb::cccc" | awk -F "::" '{print $1 $3}' # -F is field seperator and print $1 $3 extract first and third field
aaaa cccc
```

## 3.5 - PRACTICAL EXAMPLES
```bash
cat access.log | cut -d ' ' -f 1 | sort -u # Prints IP Addresses

cat access.log | cut -d ' ' -f 1 | sort | uniq -c | sort -urn # Print IP Add

cat access.log | grep <IP> | cut -d '"' -f 2 | uniq -c

cat access.log | grep <IP> | grep '/admin' | sort -u
```

# 4 - EDITING FILES FROM THE COMMAND LINE
+ It is important to have text editing skill inside terminal without any GUI-based editors like 'gedit'/leafpad
+ We will cover 2 of the most important terminal text editors `VI` and `NANO`

## 4.1 - NANO
+ Simple and fast learning text editor

```bash
nano <file name>

CTRL + O # Save changes
CTRL + K # Cut current like
CTRL + U # Paste cutted line at cursor location
CTRL + W # Search in the file
CTRL + x # Exit the file
```

## 4.2 - VI
+ Complex but powerful text editor

```bash
vi <file name>

i in command mode # Enter insert mode
esc in insert mode # Exit insert mode and enter command mode
dd in command mode # Delete and cut current line
yy in command mode # Copy current line
p in command mode # Paste clipboard content
x in command mode # Delete current character
:w in command mode # Save file
:q! in command mode # Quit without saving
:wq in command mode # Save file and quit
```


# 5 - COMPARING FILES
+ Many technically oriented proffesionals rely on this 

## 5.1 - COMM
+ Compares two files and print uniq and common lines

```bash
comm file1 file2 # Print unique lines in file1 in first collumn, uniqe lines inf file2 in second column and common files in third column
comm -12 file1 file2 # Print common lines since we suppressed columns 1,2
```

## 5.2 - DIFF
+ like COMM prints different lines wut is more complex
+ Two most popular formats, context and unified

```bash
diff -c file1 file2 # Display result in context format 
# - indicates lines in first file not in the second
# + inficates lines in second file not in the first

diff -u file1 file2 # Display result in unified format 
```


## 5.3 - VIMDIFF
+ combinations of vim and diif (opens two files and shows differences visually)

```bash
vimdiff file1 file2
CTRL + W + arrow key # Switch between windows
] + c # Jump to the next change
[ + c # Jump to the previous change
d + o # Get change from other windows and put it in current one
d + p # Get change from current windows and put it in other one
:q! # Quit vimdiff
```

# 6 - MANAGING PROCESSES
+ Linux kernel manages multi-tasking through use of processes
+ Each process is assgined a number called process ID (PID)
+ Linux also introduce concept of jobs 

```bash
cat file | wc -c # Two processes but one job
```

## 6.1 - BACKGROUNDING PROCESSES (BG)
+ Sometimes it is needed to send processes to background and gain control later
1. We can do it by putting `&` at the end of our command

```bash
ping -c 400 localhost > ping_results.txt &
```
2. We can suspend and stop execution of current foreground command with `CTRL + Z`

```bash
ping -c 400 localhost > ping_results.txt
CTRL+Z
```
3. We can resume previous suspended command with `bg` command

```bash
bg # Runs and resume job in background
```

## 6.2 - JOBS CONTROL: JOBS AND FG
+ builtin jobs utility list jobs in current terminal session
+ Specific jobs can be resumed by their PID or command name

```bash
jobs # List jobs in current terminal session
fg %1 # Resume job number 1 and returns it in foreground
fg # Resume only one job and returns it in foreground
```

## 6.3 - PROCESS CONTROL: PS AND KILL
+ ps which stands for process status
+ ps list processes wide not for curernt terminal

```bash
ps -ef
# -e displays all processes -f List all format listing

ps -fC <command name>
# -C command name
```
+ We can stop a process by `kill` command and require PID

```bash
kill <PID>
```


# 7 - FILE AND COMMAND MONITORING
+ Monitor files and commands in real time with `tail` and `watch`

## 7.1 - TAIL
+ Monitor log files entry as they are being written

```bash
sudo tail -f /var/log/apache2/access.log
# -f continuously update the tail output in real time

tail -nx filename # Prints last x lines of the file
```

## 7.2 - WATCH
+ Watch command is used to run specific commands repeatedly in specific period of time
+ By default it's preiod is 2 seconds but we can change it

```bash
watch -n <seconds> <command> # Runthe specified <command> in every <n> seconds
# Use CTRL + C to exit it and return to the terminal
```

## 8 - DOWNLOADING FILES
+ Study tools to download files from command line

## 8.1 - WGET
+ Downloads files using HTTP and FTP protocol

```bash
wget -O filename <URL LINK> # Downloads file from URL and saves it in different name by -O (capital case o) option
```

## 8.2 - CURL
+ A tools to transfer data to or from a server using a host of protocols
+ Download or Upload files

```bash
curl -o filename <URL LINK> # Downloads file from URL and saves it in different name by -o (lower case o) option
```

## 8.3 - AXEL
+ A download accelerator which transfer file through http or ftp protocol with multiple connections

```bash
axel -a -n 20 -o filename <LINK>
# -a shows all progress during download
# -n 20 number of connections which is 20 here
# -o filename to save
```

# 9 - CUSTOMIZING THE BASH ENVIRONMENT

## 9.1 - BASH HISTORY CUSTOMIZATION
+ `HISTCONTROL` environment variable defines whether or not removes duplicate commands

```bash
export HISTCONTROL=ignoredups # Remove duplicate variables
export HISTCONTROL=ignorespace # Remove commands start with space
# by default oth are enabled

export HISTIGNORE="&:ls:[bf]g:exit:history" # Ignore specific commands

export HISTTIMEFORMAT='%F %T ' # Show date and 24 hour format clock
# Other formats can be found in strftime man page
```


## 9.2 - ALIAS
+ An string defined to use as specific command with shorter name

```bash
alias lsa = 'ls -la' # When we enter lsa it will execute 'ls -la'
```

## 9.2 - PERSISTENT BASH CUSTOMIZATION
+ The behaviour of interactive shell bash is in `/etc/bash.bashrc`
+ system wide bash settings can be changed by editing `.bashrc` located in every user's home directory
+ We can put `alias` in `.bashrc` or other commands to be executed every time a bash promt 


# 10 - WRAPPING UP
