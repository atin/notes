---
title: CS 224 - Computer Networks
layout: post
---

## Framing

In the line_coding lecture we saw how to transmit a sequence of bits over a point-to-point link, or from adaptor to adaptor. From previous chapters we know that blocks of data (called *frames* at this level), not bit streams, are exchanged between nodes. The network adaptor at the nodes handles the exchange of frames. Recognizing exactly what set of bits constitute a frame, or determining start and end of a frame, is the central challenge faced by the adaptor. There are many ways to address this framing problem. Here three different protocols are described to address this problem. Here we are discussing framing in context of point-to-point link but this is a fundamental problem that must also be addressed in multiple access networks like Ethernet and Wi-Fi.

### Some Protocols
#### Byte-Oriented Protocols (PPP)
It is one of the oldest approach to framing. In this protocol each frame is viewed as a collection of bytes (characters) instead of bits. At high level, there are two approaches to this type of protocols.

The first is to use special characters known as *sentinel characters* to indicate start and end of a frame. The data portion of the frame is sometimes contained between two or more special characters STX (start of text) and ETX (end of text). BISYNC used this approach. The problem with this is that one of the sentinel characters might appear in the data portion of the frame. The standard way of solving this problem is by "escaping" the character by preceding it with a DLE (data-link-escape) character Whenever it appears in the body of a frame. This approach is often called character stuffing because extra characters are inserted in the data portion of the frame. The **Point-to-Point Protocol** (PPP), which is commonly used to carry Internet Protocol packets over various sorts of point-to-point links, uses this method.

<div style="text-align: center;">
  <img src="./img/PPP.png" alt="PPP" width="50%">
</div>

The alternative of this is to include the number of bytes in the frame at the beginning of the frame, in the frame header. DDCMP used this approach. One danger with this approach is that a transmission error could corrupt the count field, in which case the end of the frame would not be correctly detected. (A similar problem exists with the sentinel-based approach if the ETX field becomes corrupted.)

### Cyclic Redundancy Check (CRC)

Cyclic redundancy check is an error detecting code used in networks and storage devices to detect bit errors in the raw data. CRCs are so called because the check (data verification) value is a redundancy and the algorithm is based on [cyclic codes](https://en.wikipedia.org/wiki/Cyclic_code).

CRCs are used to encode messages by adding a fixed length check value for detecting errors. Cyclic codes are simple to implement and are well suited for detection of [burst errors](https://en.wikipedia.org/wiki/Burst_error). Typically an $$n$$-bit CRC applied to a data block of arbitrary length will detect any single error burst of up to $$n$$ bits.

In CRC codewords are used to detect bit errors. A codeword is formed by concatenating the $$n$$-bits of data with a $$k$$-bit check value of CRC. Each of the $$2^n$$ possible $$n$$ bit data sequence is mapped to a unique $$n+k$$ bit codeword. Whenever an error occurs in a bit the codeword changes and becomes invalid. But what if a codeword changes into another codeword?

##### Hamming Distance

Hamming distance between two equal length string is the number of positions at which the corresponding symbols are different, or the minimum number of errors that could have transformed one string into the other.

Hamming distance of a CRC is the minimum HD over all pairs of codewords. We want the hamming distance to be as large as possible for CRC so that it requires many bit error for a codeword to change into another codeword.

##### Galois Field - GF(2)

GF(2) is the Galois field of two elements with its additive and multiplicative identities respectively denoted 0 and 1.

It's addition is same as **logical XOR** and multiplication is same as **logical AND**. Also subtraction is same as addition.

<table style="text-align: center; width: 20%; margin-left: 28%; float: left;">
  <tbody>
    <tr style="background: #f9f9f9;">
      <th>+</th>
      <th>0</th>
      <th>1</th>
    </tr>
    <tr>
      <th style="background: #f9f9f9;">0</th>
      <td style="background: white;">0</td>
      <td style="background: white;">1</td>
    </tr>
    <tr>
      <th style="background: #f9f9f9;">1</th>
      <td style="background: white;">1</td>
      <td style="background: white;">0</td>
    </tr>
  </tbody>
</table>

<table style="text-align: center; width: 20%; margin-right: 28%; float: right;">
  <tbody>
    <tr style="background: #f9f9f9;">
      <th>×	</th>
      <th>0</th>
      <th>1</th>
    </tr>
    <tr>
      <th style="background: #f9f9f9;">0</th>
      <td style="background: white;">0</td>
      <td style="background: white;">0</td>
    </tr>
    <tr>
      <th style="background: #f9f9f9;">1</th>
      <td style="background: white;">0</td>
      <td style="background: white;">1</td>
    </tr>
  </tbody>
</table>

#### Generating a CRC

For generating a CRC code we need a generator polynomial. We divide the data sequence with this generator polynomial and the remainder of this division is the CRC code. 