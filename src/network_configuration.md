# Network configuration

```bash
#
# Host name
#
bat /etc/myname

#
# DNS config
#
/etc/resolv.conf

#
# List routing table
#
netstat -rn
route show

#
# Interface config file: /etc/hostname.[INTERFACE_NAME_HERE]
#
# Or you can put your static ip settings like this:
# ifconfig em0 192.168.1.x 255.255.255.0
#
bat /etc/hostname.em0

# Active change for network settings:
doas chmod +x /etc/netstart
doas sh /etc/netstart [INTERFACE_NAME_HERE]
```

</br>
