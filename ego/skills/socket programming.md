*source* - beejs guide

##### What is a Socket?
a way to speak to other programs using standard Unix file descriptors.

*file descriptor* acts as a handle to an open file(or socket/pipe/device)
what behaves like a stream or buffer is the i/o library used on top of the file descriptor.
- small non-negative integer
- private to each process
- used by program to tell kernel to do i/o on this resource.

resources live inside kernel.
when you open a file, kernel gives you a fd to read/write file.
fd maintains state for that particular opening of file in that process
content may live on memory,disk,kernel buffers and these needs kernel permission.

you make a call to `socket()` system routine. it retuns a socket descriptor, and you communicate through it using the specialized `send()` and `recv()` socket calls.

*two types of internet sockets*
- stream sockets `SOCK_STREAM`
- datagram sockets `SOCK_DGRAM` (connectionless sockets)

stream sockets are reliable two-way connected communication streams.
items arrive in order, error-free.
ssh,http uses stream sockets.

stream sockets uses TCP. `send()``
datagram sockets uses UDP. `sendto()`

*port number*, 16-bit , local address for the connection.

host-byte-order
network-byte-order
`htons()` `ntohl()`


socket descriptor - `int`
`struct addrinfo` `struct sockaddr` `struct sockaddr_in`
![[struct_addrinfo.png|500]]
![[struct_sockaddr.png|500]]
```c
struct sockaddr_in {
short int sin_family; // Address family, AF_INET
unsigned short int sin_port; // Port number
struct in_addr sin_addr; // Internet address
unsigned char sin_zero[8]; // Same size as struct sockaddr
};

struct in_addr{
	uint32_t s_addr;
};
```

`inet_pton()` - converts an IP address to `struct in_addr` presen. to network
```c
struct sockaddr_in sa;
inet_pton(AF_INET,"10.12.110.57",&(sa.sin_addr));
```
returns `-1` on error, or `0` if the address is messed up.
use this to set `struct in_addr sin_addr` field of `struct sockaddr_in`
or use `inet_addr("0.0.0.0")` to set `s_addr` field of `sin_addr`

`inet_ntop()` - extract ip from network
```c
char ip4[INET_ADDRSTRLEN];
struct sockaddr_in sa;
inet_ntop(AF_INET,&(sa.sin_addr),ip4,INET_ADDRSTRLEN)
```


*getaddrinfo()* - returns a linked list of multiple ways to connect with server. eg. google.com has multiple ip's for this domain. one entry per ip, separate ipv4/ipv6 entries,stream/dgram entries.

![[getaddrinfo.png|500]]

*socket()*  - returns a socket descriptor or `-1` on error
![[socket.png|500]]

domain - PF_INET/PF_INET6
type - SOCK_STREAM/SOCK_DGRAM
protocol - 0 (choose acc. to type) IPPROTO_TCP

`sockfd = socket(res->ai_family, res->ai_socktype, res->ai_protocol)`

*bind()* - associate socket with a port or `-1` on error
![[bind.png|500]]

`bind(sockfd, res->ai_addr, res->ai_addrlen)`

*connect()* - connect to a host or `-1` on err
![[connect.png|500]]
binds the sockfd to a port if not binded.

all these functions set global var `errno` in case of errors

*listen()* - wait for incoming connections
![[listen.png|300]]
backlog, number of connections allowed on the incoming queue.

*accept()* - gets the pending connection

	returns a brand new socket file descriptor to use for this single connection, ready to `send()` and `recv()`. old `sockfd` is still listening for more connections.

![[accept.png|530]]

`sockfd` is the listening socket descriptor. `addr` is a pointer to local `struct sockaddr_storage`, this stores client info (addr,port), `addrlen` size of sockaddr_storage.
```c
struct sockaddr_storage their_addr;
socklen_t addr_size;
// getaddrinfo()-> socket()-> bind() -> listen() -> accept()
addr_size = sizeof their_addr;
new_fd = accept(sockfd,(struct sockaddr*)&their_addr,&addr_size);
```

*send()* - send data over sockfd, returns no. of bytes sent
![[send().png|450]]

*recv()* - read from sockfd to a buffer, returns no. of bytes written
![[recv().png|450]]
0 return value means, server closed the connection.

`close(sockfd)` `shutdown(sockfd,how)`


`perror()`, prints passed string to stderr followed by : and description of error from `errno` . stdio.h

`errno`, global int that holds error code of the most recent failed system call or library function. thread local variable. errno.h

`memset()`, set given memory with given byte.

`fork()`, child inherits parent's file descriptors, so it prints to stdout using same fd as parent.