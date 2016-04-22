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

## References
- [Classful network](https://en.wikipedia.org/wiki/Classful_network)
- [IPv4 - Address Classes](http://www.tutorialspoint.com/ipv4/ipv4_address_classes.htm)
- [IPv4 - Subnetting](http://www.tutorialspoint.com/ipv4/ipv4_subnetting.htm)
- [8 Steps to Understanding IP Subnetting](https://www.techopedia.com/6/28587/internet/8-steps-to-understanding-ip-subnetting)
