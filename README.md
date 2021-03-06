# SwiftNIO IRC Server

![Swift4](https://img.shields.io/badge/swift-4-blue.svg)
![Swift5](https://img.shields.io/badge/swift-5-blue.svg)
![macOS](https://img.shields.io/badge/os-macOS-green.svg?style=flat)
![tuxOS](https://img.shields.io/badge/os-tuxOS-green.svg?style=flat)
![Travis](https://travis-ci.org/NozeIO/swift-nio-irc-webclient.svg?branch=develop)

[SwiftNIO IRC](https://github.com/NozeIO/swift-nio-irc)
is a Internet Relay Chat protocol implementation for
[SwiftNIO](https://github.com/apple/swift-nio),
a basis for building your own IRC servers and clients,
a sample IRC server, as well as some IRC bots written in the
[Swift](http://swift.org) programming language.

*SwiftNIO IRC Server* is a framework to build IRC servers on top of
[SwiftNIO IRC](https://github.com/NozeIO/swift-nio-irc).

Want to build a customer-support chat system?
And your customers happen to be Unix people from the 80s and early 90s?
What a great match!

This Swift package contains the reusable `IRCServer` module,
and the `MiniIRCd`, a small and working IRC sample server.

MiniIRCd also configures, embeds and runs:

- [swift-nio-irc-webclient](https://github.com/NozeIO/swift-nio-irc-webclient) -
  a simple IRC webclient + WebSocket gateway based on SwiftNIO IRC, and
- [swift-nio-irc-eliza](https://github.com/NozeIO/swift-nio-irc-eliza) -
  a cheap yet scalable therapist.


## What it looks like

On the surface it is a very simple chat webapp, with basic support for
channels and direct messages:

<img src="http://zeezide.de/img/irc-eliza.png" width="640" />

**Sometimes** a live demo installation is running on
[http://irc.noze.io/](http://irc.noze.io/).

Apart from the web frontend, MiniIRCd also embeds an actual IRC server, that is, 
you can connect to the server using native clients like
[Mutter](https://www.mutterirc.com),
[Irssi](https://irssi.org)
or
[Textual](https://www.codeux.com/textual/).

<img src="http://zeezide.de/img/mutter-irc-setup.gif" />


## Overview

The `IRCServer` module which is part of this package, only links against the
`NIOIRC`
protocol module of [SwiftNIO IRC](https://github.com/NozeIO/swift-nio-irc).

The included `MiniIRCd` tool on the other hand, also spins up the
[IRC/WebSocket gateway](https://github.com/NozeIO/swift-nio-irc-webclient),
includes the webapp and starts up the 
[ElizaBot](https://github.com/NozeIO/swift-nio-irc-eliza).

```
                             ┌──────────────────────────────────────────────────┐
                             │ ┌───────────────────────┐       ┌──────────────┐ │
               HTML          │ │  ┌─────────────────┐  │       │    Eliza     │ │
        ┌───────JS───────────┼─┼──│ NIO HTTP Server │  │       │     Bot      │ │
        │                    │ │  └─────────────────┘  │       └──────────────┘ │
        │                    │ │           │           │               │        │
        ▼                    │ │       Upgrades        │              IRC       │
┌──────────────┐             │ │      Connection       │               │        │
│              │             │ │           │           │               ▼        │
│  WebBrowser  │             │ │           ▼           │       ┌──────────────┐ │
│              │  WebSocket  │ │  ┌─────────────────┐  │       │              │ │
│  JavaScript  │◀────JSON────┼─┼─▶│  NIO WebSocket  │◀─┼─IRC──▶│  IRC Server  │ │
│    WebApp    │             │ │  └─────────────────┘  │       │              │ │
│              │             │ │       WebServer       │       └──────────────┘ │
└──────────────┘             │ └───────────────────────┘                        │
                             │                                                  │
                             │       All Services Run as Part of MiniIRCd       │
                             └──────────────────────────────────────────────────┘
```


## Running the Server

To get started, just checkout the repository and call `swift run`:

```
helge@ZeaPro swift-nio-irc-server (develop)*$ swift  run
Fetching https://github.com/apple/swift-nio.git
...
Compile Swift Module 'miniircd' (2 sources)
Linking ./.build/x86_64-apple-macosx10.10/debug/miniircd
 __  __ _       _ _____ _____   _____
 |  \/  (_)     (_)_   _|  __ \ / ____|   Swift IRCd
 | \  / |_ _ __  _  | | | |__) | |
 | |\/| | | '_ \| | | | |  _  /| |        Port: 6667
 | |  | | | | | | |_| |_| | \ \| |____    PID:  2343
 |_|  |_|_|_| |_|_|_____|_|  \_\\_____|

Ready to accept connections on: [IPv4]0.0.0.0:6667
IRCWebClientServer running on: [IPv4]0.0.0.0:1337
Eliza is ready and listening!
````

You can then connect via IRC on `localhost:6667` or
to the web frontend on [http://localhost:1337/](http://localhost:1337).


## Importing the module using Swift Package Manager

An example `Package.swift `importing the necessary modules:

```swift
// swift-tools-version:5.0

import PackageDescription

let package = Package(
    name: "MyOwnIRCServer",
    dependencies: [
        .package(url: "https://github.com/NozeIO/swift-nio-irc-server.git",
                 from: "0.5.0")
    ],
    targets: [
        .target(name: "MyOwnIRCServer",
                dependencies: [ "IRCServer" ])
    ]
)
```


### Who

Brought to you by
[ZeeZide](http://zeezide.de).
We like
[feedback](https://twitter.com/ar_institute),
GitHub stars,
cool [contract work](http://zeezide.com/en/services/services.html),
presumably any form of praise you can think of.

NIOIRC is a SwiftNIO port of the
[Noze.io miniirc](https://github.com/NozeIO/Noze.io/tree/master/Samples/miniirc)
example from 2016.
