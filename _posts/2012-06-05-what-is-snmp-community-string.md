---
layout: post
title: What is snmp community string
date: 2012-06-05 10:15:00 +0800
author: Yuan Jiang
tags: snmp
---

What is snmp community string?

## Definitions
{% highlight text %}
The “SNMP Community string” is like a user id or password that allows access
to a router's or other device's statistics. Community string is sent along with
all SNMP requests. If the community string is correct, the device responds with
the requested information. If the community string is incorrect, the device
simply discards the request and does not respond.

Note: SNMP Community strings are used only by devices which support SNMPv1 and
SNMPv2c protocol. SNMPv3 uses username/password authentication, along with an
encryption key.

By convention, most SNMPv1-v2c equipment ships from the factory with read-only
community string set to "public". It is standard practice for network managers
to change all the community strings to customized values in the device setup.
{% endhighlight %}

## Example usage
{% highlight bash %}
# get device sys uptime
snmpwalk -v 2c -c public x.x.x.x sysUptime

# get device sys uptime by oid
snmpwalk -v 2c -c public x.x.x.x 1.3.6.1.2.1.1.3

# set device cpu 5 minu peak util oid value to 5
snmpset -v 2c -c public x.x.x.x 1.3.6.1.4.1.9.9.109.1.1.1.1.8 i 5
{% endhighlight %}

## References
- [About SNMP Read-only Community Strings](http://www.helpsystems.com/intermapper/snmp-community-strings)
- [TUT:snmpwalk](http://net-snmp.sourceforge.net/wiki/index.php/TUT:snmpwalk)
- [TUT:snmpset](http://net-snmp.sourceforge.net/wiki/index.php/TUT:snmpset)
