firewall-cmd --list-all-zone
firewall-cmd --reload
firewall-cmd --permanent --zone=xxx --add(remove)-port(service)(interface)(......)=xxx
service firewalld restart

firewall-cmd --delete-zone=whaleszone --permanent
firewall-cmd --new-zone=whaleszone --permanent
