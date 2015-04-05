# Introduction

The identd module answers [ident](https://tools.ietf.org/html/rfc1413) requests
on a configurable port.

# Installation

Run `znc-buildmod identd.cc` to build the module. Place `identd.so` inside a
directory where ZNC will look for modules. This could be `/usr/local/lib/znc`,
`~/.znc/modules`, `/path/to/my/zncroot/modules`, or something else, depending
on your setup.

# Configuration

Load the module with `/znc LoadMod identd [[+]port]`.

This module takes a single argument: the port on which to listen for ident
requests. If this argument is omitted, identd will listen on port 9113.

Because ZNC should never be run as root, identd will not be able to listen on
the official ident port, 113. You should configure your firewall to redirect
incoming traffic on port 113 to the configured listening port. For example,
using pf on FreeBSD:

    rdr pass on $interface inet proto tcp to $address port auth -> $address port 9113

Ensure that only the port is redirected, not the address. If this is not
possible, see the following section for an alternative.

## Blocking Connect Mode

If the port argument starts with `+` (e.g. `/znc LoadMod identd +9113`), identd
will use blocking connect mode. In this mode, ZNC will connect to IRC servers
for multiple users/networks one at a time, rather than connecting to multiple
servers in parallel.

This mode must be used if your network setup requires some form of address
translation, and as a result ZNC and the IRC server have different opinions
on what the local address/port of a connection are.
