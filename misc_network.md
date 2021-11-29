# traceroute: 
  - This is a tool to understand path packet takes to reach destination without actually sending your data
  - This will also tell n/w latency between each hop
  - Uses UDP protocol, UDP packet has source IP, Destination IP, a destination UDP port, which is invalid
  - THis also sends a TTL, which gets decremented atfter each hop in the router, it also gets TTL Exceeds message from each router
  - In the destination it gets "ICMP Destination Port unreachable"
  - It sends 3 UDP packets , because it average time of 3 packets
