192.168.193.1 - adres wyjściowy
ens18 - interfejs wejściowy


sudo iptables -t nat -A PREROUTING -i ens18 -p tcp --dport 80 -j DNAT --to-destination 192.168.193.1:80
sudo iptables -t nat -A PREROUTING -i ens18 -p tcp --dport 445 -j DNAT --to-destination 192.168.193.1:445

sudo iptables -t nat -A POSTROUTING -p tcp -d 192.168.193.1 --dport 80 -j MASQUERADE
sudo iptables -t nat -A POSTROUTING -p tcp -d 192.168.193.1 --dport 445 -j MASQUERADE

sudo iptables -A FORWARD -d 192.168.193.1 -p tcp --dport 80 -m state --state NEW,ESTABLISHED,RELATED -j ACCEPT
sudo iptables -A FORWARD -d 192.168.193.1 -p tcp --dport 445 -m state --state NEW,ESTABLISHED,RELATED -j ACCEPT

sysctl net.ipv4.ip_forward
sysctl -w net.ipv4.ip_forward=1

To make this change persistent across reboots, edit /etc/sysctl.conf:
net.ipv4.ip_forward = 1

sudo apt install iptables-persistent

sudo iptables -t nat -L -v -n
sudo iptables -L -v -n