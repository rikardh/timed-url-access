# timed-url-access
I'm using Juniper Networks SRX to control what time of day my kids can access youtube. 

The idea is to use the SRX as a dns-proxy for your hosts and add dns cache entries that point to 127.0.0.1 or some other non-working adress. (Its not fool proof of course. Manually change the DNS in your device to bypass.)
The dns-proxy cache works sort of like the local hosts file of a host but here this list of entries is consulted whenever a DNS query is sent through the SRX.

You must enable netconf on the device using "set system services netconf ssh" and use keybased login.
Your DHCP server needs to offer your clients the SRX LAN IP adress as their DNS.
I am using a SRX220h with Junos version 12.1X46-D30.2
Ansible is needed including the junos.roles from Galaxy. Ansible 2.1 and later I beieve has the Junos.roles in core so they should be baked in already.

run: 
"ansible-playbook pb_enable_url_block.yml" to have the entries from /group_vars/all.yml be copied to the device.
"ansible-playbook pb_disable_url_block.yml" to disable. 

I have a section in the /group_vars/all.yml for real dns-cache entries that you want to keep always. 


_______________________________________________

For timing I'm using crontab using a non-root user.
Here is my crontab (edited using crontab -e)

The below line in crontab will enable filters Monday, Tuesday, Wednesday, Thursday, Friday at 5.00
00 5 * * 1,2,3,4,5 cd /<your directory path>/timed-url-access;/usr/local/bin/ansible-playbook pb_enable_url_block.yml

The below line will disable filters in Tuesday, Wednesday, Friday at 16.00 (Monday and Thursdays are no-youtube-days)
0 16 * * 2,3,5 cd /<your directory path>/timed-url-access;/usr/local/bin/ansible-playbook pb_disable_url_block.yml


