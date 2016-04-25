---
layout: post
title: IP address classes and subnetting
date: 2014-11-02 13:39:00 +0800
author: Yuan Jiang
tags: networking
---

IP address classes (Class A, B, C) and subnetting (CIDR).

## IP address classes
- Class A
  + First bit must be 0
  + 0NNNNNNN.HHHHHHHH.HHHHHHHH.HHHHHHHH
    - 'N': network address portion
    - 'H': host address portion
  + Starting from 1.x.x.x to 126.x.x.x
  + Default subnet mask: 255.0.0.0
  + 127.x.x.1 is reserved for loopback
  + Total networks: 2<sup>7</sup>-2
    - All-bits-zero of network portion means this network or section
    - All-bits-one of network portion means all networks
  + Total hosts: 2<sup>24</sup>-2
    - All-bits-zero of host portion means network address
    - All-bits-one of host portion means broadcast address

- Class B
  + First two bits must be 10
  + 10NNNNNN.NNNNNNNN.HHHHHHHH.HHHHHHHH
  + Starting from 128.0.x.x to 191.255.x.x
  + Default subnet mask: 255.255.0.0
  + Total networks: 2<sup>14</sup>
  + Total hosts: 2<sup>16</sup>-2
    - All-bits-zero of host portion means network address
    - All-bits-one of host portion means broadcast address

- Class C
  + First three bits must be 110
  + 110NNNNN.NNNNNNNN.NNNNNNNN.HHHHHHHH
  + Starting from 192.0.0.x to 223.255.255.x
  + Default subnet mask: 255.255.255.0
  + Total networks: 2<sup>21</sup>
  + Total hosts: 2<sup>8</sup>-2
    - All-bits-zero of host portion means network address
    - All-bits-one of host portion means broadcast address

## Subnetting
- CIDR: Classless Inter Domain Routing, provides the flexibility of borrowing bits of HOST part of the IP address and using them as NETWORK in NETWORK, called Subnet.
- CIDR ranges: /8 ~ /30
  + /8 ~ /15: can only be class A subnet address
  + /16 ~ /23: can only be class A or class B subnet address
  + /24 ~ / 30: can be class A, B, or C subnet address
{% highlight text %}
e.g. 192.168.1.1/24

1) The slash is how many subnet mask bits to use (how many bits are 1)

2) Same as 192.168.1.1 255.255.255.0

3) 255.255.255.0 is using 24 of the 32 bits to create the subnet

4) In binary it looks like this:
   11111111.11111111.11111111.00000000

5) A /30 would look like: 255.255.255.252 or in binary:
   11111111.11111111.11111111.11111100, the remaining 00 is for hosts and the
   1's are for the network
{% endhighlight %}

## Calculate subnet mask, network address and broadcast address
- Subnet mask
  + All-bits of the network and subnet are "1"s and all-bits of the host are "0"s.
  + Example:
{% highlight text %}
For the class B ip:	nnnnnnnn.nnnnnnnn.sssssshh.hhhhhhhh
The subnet mask is:	11111111.11111111.11111100.00000000
(where "n"s represent the network, "s"s the subnet and "h"s the host).

This subnet mask in dotted decimal notation is 255.255.252.0.
The "number of bits set to 1" notation is also commonly used.
In this example, there are 22 "1"s so we write "/22".
{% endhighlight %}
  + The subnet mask and class determines how many subnets and hosts you get.
  + The number of hosts is: 2<sup>number-of-host-bits</sup>-2
  + The number of subnets is: 2<sup>number-of-subnet-bits</sup>
  + Example:
{% highlight text %}
A class B ip subneted has follows: nnnnnnnn.nnnnnnnn.sssssshh.hhhhhhhh
(where "n"s represent the network, "s"s the subnet and "h"s the host)
Has got: 2^6=64 subnets and 2^10-2=1022 hosts
For each subnet:
The subnet address is nnnnnnnn.nnnnnnnn.ssssss00.00000000
And the broadcast address is nnnnnnnn.nnnnnnnn.ssssss11.11111111
{% endhighlight %}
- Subnet address
  + The subnet address is obtained doing a binary AND between the ip address and the subnet mask.
  + Example:
{% highlight text %}
For ip address: 150.10.10.10/22
The ip in binary is:	10010110.00001010.00001010.00001010	150.10.10.10
The mask is:	        11111111.11111111.11111100.00000000	255.255.252.0
Binary AND	        --------------------------------------	-------
Subnet address:	      10010110.00001010.00001000.00000000	150.10.8.0
{% endhighlight %}
- Broadcase address
  + The broadcast address is obtained doing a binary OR between the subnet sddress and the inverted subnet mask.
  + Example:
{% highlight text %}
For ip address: 150.10.10.10/22
Subnet address :	  10010110.00001010.00001000.00000000	150.10.8.0
The inverted mask:	00000000.00000000.00000011.11111111	0.0.3.255
Binary OR	        --------------------------------------	-----
Broadcast address:  10010110.00001010.00001011.11111111	150.10.11.255
{% endhighlight %}

## Examples
{% highlight text %}
Class A:
Address:   10.74.125.22         00001010.01001010.0111 1101.00010110
Netmask:   255.255.240.0 = 20   11111111.11111111.1111 0000.00000000
Wildcard:  0.0.15.255           00000000.00000000.0000 1111.11111111
=>
Network:   10.74.112.0/20       00001010.01001010.0111 0000.00000000
HostMin:   10.74.112.1          00001010.01001010.0111 0000.00000001
HostMax:   10.74.127.254        00001010.01001010.0111 1111.11111110
Broadcast: 10.74.127.255        00001010.01001010.0111 1111.11111111
Hosts/Net: 4094                  Class A, Private Internet

Class B:
Address:   128.128.10.10        10000000.10000000.000010 10.00001010
Netmask:   255.255.252.0 = 22   11111111.11111111.111111 00.00000000
Wildcard:  0.0.3.255            00000000.00000000.000000 11.11111111
=>
Network:   128.128.8.0/22       10000000.10000000.000010 00.00000000
HostMin:   128.128.8.1          10000000.10000000.000010 00.00000001
HostMax:   128.128.11.254       10000000.10000000.000010 11.11111110
Broadcast: 128.128.11.255       10000000.10000000.000010 11.11111111
Hosts/Net: 1022                  Class B

Class C:
Address:   192.168.0.100        11000000.10101000.00000000.0110 0100
Netmask:   255.255.255.240 = 28 11111111.11111111.11111111.1111 0000
Wildcard:  0.0.0.15             00000000.00000000.00000000.0000 1111
=>
Network:   192.168.0.96/28      11000000.10101000.00000000.0110 0000
HostMin:   192.168.0.97         11000000.10101000.00000000.0110 0001
HostMax:   192.168.0.110        11000000.10101000.00000000.0110 1110
Broadcast: 192.168.0.111        11000000.10101000.00000000.0110 1111
Hosts/Net: 14                    Class C, Private Internet
{% endhighlight %}

## References
- [Classful network](https://en.wikipedia.org/wiki/Classful_network)
- [IPv4 - Address Classes](http://www.tutorialspoint.com/ipv4/ipv4_address_classes.htm)
- [IPv4 - Subnetting](http://www.tutorialspoint.com/ipv4/ipv4_subnetting.htm)
- [8 Steps to Understanding IP Subnetting](https://www.techopedia.com/6/28587/internet/8-steps-to-understanding-ip-subnetting)
- [ShunIPCalc](http://www.shunsoft.net/ipcalc/help/index.html)
- [IP Calculator](http://jodies.de/ipcalc)
