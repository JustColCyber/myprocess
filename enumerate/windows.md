# Process to enumerate a Windows box

## RPC - TCP 135

RPC Endpoint Enumeration (rpcdump)
An anonymous attacker can query the Endpoint Mapper to list all registered RPC services on the host. This returns service names, network bindings, and UUIDs (Universally Unique Identifiers). It acts as a reconnaissance roadmap, helping attackers identify exactly what software or services are running on higher dynamic ports (e.g., Netlogon, Print Spooler, Task Scheduler)

### nmap

Nmap features a built-in script specifically designed to query and list Microsoft RPC endpoint maps. It provides a clean, scannable overview of registered interfaces.

nmap -p 135 --script=msrpc-enum <Target_IP>

### Impacket RPCDUMP

Connects to the target's Microsoft RPC Endpoint Mapper (typically over TCP Port 135) and queries it to dump all registered RPC interfaces, UUIDs, and service communication endpoints.

impacket-rpcdump <Target_IP>

### rpcclient

Allows you to open an interactive console or push single command queries directly to the MSRPC architecture. You can pass a null session (blank username and password) to test for unauthenticated read vulnerabilities.

rpcclient -U "" -N <Target_IP>

### Metasploit 

Dedicated auxiliary scanning module that targets the DCERPC endpoint mapper to extract the exact same map profile data.

use auxiliary/scanner/dcerpc/endpoint_mapper
set RHOSTS <Target_IP>
run
