---
title: Multi Access Networks
layout: post
---

Till now we have only discussed the protocols used in a P2P (Point-to-Point) network. A P2P network can only have 2 computers participating in the network, while a multi-access network, as suggested by it's name, can have multiple computers accessing the network.

In multi-access networks the transmission channels are not dedicated i.e. any computer in the network can use the channel to transmit data to other computers in the network. This causes data overlapping and lose of data due to collisions between signal sent by different computers. Certain protocols are used in multi-access networks to handle data transmission on non dedicated channels

The most common example of a multi-access network is Ethernet. The more general name of technology used in Ethernet is _Carrier Sense, Multiple Access with Collision Detect_ (CSMA/CD). The "carrier sense" in CSMA/CD means that all the nodes can distinguish between idle and busy link, and "collision detect" means that a node listens as it transmits and can detect when a frame it is transmitting has collided with a frame transmitted by another node. The fundamental problem faced by the Ethernet is how to mediate access to a shared medium fairly and efficiently. The core idea in Ethernet is an algorithm that controls when each node can transmit.

Modern Ethernet links are now largely P2P; that is, they connect one host to an Ethernet switch, or they interconnect switches, hence the "multi access" algorithm is not used much in today's Ethernet, but a variant is now used in [wireless networks](wireless_networks), such as 802.11 networks (also known as Wi-Fi).



