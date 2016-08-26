---
layout: post
title: Enable snmpd on rhel linux
date: 2016-08-26 11:24:00 +0800
author: Yuan Jiang
tags: snmp
---

How to enable snmpd on rhel linux for performance data monitoring via snmp.

Install snmpd
{% highlight bash %}
$ yum install net-snmp net-snmp-libs net-snmp-utils
{% endhighlight %}

Modify /etc/snmp/snmpd.conf
{% highlight text %}
## example sections

# rocommunity: a SNMPv1/SNMPv2c read-only access community name
#   arguments:  community [default|hostname|network/bits] [oid]

rocommunity  public  
rocommunity6 public
agentaddress udp:161
agentaddress udp6:161
com2sec readonly  default         public
com2sec6 readonly  default         public

...

# proc: Check for processes that should be running.
#     proc NAME [MAX=0] [MIN=0]
#   
#     NAME:  the name of the process to check for.  It must match
#            exactly (ie, http will not find httpd processes).
#     MAX:   the maximum number allowed to be running.  Defaults to 0.
#     MIN:   the minimum number to be running.  Defaults to 0.
#   
#   The results are reported in the prTable section of the UCD-SNMP-MIB tree
#   Special Case:  When the min and max numbers are both 0, it assumes
#   you want a max of infinity and a min of 1.

proc  syslog

...

# disk: Check for disk space usage of a partition.
#   The agent can check the amount of available disk space, and make
#   sure it is above a set limit.  
#   
#    disk PATH [MIN=100000]
#   
#    PATH:  mount path to the disk in question.
#    MIN:   Disks with space below this value will have the Mib's errorFlag set.
#           Can be a raw integer value (units of kB) or a percentage followed by the %
#           symbol.  Default value = 100000.
#   
#   The results are reported in the dskTable section of the UCD-SNMP-MIB tree

disk  / 100

...

# load: Check for unreasonable load average values.
#   Watch the load average levels on the machine.
#   
#    load [1MAX=12.0] [5MAX=12.0] [15MAX=12.0]
#   
#    1MAX:   If the 1 minute load average is above this limit at query
#            time, the errorFlag will be set.
#    5MAX:   Similar, but for 5 min average.
#    15MAX:  Similar, but for 15 min average.
#   
#   The results are reported in the laTable section of the UCD-SNMP-MIB tree

load  90 80 70

swap 1

...

{% endhighlight %}

Start/stop/restart snmpd
{% highlight bash %}
$ service snmpd start/status/stop/restart
or
$ /etc/init.d/snmpd start/stop/status/restart
{% endhighlight %}

Troubleshooting
{% highlight bash %}
$ service snmpd status
$ snmpd dead but subsys locked

# possible solutions:
- reinstall net-snmp
- remove /var/lock/subsys/snmpd
- check /var/log/messages
- check if snmp port(161) is available
{% endhighlight %}

See reference at: [Monitoring Performance with Net-SNMP](https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/6/html/Deployment_Guide/sect-System_Monitoring_Tools-Net-SNMP.html)
