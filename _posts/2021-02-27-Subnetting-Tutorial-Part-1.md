---
layout: post
title: Subnetting Tutorial (Part 1)
---

Subnetting is a process that divides a network into at two or more separate networks. Subnetting is a FAQ in SRE or DevOps interviews or online test which makes it essential to learn. In this post I will explain some basic subnetting.

# Key Terms

- IP Address (IPv4): A unique 32-bit address for a host on a TCP/IP network or internetwork
- CIDR / Classless Inter-Domain Routing Notation: a format for subnetting format which specify the subnet mask used and represents the classless network design.
- Network bits (b): bits used for forming subnet's network bits, usually the bits filled by ones (1s) in the octet. Network bits cannot be changed
- Host bits (h): bits used for host addresses, filled by zeroes (0s). Host bits can be changed
- Subnet mask: a component which is required by TCP/IP to work
- Network address: the first assigned address
- Broadcast address: the last assigned address
- Number of Host: number of host = (2^h-2)
- Number of Subnet: number of subnet that can be formed = (2^b)

Usually in subnetting questions, we will be asked #hosts, network address, and broadcast address. Sometimes we will be requested to form a certain number of subnets.

# How to Answer Subnetting Questions

Example: 192.168.1.168/24 

Questions:
* How many subnets we can form?
* How host we can use per subnet?
* What is the network address?
* What is the broadcast address?

Step 1: Understand the question, clearly.

Step 2: Identify all the important variables which you will get after you *convert the IP into binary octet format* and find out the *subnet mask*. You then will identify the (b), (h), num of host, and num of subnet

```
IP address: 192.168.1.168 
Subnet mask: 255.255.255.0

Converting to binary octet 

IP address: 11000000.10101000.00000001.10101000
Subnet mask: 1111111.11111111.11111111.00000000
```

b = 16
h = 8

Step 3: Identify the answers

Num of hosts = 2^8-2 = 256-2 = 254
Num of subnet = 2^16 = 65,536

Now to find the start of subnet, you have to do AND operation to the IP address bits and the subnet mask bits corresponds to it. In this case, the host bits is 11000000.10101000.00000001.10101000 and the subnet mask bits is 11111111.11111111.11111111.00000000

```
11000000.10101000.00000001.10101000
11111111.11111111.11111111.00000000
===================================== AND
11000000.10101000.00000001.00000000

```

And converting it back again to IP we will get 192.168.1.0, and to detremine the broadcast address, you can add the network address with num of hosts + 1, so we get 192.168.1.0 + 254 + 1 = 192.168.1.255

Network address = 192.168.1.0
Broadcast address = 192.168.1.255

Step 4: Done!

```
Note: I haven't covered a question with subnet restriction and private / public subnet specification. But hopefully soon.
```

# Commonly Used Subnet Classes

## Class C Subnet

Class C subnet is a group of subnet that uses 192-223 significant bit (left-side bit) as their first octet of network component. Class C uses subnet mask of 255.255.255.0. E.g. 192.168.1.168/24 

## Class B Subnet

Class B subnet is a group of subnet that uses 128-191 significant bit (left-side bit) as their first octet of network component. Class B uses subnet mask of 255.255.0.0.  E.g. 172.168.1.25/16 

## Class A Subnet

Class A subnet is a group of subnet that uses 0-127 significant bit (left-side bit) as their first octet of network component. Class A uses subnet mask of 255.0.0.0.  E.g. 10.12.12.255/8

```
Bonus Question

Which class is 192.168.1.168/24?

Answer: It's class C subnet, because it has 192 as it first octet
```

And that's all of this post, hope it helps you as much as it helps me. If you have any question you can drop it on my [twitter](https://twitter.com/antoooks) or [LinkedIn](https://www.linkedin.com/in/kurnianto-trilaksono-86a4a6104/). 

Wish you the best of luck people of the internet and cheers.

<hr/>

28 February 2021