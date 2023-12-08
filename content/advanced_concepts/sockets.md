---
title: "Sockets"
date: 2022-11-20T12:01:37+01:00
draft: false
weight: 60
---

#### If you want to send or receive data through an network, one way of achieving this is by using a concept called *sockets*.

In short sockets function as an endpoint of a local network.

For that we use UDP and TCP

| UDP | TCP |
|:---:|:---:|
|  UDP uses ports to allocate data to the correct program, so it is connectionless   |  TCP establishes a connection between two end points of a network connection (sockets)   |

---

Lets see the basics of how to setup sockets.

Before we start we need to include the header for the TCP and UDP sockets.

```cpp 
#include "../CForge/Internet/TCPSocket.h"
#include "../CForge/Internet/UDPSocket.h"
``` 
With that done, lets look at the functions those classes provide us

Both UDP and TCP differentiate between *Clients* and *Server*.

**ToDo: Einzelne Functions Parameter erläutern, sofern nötig**

##### 1) The UDPSocket class has following functions

Firstly you need to *create* the UDPSocket with the following function:
```cpp
void begin(SocketType Type, uint16_t Port);
```
To *stop* the UDPSocket we use the following function:
```cpp
void end(void);
```
*Sending data* is possible with the following function:
```cpp
void sendData(uint8_t* pData, uint32_t DataSize, std::string IP, uint16_t Port);
```
Lastly to *receive data* we use the following function:
```cpp
bool recvData(uint8_t* pBuffer, uint32_t* pDataSize, std::string* pSender, uint16_t* pPort);
```
##### 2) The TCPSocket class has following functions

First we need to *create* a TCP socket and bind it with a port, the **SocketType** is either a Client *or* a Server
```cpp
void TCPSocket::begin(SocketType Type, uint16_t Port)  //creates a socket and binds the server to the given port
```
With the **end()** function we can *close* the socket and all its connections
```cpp
void TCPSocket::end(void) //closes socket and all connections
```
The **connectTo()** function gives us an boolean that tells us if the connection between the port and the given IP was succesful.
```cpp
bool TCPSocket::connectTo(std::string IP, uint16_t Port) //returns if the connection was succesful
```
To *send Data* we can use the **sendData()** function, the parameter are as follows:
```cpp
void TCPSocket::sendData(uint8_t* pData, uint32_t DataSize, int32_t ConnectionID)
```
To *receive Data* we can us the **recvData** function, the parameter are as follows:
```cpp
bool TCPSocket::recvData(uint8_t* pBuffer, uint32_t* pDataSize, int32_t ConnectionID) 
```

##### 3) With that we now got a little overview about the provided functions, next lets see how it could work in practice.**

**UDP**
```cpp 
UDPSocket UDPServer;
UDPSocket UDPClient1;
UDPSocket UDPClient2;
int32_t UDPPort = 23456;
int32_t UDPMsgCounter = 0;
bool UDPActive = false;
```
**TCP**
```cpp
TCPSocket TCPServer;
TCPSocket TCPClient1;
TCPSocket TCPClient2;
int32_t TCPPort = 12345;
int32_t TCPMsgCounter = 0;
bool TCPActive = false;
```
In addition we need more variables for incoming data
```cpp
std::string Sender;
uint16_t Port;
uint8_t Buffer[128];
uint32_t DataSize;
```