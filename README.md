# ACL-Part5

## ACL Cases Example

### ACL - VTY Access / Telnet access

ACL to allow only 10.10.1.10 for doing telnet, beside that block.
when denying access create syslog with "log".
```
R1(config)# ip access-list standard VTY-ACCESS
R1(config-std-nacl)# permit host 10.10.1.10 
R1(config-std-nacl)# deny any log
R1(config-std-nacl)# exit
R1(config)# line vty 0 4
R1(config-line)# access-class VTY-ACCESS in
```

### ACL -  SSH Access
```
R1(config)# ip access-list extended TRAFFIC-FILTER
R1(config-ext-nacl)# 65 permit tcp host 203.0.113.40 host 10.10.2.20 eq 22
```

### Allowing any public devices HTTP and HTTPS access to SERVER(10.10.2.20)
```
R1(config)# ip access-list extended TRAFFIC-FILTER
R1(config-ext-nacl)# permit tcp any host 10.10.2.20 eq www
R1(config-ext-nacl)# permit tcp any host 10.10.2.20 eq 443
R1(config)# interface Ethernet 0/3
R1(config-if)# ip access-group TRAFFIC-FILTER in
```

### Allowing ping replies to the 10.10.1.0/24 network from any public devices
```
R1(config)# ip access-list extended TRAFFIC-FILTER
R1(config-ext-nacl)# permit icmp any 10.10.1.0 0.0.0.255 echo-reply
R1(config)# interface Ethernet 0/3
R1(config-if)# ip access-group TRAFFIC-FILTER in
```

### Allowing DNS replies to the 10.10.1.0/24 network from SERVER(203.0.113.30)
```
R1(config)# ip access-list extended TRAFFIC-FILTER
R1(config-ext-nacl)# permit udp host 203.0.113.30 eq domain 10.10.1.0 0.0.0.255
R1(config)# interface Ethernet 0/3
R1(config-if)# ip access-group TRAFFIC-FILTER in
```

### Allowing HTTP and HTTPS replies to the 10.10.1.0/24 network from any public devices
```
R1(config)# ip access-list extended TRAFFIC-FILTER
R1(config-ext-nacl)# permit tcp any eq www 10.10.1.0 0.0.0.255 established
R1(config-ext-nacl)# permit tcp any eq 443 10.10.1.0 0.0.0.255 established
R1(config)# interface Ethernet 0/3
R1(config-if)# ip access-group TRAFFIC-FILTER in
```

### VERIVY
```
R1# sh access-lists TRAFFIC-FILTER
```
