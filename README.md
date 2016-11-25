# timed-url-access
Using Juniper Networks SRX to control what time of day my kids can access certain URLs

The idea is to use the SRX as a dns-proxy for your hosts and add dns cache entries that point to 127.0.0.1 some other non-working adress.
the dns-proxy cache works sort of like the local hosts file of a host but here this list of entries is consulted whenever a DNS query is sent thrugh the SRX.

You must enable netconf on the device using "set system services netconf ssh" and use keybased login.
Your DHCP server needs to offer the client the SRX LAN IP adress as their DNS.
I am using a SRX220h with Junos version 12.1X46-D30.2

run: 
"ansible-playbook pb_enable_url_block.yml" to have the entries from /group_vars/all.yml be copied to the device.
"ansible-playbook pb_disable_url_block.yml" to disable. 

I have a section in the /group_vars/all.yml for real dns-cache entries that you want to keep always. 
