> Go for it now. The future is promised to no one.
>
> - <cite>Wayne Dyer</cite>✍️

# CNT4007 Review !!!!
## Chapters 3 and 4
### Homework
1. What are the two key functions of a router at the network layer?
	1. The router builds a "forwarding table", so that it knows where to send packets when it receives them. Also, it handles packet forwarding, which utilizes the aforementioned forwarding table in order to determine each packet's output.
2. Why is packet queueing needed at the output ports of the router?
	1. If the case happens where more packets are received at the input of the router than the output can handle sending (input rate > max output rate), queueing is needed in order to buffer the excess packets.
3. If there is a 4000 byte packet (20 bytes header and 3980 data bytes), that is passing through a link with MTU (max transfer unit, this is just the total amt. of bytes that can pass through at a time) of 1500 bytes, how many fragments will the packet be split into?
	1. 3 Fragments. First two carry 1480 bytes of data, third carries 1020. 1480+1480+1020=3980 bytes + 20 (header bytes) = 4000 bytes
```mermaid
classDiagram
Packet-->Frag1
Packet-->Frag2
Packet-->Frag3
Packet : 4000 bytes total
Packet : 20 byte header
Packet : 3 Fragments
Packet : 1500 byte MTU
Frag1 : Length = 1500 (1480 + 20)
Frag1 : header = 20 bytes
Frag1 : data = 1480 (1500+20)
Frag1 : ID = x
Frag1 : fragflag = 1 (is there another frag coming)
Frag1 : offset = 0 (prev. offset + prev. frag data amt. / 8)
Frag2 : Length = 1500 (1480 + 20)
Frag2 : header = 20 bytes
Frag2 : data = 1480 (1500+20)
Frag2 : ID = x
Frag2 : fragflag = 1 (is there another frag coming) 
Frag2 : offset = 185 (0 + 1480 / 8)
Frag3 : Length = 1040 (1020 + 20)
Frag3 : header = 20 bytes
Frag3 : data = 1480 (1500+20)
Frag3 : ID = x
Frag3 : fragflag = 0 (is there another frag coming)
Frag3 : offset = 370 (0 + 1480 / 8)
```
4. If you have a network with one publicly routable address, with 10 devices connected to it, can an external host initiate communication to these devices? 
	1. No. In order to allow this to happen, the NAT entries for the devices must manually be inserted into the NAT table of the router.
5. If you have a datagram network with 32-bit host addresses, and a router with 4 links (0 through 3) and packets are to be forwarded as below:
	1. Provide forwarding table that has five entries, uses longest prefix matching, and forwards packets to the correct link interfaces. 
	2. Describe how your forwarding table determines the appropriate link interface for datagrams with destination addresses:
		1. 11001000 10010001 01010001 01010101 
		2. 11100001 01000000 11000011 00111100 
		3. 11100001 10000000 00010001 01110111

![[Pasted image 20220919102414.png]]
a)
Prefix Match | Link Interface
-------------|---------------
11100000 00 | 0
11100000 01000000 | 1
1110000 | 2
11100001 1 | 3
otherwise | 3

b)
1. This matches prefix 3 (otherwise) as it doesn't match any of the prefixes in the table
2. This matches prefix 2 (1110000) as it is the longest prefix matched from the table
3. This matches prefix 3 (11100001 1) as it is the longest prefix matched from the table

Notes:
- For the first (and longest) prefix you make, choose the range that is longest before the rest of the range becomes ones
	- After this, choose the next longest before the range becomes ones and select everything up to the last one
	- Repeat until the otherwise case
- For otherwise case, find the longest string of like bits and flip the last zero to a one
- Include otherwise as well

6. There is a datagram network with 8-bit (00000000 through 11111111) host addresses. The router uses longest-prefix matching with the below table. For each prefix match, give the range of destination addresses and the number of addresses in the range.

Prefix Match | Interface
-|-
1 | 0
10 | 1
111 | 2
otherwise | 3

Destination Range | Interface | Address Count
-|-|-
110*00000* -> 110*11111* | 0 | 32 (5 "new" bits = 2^5 bits)
10*000000* -> 10*111111* | 1 | 64 (6 "new" bits = 2^6 bits)
111*00000* -> 111*11111* | 2 | 32 (5 "new" bits = 2^5 bits)
0*0000000* -> 1*1111111* | 3 | 128 (7 "new" bits = 2^7 bits)

7. Consider a subnet with prefix 128.119.40.128/26. Give an example of one IP address (of form xxx.xxx.xxx.xxx) that can be assigned to this network. Suppose an ISP owns the block of addresses of the form 128.119.40.64/26. Suppose it wants to create four subnets from this block, with each block having the same number of IP addresses. What are the prefixes (of form a.b.c.d/x) for the four subnets?
	1. Any IP in the range 128.119.40.128 to 128.119.40.191 (64 possible addresses)
		1. The 64 addresses is due to the 26 in the subnet bit number - there are 32 bits (8 + 8 + 8 + 8) total, but 26 of them are for the subnet, so 32-26 = 6. 2^6 = 64, so you can have addresses from the first, up to 128 + 64 in the subnet. 
	2. The subnets would be 128.119.40.64/28, 128.119.40.80/28, 128.119.40.96/28, 128.119.40.112/28
		1. This is same as above - 128.119.40.64/26 gives an upper limit on addresses to be 128.119.40.128, so each block gets 16 addresses, so the subnet bits becomes 28 (32-28=4, 2^4=16)

### Lecture Notes
- Router Switching
	- This describes transferring the packet from input to the correct output port
	- *Switching Rate* - rate at which packets can be transferred from input to output, measured as a multiple of input/output line rate
	- 3 different methods: memory, bus, and crossbar
	- Memory Switching
		- First generation routers used this, they were computers with switching that was under direct control of the CPU
		- Involved copying the packet into system memory and then copying back to the output port
		- Speed was limited by memory bandwidth, because there were 2 bus crossings per packet
	- Bus Switching
		- Packet sent from input port memory to output port memory via shared bus
		- Switching speed is limited by bus bandwidth
	- Crossbar Switching
		- Overcame the bandwidth limitations in place with bus switching
- DHCP - Dynamic Host Configuration Protocol
	- Allows host devices to be given an IP address when they are connected to the network, and only when they are connected - this allows reuse of IP addresses, and support for devices that connect more temporarily (mobile devices)
	- Can deliver address of first-hop router for client, name and IP of DNS server, and network mask (allows delineation between host and network addresses)
- TCP - Transmission Control Protocol
- UDP - User Datagram Protocol
- NAT - Network Address Translation
	- Since a local network sends all data from the same address, NAT is needed to translate incoming and outgoing traffic addresses so they get where they need to be
Host | Destination
-|-
10.0.0.1, 3345 | 128.119.40.186
Translated to 138.76.29.7, 5001 | 128.119.40.186
Reply | Original Host
128.119.40.186 | 138.76.29.7, 5001
128.119.40.186 | Translated back to 10.0.0.1, 3345

NAT Traversal Problem
- If a host wants to connect to a local address, it can't - since these are not accessible from outside of the network
- 3 Solutions:
	- 1. Forward requests made to specific ports on the server to a specific local address
	- 2. UPnP (Universal Plug n Play) automates port map configuration
	- 3. Host and router both connect to a relay outside of the network which bridges packets between destinations
- IPv6
	- Changed header size to 40 bytes, with no fragmentation allowed
	- Removed checksum entirely
	- IPv6 datagrams are carried as payloads in an IPv4 datagram to allow backwards compatibility
		- IPv6 router would connect to x amount of IPv4 routers, which would transmit this "wrapped" IPv6 packet in IPv4, then once it reaches the destination IPv6 router the wrapper would be removed

## Chapter 5
### Homework
1. Describe how a link state routing protocol works. Name a link state routing protocol. Give the pseudo code of the Dijkstra’s algorithm. Based on the network topology below, produce a shortest path tree from the Dijkstra’s algorithm with u being the source node. Construct a forwarding table from the shortest path tree.
```mermaid
graph LR;
	id1([u])-- 2 ---id2([v]);
	id1-- 1 ---id3([x]);
	id2-- 2 ---id3;
	id2-- 3 ---id4([w]);
	id4-- 3 ---id3;
	id1-- 5 ---id4;
	id4-- 5 ---id5([z]);
	id4-- 1 ---id6([y]);
	id6-- 2 ---id5;
	id3-- 7 ---id6;
```
a) Link State Routing Protocol
Each router sends its link state, along with the cost of adjacent links, to all other routers. Once it receives link states from the other routes, it uses Dijkstra's algorithm to find the shortest path to any other node, and constructs a forwarding table from this.

b) Dijkstra's algorithm
```
Initialization
Visited = ({u,0})
node u = source
for v in nodes:
	if v adjacent to u:
		Dist(v) = cost(u, v)
		prev(v) = u
	else 
		Dist(v) = inf
		prev(v) = null

Loop
find the next node x not in Visited with shortest Dist
add x to N
update Dist(x) for all adjacent to x and not in N:
	Dist(v) = min(Dist(v), Dist(x)+cost(x, v))
	if Dist(x) + cost(x, v) < Dist(v)
		prev(v) = x
	# new cost to v is either old cost to v or known shortest path cost to x plus cost from x to v
loop until every node is in Visited
```
c) Shortest Path
```mermaid
graph LR
	id1([u])-- 2 ---id2([v]);
	id1-- 1 ---id3([x]);
	id4([w])-- 3 ---id3;
	id4-- 1 ---id6([y]);
	id6-- 2 ---id5([z]);
```
d) Forwarding Table
Destination | output Link
-|-
v | (u,v)
x | (u,v)
w | (u,x)
y | (u,x)
z | (u,x)

2. Describe how a distance vector routing protocol works. Name a link state routing protocol. Give the pseudo code of the Bellman-Ford algorithm. Suppose a router x has two neighbors, u and v. Suppose cost(x, u) = 1 and cost(x, v) = 5. Update the distance vector at x based on the distance vectors of u and v below.
a) Distance Vector Routing Protocol
A router transmits the cost to each destination to its neighbors. Each router saves the most recent cost to its neighbors. This changes when it receives a different cost from before, or a link goes down.

b) Link State Routing Protocol
One example is OSPF (Open Shortest Path First)

c) Bellman-Ford Code
```
dist(source,dest) = min{cost(source,neighbor 1) + dist(neighbor 1, dest), cost(source,neighbor 2) + dist(neighbor 2, dest), repeat for # neighbors}
```
d) Distance Vector
At u, 
Destination | distance 
-|-
x | 1 
y | 4 
z | 9 
u | 0 
v | 6 
w | 10 
At v, 
Destination | distance 
-|-
x | 5 
y | 14
z | 2
u | 6
v | 0
w | 5
At x,
Destination | distance
-|-
x | 0
y | 5
z | 7
u | 1
v | 5
w | 10

3. How does router 1a learns the existence of X? How does router 2d build its forwarding table that includes both internal address prefixes within its AS and external address prefixes in other ASes?
![[Pasted image 20220919133842.png]]
a) 
3a first learns of X via intra-AS routing protocol. This is advertised to router 2c via eBGP. This is disseminated to 2a via iBGP, and then advertised to 1c via eBGP again. Finally, iBGP is used to get this information from 1c to 1a.

AS : Autonomous system, or "domain" - this is an aggregate region of routers
intra-AS routing : tells one router where all others are in an AS
iBGP : intra Border Gateway Protocol : sends router location from one router to another in an AS
eBGP : extra Border Gateway Protocol : sends router location from one router to another between ASes

b) 
Router 2d builds its local forwarding tables, with entries for internal address prefixes being learned by an intra-AS routing protocol and entries for external address prefixes being learned by BGP.

4. Name one advantage of software-defined networking vs traditional router design.
Easier network management, flexibility in traffic engineering, cheaper or better-performing switches than routers in packet forwarding.

### Lecture Notes
- Traditional Routing Algorithms
	- Goal of routing protocol is to determine "good" / "best" paths for packets to take
	- Global information
		- all routers have complete topology stored
		- "link state" algos
		- Dijkstra's algorithm
			- From start node, find path of least cost to adjacent node
			- Add this node to list of visited nodes
			- Loop, checking all available adjacent nodes to visited nodes, and add the adjacent node that hasn't been visited yet with lowest cost until every node has been visited
			- O(n^2) time complexity, but can be O(nlogn + e)
	- Decentralized information
		- router knows physically-connected neighbors, and link costs to their neighbors
		- "distance vector" algos
<<<<<<< HEAD
=======
		- Bellman-ford algorithm
			- Dynamic Programming solution
			- Find path of least distance to each node on the graph
			- dist(start, goal) = minimum { cost(start, adjacent) + dist(adjacent, goal), etc for each adjacent node }
			- dist function call runs recursively to return shortest distance
			- O(n^2) per node
>>>>>>> origin/main
- Software Defined Networking
	- Two network-layer functions: forwarding (data plane), and routing (control plane)
	- There are two approaches to structuring the control plane:
		- Per-router control (traditional)
			- Algorithms in each and every router interact with each other in order to compute forwarding tables
		- Logically centralized control (Software Defined Networking, or SDN)
			- A distinct (usually remote) controller interacts with local control agents (CAs) in routers in order to compute forwarding tables
- ICMP (Internet Control Message Protocol)
	- 
- Network Management
- Hierarchical OSPF
	- two-levels : local area, and backbone
		- Local Area
			- Summarize distances to nets in own area, advertise to other area border routers
		- Backbone
			- run OSPF routing limited to backbone
		- Boundary
			- Connect to other ASes
- BGP
	- prefix + attributes = "route"
		- i.e. BGP Advertisement: AS3, X

## Chapter 6
### Homework
1. Suppose the information content of a packet is the bit pattern 1110 0110 1001 1101 and an even parity scheme is being used. What would the value of the field containing the parity bits be for the case of a two-dimensional parity scheme? Your answer should be such that a minimum-length checksum field is used.

Right Column and Bottom Row are for parity bits

1 1 1 0 1
0 1 1 0 0
1 0 0 1 0
1 1 0 1 1
1 1 0 0 0

2. Consider the 7-bit generator, G=10011, and suppose that D has the value 1010101010. What is the value of R?

First, use G to find the amount of zeroes to add to D. Bit one has a 1, so therefore its degree is 4, so we add 4 zeroes to D.

Then, we divide D  + 0000 / G to find R.

10011 / 1010101010*0000* = 101101110 R 0100

![[Pasted image 20220919195921.png]]

3. Suppose four active nodes—nodes A, B, C and D—are competing for access to a channel using slotted ALOHA. Assume each node has an infinite number of packets to send. Each node attempts to transmit in each slot with probability p. The first slot is numbered slot 1, the second slot is numbered slot 2, and so on. 

a. What is the probability that node A succeeds for the first time in slot 5? 
We need to calculate p(A succeeds and no others do) = p(A)
p(A) = p(A)p(not B)p(not C)p(not D) = p(1-p)(1-p)(1-p) = 3p * (1-p)
Now, we need to use this for the 5th slot.
p(A succeeds for the first time in slot 5) = 4 * p(A fails) * p(A succeeds) = 4 * (1 - p(A)) * p(A) = 4 * (1 - (3p * (1-p))) * 3p(1-p)

b. What is the probability that some node (either A, B, C or D) succeeds in slot 4? 
p(one node succeeds) = 3p * (1-p)
p(any one of the nodes succeeds) = 4 * 3p * (1-p)

c. What is the probability that the first success occurs in slot 3? 
p(no node succeeds) = 1 - 12p * (1-p)
p(no success in first two slots but success in third) = p(fail in first two) * p(success in third) = 2 * (1 - 12p * (1-p)) * (12p * (1-p))

d. What is the efficiency of this four-node system?
efficiency = p(success) = 12p * (1-p)

4. Recall that with the CSMA/CD protocol, the adapter waits K * 512 bit times after a collision, where K is drawn randomly. For K= 100, how long does the adapter wait until returning to Step 2 for a 10 Mbps broadcast channel? For a 100 Mbps broadcast channel?

K * 512 / 10,000,000 = 5.12 milliseconds
K * 512 / 100,000,000 = 512 microseconds

5. Let’s consider the operation of a learning switch in the context of a network in which 6 nodes labeled A through F are star connected into an Ethernet switch. Suppose that (i) B sends a frame to E, (ii) E replies with a frame to B, (iii) A sends a frame to B, (iv) B replies with a frame to A. The switch table is initially empty. Show the state of the switch table before and after each of these events. For each of these events, identify the link(s) on which the transmitted frame will be forwarded, and briefly justify your answers.

Action | Switch Table State | Link(s) Packet is Forwarded To | Explanation
-|-|-|-
B -> E | Switch learns interface for MAC of B | A,C,D,E,F | Table is empty, so it needs to send to all connections to find the interface corresponding with the MAC of E
E -> B | Switch learns interface for MAC of E | B | Switch already knows interface corresponding with MAC of B
A -> B | Switch learns interface for MAC of A | B | Switch already knows interface corresponding with MAC of B
B -> A | Switch state remains the same | A | Switch already knows interface corresponding with MAC of A

![[Pasted image 20220919204434.png]]