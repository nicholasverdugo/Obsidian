> Go for it now. The future is promised to no one.
>
> - <cite>Wayne Dyer</cite>✍️

# CNT4007 Review !!!!
1. What are the two key functions of a router at the network layer?
	1. The router builds a "forwarding table", so that it knows where to send packets when it receives them. Also, it handles packet forwarding, which utilizes the aforementioned forwarding table in order to determine each packet's output.
2. Why is packet queueing needed at the output ports of the router?
	1. If the case happens where more packets are received at the input of the router than the output can handle sending (input rate > max output rate), queueing is needed in order to buffer the excess packets.
3. If there is a 4000 byte packet (20 bytes header and 3980 data bytes), that is passing through a link with MTU (max transfer unit, this is just the total amt. of bytes that can pass through at a time) of 1500 bytes, how many fragments will the packet be split into?
	1. 3 Fragments. First two carry 1480 bytes of data, third carries 1020. 1480+1480+1020=3980 bytes + 20 (header bytes) = 4000 bytes
```mermaid
classDiagram
Packet1-->Packet2
Packet1 : Length = 1500 (1480 + 20)
Packet1 : header = 20
Packet1 : data = 1480 (1500+20)
Packet1 : ID = x
Packet1 : fragflag = 1 (is there another frag coming)
Packet1 : offset = 0 (prev. offset + prev. frag data amt. / 8)
Packet2-->Packet3
Packet2 : Length = 1500 (1480 + 20)
Packet2 : header = 20
Packet2 : data = 1480 (1500+20)
Packet2 : ID = x
Packet2 : fragflag = 1 (is there another frag coming) 
Packet2 : offset = 185 (0 + 1480 / 8)
Packet3 : Length = 1040 (1020 + 20)
Packet3 : header = 20
Packet3 : data = 1480 (1500+20)
Packet3 : ID = x
Packet3 : fragflag = 0 (is there another frag coming)
Packet3 : offset = 370 (0 + 1480 / 8)
```