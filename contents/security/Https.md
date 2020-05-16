<frontmatter>
  title: Security - https
  header: pagetop.md
  footer: footer.md
  head: head.md
  siteNav: mainNav.md
  pageNav: 3
</frontmatter>

<div class="website-content">

{{ booktitle | safe }}

# Security - https

Author: [Boxin][1]

<box id="article-toc">

* [Overview‎](#overview)
  * [Why Do We Need HTTPS‎?](#why-do-we-need-https)
  * [Why Is HTTPS Secure‎?](#why-is-https-secure)
  * [How to Set up HTTPS‎?](#how-to-set-up-https)
* [Misuse of HTTPS‎](#misuse-of-https)
* [Conclusion‎](#conclusion)
</box>

## Overview

HTTPS is the end-to-end encryption on data on top of HTTP to prevent network sniffing (eavesdropping data packets). In this tutorial, we will cover four pointers to have a better understanding of https. They are:

- [Why do we need HTTPS?](#why-do-we-need-https)
- [Why is HTTPS secure?](#why-is-https-secure)
- [How to set up HTTPS?](#how-to-set-up-https)
- [Misuse of HTTPS](#misuse-of-https)

### Why Do We Need HTTPS?

The web application usually runs over IP network, which is vulnerable to network sniffing. The old HTTP transmits data packets in plain text and if the network is sniffed, the sniffer can see confidential information in the data packets such as the password or [session tokens][2]. Here are some examples on how a plain text could be sniffed.

- On public free wifi, a Wifi adapter in monitor mode would be able to capture all the ongoing packets to and from the wifi access point, regardless of its destination. If the traffic is transmitted over HTTP, the data sent over wifi is in plain text and the session token and password can be stolen. One famous example would be [Firesheep][3], a Firefox plugin to sniff session token used in websites such as Facebook. This has made Facebook [change its default protocol from HTTP to HTTPS][4]

- Our network packets usually travel through switches and routers around the globe to reach the destination. Any one of them, if compromised, could expose our network traffic to the sniffer. [Network tap][5] is an example of a device used to sniff network traffic.

- Our Internet architecture relies on [DNS][6] for domain name and IP mapping and [ARP][7] for MAC address and IP mapping. None of the above is built with security in mind. Common attacks such as [DNS cache poisoning][8] or [ARP poisoning][9]
could redirect your traffic for monitoring.

All in all, the Internet architecture that we rely on for network transmission is very vulnerable to network sniffing. If we were to use HTTP, which transmits packets in plain text, no confidentiality could be guaranteed for our web application. Therefore, we need to use HTTPS as an end to end encryption to secure our network packets.

### Why Is HTTPS Secure?

As aforementioned, our network is not secure, so how could HTTPS help? HTTPS is built on top of HTTP with the addition of [SSL][10] to encrypt the plain text message. The purpose of this encryption is to make sure only client and the server could decrypt the message with required keys, and sniffer cannot decrypt packets even though they may sniff packets.

There are mainly 3 encryption algorithms used in HTTPS, namely RSA, Diffie Hellman and Elliptic Curve Algorithm. They are more thoroughly explained in [Introduction to Cryptography][11] section. These algorithms prevent sniffers from decrypting packets without knowing the keys used because the best attack algorithms known at the moment run in [sub-exponential][12] time. Therefore, the attack is believed to be computationally infeasible when the keysize is large enough (e.g. 2048 bits for RSA), though it is not mathematically proven.

Thus, by using https, we can be sure that even though our network packets are transmitted over an insecure network, sniffers cannot understand the content of our encrypted packets.

Besides providing secure network traffic, HTTPS also provides server validation through Certificate Authority (CA) architecture. A detailed explanation on CA is [here][13] . In short, CA works by issuing the server a digital certificate that can only be produced by CA. When the server sends its digital certificate to the client, client browsers verify the digital certificate with CA to check whether the server is indeed the intended server. To obtain such digital certificate, the server needs to apply to CA and CA will verify the server before issuing the digital certificate.

### How to Set up HTTPS?
In order set up HTTPS on your server, you would need to have:

1. A dedicated IP address for your server.
2. Obtain a digital certificate from a CA.

For example, one of the CA is [digicert][14] and they provide SSL [installation guide][15] for different platforms. Usually, the SSL setup could be found on the CA that you obtained a digital signature from.

However, most CAs are not free of charge. One free initiative to provide free domain validation certificates is [Lets Encrypt][16].

## Misuse of HTTPS

Up to this moment, it seems that nothing could go wrong with HTTPS. However, in real life, there could be weakness on the implementation of HTTPS.

#### SHA-1 Collision Attack
As aforementioned, Certificate Authority (CA) signs a digital certificate provided by the server to prove the identity of the server. However, in the real-world implementation, the CA does not sign the digital certificate directly, rather CA signs the fixed length [hashed digest][17] of the digital certificate for efficiency. This introduces the possibility of two different digital certificates with the same hashed digest. Thus, if the attackers manage to forge a fake digital certificate with the same hashed digest of another valid digital certificate, the browser would trust the attackers server and all the servers signed by the attackers. This loophole occurs with SHA-1 hashing algorithm and SHA-1 is no longer used in HTTPS after 2016. In 2017, Google has announced an algorithm to forge a duplicated SHA-1 hash and the report is [here][18]. A more detailed explanation of this problem is found [here][19].

#### Weak Diffie-Hellman Attack
During the [cipher suite negotiation][20] process of HTTPS, client and server send each other in plain text the HTTPS standard they support and the most secure standard is chosen to be used. However, this can be exploited for HTTPS downgrade by a [man in the middle attack][21]. The man in the middle could send forged cipher suite negotiation to both server and client to indicate the maximum security supported is only 512 bits Diffie Hellman and trick server and client to encrypt with 512 bits Diffie Hellman. At the moment, without knowing the keys, 512 bits Diffie Hellman algorithm could be decrypted with sufficient resources. A more detailed description could be found [here][22]

#### Session Hijacking on Partially Protected Websites
Some websites use HTTPS only on the login page to encrypt the login credentials of users. However, this is vulnerable to [session hijacking][23] attack. Most websites use cookies and session tokens to maintain a stateful connection with its users, and the session token is embedded into each packet to authenticate the identity of users. Without using HTTPS in all the websites in the domain, the packets containing session token is transmitted in plaintext and sniffers can easily obtain the session token to impersonate the victim. A demonstration of this attack on Qoo10.com user can be found [here][24]

## Conclusion
HTTPS provides security to a web application.  If the web application requires secure network traffic (e.g. online banking), HTTPS should be implemented. However, servers usually need to pay for the digital certificate used by HTTPS. Also, the additional layer of encryption and decryption adds overhead to network traffic (though the impact is not significant). If secure network traffic is not required (e.g. University home page), HTTPS may not be used.

[1]: https://github.com/boxin-yang
[2]: https://searchsoftwarequality.techtarget.com/definition/session-ID
[3]: https://github.com/codebutler/firesheep
[4]: https://www.facebook.com/notes/facebook-engineering/secure-browsing-by-default/10151590414803920/
[5]: https://searchnetworking.techtarget.com/definition/Network-tap
[6]: https://en.wikipedia.org/wiki/Domain_Name_System
[7]: https://en.wikipedia.org/wiki/Address_Resolution_Protocol
[8]: https://en.wikipedia.org/wiki/DNS_spoofing
[9]: https://en.wikipedia.org/wiki/ARP_spoofing
[10]: https://www.digicert.com/ssl.htm
[11]: ../security/cryptography.md
[12]: https://en.wikipedia.org/wiki/Time_complexity#Sub-exponential_time
[13]: https://www.globalsign.com/en-sg/ssl-information-center/what-are-certification-authorities-trust-hierarchies/
[14]: https://www.digicert.com
[15]: https://www.digicert.com/ssl-certificate-installation.htm
[16]: https://letsencrypt.org/
[17]: https://en.wikipedia.org/wiki/Cryptographic_hash_function
[18]: https://security.googleblog.com/2017/02/announcing-first-sha1-collision.html?m=1
[19]: https://www.sott.net/article/275524-Why-HTTPS-and-SSL-are-not-as-secure-as-you-think
[20]: https://en.wikipedia.org/wiki/Cipher_suite
[21]: https://en.wikipedia.org/wiki/Man-in-the-middle_attack
[22]: https://weakdh.org/
[23]: https://en.wikipedia.org/wiki/Session_hijacking
[24]: https://www.youtube.com/watch?v=BjTwNzoMUuk
</div>
