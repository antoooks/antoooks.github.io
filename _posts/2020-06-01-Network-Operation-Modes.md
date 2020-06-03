---
layout: post
title: Network Operation Modes
---

In this post I will talk about two network operation modes which every Network Interface Card (NIC) possesses. Why? because I found it in one of an online interview question which I attempted.

Okay, so because every NIC are capable of operation modes, that means any device that establish networking can assume these modes may it be router, switch, LAN, WAN, or personal computer. There are 2 modes that can be applied to the network which are **promiscuous mode** and **non-promiscuous** mode.

For your info, NIC hardware located on the Layer 1 of OSI layer but it also handles traffic on the Layer 2.

We start with the first one,

## 1. Promiscuous mode

Promiscuous word literally means **indiscriminate** or **casual**. When the NIC assumes promiscuous mode, the network will intercepts traffic that come through the device indiscriminatively, this mode specifically work on Layer 2 of the device which is Data Link Layer.

In **promiscuous** mode, the NIC will capture:
* Packets destined to your network interface
* Broadcasts
* Multicasts
* Packets destined to another layer 2 network interface

Basically, if a switch or router has promiscuous mode turned on, it will reveal the **MAC addresses**, **IP addresses** + **port**, and **arrival time** of source and destination of every network packets passing through the device. Even if the destination is not the listening device itself. 

With this data, you can derive which networks the device are connected, you can even map the network if you want.

## 2. Non-promiscuous mode

When the NIC assumes non-promiscuous mode, the network will only listens to packets that directed to that specific NIC.

In **non-promiscuous** mode, the NIC will capture:
* Packets destined to your network interface
* Broadcasts
* Multicasts

## Discussion

These modes increase and decrease visibility of network traffic, promiscuous mode will introduce noises to the data captured for traffic monitoring. So, it depends on what kind of traffic that you want to observe. However, it has a certain condition where the device cannot capture the packet: *it's when a packet was dropped by the network interface before the capture library can be aware of them*

It has nothing to do with security as it's only function like verbosity flag for the network traffic data.

Hope that helps! Good luck with your interviews!

## Reference

- Hosmer, C. Python Forensics. 2014.
- Adds pretty good details on this topic. https://medium.com/@debookee/promiscuous-vs-monitoring-mode-d603601f5fa

<hr/>

01 June 2020