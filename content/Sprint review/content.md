**WebRTC** (Web Real-Time Communication) is a technology which enables Web applications and sites to capture and optionally stream audio and/or video media, as well as to exchange arbitrary data between browsers without requiring an intermediary. The set of standards that comprise WebRTC makes it possible to share data and perform teleconferencing peer-to-peer, without requiring that the user installs plug-ins or any other third-party software.

WebRTC consists of several interrelated APIs and protocols which work together to achieve this. The documentation you'll find here will help you understand the fundamentals of WebRTC, how to set up and use both data and media connections, and more.

## Interoperability

<CodeExercise id="EypMlUjZXeEKTTyrULTN" title="Sprint review">
  
  # titel
  
  **Maak een mooie functie**
  ```
  const x ='test';
  ```
  
</CodeExercise>

Because implementations of WebRTC are still evolving, and because each browser has [different levels of support for codecs](https://developer.mozilla.org/en-US/docs/Web/Media/Formats/WebRTC_codecs) and WebRTC features, you should _strongly_ consider making use of [the Adapter.js library](https://github.com/webrtcHacks/adapter) provided by Google before you begin to write your code.

Adapter.js uses shims and polyfills to smooth over the differences among the WebRTC implementations across the environments supporting it. Adapter.js also handles prefixes and other naming differences to make the entire WebRTC development process easier, with more broadly compatible results. The library is also [available as an NPM package](https://www.npmjs.com/package/webrtc-adapter).

To learn more about Adapter.js, see [Improving compatibility using WebRTC adapter.js](https://developer.mozilla.org/en-US/docs/Web/API/WebRTC_API/adapter.js).

## WebRTC concepts and usage

WebRTC serves multiple purposes; together with the [Media Capture and Streams API](https://developer.mozilla.org/en-US/docs/Web/API/Media_Streams_API), they provide powerful multimedia capabilities to the Web, including support for audio and video conferencing, file exchange, screen sharing, identity management, and interfacing with legacy telephone systems including support for sending [DTMF](https://developer.mozilla.org/en-US/docs/Glossary/DTMF "DTMF: Dual-Tone Multi-Frequency (DTMF) signaling is a system by which audible tones are used to represent buttons being pressed on a keypad. Frequently referred to in the United States as "touch tone" (after the Touch-Tone trademark used when the transition from pulse dialing to DTMF began), DTMF makes it possible to signal the digits 0-9 as well as the letters "A" through "D" and the symbols "#" and "*". Few telephone keypads include the letters, which are typically used for control signaling by the telephone network.") (touch-tone dialing) signals. Connections between peers can be made without requiring any special drivers or plug-ins, and can often be made without any intermediary servers.

Connections between two peers are represented by the [`RTCPeerConnection`](https://developer.mozilla.org/en-US/docs/Web/API/RTCPeerConnection "The RTCPeerConnection interface represents a WebRTC connection between the local computer and a remote peer. It provides methods to connect to a remote peer, maintain and monitor the connection, and close the connection once it's no longer needed.") interface. Once a connection has been established and opened using `RTCPeerConnection`, media streams ([`MediaStream`](https://developer.mozilla.org/en-US/docs/Web/API/MediaStream "The MediaStream interface represents a stream of media content. A stream consists of several tracks such as video or audio tracks. Each track is specified as an instance of MediaStreamTrack.")s) and/or data channels ([`RTCDataChannel`](https://developer.mozilla.org/en-US/docs/Web/API/RTCDataChannel "The RTCDataChannel interface represents a network channel which can be used for bidirectional peer-to-peer transfers of arbitrary data. Every data channel is associated with an RTCPeerConnection, and each peer connection can have up to a theoretical maximum of 65,534 data channels (the actual limit may vary from browser to browser).")s) can be added to the connection.

Media streams can consist of any number of tracks of media information; tracks, which are represented by objects based on the [`MediaStreamTrack`](https://developer.mozilla.org/en-US/docs/Web/API/MediaStreamTrack "The MediaStreamTrack interface represents a single media track within a stream; typically, these are audio or video tracks, but other track types may exist as well.") interface, may contain one of a number of types of media data, including audio, video, and text (such as subtitles or even chapter names). Most streams consist of at least one audio track and likely also a video track, and can be used to send and receive both live media or stored media information (such as a streamed movie).

You can also use the connection between two peers to exchange arbitrary binary data using the [`RTCDataChannel`](https://developer.mozilla.org/en-US/docs/Web/API/RTCDataChannel "The RTCDataChannel interface represents a network channel which can be used for bidirectional peer-to-peer transfers of arbitrary data. Every data channel is associated with an RTCPeerConnection, and each peer connection can have up to a theoretical maximum of 65,534 data channels (the actual limit may vary from browser to browser).") interface. This can be used for back-channel information, metadata exchange, game status packets, file transfers, or even as a primary channel for data transfer.

_**more details and links to relevant guides and tutorials needed**_

## WebRTC interfaces

Because WebRTC provides interfaces that work together to accomplish a variety of tasks, we have divided up the interfaces in the list below by category. Please see the sidebar for an alphabetical list.

### Connection setup and management

These interfaces are used to set up, open, and manage WebRTC connections. Included are interfaces representing peer media connections, data channels, and interfaces used when exchanging information on the capabilities of each peer in order to select the best possible configuration for a two-way media connection..

### Telephony

These interfaces are related to interactivity with Public-Switched Telephone Networks (PTSNs).

## Specifications

| Specification | Status | Comment |
| --- | --- | --- |
| [WebRTC 1.0: Real-time Communication Between Browsers](https://w3c.github.io/webrtc-pc/ "The 'WebRTC 1.0: Real-time Communication Between Browsers' specification") | Candidate Recommendation | The initial definition of the API of WebRTC. |
| [Media Capture and Streams](https://w3c.github.io/mediacapture-main/ "The 'Media Capture and Streams' specification") | Candidate Recommendation | The initial definition of the object conveying the stream of media content. |
| [Media Capture from DOM Elements](https://w3c.github.io/mediacapture-fromelement/ "The 'Media Capture from DOM Elements' specification") | Working Draft | The initial definition on how to obtain stream of content from DOM Elements |

In additions to these specifications defining the API needed to use WebRTC, there are several protocols, listed under [resources](https://developer.mozilla.org/en-US/docs/Web/API/WebRTC_API#Protocols).
