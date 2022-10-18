# Container Private Network

The container can be configured to have a independent public IP address (fully bridged), a private internal IP (internal NAT) or both. All options have Pros/Cons and need to be considered for final deployment.&#x20;

A common setup is to have the container entirely private using nginx as a proxy or iptables to port forward to the container.&#x20;

Block diagram below shows Private only Network Topology

![Private LXC Networking](<../.gitbook/assets/image (130).png>)

With external access using NGINX proxy such as below.

![FMADIO NGINX Proxy to Private Container](<../.gitbook/assets/image (124) (1).png>)

Or use iptables to port forward packets thru the NAT, such as below

![FMADIO  Private Container Port Forwarding](<../.gitbook/assets/image (120).png>)

## IPTables Configuration

To enable the NATed bridge between man0 (public) network and the fmad0 (private)network the following IPTables config needs to be set

```
echo 1 >/proc/sys/net/ipv4/ip_forward
sudo iptables -A FORWARD -i fmad0 -o man0 -j ACCEPT
sudo iptables -A FORWARD -i man0 -o fmad0 -m state --state ESTABLISHED,RELATED -j ACCEPT
sudo iptables -t nat -A POSTROUTING -o man0 -j MASQUERADE
sudo iptables-save > /opt/fmadio/etc/iptables.conf
```

Alternatively the following /opt/fmadio/etc/iptables.conf file can be used (requires a system reboot or iptables-restore) to take effect

```
# Generated by iptables-save v1.6.1 on Tue Apr 19 16:29:55 2022
*nat
:PREROUTING ACCEPT [10:2058]
:INPUT ACCEPT [2:104]
:OUTPUT ACCEPT [2:260]
:POSTROUTING ACCEPT [2:260]
-A POSTROUTING -o man0 -j MASQUERADE
COMMIT
# Completed on Tue Apr 19 16:29:55 2022
# Generated by iptables-save v1.6.1 on Tue Apr 19 16:29:55 2022
*filter
:INPUT ACCEPT [22:3579]
:FORWARD ACCEPT [0:0]
:OUTPUT ACCEPT [23:3592]
-A FORWARD -i fmad0 -o man0 -j ACCEPT
-A FORWARD -i man0 -o fmad0 -m state --state RELATED,ESTABLISHED -j ACCEPT
COMMIT
# Completed on Tue Apr 19 16:29:55 2022

```

## Container Network Settings

Container network settings need to be the following

```
Gateway : 192.168.255.2
DNS     : 8.8.8.8   (or something else)
```

## Container Network IP Address

List of private container addresses&#x20;

| IP Address      | Container          | Description                     |
| --------------- | ------------------ | ------------------------------- |
| 192.168.255.2   | FMADIO Host        | FMADIO Host IP Address          |
| 192.168.255.10  | FShark             | FMADIO Internal Wireshark Lite  |
| 192.168.255.100 | Ubuntu Desktop     | Ubuntu Desktop                  |
| 192.168.255.110 | Elastic Search 7.x | Elastic Search 7.x Container    |
| 192.168.255.111 | Elastic Search 8.x | Elastic Search 8.x Container    |
| 192.168.255.120 | Suricata 6.x       | Suricata 6.x Container (CentOS) |
| 192.168.255.130 | Zeek               | Zeek Container (CentOS)         |