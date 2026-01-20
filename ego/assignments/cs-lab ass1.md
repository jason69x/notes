
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

###### RTT and Packet Loss Analysis

*selected six hosts*

| Hosts          | avg.RTT (2pm) | avg.RTT (8pm) | avg.RTT (9am) |
| :------------- | :-----------: | :-----------: | :-----------: |
| leetcode.cn    |    436.490    |               |    230.565    |
| ocw.mit.edu    |    17.278     |               |    16.785     |
| www3.nhk.or.jp |    12.460     |               |    13.336     |
| github.com     |    17.210     |               |    16.623     |
| nptel.ac.in    |    11.835     |               |    11.991     |
| youtube.com    |    10.651     |               |    11.960     |

//**todo** Analyze whether RTT values show strong or weak dependence on geographical
distance.

*for packet sizes from 64-2048 bytes*

host: youtube.com , number of packets: 30

| packet size |  2pm   | 8pm |  9am   |
| :---------- | :----: | :-: | :----: |
| 64 bytes    | 10.651 |     | 11.960 |
| 128 bytes   | 10.842 |     | 10.851 |
| 256 bytes   | 11.768 |     | 11.280 |
| 512 bytes   | 12.534 |     | 11.594 |
| 1024 bytes  | 12.395 |     | 14.011 |
| 2048 bytes  |   -    |     |   -    |
//**todo** ● The effect of packet size on RTT
● The influence of time-of-day on network latency


###### ifconfig and route commands

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
