---
layout: post
title: OSI model and DoD model
date: 2014-11-02 10:42:00 +0800
author: Yuan Jiang
tags: networking
---

OSI model and DoD (TCP/IP) model in computer networking.

## OSI model
- Layer 7: Application Layer
  + Protocol data unit: Data
  + Closest to end-users, high-level APIs (resource sharing, remote file access, directory services, virtual terminals)
  + Examples: dns, ftp, http, nfs, snmp, smtp, telnet, and etc.
- Layer 6: Presentation Layer
  + Protocol data unit: Data
  + Translation of data between a networking service and an application, char encoding, data compression, and encryption/decryption
  + Examples: mime, ssl, tls, and etc.
- Layer 5: Session Layer
  + Protocol data unit: Data
  + Managing communication sessions
  + Examples: sockets (session establishment)
- Layer 4: Transport Layer
  + Protocol data unit: Segment(TCP)/Datagram(UDP)
  + Reliable transmission of data segments between points on a network (segmentation, acknowledgement, multiplexing)
  + Examples: tcp, udp, and etc.
- Layer 3: Network Layer
  + Protocol data unit: Packet
  + Addressing, routing and traffic control
  + Examples: ip, icmp, ospf, rip, and etc.
- Layer 2: Data Link Layer
  + Protocol data unit: Frame
  + Reliable transmission of data frames between two nodes connected by a physical layer
  + Examples: ppp, atm, mac, ieee 802.2 and etc.
- Layer 1: Physical Layer
  + Protocol data unit: Bit
  + Transmission and receiption of raw bit streams over a physical medium
  + Examples: ethernet physical layer, isdn, and etc.

## TCP/IP (DoD) model
- Layer 4: Application Layer
  + dns, dhcp, ftp, http, ssh, telnet, smtp, and etc.
- Layer 3: Transport Layer
  + tcp, udp, and etc.
- Layer 2: Internet Layer
  + ip, icmp, ipsec, and etc.
- Layer 1: Link Layer
  + arp, mac, ppp, and etc.

## References
- [OSI model](https://en.wikipedia.org/wiki/OSI_model)
- [Internet protocol suite](https://en.wikipedia.org/wiki/Internet_protocol_suite)
