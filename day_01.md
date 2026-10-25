Day 1: Advanced Nmap Scanning & Enumeration
1. Advanced Port Scanning Techniques (Bypassing Firewalls)

When standard TCP SYN scans (-sS) are blocked by firewalls or packet filters, stealthier scan types can be used:

    Null Scan (-sN): Sets no flags at all (TCP header flag is 0).

        Response: Closed ports reply with RST. Open/filtered ports ignore the packet (no response).

    FIN Scan (-sF): Sets the FIN flag.

        Response: Closed ports reply with RST. Open/filtered ports give no response.

    Xmas Scan (-sX): Lights the packet up like a Christmas tree by setting the FIN, PSH, and URG flags.

        Response: Same as Null and FIN (closed = RST, open/filtered = no response).

    Maimon Scan (-sM): Sets FIN and ACK flags (-sM).

        Note: Originally designed for BSD systems where open ports drop the packet and closed ports return RST. On modern systems, it is mostly obsolete because most OSes reply with RST to both.

    ACK Scan (-sA): Sends an ACK flag (-sA) to map out firewall rules rather than finding open ports.

        Response: Unfiltered ports (whether open or closed) will reply with RST. Filtered ports will not respond or will return ICMP unreachable messages.

    Window Scan (-sW): Similar to an ACK scan, but inspects the TCP Window field of the returning RST packet to differentiate between open and closed ports on certain operating systems.

    Custom Scans (--scanflags): Allows you to manually configure any combination of TCP flags (e.g., SYN, RST, PSH, ACK, URG, FIN) in any order.

2. Evasion, Spoofing, and Network Tricks

Nmap provides built-in mechanisms to mask your scans and bypass security controls:

    Host Discovery: By default, Nmap pings hosts (using ARP, TCP, or ICMP) to verify they are live before scanning ports.

    IP Spoofing (-S <IP>): Fakes the source IP address in packet headers (requires network monitoring/sniffing capability to see replies).

    MAC Spoofing (--spoof-mac): Spoofs the MAC address of the scanning interface.

    Decoy Scans (-D <IP1,IP2,ME>): Makes the scan appear as though it is originating from multiple IP addresses alongside your own, hiding your real IP in the noise.

    Packet Fragmentation (-f): Splits the IP data into smaller 8-byte fragments (or use --mtu for custom sizes) to bypass basic packet inspection filters.

    Idle / Zombie Scan (-sI <zombie_ip> <target_ip>): A completely blind scan using a third-party idle host ("zombie"). It tracks incremental IP ID sequence numbers to deduce whether a target port is open or closed without sending packets directly from your IP.

3. Post-Port Scanning, OS Detection, and Scripts

Once ports are discovered, deeper enumeration can be performed:

    Service & Version Detection (-sV): Probes open ports to determine what software and version are running behind them.

    OS Detection (-O): Uses TCP/IP stack fingerprinting to guess the target's Operating System. (Note: Virtualization, load balancers, and strict firewalls can sometimes cause inaccurate OS results).

    Traceroute (--traceroute): Traces the network path and routers between your machine and the target.

    Verbosity & Debugging: Use -v or -vv for increased verbosity, and add --reason to see why Nmap classified a port a specific way.

    Nmap Scripting Engine (NSE) (-sC or --script):

        -sC runs the default set of safe scripts.

        --script <script_name> runs a specific script.

        Scripts are categorized (e.g., default, auth, vuln, discovery, safe) and a single script can belong to multiple categories.

4. Output Formats

Nmap lets you save your results in different formats depending on your needs:

    -oN: Normal text output.

    -oG: Grepable output (easy to parse with grep).

    -oX: XML output (great for importing into tools or reports).

    -oS: "Script kiddie" output (fun l33t-speak format).
