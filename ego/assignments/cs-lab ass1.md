
###### Q1. Ping Command Exploration
1.*detect packet loss*

 ping can be used to detect packet loss by sending packets to a host and when a appropriate amount of packets have been sent, we can quit ping and it will display all statistics (with packet loss %) at the end. below is an example `ping youtube.com`
 
 ![[ping_packet_loss.png | 600]]

2.*time to live*

the time-to-live (TTL) field is included to ensure that ping packets do not circulate forever (eg. routing loop) in the network. this field is decremented by one each time the packet is processed by a router. if the TTL field reaches 0, a router must drop that packet.

we can use `-t <ttl>` flag to modify its value.
`ping -t 64 youtube.com`

3.*total duration*

we can set a total duration in seconds for which the ping command should run by using `-w <deadline>` flag.
`ping -w 10 youtube.com`

4.*timeout value for waiting for an Echo Reply*

we can set this using `-W <timeout>` flag. after using this flag, the ping command will wait `<timeout>` seconds for each reply. if the timeout expires, ping doesn't output anything for that packet. these timed out packets count towards total packet loss. total runtime of ping can stretch.

###### Q2. RTT and Packet Loss Analysis

*selected six hosts*

| Hosts          | avg.RTT (2pm) | avg.RTT (8pm) | avg.RTT (9am) | distance (km) |
| :------------- | :-----------: | :-----------: | :-----------: | :-----------: |
| leetcode.cn    |    436.490    |    232.425    |    230.565    |    2810.72    |
| ocw.mit.edu    |    17.278     |    17.716     |    16.785     |   11911.14    |
| www3.nhk.or.jp |    12.460     |    12.352     |    13.336     |   12681.94    |
| github.com     |    17.210     |    16.619     |    16.623     |   12061.56    |
| nptel.ac.in    |    11.835     |    11.985     |    11.991     |   12730.57    |
| youtube.com    |    10.651     |     9.992     |    11.960     |   12114.16    |

geographical distances doesn't have much effect on RTT values, like they just provide a lower bound on RTT but it largely depends on routing paths, queueing delays etc.

*for packet sizes from 64-2048 bytes*

host: youtube.com , number of packets: 30

| packet size |  2pm   |  8pm   |  9am   |
| :---------- | :----: | :----: | :----: |
| 64 bytes    | 10.651 | 9.992  | 11.960 |
| 128 bytes   | 10.842 | 10.846 | 10.851 |
| 256 bytes   | 11.768 | 10.122 | 11.280 |
| 512 bytes   | 12.534 | 11.411 | 11.594 |
| 1024 bytes  | 12.395 | 10.815 | 14.011 |
| 2048 bytes  |   -    |   -    |   -    |
//**todo** ● The effect of packet size on RTT
● The influence of time-of-day on network latency


###### Q3. ifconfig and route commands

*1.ifconfig* - network interface management utility.

![[ifconfig_output.png | 400]]

`eth0` - name of the network interface
`UP` - interface is enabled
`BROADCAST` - interface supports broadcast packets
`RUNNING` - interface is operational (link is active)
`MULTICAST` - interface supports multicast packets
`mtu` - maximum transmission unit, largest packet that can be sent over this interface without fragmentation.
`inet` - ip address assigned to the interface.
`netmask` - subnet mask, determines which ip are on the same local subnet
`ether` - MAC address of the interface.
`txqueuelen` - maximum number of packets that can be queued before transmission.
`(Ethernet)` - link layer type/protocol of the interface.
`RX` - received, incoming traffic statistics
`TX` - transmitted, outgoing traffic statistics

*2.static ip address and subnet mask*

![[ifconfig_ip_change.png | 500]]
![[ifconfig_defaultgw.png|300]]

precautions to take -
- assign correct ip that is in subnet range.
- assign correct netmask,broadcast.
- assign correct default gateway ip.
- don't assign subnet ip or broadcast ip to the host.
- don't assign same ip as some other host on the subnet.

*3.enable/disable interface*

![[ifconfig_down.png|300]]
we can see that the interface `eth0` is disabled and only `lo` is in the output of ifconfig.

![[ifconfig_up.png|350]]
now `eth0` is back up.

*4.route traffic through specific interface*

we can use `route` to make a interface our default gateway and route our traffic via that network interface.

![[ifconfig_defaultgw.png|350]]
###### Q4. network statistics using netstat
*1. primary purpose of netstat*

primary purpose of netstat is to display various network related information like listening ports, routing tables etc.

*2. display listening tcp ports*

`netstat -tl` displays listening tcp ports.
these listening ports signify that a service is running and is reachable to connect to.

*3. difference between netstat -a and netstat -l*

`netstat -a` used to show all sockets
`netstat -l` used to show only listening sockets

netstat shows all the connections which may be in connected,establish,listening, non-listening state.

*4. monitor network statistics continuously*

`netstat -c` used for continuous update display

*5. udp connection statistics*

`netstat -u` shows only udp connections.
output shows connections with proto column set to udp and corresponding addresses and state.

*6. netstat help in troubleshooting*

netstat can help by displaying statistics for errors,dropped packets using `netstat -i` and `netstat -s` for retransmission, errors for each protocol.
we can use netstat to identify suspicious ip addresses and unknown ports.
###### Q5. traceroute
*1. role of ttl field*

source sends a series of IP datagrams carrying UDP segments with an unlikely UDP port number. the first of these datagrams has a TTL of 1, second of 2, and so on. when the $n^{th}$ datagram arrives at $n^{th}$ router, it observes that TTL of the datagram has expired and sends a ICMP TTL expired message to the source. this message includes the name and ip address of the router. it repeats the experiment 3 times.

*2. significance of round-trip time*

the RTT represents how much time did the packet took to reach a router + time took by router reply to reach the host. this information can help us to find out where delay or congestion occurs on the path. 

*3. ICMP vs UDP/TCP traceroute*

ICMP-based traceroute sends ICMP Echo requests while UDP/TCP-based traceroute sends UDP or TCP SYN packets. there paths may differ because firewalls or load balancers may treat these packets differently, eg. some firewalls block ICMP Echo requests causing some hops to be hidden.

*4. effects of load balancing*

load balancers may route traffic to different nodes and output of traceroute may differ for same destination. this can show varying RTT values due to different paths have varying latency and congestion.
###### Q6. Address Resolution Protocol
*1. purpose of ARP*

ARP is used for translation of IP addresses to MAC addresses. a host provides IP address to ARP and it returns mac address corresponding given IP address.resolves only for hosts and router interfaces on the same subnet.
ARP operates in between link-layer and network-layer but we can say that it operates in link-layer.

*2. ARP request when ip isn't on same subnet*

if this is the case then the ARP request doesn't get any ARP reply as all other hosts on the subnet rejects the request. the sender gets a timeout and gets destination unreachable error.

*3. duplicate ip detection*

a host can detect if the ip address it wants to use is already is in use by sending the arp request with its own ip and broadcast mac address. if the sender gets ARP reply back than that means some other host is already using this IP address, so this host should use a different address.

*4. ARP spoofing*

associating your mac address with another hosts ip address is known as ARP spoofing. eg if a hosts need MAC address of default gateway, some malicious host can send its MAC address as a reply pretending to be default gateway and sniff all traffic going through victim host before passing it to default gateway.

by using static ARP entries, a host fixes a ip with a mac address and prevents further updates to this entry.

disadvantages of static ARP entries, NIC of a host can change due to failure. need to manually set ARP entry on every host in the subnet.

###### Q7. LAN scanning using NMAP