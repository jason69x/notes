 **ICMP**

*destination network unreachable* . at some point, an IP router was unable to find a path to the host specified in your HTTP request. that router created and sent an ICMP message to your host indicating the error.
they are carried inside IP datagrams. (upper layer protocol no. 1)
architecturally it lies just above IP.

ICMP messages have a type and code field, and contain header and first 8 bytes of the IP datagram that caused the ICMP message to be generated

*ping* program sends ICMP type 8 code 0 message to the specified host (echo request) and receives a ICMP type 0 code 0 echo reply.

*traceroute* - trace a route from a host to any other host. used to determine names and addresses of routers between source and destination.
source sends a series of IP datagrams carrying UDP segments with an unlikely UDP port number. the first of these datagrams has a TTL of 1, second of 2, and so on. the source also starts timers for each datagram

when the $n^{th}$ datagram arrives at $n^{th}$ router, it observes that TTL of the datagram has expired and sends a ICMP TTL expired (type 11 code 0) msg to the source. this msg includes the name and ip addr. of d router.
the source can calculate rtt from timer.
dest. sends a port unreachable (type 3 code 3) ICMP msg.

---

a routing loop may arise due to slow convergence or wrong static routing tables, DVR.

the boundary between router and any one of its link is also an *interface*. IP requires each host and router interface to have its own IP address. Thus, an IP address is technically associated with an interface, rather than with the host or router containig that interface.

each interface on every host and router in the global internet must have an IP address that is globally unique (expect for interfaces behind NATs).
a portion of interface's IP address will be determined by the subnet to which it is connected.

`/24` - subnet mask, indicates that leftmost 24bits define the subnet address.

`NIC` - your ip  `Router interface` - default gateway ip

*loopback* 
virtual network interface, allows a computer to communicate with itself, without sending packets onto the network. never hits the nic.
looped back by os.
router-loopback-interface, identity of the device.

NAT, windows act as nat , wsl->windows->router
Mirrored, same network view as windows without giving separate ip to wsl, its like a app running on windows
Bridged, separate ip to wsl,  wsl->router

`PROMISC` - sniff all traffic

*network address*
identifies the entire subnet/network itself, not any single device
routers, switches uses it internally for routing decisions.
(it checks network addr. in its routing table and forwards to it)

*address resolution protocol*

translation between ip addr. and mac addr.
a host provides ip addr. to ARP and it returns mac addr. corresponding to that ip.
resolves only for hosts and router interfaces on the same subnet.
each router and host has ARP table in their memory.

*socket programming*

kernel closes socket only when last reference is gone.

listening socket, tcp listen state
connected socket, tcp established state

