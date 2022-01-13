**For Back of Envelope calc:

If requirement is say 100B requests a day, how do you capacity plan for a web server.
In a day 86,400 sec , round it to 100,000 sec, which is 10**5
100B request is 100*(10**9)=10**11
it'll  be 10**11/10**5=10**6 which is 1 Million rps
Lets say you are using a VM, which got 1GHZ cpu cycle, which is 10*9 cycle
Earch request will take about 10000 cpu cycle
SO without any bottleneck it can handle 10**9/10*4=10*5 rps
Lets talk I/O: disk seek is 10ms, so calculate here, am tired now.
Lets talk about N/W: say 10 Gbps. which is lets say about (10/8) = 1 GBPS
Average page size is consider 100KB
in a sec it can handle 1GBPS/100KB= 10**9/10**5= 10,000 request
AS we see this is the bottleneck here so a node can handle 10k request and we need 1M/10k =100
We need 100 servers to handle this traffic.
SLowest componenet will decide your sizing as they'll choke.
