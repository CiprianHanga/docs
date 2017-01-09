## Install Software on Linux

### RPM Packages

- when you have to choose between .deb and .rpm files, always choose .rpm files (CentOS, Fedora, etc.)
- .rpm files can be installed with zypper:

```bash
$ sudo zypper install file.rpm
```

### DEB Packages

- .deb files are not native on SUSE and needs to be converted to .rpm with the ```alien``` package (haven't tried it yet myself)

### Source Files (.tar.gz, .tar.bz2)

- download the file
- extract command for .tar.gz files:

```bash
$ tar -xzvf file.tar.gz
```

- extract command for .tar.bz2 files:

```bash
$ tar -xjvf file.tar.bz2
```

- cd <name of the new created dir>
- then:

```bash
$ ./configure
$ make
$ sudo make install
```

- if, at the ```./configure``` command, we get an error like: "The intltool scripts were not found. Please install intltool.", we should install the intltool package first (see zypper or apt-get methods for installing software)
- in case of errors consult the README or INSTALL file, which will give further explanations about the required packages, which will most likely need to be installed first
- when everything was installed, try again, or follow the steps indicated in the README/INSTALL file(s)

### Install from a .run file

- download the .run file (with browser or wget)
- run these:
```bash
$ chmod +x file.run
$ ./file.run
```
