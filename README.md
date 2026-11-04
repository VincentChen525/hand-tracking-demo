# hand-tracking-demo

A built copy of the starter game from [abouchedidxcode/open-source-hands](https://github.com/abouchedidxcode/open-source-hands) (MIT), hosted on GitHub Pages so it can be tested on a phone. Webcam hand tracking runs locally in the browser (MediaPipe); no video is uploaded. See LICENSE and THIRD_PARTY_NOTICES.md.

## Phone as camera

No webcam on the computer (e.g. a Mac mini)? Click **Use my phone as camera**. The page shows a QR code for `…/?phone=<room>`. Open it on the phone, tap **Start camera**, and prop the phone up in landscape facing you. The phone runs MediaPipe hand tracking itself and sends only the 21 hand landmarks per frame (screen + world coordinates, handedness, aspect, timestamp) to the computer over a WebRTC DataChannel. No video leaves the phone.

- Signalling uses the free public [PeerJS](https://peerjs.com/) cloud broker (the `peerjs` npm package, bundled). The broker only relays connection setup info (offers and ICE candidates). Landmarks travel peer-to-peer.
- ICE uses public STUN servers only, with no TURN relay. Some networks won't connect, such as strict NATs or a phone on cellular with the Mac on Wi-Fi. If that happens, put the phone and the Mac on the same Wi-Fi.
- Landmarks go over an unordered channel with no retransmits (`ordered:false, maxRetransmits:0`), and the newest frame wins.
- The phone requests a screen wake lock while streaming, where the browser supports it. iOS Safari needs the tap on **Start camera** before it will open the camera.

Licenses for bundled libraries: PeerJS (MIT), qrcode-generator (MIT).
