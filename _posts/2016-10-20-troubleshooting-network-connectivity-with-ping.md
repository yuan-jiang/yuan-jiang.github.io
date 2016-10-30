---
layout: post
title: Troubleshooting network connectivity with ping
date: 2016-10-20 20:51:00 +0800
author: Yuan Jiang
tags: networking
---

Ping is the most commonly-used tool for simply troubleshooting network connectivity.

# Basic usage:
{% highlight bash %}
$ ping 1.1.1.1 # ping an ip address
$ ping xxx.com # ping a hostname
{% endhighlight %}

# Common errors:
- Unknown Host
{% highlight text %}
This error message indicates that the requested host name cannot be resolved to its IP address; check that the name is entered correctly and that the DNS servers can resolve it.
{% endhighlight %}

- Request Timed Out
{% highlight text %}
This message indicates that no Echo Reply messages were received within the default time of 1 second. This can be due to many different causes; the most common include network congestion, failure of the ARP request, packet filtering, routing error, or a silent discard. Most often, it means that a route back to the sending host has failed. This might be because the destination host does not know the route back to the sending host, or one of the intermediary routers does not know the route back, or even that the destination host's default gateway does not know the route back. Check the routing table of the destination host to see whether it has a route to the sending host before checking tables at the routers.
{% endhighlight %}

- Destination Host Unreachable
{% highlight text %}
This message indicates one of two problems: either the local system has no route to the desired destination, or a remote router reports that it has no route to the destination. The two problems can be distinguished by the form of the message. If the message is simply "Destination Host Unreachable," then there is no route from the local system, and the packets to be sent were never put on the wire. Use the Route utility to check the local routing table.
If the message is "Reply From < IP address >: Destination Host Unreachable," then the routing problem occurred at a remote router, whose address is indicated by the "< IP address >" field. Use the appropriate utility or facility to check the IP routing table of the router assigned the IP address of < IP address >.
If you pinged using an IP address, retry it with a host name to ensure that the IP address you tried is correct.
{% endhighlight %}

- TTL Expired in Transit
{% highlight text %}
The number of hops required to reach the destination exceeds the TTL set by the sending host to forward the packets. The default TTL value for ICMP Echo Requests sent by Ping is 32. In some cases, this is not enough to travel the required number of links to a destination. You can increase the TTL using the -i switch, up to a maximum of 255 links.
{% endhighlight %}

# Refereneces
- [Test Network Connection with Ping and PathPing](https://technet.microsoft.com/en-us/library/cc940095.aspx)
- [ping response “Request timed out.” vs “Destination Host unreachable”](http://stackoverflow.com/questions/22110622/ping-response-request-timed-out-vs-destination-host-unreachable)
