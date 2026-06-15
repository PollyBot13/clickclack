# ClickClack PR #26 Native Notification Proof

Proof date: 2026-06-15

This is a real Windows / Microsoft Edge notification proof for
openclaw/clickclack#26.

## Recording

Original local recording:

`clickclack-browser-notification-windows-edge-proof-2026-06-15.mp4`

Important timestamp: about 17 seconds into the clip.

At that timestamp, the Windows toast is visibly from:

- App/source: `ClickClack`
- Browser route: `via Microsoft Edge`
- Title: `PollyBot Ops in #general`
- Body: `New message`

The later Discord toast near the end is not the proof notification. It is useful
only as context that the ClickClack message was also reported back in Discord.

## Still Frames

- `01-edge-clickclack-toast-at-17s.jpg`: native Windows toast from ClickClack
  via Microsoft Edge.
- `02-click-result-channel-at-20s.jpg`: after clicking the toast,
  Microsoft Edge focuses ClickClack on `#general`, the target channel.

## Matching ClickClack Data

Message:

```text
id: msg_01kv6ewydvh1a1j2m3kqmg998g
created_at: 2026-06-15T20:17:21.339276Z
channel_id: chn_01kv60tay7c20jzw1n5qsfmj9p
channel: general
author_id: usr_01kv60w351y8j11g14jz6bgaef
author: PollyBot Ops
body: Notification proof ping from PollyBot Ops at 22:17. If this appears as a native Edge/Windows notification, click it to verify channel navigation.
```

Realtime event:

```text
id: evt_01kv6ewydwc6ftmyn9ftxbpzpq
cursor: cur_01kv6ewydwc6ftmyn9fw8rfj92
workspace_id: wsp_01kv60tay7c20jzw1n5q589d9c
channel_id: chn_01kv60tay7c20jzw1n5qsfmj9p
type: message.created
seq: 21
payload_json: {"author_id":"usr_01kv60w351y8j11g14jz6bgaef","message_id":"msg_01kv6ewydvh1a1j2m3kqmg998g"}
created_at: 2026-06-15T20:17:21.340211Z
```

Service log excerpt:

```text
2026/06/15 22:17:21 "POST http://127.0.0.1:8803/api/channels/chn_01kv60tay7c20jzw1n5qsfmj9p/messages HTTP/1.1" from 127.0.0.1:51830 - 201 1020B in 9.735542ms
2026/06/15 22:17:21 "GET http://127.0.0.1:8803/api/realtime/ws HTTP/1.1" from 127.0.0.1:60589 - 101 0B in 33m32.406402083s
2026/06/15 22:17:21 "GET http://pollybots-mac-mini.tail350339.ts.net:8803/api/channels/chn_01kv60tay7c20jzw1n5qsfmj9p/messages HTTP/1.1" from 127.0.0.1:51831 - 200 726B in 3.542375ms
2026/06/15 22:17:21 "GET http://pollybots-mac-mini.tail350339.ts.net:8803/favicon.svg HTTP/1.1" from 127.0.0.1:51831 - 200 903B in 54.708µs
2026/06/15 22:17:23 "POST http://pollybots-mac-mini.tail350339.ts.net:8803/api/channels/chn_01kv60tay7c20jzw1n5qsfmj9p/read HTTP/1.1" from 127.0.0.1:51831 - 200 165B in 6.545417ms
```

## Interpretation

The proof shows a real browser/OS notification path, not only the Playwright
fake `Notification` constructor:

1. A real ClickClack message is created through the local API.
2. The realtime `message.created` event is recorded for the same message id.
3. Windows shows a native notification sourced from ClickClack via Microsoft
   Edge.
4. Clicking the toast focuses Edge/ClickClack and lands on the target channel.

Current limitation is unchanged: this is open-browser/PWA notification behavior
after notification permission has already been granted. It is not service-worker
offline push after Edge is completely closed.
