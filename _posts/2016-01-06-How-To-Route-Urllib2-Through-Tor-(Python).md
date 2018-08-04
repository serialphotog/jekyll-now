---
layout: post
title: How to Route Urllib2 Through Tor (Python)
category: Development
tags: [python, programming, Tor]
---

I’ve recently been experimenting on a new project to scrape data from webpages located on the Tor network. For simplicity sake, I decided to write this bit of code in Python and use the handy urllib2 library to handle the HTTP requests.

For those that don’t know, Tor runs a SOCKS5 proxy, which, by defaulted, runs on **127.0.0.1:9050**. I thought things would be as simple as telling urllib2 to use a proxy located at IP *127.0.0.1* and port *9050*, but I quickly found that this doesn’t work.

Luckily, after a bit of digging, I found a solution. It turns out that urllib2 uses Python’s socket module, which contains the method **create_connection()**. If we take a look at the code for this method we can see where our problem lies:

```python
def create_connection(address, timeout=_GLOBAL_DEFAULT_TIMEOUT,
                      source_address=None):
    """Connect to *address* and return the socket object.
 
   Convenience function.  Connect to *address* (a 2-tuple ``(host,
   port)``) and return the socket object.  Passing the optional
   *timeout* parameter will set the timeout on the socket instance
   before attempting to connect.  If no *timeout* is supplied, the
   global default timeout setting returned by :func:`getdefaulttimeout`
   is used.  If *source_address* is set it must be a tuple of (host, port)
   for the socket to bind as a source address before making the connection.
   An host of '' or port 0 tells the OS to use the default.
   """
 
    msg = "getaddrinfo returns an empty list"
    host, port = address
    for res in getaddrinfo(host, port, 0, SOCK_STREAM):
        af, socktype, proto, canonname, sa = res
        sock = None
        try:
            sock = socket(af, socktype, proto)
            if timeout is not _GLOBAL_DEFAULT_TIMEOUT:
                sock.settimeout(timeout)
            if source_address:
                sock.bind(source_address)
            sock.connect(sa)
            return sock
 
        except error, msg:
            if sock is not None:
                sock.close()
 
    raise error, msg
```

In looking at this we can see that, even though we specified that Tor should be used as our proxy, the **create_connection()** function will still perform the DNS request using the default settings, hence bypassing the Tor network. Luckily, we can create our own **create_connection()** method and jerry-rig it into the socket class before we load urllib2. In doing this we can force the DNS request to go though Tor, thus allowing us to route our urllib2 traffic through the Tor network. This can be achieved with the following bit of code:

```python 
import socket
import socks
 
# urllib2 uses the socket module's create_connection() function.
# The way the DNS request is done won't work for our Tor connection,
# so we need to jerry-rig our own create_connection() for urllib2
def create_connection(addr, timeout=None, src=None):
  sock = socks.socksocket()
  sock.connect(addr)
  return sock
 
# Set our proxy to TOR
socks.setdefaultproxy(socks.PROXY_TYPE_SOCKS5, '127.0.0.1', 9050)
 
socket.socket = socks.socksocket
socket.create_connection = create_connection # force the socket class to use our new create_connection()
 
import urllib2 # now we can import the urllib :D
```

With this done any request that we make using the urllib2 class will be performed over the Tor network!