---
title: "Road to WebRTC"
date: "2018-11-08"
type: "article"
---

WebRTC allows peers to see and talk to each other worldwide in real-time. It can look and sound like magic at the outset, till you learn to wield it.

> “Any sufficiently advanced technology is indistinguishable from magic.”
> — Profiles of the Future, Arthur Clarke

Imagine a network of devices which could connect to each other directly, peer to peer, and share video and data. These devices could get user media and transmit it across to the device at the other end, which could then render those media data to its user. They had a camera, a video display and an interface fused together. They could all connect to each other as long as the person connecting knew what to connect to. So, there was some signaling layer in between, but you needed to know the address of the other device in order to connect to it.

A bunch of technologies made their effort to solve the issues with real-time communications (RTC). Some of the biggest inventions in RTC were made in the last 2 decades, almost after 125 years since telephone was invented. We got VoIP built using open tools and technologies. We got IRC and XMPP. The second wave of VoIP gave us Skype and the entire gamut of Flash-based chat applications. At one point, the proprietary technologies got better than the ones in public domain. And the result was fragmentation. Lots of it. Each vendor and technology provider tried to build their own (and often better) version of RTC software. Without any standardization, everyone almost always needed to reinvent the RTC wheel.

Then, in the end of May 2011, Google open-sourced WebRTC. The web had already proven to be a valuable platform. The browsers have been the best of the apps. W3C and IETF put their armies together to standardize WebRTC so that it can be implemented properly across browsers. They envisioned the trust-worthy browsers to become the clients for real-time media communication of the future. No more downloading and installing proprietary clients for each services. Your web browser was the client. Google led the way. Mozilla and Opera followed close behind.

WebRTC is a big step forward from previous attempts at real-time communication. It is relatively new and highly flexible; it is just a communication technology with many moving parts. It can be used in wildly different communication topologies — as unidirectional and bidirectional, for 1:1, 1:N or N:N use cases. An end user of a WebRTC application only sees and interacts with the thin UI layer on top. As a developer, if you want only basic peer to peer communication, you can put up a signaling server, write a few lines of code and, lo, you can put together a video chat application in 10 minutes. It almost sounds magical.

Magical, till you try doing something serious with WebRTC. Most of the developers who are lured by the promise of an “easy video chat that needs no plugins” find themselves to be the Pippin of the WebRTC world.

WebRTC has a lot happening under the hood. Its fundamentally peer-to-peer nature makes it tricky to build complex topologies, unless you understand it very deeply. WebRTC is still too complex.

If you are willing to brave the challenge of learning WebRTC today, you may want to understand the exact science of how all the moving pieces fit together. You would want your disillusionment to result in practical knowledge that you can use in building RTC applications. It will require more effort, but that is definitely worth it.

So, where to start if you want to get knee deep?

- Go through the [WebRTC Getting Started guide](https://webrtc.org/start/) on webrtc.org.
- Check out [WebRTC samples](https://webrtc.github.io/samples/) and their [codebase](https://github.com/webrtc/samples/).
- WebRTC still needs servers. Get to know [STUN and TURN](https://www.html5rocks.com/en/tutorials/webrtc/infrastructure/) well.

Once you have learnt enough about WebRTC, it will not be an enigmatic magical puzzle any more. You would have learnt the science behind the magic.
