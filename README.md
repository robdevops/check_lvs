# check_lvs_pool
check_lvs_pool plugin for Nagios / Icinga. Checks the number of available real servers for a specified IPVS virtual server. Intended or use with LVS managers like ipvsadm and Keepalived.

## Dependencies
* ipvsadm
* coreutils
* bash
* grep

## Installation
1. Copy the script to /usr/local/sbin/check_lvs_pool
1. Use chown and chmod to ensure the test user can execute it.
1. Give the test user sudo access:
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

Usage example: check_lvs_pool -v 192.168.0.1:80 -t -w1 -c0
```

### Example output
```
sudo /usr/local/sbin/check_lvs_pool -v 192.168.0.1:80 -t -w1 -c0
OK: 2 up.

echo $?
0
```
```
sudo /usr/local/sbin/check_lvs_pool -v 192.168.0.1:80 -t -w2 -c0
WARNING: 2 up.

echo $?
1
```
```
sudo /usr/local/sbin/check_lvs_pool -v 192.168.0.1:80 -t -w1 -c2
CRITICAL: 2 up.

echo $?
2
```
