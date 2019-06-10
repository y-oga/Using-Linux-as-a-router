# Using a Linux machine as a OSPF(Open Shortest Path First) router

## Overview

This guide provides how to setup a Linux machine as a OSPF network router using Quagga.

```
<LAN>                          <LAN>
|          +---------+          |
|          | Cent OS |          |
|          | Quagga  |          |
+----------+         +----------+
|          |         |          |
|          +---------+          |
|                               |
|                               |
|                               |
192.168.142.0/24           192.168.145.0/24
```

## Product Versions on the verification

- CentOS 7.4 
- Quagga version 0.99.22.4


## System setup

All commands and settings are to be executed by a user with a root previlage.


### Enable packet forwarding setting

- Add the below line in `/etc/sysctl.conf`

	```
	net.ipv4.ip_forward = 1
	```

- Enable the setting

	```
	# sysctl -p
	```


### Install Quagga

```
# yum -y install quagga
```

### Setup config files, `zebra.conf` and `ospfd.conf`

- Create or edit the config files under `/etc/quagga`

	Add the below lines.
	
	```
	hostname <hostname>
	password <password>
	log stdout
	```
	
- Change owner and owner group of the config files

	```
	# chown quagga:quagga zebra.conf
	# chown quagga:quagga ospfd.conf
	```


### Start zebra and ospfd and change those start-types

```
# systemctl start zebra
# systemctl enable zebra
# systemctl start ospfd
# systemctl enable ospfd
```


### Setup OSPF

- Connect ospfd

	```
	# telnet localhost 2604
	```
- Setup ospf routing

	```
	> en
	# conf t
	(config)# router ospf
	(config-router)# network 192.168.142.0/24 area 0
	(config-router)# network 192.168.145.0/24 area 0
	(config-router)# end
	# write memory
	```


### Check OSPD routing table

```
# telnet localhost ospf
> show ip ospf route
```

### In progress...