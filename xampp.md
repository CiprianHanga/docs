## XAMPP

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
