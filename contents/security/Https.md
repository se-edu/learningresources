# Security

Author: [Boxin](https://github.com/boxin-yang)

# Overview

Https is the end to end encryption on data on top of http to prevent network sniffing(eavesdropping data packets). In this tutorial, we will cover four questions to have a better understanding of https. The questions are :

- [Why do we need https](#why-do-we-need-https)
- [Why https is secure](#why-https-is-secure)
- [How to set up https](#how-to-set-up-https)
- [Misuse of https](#misuse-of-https)

## Why do we need https

The web application usually runs over IP network, which is vulnerable to network sniffing. The old http transmits data packet in plain text and if network is sniffed, the sniffer can see confidential information in the data packet such as password or [session token](http://searchsoftwarequality.techtarget.com/definition/session-ID). Here are some examples on how a plain text could be sniffed.

- On a public free wifi, a Wifi adapter with monitor mode would be able to capture all the ongoing packets to and from the wifi access point, regardless of its destination. If the traffic is transmitted over http, the data sent over wifi is in plain text and the session token and password can be stolen. One famous example would be [Firesheep](https://github.com/codebutler/firesheep) , a Firefox plugin to sniff session token used in websites such as Facebook. This has made Facebook [change its default protocol from http to https](https://www.facebook.com/notes/facebook-engineering/secure-browsing-by-default/10151590414803920/)

- Our network packets usually travel through switches and routers around the globe to reach the destination. Any one of them, if compromised, could expose our network traffic to sniffer. [Network tap](http://searchnetworking.techtarget.com/definition/Network-tap) is an example of device used to sniff network traffic.

- Our Internet architecture relies on [DNS](https://en.wikipedia.org/wiki/Domain_Name_System) for domain name and IP mapping and [ARP](https://en.wikipedia.org/wiki/Address_Resolution_Protocol) for MAC address and IP mapping. None of the above is built with security in mind. Common attacks such as [DNS cache poisoning](https://en.wikipedia.org/wiki/DNS_spoofing) or [ARP poisoning](https://en.wikipedia.org/wiki/ARP_spoofing)
could redirect your traffic to monitored traffic by the attackers.

All in all, the Internet architecture that we rely on for network transmission is very vulnerable to network sniffing. If we were to use http, which transmits packets in plain text, no confidentiality could be guaranteed for our web application. Therefore, we need to use https as an end to end encryption to secure our network packets.

## Why https is secure

As aforementioned, our network is not secure, so how could https help? Https is built on top of http with addition of [SSL](https://www.digicert.com/ssl.htm) to encrypt the plain text message. The purpose of this encryption is to make sure only client and the server could decrypt the message with required keys, and sniffer cannot decrypt packets even though they may sniff packets. 

There are mainly 3 encryption algorithms used in https, namely RSA, Diffie Hellman and Elliptic Curve Algorithm. They are more thoroughly explained in [Introduction to Cryptography](../security/cryptography.md) section. These algorithms prevent sniffers from decrypting packets without knowing the keys used because without knowing the keys all the decryption algorithms known at the moment run in exponential time. As keys are kept secret by the clients and servers and are not known to sniffers, it is computational infeasible for sniffers to decrypt when the keysize is large enough(Though it is not mathematically proven that polynomial decryption algorithms without keys do not exist).

Thus, by using https, we can be sure that even though our network packets are transmitted in an insecure network, sniffers cannot understand the content of our encrypted packets. 

Besides providing secure network traffic, https also provides server validation through Certificate Authority(CA) architecture. A detailed explanation on CA is [here](https://www.globalsign.com/en-sg/ssl-information-center/what-are-certification-authorities-trust-hierarchies/) . In short, CA works by issuing the server a digital certificate that can only be produced by CA. When server sends its digital certificate to the client, client browsers verify the digital certificate with CA to check whether the server is indeed the intended server. To obtain such digital certificate, server needs to apply to CA and CA will verify the server before issuing the digital certificate.

## How to set up https
In order set up https on your server, you would need to have:

1. A dedicated IP address for you server.
2. Obtain a digital certificate from a CA.

For example, one of the CA is [digicert](https://www.digicert.com) and they provide SSL [installation guide](https://www.digicert.com/ssl-certificate-installation.htm) for different platforms. Usually the SSL setup could be found on the CA that you obtained digital signature from.

However, most CA are not free of charge. One free initiative to provide free domain validation vertificates is [Lets Encrypt](https://letsencrypt.org/).

## Misuse of https

Up to this moment, it seems that nothing could go wrong with https. However, in real life, there could be weakness on the implementation of https.

### SHA-1 Collision Attack
In the implementation of https, the CA does not sign the digital certificate directly, rather it signs the fixed length [hashed digest](https://en.wikipedia.org/wiki/Cryptographic_hash_function) of the digital certificate for efficiency. This introduces the possibility of two different digital certificate with the same hashed digest. Thus, if the attackers manage to forge a fake digital certificate with the same hashed digest of another valid digital certificate, the browser would trust the attackers server and all the servers signed by the attackers. This loop hole occurs with SHA-1 hashing algorithm and SHA-1 is no longer used in https after 2016. In 2017, Google has announced an algorithm to forge a duplicated SHA-1 hash and the report is [here](https://github.com/se-edu/learningresources/pull/18/files). A more detailed explanation of this problem is found [here](https://www.sott.net/article/275524-Why-HTTPS-and-SSL-are-not-as-secure-as-you-think).

### Weak Diffie-Hellman Attack
During the [cipher suites](https://en.wikipedia.org/wiki/Cipher_suite) process of https, client and server send each other in plain text the https standard they support and the most secure standard is chosen to be used. However, this can be exploited for https downgrade by a man in the middle attack. The man in the middle could send forged hand shake message to both server and client to indicate the maximum security supported is only 512 bits Diffie Hellman and trick server and client to encrypt with 512 bits Diffie Hellman. At the moment, without knowing the keys, 512 bits Diffie Hellman algorithm could be decrypted with sufficient resources. A more detailed description could be found [here](https://weakdh.org/)

### Session Hijacking on partially protected websites
Some websites use https only in the login page to encrypt the login credentials of users. However, this is vulnerable to [session hijacking](https://en.wikipedia.org/wiki/Session_hijacking) attack. Most websites use cookie and session token to maintain a stateful connection with its users, and session token is embedded into each packets to authenticate the identity of users. Without using https in all the websites in the domain, the packets containing session token is transmitted in plaintext and sniffers can easily obtain the session token to impersonate the victim. A demonstration of this attack on Qoo10.com user can be found [here](https://www.youtube.com/watch?v=BjTwNzoMUuk
)

## Reflection
Https provides security to web application.  If the web application requires secure network traffic(e.g. online banking), https should be implemented. However, servers usually need to pay for the digital certificate used by https. Also, the additional layer of encryption and decryption adds overhead to network traffic(though the impact is not significant). If secure network traffic is not required(e.g. University home page), https may not be implemented.
