# *Networking* :-

Note:- some commands are valid till system restart. for perme

### list and modify interfaces of the host
```
ip link		
```

## to check ip addresses assigned to those interfaces
```
ip addr		
```

### to set the ip address to host machine
```
ip addr add 192.168.1.10/24 dev ehth0		
```

### to check the entries in routing table.
```
ip route		
```

### to check entries in routing table
```
route			
```

### to add ip routes
```
ip route add 192.168.2.0/24 via 192.168.1.1
```

```
ip route add default via 192.168.2.1
```

### 0. systems-A,B,C. A connected to B at eth0. B connect to C at eth1. system B with two network cards eth0 and eth1. etho0 does not send packets to eth1 if the value is 0.
```
cat /proc/sys/net/ipv4/ip_forward
```


### enable ip forwarding on host.
```
echo 1 > /proc/sys/net/ipv4/ip_forward		
```

### 1 ### command to check ip forwarding is enabled on the host
```
cat /proc/sys/net/ipv4/ip_forward		
```


# *DNS* :-


### add entry for DNS. this is also known as Name Resolution.
```
cat >> /etc/hosts		
```

note: if one of the hosts ip changes, then we need to configure it in all files in different systesm

### dns server ip maintained at this location of the host.
```
cat /etc/resolve.conf		
```

---
record types.
| A     | for ipv4 |
|-------|-------   |
| AAAA  | for ipv6 |
|-------|-------   |
| CNAME | for name change records.|
|-------|-------   |

---

### it queries DNS server not local
```
nslookup		
```
### dig information
```
dig			

```

Network NS

### 
```
ip netns add red	
```
### cni plugins list
```
ls /opt/cni/bin  
```

### to check the networking solution used by cluster
```
cat /etc/cni/net.d/		
```

### to check teh logs of network plugins for ip address range.
```
kubectl -n kube-system logs weave-net-h2x9w -c weave	
```

```
kubectl logs kube-proxy-ft6n7 -n kube-system
```


### main file for coredns
```
cat /etc/coredns/Corefile		
```
