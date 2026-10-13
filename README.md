# TAKVox

**Push-to-talk voice channels and video calls for ATAK-CIV, on a media server you host yourself (LiveKit).** No cloud account, no third-party service: the plugin talks to your own server.

*Português: [README.pt-BR.md](README.pt-BR.md)*

> Independent community project. Not affiliated with or endorsed by the TAK Product Center or any TAK vendor.

## Screenshots

<p align="center">
  <img src="https://github.com/user-attachments/assets/29318dba-fe64-4a55-86b6-ad6d256c35db" width="300" alt="TAKVox screen 1">
  &nbsp;&nbsp;
  <img src="https://github.com/user-attachments/assets/cee9842b-e05c-4a73-88a0-cd4f95ce05de" width="300" alt="TAKVox screen 2">
</p>

<p align="center">
  <img src="https://github.com/user-attachments/assets/a137ed36-2d73-48a7-855b-9a4416fd444c" height="400" alt="TAKVox screen 3">
  &nbsp;&nbsp;
  <img src="https://github.com/user-attachments/assets/bca3f372-bfc8-41e0-a858-bc7c3790d884" height="400" alt="TAKVox screen 4">
</p>

## Download

Get the signed plugin from **[Releases](../../releases/latest)**: `ATAK-Plugin-TakVox-<version>--5.6.0-civ-release.apk`.
It is signed by the TAK.gov Third Party Pipeline, so it loads in the **Play Store ATAK-CIV 5.6**.

1. Install the APK on the Android device (`adb install -r <file>.apk`, or open it on the device).
2. Open ATAK, accept the plugin load prompt, then open **TAKVox** from the toolbar.
3. In **Settings**, enter your token server URL and tap **Test**.

> If you previously installed a locally built (debug) version, uninstall it first: the signature is different.

You also need the server. See **[server/](server/)** for the LiveKit and token-service setup.

## Features

- **Voice channels.** Each channel is a LiveKit room. Fixed channels are defined by the server admin, with optional per-user restrictions and a voice-only option.
- **Create and delete channels from the app.** Team-created channels can have an optional password. Open channels can be deleted by any authenticated user after a confirmation; protected channels ask for the channel password. Fixed channels cannot be deleted from the app.
- **Two modes:** classic **PTT** (hold or toggle) and **Call** mode (open microphone plus video, like a video call).
- **Floating PTT button** over the map, so you can keep using the map while talking. Drag up to lock continuous transmission, drag down to unlock.
- **Hardware PTT:** physical or Bluetooth buttons through intents, including rugged-device PTT actions configured in Settings without recompiling.
- **Live roster** showing who is speaking. Tap a participant to center the map on their marker.
- **Video:** publishes the camera (rear by default, switchable), shows other participants' video on a stage with a self-view, and a participant strip.
- **No extra login.** The plugin reuses the **client certificate ATAK already has** for your TAK server (mutual TLS). A shared team key is also supported for quick tests.

## How it works

```
 ATAK + TAKVox --HTTPS + TAK client certificate--> token-service --(JWT)--+
        |                                                                 |
        +------------- WebRTC (audio/video) -----------------> LiveKit <--+
```

The token service only issues an access token to someone who presents a certificate signed by your TAK server's CA. It never talks to the TAK server, so it does not depend on which TAK server you run. It has been tested with OpenTAKServer.

## Known limitations

- **Background microphone:** with ATAK minimized or the screen off, Android 14+ may silence transmission. Receiving works; keep ATAK on screen to talk.
- **Pre-warmed microphone** (default) keeps Android's mic indicator on while connected, but the track stays published muted. Disable it in Settings if you prefer.
- **Identity:** anyone with a valid certificate can claim another user's `uid` and drop them from the room. Fine for a trusted team, not for hostile environments.
- **Network:** the phone must reach your LiveKit server (VPN or public IP). Multicast is not used.
- **Not included:** thermal/UVC camera, text chat, private calls, speaking indicator on the map marker.

## Requirements
- ATAK-CIV 5.6 on Android, microphone (and camera for video).
- A LiveKit server and the token service from [server/](server/).

## Support the project

If TAKVox is useful to you, you can support its development:

[![Buy Me a Coffee](https://img.shields.io/badge/Buy%20Me%20a%20Coffee-donate-FFDD00?logo=buymeacoffee&logoColor=black)](https://buymeacoffee.com/tiagoufbao)

Or with a Pix donation (Brazil): `tiagoufba@gmail.com`

## Terms of use
The plugin is distributed free of charge, as a binary only. The plugin source code is not published. You may use it, but you may not redistribute, modify or resell it without permission. Provided "as is", without warranty. ATAK and its SDK are subject to their own terms. The server kit in [server/](server/) has its own license.

## Third-party software
The plugin includes the **LiveKit Android SDK** (Apache-2.0, https://github.com/livekit/client-sdk-android) and **WebRTC** (BSD-3-Clause). Source code is available at those links.
