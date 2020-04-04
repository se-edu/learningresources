<frontmatter>
  title: Introduction to ZeroMQ
  header: pagetop.md
  footer: footer.md
  head: head.md
  siteNav: mainNav.md
  pageNav: 3
</frontmatter>

<div class="website-content">

{{ booktitle | safe }}

# Introduction to ZeroMQ

Author: [Tan Yuanhong](https://github.com/le0tan)

## What is ZeroMQ?

[ZeroMQ](https://zeromq.org/) is a high-performance asynchronous messaging library. It provides a message queue (MQ) for all processes communicating using the library, without a dedicated message broker - thus the *zero* in the name of *ZeroMQ*. 

### What is asynchronous messaging and message queue?

Asynchronous (or async, in short) communication means the two (or more than two) ends of the communication don't need to be occupied at the same time to send messages. You may consider making a phone-call synchronous and sending an email async. And **message queue** is one of the async communication protocols that puts the messages in a queue, store them temporarily until the recipient retrieves them.

Notice the word **"protocol"** used in the description above - communication protocol remains the same in spite of the actual medium of transport. It doesn't matter if your message is sent and received within process among threads, or among processes, or among different physical machines across the network. It merely specifies how communicators are supposed to encode/send and decode/receive their messages.

## Why ZeroMQ?

**TL;DR**: By adopting ZeroMQ in your concurrent application, you're less likely to have (or at least have less) bottleneck on inter-process/thread/machine communication, while still being able to write communication logic in familiar socket APIs.

### Ease of use like a networking library

Most operating systems provides system calls or equivalent support for message queues (e.g. [POSIX mq](http://man7.org/linux/man-pages/man7/mq_overview.7.html), [Win32 winmsg](https://docs.microsoft.com/en-us/windows/win32/winmsg/about-messages-and-message-queues)). However the way they operate is far from high-level and making your it directly interact with underlying OS APIs could possibly hurt the scalability (more on that later) of your application as you may find yourself in need of distributing the workload to different machines.

ZeroMQ looks like an embeddable networking library (read: easy to use) but acts like a concurrency framework (read: high performance). Using Python's built-in socket library as an example, if we are to write a simple TCP server in Python, it looks like this:

```python
import socket

s = socket.socket() # Create the socket
s.bind(('', 12345)) # Bind socket to a port
s.listen(5) # Listen on the socket
  
# Communicate using the socket
while True:
   c, addr = s.accept() # Receive input
   c.send('Connected. Now closing connection.') # Send response
   c.close()
```

Implementing the same functionality in ZeroMQ, it looks like this:

```python
import time
import zmq

context = zmq.Context()
socket = context.socket(zmq.REP) # Create the socket
socket.bind("tcp://*:12345") # Bind socket to a port

# Communicate using the socket
while True:
    message = socket.recv() # Receive input
    socket.send(b"Connected. Now closing connection.") # Send response
```

The similarity between the two is rather obvious - as long as you have experience with socket programming, using ZeroMQ to make communication between your concurrent processes possible and efficient should be no problem at all.

### Multi-transport

Unlike system calls, ZeroMQ supports various transports like in-process, inter-process, TCP, and multicast. And you don't need to change your code to switch underlying transport as ZeroMQ abstracted the details away, leaving you a reusable messaging layer. All you need to do is to update the definition (i.e. `context.socket(...)` and `socket.bind(...)`) of the socket and you're ready to scale your application from multi-core concurrency to a cluster of machines.

### High performance

Unlike networking libraries, ZeroMQ performs well enough to "be the fabric for clustered products". To give you a rough idea of how lightweight ZeroMQ is, the C++ source code for the ZeroMQ core engine [libzmq](https://github.com/zeromq/libzmq/tree/master/src) is only 1.5 megabytes. Moreover, in the [benchmark](http://wiki.zeromq.org/area:results) for ZeroMQ, the latency is measured in microsecond and the throughput is measured in million messages per second. Even CERN [approved](http://cds.cern.ch/record/1391410/files/CERN-ATS-2011-196.pdf?version=1) the performance of ZeroMQ in the setting of operating accelerators.

## How to Get Started with ZeroMQ?

Developing software for concurrent operation is by no means entry-level content. Luckily, the creator(s) of ZeroMQ has written a guide explaining not only the basic use cases of ZeroMQ, but also the ideas behind their design choices and probably most importantly, why they feel necessary to develop a library like this.

[ZeroMQ - The Guide](http://zguide.zeromq.org/)

If you simply want to get started soon and get to know the details as you adopt the library, you can download the library in your favorite language [here](https://zeromq.org/download/), then follow the corresponding examples written in the language of your choice.

[ZeroMQ - Get Started](https://zeromq.org/get-started/)