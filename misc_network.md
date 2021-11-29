# traceroute: 
  - This is a tool to understand path packet takes to reach destination without actually sending your data
  - This will also tell n/w latency between each hop
  - Uses UDP protocol, UDP packet has source IP, Destination IP, a destination UDP port, which is invalid
  - THis also sends a TTL, which gets decremented atfter each hop in the router, it also gets TTL Exceeds message from each router
  - In the destination it gets "ICMP Destination Port unreachable"
  - It sends 3 UDP packets , because it average time of 3 packets

# How SSH works:
  ## Verification of server by client: 
      Client verifies servers public key which is in known_hosts. 
  ## Session Key generation to encrypt all communication:
      A session key is generated using Diffie-Hellman algorithm. This key is symmetric as tghis will be used for both encryption and decryption
  ## Authentication of Client:
      - Client sends an ID for the key pair to server
      - Server checks authorized_keys for the ID
      - If a public key matching ID is found, server generates random numberand uses public key to encrypt the number and sends client this encrypted message
      - If client has correct private key, it decrypt the number
      - Client combines this random number along with shared session key and calculate MD5 hash
      - Client sends MD5 hash back to server
      - Server uses same session key and original random number to calculate MD5 hash. If clent and server MD5 hash value matches, client is authenticated
