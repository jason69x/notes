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

these file descriptors are index to entries containing structs that have information about the device/file pointed by the descriptor.

*two types of internet sockets*
- stream sockets `SOCK_STREAM`
- datagram sockets `SOCK_DGRAM` (connectionless sockets)

stream sockets are reliable two-way connected communication streams.
items arrive in order, error-free.
ssh,http uses stream sockets.

stream sockets uses TCP. `send()`
datagram sockets uses UDP. `sendto()`

*port number*, 16-bit , local address for the connection.

host-byte-order
network-byte-order
`htons()` `ntohl()`

*sockaddr structs*
![[struct_sockaddr.png|500]]
![[sockaddr_in.png|450]]
![[in_addr.png|450]]
setting `sin_zero[8]` to zero isnt' necessary.
use `sockaddr_storage` if trying to write ip agnostic code

*inet_pton() & inet_ntop()*
![[inet_pton_ntop.png|450]]
returns `-1` on error, or `0` if the address is messed up.
use this to set `struct in_addr sin_addr` field of `struct sockaddr_in`
or use `inet_addr("0.0.0.0")` to set `s_addr` field of `sin_addr`

*socket()*  - allocate a socket descriptor
![[socket.png|450]]
domain - PF_INET/PF_INET6
type - SOCK_STREAM/SOCK_DGRAM
protocol - 0 (choose acc. to type) IPPROTO_TCP

`sockfd = socket(res->ai_family, res->ai_socktype, res->ai_protocol)`

*bind()* - associate a socket with an IP address and port number
![[bind.png|500]]

`bind(sockfd, res->ai_addr, res->ai_addrlen)`

*connect()* - connect a socket to a server 
![[connect.png|500]]
binds the sockfd to a port if not binded.

all these functions set global var `errno` in case of errors

*listen()* - tell a socket to listen for incoming connections
![[listen.png|300]]
backlog, number of connections allowed on the incoming queue.

*accept()* - Accept an incoming connection on a listening socket 
![[accept.png|530]]
returns a brand new socket descriptor to use for this single connection, ready to `send()` and `recv()`. old `sockfd` is still listening for more connections. this socket is open and connected to the remote host.

`sockfd` is the listening socket descriptor. `addr` is a pointer to local `struct sockaddr_in`, this stores client info (addr,port), `addrlen` size of sockaddr_in.

*send()* - send data over sockfd, returns no. of bytes sent
![[send().png|450]]

*recv()* - read from sockfd to a buffer, returns no. of bytes written
![[recv().png|450]]
0 return value means, server closed the connection.

`close(sockfd)`  - close and free a socket descriptor
`shutdown(sockfd,how)`, `SHUT_RD SHUT_WR SHUT_RDWR`

`perror()`, prints passed string to stderr followed by : and description of error from `errno` . stdio.h . prints error as human readable string.

`errno`, global int that holds error code of the most recent failed system call or library function. thread local variable. errno.h

`memset()`, set given memory with given byte.

`fork()`, child inherits parent's file descriptors, so it prints to stdout using same fd as parent.

*select()* - check if sockets descriptors are ready to read/write
![[select().png|450]]

used to simultaneously check multiple sockets to see if they have data waiting to be `recv()` or if you can `send()` data to them without blocking

`FD_ZERO` - all fds that are not ready are cleared from the set

After select() returns:
- rfds is modified by select call → only ready fds remain set
- master stays untouched → it still contains all currently connected clients
So next loop:
- we copy fresh master again → rfds starts with all clients + listener
- we don't lose track of clients that were not ready this time

```c
define FD_SETSIZE   1024   
typedef struct {
    long      __fds_bits[FD_SETSIZE / (8 * sizeof(long))];
} fd_set;
```

each bit in `fd_set` represents a file descriptor number
`select()` only looks for `0 to max_fd-1` bits, that why we pass `max_fd+1`

fds which are not ready are set 0 by select() , only ready fds remain 1