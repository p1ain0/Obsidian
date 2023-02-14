firewall-cmd --list-all-zone
firewall-cmd --reload
firewall-cmd --permanent --zone=xxx --add(remove)-port(service)(interface)(......)=xxx
service firewalld restart

firewall-cmd --delete-zone=whaleszone --permanent
firewall-cmd --new-zone=whaleszone --permanent


#!/bin/sh 
sed -i '/net.ipv4.ip_forward/ s/\(.*=\).*/\11/' /etc/sysctl.conf
sysctl -p /etc/sysctl.conf
firewall-cmd --permanent --zone=public --set-target=ACCEPT
firewall-cmd --reload