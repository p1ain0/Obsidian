虚拟机配置网卡
vboxmanage hostonlyif create
vboxmanage hostonlyif ipconfig vboxnet0 --ip xx.xx.xx.xx --netmask 255.255.255.0

vboxmanage dhcpserver add --netname HostInterfaceNetworking-vboxnet0 --ip 192.168.166.254 --netmask 255.255.255.0 --lowerip 192.168.166.2 --upperip 192.168.166.250 -- enable
vboxmanage dhcpserver modify --netname HostInterfaceNetworking-vboxnet0 --ip 192.168.166.254 --netmask 255.255.255.0 --lowerip 192.168.166.2 --upperip 192.168.166.250 -- enable