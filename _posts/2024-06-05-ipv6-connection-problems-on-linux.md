---
title: "IPv6 connectivity problems on Linux"
categories:
  - Troubleshooting
tags:
  - Linux
  - IPv6
  - HTTP
---

The problem with IPv6 connection got me each time with Git hostings and
recently with ruby gem registry. I don't know the root of this problem, just
turned off IPv6 and everything is fine.

There a several ways to do it, though. One helps me with GitLab SSH access and
second with ruby gem registry.

## Configure SSH to work with IPv4 instead of IPv6

Run `sudo $EDITOR /etc/ssh/ssh_config`, find the field `AddressFamily` and set
it to `inet`. You are done :)

## Configue NetworkManager to use Link-Local Only Method for IPv6 with corresponding network

Open `Edit Connections...` menu in nm-applet, choose the network you're using
right now, double-click on it, open `IPv6 Settings` tab and set `Method` to
`Link-Local Only`. This settings solved my problems with other registers like
ruby gems and bun.js.

Good luck!
