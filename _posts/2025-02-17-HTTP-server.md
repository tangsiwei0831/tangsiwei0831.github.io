---
title: 'HTTP Server'
date: 2025-01-27
permalink: /posts/http
tags:
  - Software
---
<h1>TCP</h1>
<details>
  
  <h2>Layer</h2>
  TCP: Byte Stream<br>
  UDP: Packet with port number<br>
  IP: Packet(sender, receiver and data)<br>
  DNS: Protocol(domain name to IP address lookup), runs on UDP normally, a 2-byte length field is prepended to each DNS message so that the server or client can tell which part of the byte stream is which message. <br>

  <h2>Build Connection</h2>
    To establish a TCP connection, there should be a client and a server (ignoring the simultaneous case). The server waits for the client at a specific address (IP + port), this step is called bind & listen. Then the client can connect to that address. The “connect” operation involves a 3-step handshake (SYN, SYN-ACK, ACK).

  <h2>Socket</h2>
  Listening socket
    <li> bind & listen </li>
    <li> accept </li>
    <li> close </li> 
  Connection socket
    <li> read </li>
    <li> write </li>
    <li> close </li>
</details>