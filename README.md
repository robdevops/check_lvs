# Table of Contents
1. [check_lvs_pool](#check_lvs_pool)
2. [check_lvs_virtualips](#check_lvs_virtualips)
3. [check_ipv4_failover](#check_ipv4_failover)


# check_lvs_pool
check_lvs_pool plugin for Nagios / Icinga. Checks the number of available real servers for a specified IPVS virtual server. 

## Dependencies
* bash 4.2.46 or newer
* ipvsadm
* coreutils
* grep

## Installation
1. Copy the script to `/usr/local/sbin/check_lvs_pool`
1. Use `chown` and `chmod` to ensure the test user can execute it.
1. Give the test user `sudo` access:
```
visudo 
```
```
icinga ALL=(root) NOPASSWD: /usr/local/sbin/check_lvs_pool
``` 

## Usage
```
check_lvs_pool -v <vip:port> [-t|-u] [-w <number>] [-c <number>]
-v:     IPv4 address and port of virtual server (Required).
-t:     Specify TCP protocol (This is the default).
-u:     Specify UDP protocol.
-w:     Warning threshold for number of real servers (Defaults to 1).
-c:     Critical threshold for number of real servers (Defaults to 0).
-h:     This help.

Usage example: check_lvs_pool -v 192.168.8.8:80 -t -w1 -c0
```

### Example output
```
sudo /usr/local/sbin/check_lvs_pool -v 192.168.8.8:80 -t -w1 -c0
OK: virtual_server "192.168.8.8:80" has 2 real_server available: 192.168.10.10:80 192.168.10.20:80

echo $?
0
```
```
sudo /usr/local/sbin/check_lvs_pool -v 192.168.8.8:80 -t -w2 -c0
WARNING: virtual_server "192.168.8.8:80" has 2 real_server available: 192.168.10.10:80 192.168.10.20:80

echo $?
1
```
```
sudo /usr/local/sbin/check_lvs_pool -v 192.168.8.8:80 -t -w3 -c2
CRITICAL: virtual_server "192.168.8.8:80" has 2 real_server available: 192.168.10.10:80 192.168.10.20:80

echo $?
2
```

# check_lvs_virtualips
check_lvs_virtualips plugin for Nagios / Icinga. Warns if the host is missing any virtual_ip known to LVS. If the host does not own the specified address (`-g`), the test is inverted i.e. we warn if the host *does* own any virtual_ip known to LVS.

## Dependencies
* bash 4.2.46 or newer
* ipvsadm
* iproute
* coreutils
* grep
* awk

## Installation
1. Copy the script to `/usr/local/sbin/check_lvs_virtualips`
1. Use `chown` and `chmod` to ensure the test user can execute it.
1. Give the test user `sudo` access:
```
visudo 
```
```
icinga ALL=(root) NOPASSWD: /usr/local/sbin/check_lvs_virtualips
``` 

## Usage
```
check_lvs_virtualips -g <gateway ip>
-g:	Gateway IP address (required).
-h:	This help.
```

### Example output
```
sudo /usr/local/sbin/check_lvs_virtualips -g 192.168.0.1
OK: I own the gateway and 3 of 3 virtual_ip are active: 192.168.10.10 192.168.10.20 192.168.10.30

echo $?
0
```
```
sudo /usr/local/sbin/check_lvs_virtualips -g 192.168.0.1
OK: I don't own the gateway and 0 virtual_ip are active.

echo $?
0
```
```
sudo /usr/local/sbin/check_lvs_virtualips -g 192.168.0.1
WARN: I own the gateway and 2 of 3 virtual_ip are active: 192.168.10.10 192.168.10.20

echo $?
1
```
```
sudo /usr/local/sbin/check_lvs_virtualips -g 192.168.0.1
WARN: I don't own the gateway and 1 of 3 virtual_ip are active: 192.168.10.10

echo $?
1
```

# check_ipv4_failover
check_ipv4_failover plugin for Nagios / Icinga. Warns if the host does not own the specified ipv4 address. If `-s` is specified, we treat this host as a standby, and warn if the host *does* own the address.

## Dependencies
* bash
* iproute
* coreutils
* grep
* awk

## Installation
1. Copy the script to /usr/local/bin/check_ipv4_failover
1. Use `chown` and `chmod` to ensure the test user can execute it.


## Usage
```
-a:	IPv4 address (required).
-s:	This host is a standby.
-h:	This help.
```

### Example output
```
/usr/local/bin/check_ipv4_failover  -a 192.168.0.1
OK: I am the primary for 192.168.10.1.


echo $?
0
```
```
/usr/local/bin/check_ipv4_failover -a 192.168.0.1 -s
OK: I am the standby for 192.168.0.1.


echo $?
0
```
```
/usr/local/bin/check_ipv4_failover -a 192.168.0.1
WARN: 192.168.0.1 is in failover. Ensure the secondary has taken over.

echo $?
1
```
```
/usr/local/bin/check_ipv4_failover -a 192.168.0.1 -s
WARN: I have taken over 192.168.158.141. Ensure the primary has released it.

echo $?
1
```
