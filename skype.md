### Skype problems

- type on chat:

```bash
/dumpmsnp
```

The output should look like:

```bash
[2:08:43 PM] System: MSNP: Connection Data:
 * Status: LoggedOut
 * Server Current: s.gateway.messenger.live.com
 * Server Saved:   s.gateway.messenger.live.com
 * Login:  (UIC)
 * Skypename: ciprian.hanga
 * EPID: 03efb476-2475-c552-9ec8-50d2e0c2a182
 * ClientVersion: 2/4.3.0.37/173
 * OSVersion: Linux 4.1.36-41-defa
 * B: 0, T: 0, CFR: 0
 * Time: TZ: UTC+2, Server: 0, Local: 1481890126
 * Push: None (Unregistered)
```

- try:

```bash
/msnp24
```

- then restart Skype, it should work

