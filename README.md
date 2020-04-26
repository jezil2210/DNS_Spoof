# DNS_spoof

## Used Commands

these commands clean the Iptables, and redirect the packets entering and leaving my pc for a queue 0 that i'm creating, this way all the packets that came through my computer instead of just pass (because of my arp_spoof when i became the Man-in-the-Midle) i'm gonna store all the packets in a queue.  

iptables --flush
iptables -I INPUT -j NFQUEUE --queue-num  0
iptables -I OUTPUT -j NFQUEUE --queue-num  0

Internet sharing on Linux is done through Netfilter / Iptables, which is the native firewall for distributions starting with the 2.4 kernel. So is necessary to use this command, this command writes the number 1 into the ip_forward file, enabling packet routing. The default is 0. With this, Linux starts to route packages from one interface to the other and vice versa(My target and my host).

echo > 1 /proc/sys/ipv4/ip_forward


