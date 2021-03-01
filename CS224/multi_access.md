---
title: Multi Access Networks
layout: post
---

Till now we have only discussed the protocols used in a P2P (Point-to-Point) network. A P2P network can only have 2 computers participating in the network, while a multi-access network, as suggested by it's name, can have multiple computers accessing the network.

In multi-access networks the transmission channels are not dedicated i.e. any computer in the network can use the channel to transmit data to other computers in the network. This causes data overlapping and lose of data due to collisions between signal sent by different computers. Certain protocols are used in multi-access networks to handle data transmission on non dedicated channels

The most common example of a multi-access network is Ethernet. The more general name of technology used in Ethernet is _Carrier Sense, Multiple Access with Collision Detect_ (CSMA/CD). The "carrier sense" in CSMA/CD means that all the nodes can distinguish between idle and busy link, and "collision detect" means that a node listens as it transmits and can detect when a frame it is transmitting has collided with a frame transmitted by another node. The fundamental problem faced by the Ethernet is how to mediate access to a shared medium fairly and efficiently. The core idea in Ethernet is an algorithm that controls when each node can transmit.

Modern Ethernet links are now largely P2P; that is, they connect one host to an Ethernet switch, or they interconnect switches, hence the "multi access" algorithm is not used much in today's Ethernet, but a variant is now used in [wireless networks](wireless_networks), such as 802.11 networks (also known as Wi-Fi).

## Access Protocol

We'll now discuss the algorithm that controls access to the shared Ethernet link. This algorithm is commonly called the Ethernet's *media access control* (MAC). It is implemented in the hardware on the network adapter.

First let's describe the Ethernet's frame format and addresses.

#### Frame Format and Ethernet Addresses

Each Ethernet frame is of the following format.

<div style="text-align: center;">
  <img src="./img/ethernet_frame.png" alt="ethernet frame" width="50%">
</div>

The preamble helps the receiver to synchronize with the signal. Both the source and destination host are identified with 48-bit address. Finding the correct higher level protocol to process the received packet is called demultiplexing, the packet type field serves as the demultiplexing key. Each frame contains up to 1500 bytes of data. Minimally, a frame must contain at least 46 bytes of data. The reason for this minimum size is that the frame must be long enough to detect a collision. Finally each frame also have a 32-bit CRC. The sending adapter attaches the preamble and CRC before transmitting, and the receiving adapter removes them.

Every Ethernet adaptor in the world has a unique Ethernet address. Ethernet addresses are printed as six numbers separated by colons. Each number is given by a pair of hexadecimal digits and corresponds to 1 byte of the 6 byte address

### Transmitter Algorithm

The receiver side of the Ethernet is simple, the main algorithm implemented at the sender's side. The transmitter algorithm is defined as follows.

If the line is idle, the adaptor transmits the frame without negotiating other nodes. The upper bound of 1500 bytes in the message means that the adaptor can occupy the line for only a fixed length of time.

If the line is busy, the adapter waits for the line to go idle and then it waits for $$9.6 \mu s$$ (so that the receiver can process the previous frame) and then transmits the frame.



