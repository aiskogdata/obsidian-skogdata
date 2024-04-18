# Netstat in Bash
`netstat -na | grep "8080"'

lists all active uses of port 8080
-a shows all sockets
-in shows all interfaces
-d shows all packets incoming for each layer and subsystem
-n displays ports in numerical form
-na takes all ports and uses numerical form
-s shows per protocol statistics
-p shows all connections of a given protocol. For example `netstat -p tcp`. Accepts at least tcp, http, https, udp, ip, icmp, and v4 and v6 version of  ip, udp and tcp.
-o displays the names of associated processes

Generally lists in order 
protocol, internal ip, external ip, status, process id
