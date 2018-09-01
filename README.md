# check_ipvsadm
check_ipvsadm plugin for Nagios / Icinga. Checks the number of available real servers for a specified IPVS virtual server.

## Dependencies
* ipvsadm
* coreutils
* bash
* grep

## Usage
```
check_ipvsadm -v <vip:port> [-t|-u] [-w <number>] [-c <number>]
-v:     IPv4 address and port of virtual server (Required).
-t:     Specify TCP protocol (This is the default).
-u:     Specify UDP protocol.
-w:     Warning threshold for number of real servers (Defaults to 1).
-c:     Critical threshold for number of real servers (Defaults to 0).
-h:     This help.

Usage example: check_ipvsadm -v 192.168.0.1:80 -t -w1 -c0
```

### Example output
```
sudo /usr/local/sbin/check_ipvsadm -v 172.16.253.10:3306 -t -w1 -c0
OK: 2 up.

echo $?
0
```
```
sudo /usr/local/sbin/check_ipvsadm -v 172.16.253.10:3306 -t -w2 -c0
WARNING: 2 up.

echo $?
1

```
```
sudo /usr/local/sbin/check_ipvsadm -v 172.16.253.10:3306 -t -w1 -c2
CRITICAL: 2 up.

echo $?
2
```
