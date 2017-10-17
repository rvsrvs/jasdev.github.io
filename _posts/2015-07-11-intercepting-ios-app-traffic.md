---
layout: post
title: Intercepting iOS Network Traffic
permalink: intercepting-ios-traffic
---

Ever wanted to write a small wrapper for an iOS app that has an undocumented API? Itching to reverse engineer how it communicates with its backend[^1]? You‘re in luck! 😎 In the past, my friends and I have used this technique to figure out how to programmatically send and receive Snapchats, months after the application‘s release. This lead to one of the [greatest stories](https://medium.com/@binroot/the-best-jokes-have-no-punchline-5f4713a963d6) we tell today.

To sniff an application‘s network traffic, we‘re going to use [mitmproxy](http://mitmproxy.org). It‘s a powerful man-in-the-middle proxy that allows you to intercept, modify, replay, and save HTTP/S traffic.

### Installing mitmproxy and CA certificate

First, let‘s install mitmproxy (I use `pip` in this example but there are other install methods in their [docs](http://mitmproxy.org/doc/install.html))

```
$ pip install mitmproxy
```

Next, we‘ll need to alter the proxy settings on our iOS device to point to the machine running mitmproxy:

- Determine your computer‘s IP on the network
  - System preferences > Network > Advanced > TCP/IP  
  ![](/public/images/osxip.png)

- Launch mitmproxy
    - `$ mitmproxy`

- Set iOS manual proxy settings
  - Settings > Wi-Fi > [select current network]  
  ![](/public/images/iosproxy.png)

- Install CA certificate on device
  - Open Safari and go to `mitm.it`, if everything is setup correctly, you should see this screen below. Install the Apple certificate.  
  ![](/public/images/mitmit.png)

### Viewing Network Requests

Setup is done! Now the fun begins. You may have noticed the output pouring in from of the `mitmproxy` command you ran earlier. Those are all of the network requests your device is making! You can navigate through the requests with the arrow keys and press enter on any request to inspect it (once in the inspection mode, use `h` and `l` keys to switch tabs). If you need any additional help with possible commands, just type `?`.

You‘re all set! Inspect away and go make that unofficial API wrapper you always wanted 🚀

---

## Footnotes:

[^1]: If you find any security issues, do the right thing and let the developers know. It shows character and may even land you an interview with the company 😊
