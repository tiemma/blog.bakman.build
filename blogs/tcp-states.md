---
layout: post_no_side_bar
title: TCP States
date: Feb 1, 2024
tags: ["tcp", "ip", "protocols", "internet", "networking"]
---

## Good 'ol IP
The internet is a collection of smaller networks talking with each other. As DARPA evolved in its research work, it brought about the IP (Internet Protocol) which we colloquially regard
as TCP(Transmission Control Protocol)/IP. The IP terminology has evolved to include a suite of protocols across different layers of the stack which we summarise as the OSI layer for easier
comprehension, depending on your reference, you might see different interpretations of the layers depicted in IP by the OSI model. We will be considering the transport layer of the OSI model 
for this article.

A good analogy for the 7-layer OSI model is
| Layer  | Description | Analogy |
| --- | --- | --- |
| Physical | Media used to establish network connections eg coaxial, fibre | Please |
| Data  | Data transfer and access | Do |
| Network | Media used to establish network connections eg coaxial, fibre | Please |
| Physical | Media used to establish network connections eg coaxial, fibre | Please |
| Physical | Media used to establish network connections eg coaxial, fibre | Please |
| Physical | Media used to establish network connections eg coaxial, fibre | Please |
| Physical | Media used to establish network connections eg coaxial, fibre | Please |

> Physical Layer - Please
> Data Layer    - Do
> Network Layer - Not
> Transport Layer - Throw
> Session Layer - Sausage 
> Presentation Layer - Pizza
> Application Layer - Away

The internet is neither a collection of TCP based protocols but an amalgamate of different other transport layer protocols, the most popular antithetical one to TCP being UDP.
In recent times, the benefits of an ordered connection based protocol and a connectionless unreliable transport protocol have been merged into QUIC which is said to be HTTP/3.

In this article, I will only be considering TCP as it makes for most of the collective networking work folks on the internet will ever use. Most of this thanks goes
to the wide adoption of HTTP as the primary protocol for application use. As such, we will consider TCP/IP states and explaining the various inner working of how a 
TCP connection is established along with all the transitions trending down to its CLOSE. 


## What are TCP states?