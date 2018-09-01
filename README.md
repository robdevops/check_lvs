# check_ipvsadm
check_ipvsadm plugin for Nagios / Icinga. Checks how many real servers are available for the specified virtual server. 

## Description   
check_ipvsadm checks the number of available real servers for a specified IPVS virtual server.

## Dependencies
* ipvsadm
* coreutils
* bash
* grep

## Usage
```check_ipvsadm -v <vip:port> [-t|-u] [-w <number>] [-c <number>]
-v:     IPv4 address and port of virtual server (Required).
-t:     Specify TCP protocol (This is the default).
-u:     Specify UDP protocol.
-w:     Warning threshold for number of real servers (Defaults to 1).
-c:     Critical threshold for number of real servers (Defaults to 0).
-h:     This help.

Usage example: check_ipvsadm -v 192.168.0.1:80 -t -w1 -c0
