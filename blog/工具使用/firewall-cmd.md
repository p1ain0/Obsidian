firewall-cmd --list-all-zone
firewall-cmd --reload
firewall-cmd --permanent --zone=xxx --add(remove)-port(service)(interface)(......)=xxx
service firewalld restart
