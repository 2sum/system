https://join.slack.com/t/fb-pe/shared_invite/zt-epbxukma-IW9VZoU~PTXQkHigrq~5Yw
https://github.com/alex/what-happens-when#arp-process

- HSTS: Browser Checks it's preloaded HSTS(HTTP Strict Transport Security) list. THis is list of websites request to be called via HTTPS
If it's in list browser send it's list via HTTPS else via HTTP

https://www.cloudflare.com/learning/dns/what-is-dns/
- DNS: Browser check if domain is in cache, if not found browser calls gethostbyname library function to do lookup.
  This checks if hostname can be resolved via local host file (/etc/hosts).
  If it's not in cache and not in hosts file , Browser contacts DNS configured in the n/w stack. This is typically local router or  ISP caching DNS server.
 If DNS server is on the same N/W 	library follows ARP process below for the DNS Server
 If DNS server is not on the same N/W, 	library follows ARP process below for the default gateway server

 ARP Process:
 In order to send an ARP broadcast, Network stack library needs target IP address to look up.
 It also needs to know MAC adddress of interface it will use to send out ARP broadcast.
ARP cache is first checked to find ARP entry of target IP. If it's in cache library function returns result. Target IP is MAC.
If the entry is not in ARP cache,
	* Route table looked up to check if target IP is in any of subnets on local route table. If it is, then n/w library uses the interface associated with that subnet, if not then, it uses interface that has subnet of our default gateway
	* MAC address of selected n/w interface is looked up
	* N/W library sends a layer2(data link) ARP request

	ARP Request:
	Sender MAC: interface:mac:address:here
	Sender IP: interface.ip.goes.here
	Target MAC: FF:FF:FF:FF:FF:FF (Broadcast)
	Target IP: target.ip.goes.here


If The computer is directly connected to router, router responds with ARP reply
If computer is connected to Hub, hub will broadcast ARP request out all other ports. If router is connected on same wire, it'll respond with ARP reply
If the computer is connected to switch, switch will check its local CAM/MAC table to see which port has the MAC address we are looking for. If switch has no entry for MAC address, it'll
rebroadcast ARP request to all other ports.
	If switch has entry in MAC/CAM table, it'll send ARP request to that port that has MAC address we are looking for.
	If router is on the same wire, it'll respind with an ARP reply

ARP Reply:

Sender MAC: target:mac:address:here
Sender IP: target.ip.goes.here
Target MAC: interface:mac:address:here
Target IP: interface.ip.goes.here

Now N/w library has IP address of DNS server or default gateway it can resume its DNS process.
	* DNS client establish a socket to UDP port 53 on DNS server using a source port above 1023
	* If response size is too large, tcp port will be used
	* If local/ISP DNS server does not have it then a  recursive search is requested as below to find IP address.
		DNS RESOLVER -> ROOT Server -> TLD Server -> Authoritaive server




 Opening of Socket:
 	Once browser recieves IP address of destination server, it takes that and given port number of URL and makes call to system library function named socket and request a TCP socket stream.
 		* This request is first passed to transport layer where a TCP segment is crafted. Destination port is added to header and a source port is choosen (>1023) from within kernel's dynamic port range (ip_local_port_range in linux)
 		* This segment is sent to Network Layer, which wraps an additional IP header. IP of destination and current machine is inserted to form a packet.
 		* This packet next arrives to Link Layer. A frame header is added that includes MAC address of the m/c NIC as well as MAC adress of the gateway (Local router). If kernel does not know MAC address of gateway, it must broadcast an ARP query to find it as explained before in DNS.
 	Now, packet is ready to be transmitted through wireless/ethernet/celular N/W. Then it pass from computer through local N/W, and then through Modem (Which converts 1/0 to analog). ON other end there will be another modem which again translates analog to digital.

 	Eventually packet will reach Router managing local subnet. From there it'll travel to Autonomous System's border router, other AS'es and then finally destination server. Each router along the way, extracts destination IP header and routes it to appropriate next hop. TTL in IP header is decreamented by one for each router it passes. Packet will be dropped if the TTL field reaches 0 or if the current router has no space in its queue (may be due to n/w congetions).

 	This send and recieve happens multiple times following TCP connection flow:
 		* Client chooses Initial Sequence Number(ISN) and sends packet to server with the SYN bit set to indicate its setting ISN
 		* Server recieves SYN and if it in agreeable mood:
 			- Server chooses its own Initial Sequence Number(ISN)
 			- Server sets SYN to indicate it is choosing its ISN
 			- Server increaments client ISN by 1 and copies that to its ACK field and adds ACK flag to indicate its acknoweldging recipt of first packet
 		* Client acknowledges connection by sending a packet
 			- Increases its own Sequence Number
 			- Increases reciever acknowledge number
 			- Sets ACK field
 		* Data is transferred as follow
 			- As one side sends N bytes of data, it increases its sequence by that number
 			- When other side acknoweldegs recipt of that packet/packets, it sends an ACK packet with the ACK value equal to the last received sequence from the other
 		* To close connection
 			- Closer sends FIN request
 			- Other sides ACKs FIN packet and sends its own FIN
 			- Closer acknoweldges other side's FIN with an ACK


--------------------------------------


 ARP: INside LAN , machines need MAC address to communicate and ARP provides MAC address.
 computer A looks at its internal list (ARP cacche) to get MAC address of Computer B to communicate.
 If its not in cache, A sends a broadcast message asking MAC address, now computer B sends message saying it has MAC address.




 DHCP: DHCP server automatically assigns a computer IP address, SUbnet mask, default gateway, DNS server
 Requesting computer sends a broadcast IP address request on its network. DHCP server assign an IP address from its pool and assign it to requesting computer.
 DHCP server pulls IP addtress from a scope which has both start and end IP address.
 DHCP server assigns IP address on a lease, lease is amount of time IP is assigned to computer. It's to make sure DHCP server does not run out of IP address and scope.
 computers asking DHCP server to renew its lease. IF a computer is out of N/W, it can not ask to renew lease and that IP return back to IP pool.
 If you want a computer to always have sam address, then add that IP address and MAC address to Address reservation.
