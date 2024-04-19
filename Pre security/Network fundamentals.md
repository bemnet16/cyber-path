### What is networking?
*Network* - It is just a devices connected to each other.
*Internet* - it a network of networks.
*Device Identification* 
- IP Address - it is a logical address to identify devices on a network.
- MAC Address - it is a physical network interface identification which can be spoofed to pass less secure network rules.
*Ping* - determine the performance of a connection, average time to receive sent packets.
### **Intro. to LAN**
***Topology*** 
- *Start topology* - devices are connected to each other by using a central networking device.
- *Bus topology* - there is one line to connect all the devices.
- *Ring topology* - devices are connected to each other to form a loop.
*Switch* - layer 2 networking device use ethernet cable to connect end points.  
*Router* - networking device connects networks and pass data between them.

***Subnetting*** 
It is splitting up a network in to smaller miniature networks.
It uses IP addresses in three different ways
- Identify the network address
- Identify the host address
- Identify the default gateway
It has several benefits. like 
- Efficiency
- Security
- Full control

***ARP(Address Resolution Protocol)*** 
It is a technology responsible for allowing a device to identify themselves on the network.
It is used to map MAC address to IP address and also the reverse.
There are two type of messages send related to ARP
- ***ARP Request*** - send broadcast message to know the MAC address of requested IP address. 
- ***ARP Replay*** - acknowledge the *ARP Request* with the required MAC address.

***DHCP protocol***
As the name (Dynamic Host Configuration Protocol) implies it assign IP address automatically for devices.
there are communications between the device and the DHCP server 
- *DHCP Discover* - explore if there is any DHCP server are there.
- *DHCP Offer* - the server return with the IP to the device.
- *DHCP Request* - device reply it will use the provided IP address.
- *DHCP ACK* - the server acknowledge the device can start using that IP.

### OSI Model
Open System Interconnection model is a framework dictating how all networking devices are send, receive and interpret data

it has **7*** layers
1. **Physical layer**
	- It is the lowest layer and the physical components of the hardware used in networking.
2. **Data-link layer
	- Focuses on the physical addressing of the transmission
1. **Network layer**
	- This the layer on which routing(determine the most optimal path in which chunks of data should be sent) and re-assembly of data takes place.
2. **Transport layer
	- There are two protocols in transport layer 
		- **TCP*** - accurate, reliable, connection-oriented, error-checking, slow
		- **UDP*** - connection-less, fast, non-synchronize the devices
3. **Session layer**
	- After the presentation layer takes place the two devices start establishing a connection/session. whilst the connection is active it is a ***session***.
	- It synchronizes the two computers to ensure that they are on the same page before data is send  and received.
4. **Presentation layer 
	 - It is the layer in which standardization starts to take place.
	 - It translates data to and from application layer.
5. **Application layer** 
	- This layer is more human friendly 
	- Primarily concerned with providing network services to the end-users.


### Packets and Frames
- These are small pieces of data and when we talk about *packet* we mean layer-3 and IP Address and by *Frame* it is related to layer-2 and MAC Address.

	**Packet headers**
	 - *Time to Live* - expiry time for the packet.
	 - *Checksum* - error/integrity checking.
	 - *Source Address* - senders IP address.
	 - *Destination Address* - receiver IP address.

***TCP/IP Model***
- It can be defined as a summarized form of OSI model
- It has 4 layers
	- Application
	- Transport
	- Internet
	- Network interface

	*TCP headers*
	- source/destination port
	- source/destination IP
	- sequence number
	- acknowledgement number
	- checksum
	- data
	- flag
	*UDP headers*
	- Time to live
	- source/destination address
	- source/destination port
	- data
	
### Extending you network
***Port forwarding***
It is a networking technique that allows data traffic to flow through a specific network port on a router or firewall, directing it to a particular device or service within a local network.
Port forwarding is configured at the router of a network.
***Firewall***
- A firewall is a device within a network responsible for determining what traffic is allowed to enter and exit.
- There are two primary categories of firewalls ==Statefull== and ==stateless==
***VPN***
- It is a technology that allows devices on separate networks to communicate securely.
