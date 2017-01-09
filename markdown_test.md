### Markdown test

#### Add Repos

To add software repos using zypper:
```bash
# zypper ar -f <URL> <alias>
```
- it is required that you add an alias after the URL part--add a short keyword
- you have to have root privileges (sudo or su)

### Viewing markdown files in Linux

- install pandoc
- install lynx
- run:
```bash
# pandoc file.md | lynx -stdin
```

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

### *.run files

- run these commands:

```bash
chmod 755 <filename>
ls -la // check it to be executable for the owner
sudo ./<filename>
```

### XAMPP

- default installation for xampp on Linux: `opt/lampp`
- start the service: `sudo opt/lampp/lampp start`
- stop the service: `sudo opt/lampp/lampp stop`
- start the GUI:

```bash
cd /opt/lampp
sudo ./manager-linux.run (or manager-linux-x64.run)
```

- uninstall
	- first, stop the service (see above)
	- then: `sudo rm -rf /opt/lampp`

### Sandbox Installation



