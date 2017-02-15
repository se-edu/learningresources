# Introduction to Cryptography

Authors: Dickson Tan

# Overview

Cryptography is the discipline of developing methods for secure communication. 
Understanding cryptography wwill enable you to correctly use cryptography libraries and combine cryptographic primitives to match the security needs of your application.

Modern cryptography involves developing methods to achieve the following:

* Confidentiality: ensuring that a message can only be read by its intended recipient. For example, websites use encryption in the Transport Layer Security (TLS) protocol to prevent interception of sensitive information such as credit card numbers and passwords.
* Integrity: ensuring a message has not been changed. For example, hash functions are used in the BitTorrent protocol to ensure files received have not been modified by malicious peers.
* Authentication: the recipient should be able to verify the sender of a message. For example, <TODO: add https example>
* Non-repudiation: the sender should not be able to deny sending a message after doing so. For example, the parties of a digital contract should not be able to deny signing the contract in a legal dispute.

# Encryption

Encryption is the process of encoding messages so that it is readable only by the intended recipient. The message being encrypted is called the plaintext. The encryption algorithm, or cipher, "scrambles" the message, producing ciphertext, which should only be readable by the recipient.

One of the earliest encryption algorithms or ciphers is the substitution cipher, which encrypts text by substituting each letter of the message with another letter. This page introduces the [Caesar cipher](http://www.cs.trincoll.edu/~crypto/historical/caesar.html), a substitution cipher, and how it can be defeated using statistical analysis. 
Modern ciphers today operate on bits rather than letters.

## Symmetric Key Ciphers

In a symmetric key cipher, the same key is used for both encryption and decryption. 
The parties wishing to communicate securely share the same key. For example, in the substitution cipher, the same key (the substitution table) is used for both encryption and decryption.

### Stream ciphers

Stream ciphers are inspired by the [one-time pad,](https://en.wikipedia.org/wiki/One-time_pad), a remarkably simple cipher. 
It is also the only known cipher that cannot be cracked, even if the attacker has infinite computing power. 
This property is known as [perfect secrecy](https://crypto.stackexchange.com/questions/3896/simply-put-what-does-perfect-secrecy-mean); the ciphertext gives no additional information about the plaintext, so knowing the ciphertext does not provide any advantage to the attacker trying to recover the plaintext.
Unfortunately, Shannon proved that any cipher that achieves perfect secrecy has the following limitations, making them [impractical](https://www.schneier.com/crypto-gram/archives/2002/1015.html#7).

* The key must be truely random, not pseudorandomly generated, and must never be reused.
* The key must be securely distributed, and be at least as long as the message being generated. For example, to send a 10gb file to someone encrypted with the one-time pad requires sending 10gb of key material + 10gb of ciphertext = 20gb.

Hence, modern ciphers aim to be secure against attackers with limited computational power, which is a more realistic scenario.

Stream ciphers approximate the operation of a one-time pad, using a much shorter key (e.g, 256 bits). 
The initial key is used as the seed of a [cryptographically secure pseudorandom number generator](https://en.wikipedia.org/wiki/Cryptographically_secure_pseudorandom_number_generator), which is used to generate the keystream, a stream of bits. 
Like in the one-time pad, plaintext bits are combined (usually xored) with bits of the keystream to produce the ciphertext. 
They are used for efficiency, ease of implementation in hardware, and when the length of the plaintext is unpredictable. 
However, block ciphers are more popular than stream ciphers for symmetric encryption, as they can be used as stream ciphers when in counter mode (to be explained in future section), which reduces the need for dedicated stream ciphers.

[RC4](https://en.wikipedia.org/wiki/RC4) is the most widely used stream cipher. Though its use is now discouraged. 