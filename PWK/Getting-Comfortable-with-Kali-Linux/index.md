# Getting Comfortable with Kali Linux
+ Kali Linux is developed by offensive security and has many tools for security purposes


# 1 - BOOTING UP KALI LINUX
+ Boot on VM and Change default password

```bash
passwd
```
+ Don't use root user (use sudo instead)

```
sudo command
```

# 2 - THE KALI MENU
+ A good structure for different tools and categories
+ explore through them to find your special tool

# 3 - KALI DOCUMENTATION
+ https://docs.kali.org (official Kali Linux documentation)
+ https://forums.kali.org (Kali Linux support forums) Read the rules and compliances
+ https://tools.kali.org (Tools ins Kali Linux)
+ https://bugs.kali.org (Track bugs in specific tools or OS)
+ https://kali.training (Official Kali Linux training course)


# 4 - FINDING YOUR WAY AROUND KALI

## 4-1 - THE LINUX FILESYSTEM
+ /bin/ -> contains basic programs like ls, cd, cat
+ /sbin/ -> contains system programs like fdisk, mkfs, sysctl
+ /etc/ -> contains configurations files
+ /tmp -> contains temp files deleted on boot
+ /usr/bin -> conatins applications like apt, ncat, nmap
+ /usr/share/ -> contains application support and data files


## 4-2 BASIC LINUX COMMANDS
+ **man pages** (formal piece of documentation about commands)

```bash
man passwd
```

+ search man pages with keyword

```bash
man -k passwd 
```

+ **apropos** (description-based search)

```bash
apropos partition # Search commands which have partition in their description
```

+ **Listing files**

```bash
ls # List files
ls *.* ?.? # List files based on wildcard
ls -a1 # List all files in single line (automation)
```


+ **Moving around**

```bash
cd directory # Change directory to specified directory
pwd # show current directory
cd ~ # change to home directory
```

+ **Creating directories**

```bash
mkdir # Create Directory
mkdir a b # Create to directories names a,b
mkdir "a b" # Create one directory named "a b"
mkdir -p ./a/b/c/d # Create recursive directories
mkdir -p test/{recon,exploit,report}
```

## 4-3 FINDING FILES IN KALI LINUX
```bash
# Searches in $PATH variable
which filename

# Quickest way to find file, searches in built-in DB locate.db, updatebd command updates it
locate filename

# Most complex and flexbile tool to find files based on timestamp,name,permission,...
find
```

# 5 - MANAGING KALI LINUX SERVICES
+ Some Services like SSH, HTTP Web Server, MySql, ...
+ Netword services start by default

## 5-1 SSH SERVICE
```bash
sudo systemctl start ssh # Starts SSH Service
sudo ss -alnpt | grep sshd # Verify SSH service is up and running
sudo systemctl enable ssh # Starts SSH on boot time
```

## 5-2 HTTP SERVICE
+ Used for hosting files for downloading

```bash
sudo systemctl start apache2 # Starts Apache Service
sudo ss -alnpt | grep sshd # Verify Apache service is up and running
sudo systemctl enable ssh # Starts Apache on boot time
sudo systemctl list-unit-files # See all services
```

# 6 - SEARCHING, INSTALLING, AND REMOVING TOOLS
Explore package tools, Not all tools can be installed by default

## 6.1 - APT UPDATE
+ Information cached locally
+ update the list of repositories

```bash
sudo apt update # update list of repositories
```

## 6.2 - APT UPGRADE
+ after updadint APT db we can use it t upgrade packages

```bash
# Upgrade single packege with name
sudo apt upgrade <package name>

# Upgrade entire OS but better to take snapshot before upgrading
sudo apt upgrade
```

## 6.3 - APT-CACHE SEARCH AND APT SHOW
```bash
# Search for package name in cached repositories based on name and description
apt-cache search <package name>

# Show all information about package name
apt show <package name>
```

## 6.4 - APT INSTALL
```bash
# Add and install package to system
sudo apt install <package name>
```

## 6.5 - APT REMOVE --PURGE
```bash
# Completely removes and all package data but leaves config files
sudo apt remove <package name>

# Completely remove all package data including config files
sudo apt remove --purge <package name>
```

## 6.6 - DPKG
+ Core tool to install a package either directly or indirectly through `APT`
+ used offline no internet conneciton needed
+ It does not install dependent packages

```bash
# install <package-name>.deb file 
sudo dpkg -i <package name>.deb
```

# 7 -  WRAPPING UP
