## Vagrant Linux Setup

[Source](https://www.youtube.com/watch?v=crvJ2C7Hr_g)

- Vagrant works with other VM software such as VirtualBox, VMWare, AWS, etc.
- Vagrant calls them "providers"

### First Steps

1. Install Vagrant
2. Install VirtualBox

- after installation, run:

```bash
$ vagrant init hashicorp/precise64
$ vagrant up
```

**Note:** If you get an error, check the on-screen info on how to fix it, and then run ```vagrant up``` again.

**Explanation**

- the first command ```vagrant init``` creates a file in the current directory, named simply ```Vagrantfile``` (no extension)
- the ```hashicorp/precise64``` bit creates a line in this Vagrantfile ```config.vm.box = "hashicorp/precise64"``` which tells vagrant what *box* to use
	- *boxes* are a key concept in Vagrant, being the name of the software server that is going to be used for the VM
- ```vagrant up``` command actually goes on [Atlas](https://atlas.hashicorp.com/) and downloads the indicated box, so it could take a while
- after downloading, some automated initial setup steps are performed (guest addition installation for VirtualBox, port forwarding, setting up shared folders, etc.) and the VM is then booted up
- the newly created machine can then be *destroyed* with:

```bash
$ vagrant destroy
```

> **[Cip's Personal Note]**: compare these ```vagrant up``` / ```vagrant ssh``` with the long and cumbersome method of setting up your own Sandbox using the Jon Peck method. (Although that one is still very informative and useful).

- the files downloaded from Atlas are stored in ```~/.vagrant.d/boxes``` folder, located in the ```/home/user``` directory
- the actual VM is located in ```/VirtualBox VMs``` folder
- *destroying* a VM removes it from that folder, but the "box" downloaded from Atlas is NOT removed from the ```./vagrant.d``` folder
	- so if you want to create another VM using the precise64 box, the files won't need to be downloaded again
- Note: boxes *might* need to be downloaded if you decide on using a different provider (such as hyperv, or vmware_fusion) (TODO: verify)
- to completely remove the box file, you can use the ```vagrant box remove``` command


### Project Setup

- as it was said, ```vagrant init``` creates a new Vagrantfile
- the purpose of the Vagrantfile is twofold:
	1. It marks the root directory of your project. Many of the configuration options in Vagrant are relative to this root directory.
	2. Describe the kind of machine and resources you need to run your project, as well as what software to install and how you want to access it.
- that being said, it makes perfect sense to create a dedicated folder for your project:

```bash
$ mkdir vagrant_getting_started
```

- the Vagrant file is meant to be committed to version control with your project -- this way, every person working with that project can benefit from Vagrant without any upfront work


### Boxes

- new boxes can be added with the command:

```bash
$ vagrant box add hashicorp/precise64
```

- Note: if you already added the box you will get an error: 

```
"The box you're attempting to add already exists. Remove it before
adding it again or add it with the `--force` flag."
```

- find more boxes on [HashiCorp's Atlas box catalog](https://atlas.hashicorp.com/boxes/search)
- notice that the boxes you'll find are suited to different platforms and technologies (providers)

- in the above command, you will notice that boxes are namespaced. Boxes are broken down into two parts - the username and the box name - separated by a slash. In the example above, the username is "hashicorp", and the box is "precise64".

#### Using a Box

- if you already did the steps above, then you already have the hashicorp/precise64 box available
- cd into your new ```vagrant_getting_started``` folder
- run the ```vagrant init``` command
- now you have a Vagrantfile, open it for editing
- in the ```config.vm.box = "base"``` line, change the ```"base"``` part with ```"hashicorp/precise64"```
- now you can run ```vagrant up```
- Vagrant will boot up your new VM
	- you won't see anything, because the VM is running without a GUI
- when you get back the control of the prompt, you can ssh into your VM with ```vagrant ssh```:

```bash
ciprian@linux-3uc4:~/vagrant_getting_started> vagrant ssh
Welcome to Ubuntu 12.04 LTS (GNU/Linux 3.2.0-23-generic x86_64)

 * Documentation:  https://help.ubuntu.com/
New release '14.04.5 LTS' available.
Run 'do-release-upgrade' to upgrade to it.

Welcome to your Vagrant-built virtual machine.
Last login: Fri Sep 14 06:23:18 2012 from 10.0.2.2
vagrant@precise64:~$ 
```

- you are now logged into your new VM

> *Warning:* do not run the ```rm -rf /``` command, it will remove everything inside the shared folder

- you can log out of the VM and terminate the SSH session with ```Ctrl + D```


### Synced Folders

- by default Vagrant shares your project directory (```/vagrant_getting_started```) to the ```/vagrant``` directory in your guest
- you can check this on your VM running: ```$ ls /vagrant``` which will show the actual Vagrantfile you have on your project folder on your host machine
- this way you can create files on your project folder and have them automatically available in the guest VM


### Provisioning

- Vagrant has built-in support for *automated provisioning*
- using this feature, Vagrant will automatically install software when you ```vagrant up``` so that the guest machine can be repeatably created and ready-to-use

#### Installing Apache

- create a new file in the project folder, named ```bootstrap.sh```, with the following content:

```bash
#!/usr/bin/env bash

apt-get update
apt-get install -y apache2
if ! [ -L /var/www ]; then
  rm -rf /var/www
  ln -fs /vagrant /var/www
fi
```

- now we configure Vagrant to run this shell script when setting up our machine
- in the Vagrantfile, after the ```config.vm.box``` line add a new line with the following setting: ```config.vm.provision :shell, path: "bootstrap.sh"```

- now the ```vagrant up``` command can be run, and it will use the provisioning file we've supplied
- but because our VM is still running, we'll use a different command, which will re-initialize the VM skipping the import step but running the provisioners: ```vagrant reload --provision```

- after this we can watch how the apache2 server is being installed in the VM
- this step also changes the default ```/var/www``` of apache with the ```/vagrant``` folder, so we can use the project folder as the www folder of our VM
- the web server is up and running, and we can test this running the following command on the VM:

```bash
vagrant@precise64:~$ wget -qO- 127.0.0.1
```

- this will output the source code of the Apache first run page, proving the server is up and running


### Networking

- the server is up and running but we want to be able to access it from the browser as well
- for this we need to set up the port forwarding

- we need to *uncomment* the ```config.vm.network``` line in the Vagrantfile by removing the ```#``` character in front of it
- this is how it should look:

```
config.vm.network :forwarded_port, guest: 80, host: 8080
```

- if it doesn't look like this, correct it by removing ```"``` chars and adding the ```:```
- in doubt, [check the documentation page](https://www.vagrantup.com/docs/getting-started/networking.html)


- now we run ```vagrant up``` (or ```vagrant reload```, if the VM is already running)
- opening 127.0.0.1:8080 in the broser should list the content of the ```/vagrant_getting_started``` folder, but as seen from the VM (where it is the ```/vagrant``` folder)

> **Note:** If opening the 127.0.0.1:8080 in browser on host makes the browser download the index.php file instead of rendering it, it's because Apache is not yet set up for php support on the VM

> **[Cip's Personal Note]**: To get rid of LOTS of issue (such as manually setting up a server), or if you are in a hurry and just want a working server ASAP, use [this tip](https://www.sitepoint.com/quick-tip-get-homestead-vagrant-vm-running/) from the good people over at [Sitepoint](www.sitepoint.com). It will provide a server with the latest technologies up and running. The provisioning part will take a while at the first run, but after that is really easy. Check out [my notes](./homestead.md) about it.

### Addenda

The full output of my first experience with Vagrant:

```bash
ciprian@linux-3uc4:~> vagrant --version
Vagrant 1.9.1
ciprian@linux-3uc4:~> vagrant init hashicorp/precise64
A `Vagrantfile` has been placed in this directory. You are now
ready to `vagrant up` your first virtual environment! Please read
the comments in the Vagrantfile as well as documentation on
`vagrantup.com` for more information on using Vagrant.
ciprian@linux-3uc4:~> vagrant up
VirtualBox is complaining that the kernel module is not loaded. Please
run `VBoxManage --version` or open the VirtualBox GUI to see the error
message which should contain instructions on how to fix this error.
ciprian@linux-3uc4:~> VBoxManage --version
WARNING: The vboxdrv kernel module is not loaded. Either there is no module
         available for the current kernel (4.4.36-8-default) or it failed to
         load. Please recompile the kernel module and install it by

           sudo /sbin/vboxconfig

         You will not be able to start VMs until this problem is fixed.
5.1.12r112440
ciprian@linux-3uc4:~> sudo /sbin/vboxconfig
root's password:
vboxdrv.sh: Building VirtualBox kernel modules.
This system is not currently set up to build kernel modules (system extensions).
Running the following commands should set the system up correctly:

  zypper install kernel-default-devel-4.4.36-8.1.x86_64
(The last command may fail if your system is not fully updated.)
  zypper install kernel-default-devel
vboxdrv.sh: failed: Look at /var/log/vbox-install.log to find out what went wrong.
This system is not currently set up to build kernel modules (system extensions).
Running the following commands should set the system up correctly:

  zypper install kernel-default-devel-4.4.36-8.1.x86_64
(The last command may fail if your system is not fully updated.)
  zypper install kernel-default-devel

There were problems setting up VirtualBox.  To re-start the set-up process, run
  /sbin/vboxconfig
as root.
ciprian@linux-3uc4:~> 
ciprian@linux-3uc4:~> sudo zypper install kernel-default-devel-4.4.36-8.1.x86_64
Loading repository data...
Reading installed packages...
Resolving package dependencies...

The following 3 NEW packages are going to be installed:
  kernel-default-devel kernel-devel kernel-macros

3 new packages to install.
Overall download size: 15.9 MiB. Already cached: 0 B. After the operation, additional 55.2 MiB
will be used.
Continue? [y/n/? shows all options] (y): 
Retrieving package kernel-macros-4.4.36-8.1.noarch          (1/3),   1.1 MiB (  6.3 KiB unpacked)
Retrieving: kernel-macros-4.4.36-8.1.noarch.rpm .............................[done (616.4 KiB/s)]
Retrieving package kernel-devel-4.4.36-8.1.noarch           (2/3),  11.3 MiB ( 51.7 MiB unpacked)
Retrieving: kernel-devel-4.4.36-8.1.noarch.rpm ..............................[done (859.5 KiB/s)]
Retrieving package kernel-default-devel-4.4.36-8.1.x86_64   (3/3),   3.4 MiB (  3.6 MiB unpacked)
Retrieving: kernel-default-devel-4.4.36-8.1.x86_64.rpm ......................[done (903.7 KiB/s)]
Checking for file conflicts: ..............................................................[done]
(1/3) Installing: kernel-macros-4.4.36-8.1.noarch .........................................[done]
(2/3) Installing: kernel-devel-4.4.36-8.1.noarch ..........................................[done]
(3/3) Installing: kernel-default-devel-4.4.36-8.1.x86_64 ..................................[done]
ciprian@linux-3uc4:~> sudo zypper install kernel-default-devel
Loading repository data...
Reading installed packages...
'kernel-default-devel' is already installed.
No update candidate for 'kernel-default-devel-4.4.36-8.1.x86_64'. The highest available version is already installed.
Resolving package dependencies...

Nothing to do.
ciprian@linux-3uc4:~> sudo /sbin/vboxconfig
vboxdrv.sh: Building VirtualBox kernel modules.
vboxdrv.sh: Starting VirtualBox services.
ciprian@linux-3uc4:~> vagrant up
Bringing machine 'default' up with 'virtualbox' provider...
==> default: Box 'hashicorp/precise64' could not be found. Attempting to find and install...
    default: Box Provider: virtualbox
    default: Box Version: >= 0
==> default: Loading metadata for box 'hashicorp/precise64'
    default: URL: https://atlas.hashicorp.com/hashicorp/precise64
==> default: Adding box 'hashicorp/precise64' (v1.1.0) for provider: virtualbox
    default: Downloading: https://atlas.hashicorp.com/hashicorp/boxes/precise64/versions/1.1.0/providers/virtualbox.box
==> default: Successfully added box 'hashicorp/precise64' (v1.1.0) for 'virtualbox'!
==> default: Importing base box 'hashicorp/precise64'...
==> default: Matching MAC address for NAT networking...
==> default: Checking if box 'hashicorp/precise64' is up to date...
==> default: Setting the name of the VM: ciprian_default_1483446133167_15033
==> default: Clearing any previously set network interfaces...
==> default: Preparing network interfaces based on configuration...
    default: Adapter 1: nat
==> default: Forwarding ports...
    default: 22 (guest) => 2222 (host) (adapter 1)
==> default: Booting VM...
==> default: Waiting for machine to boot. This may take a few minutes...
    default: SSH address: 127.0.0.1:2222
    default: SSH username: vagrant
    default: SSH auth method: private key
    default: 
    default: Vagrant insecure key detected. Vagrant will automatically replace
    default: this with a newly generated keypair for better security.
    default: 
    default: Inserting generated public key within guest...
    default: Removing insecure key from the guest if it's present...
    default: Key inserted! Disconnecting and reconnecting using new SSH key...
==> default: Machine booted and ready!
==> default: Checking for guest additions in VM...
    default: The guest additions on this VM do not match the installed version of
    default: VirtualBox! In most cases this is fine, but in rare cases it can
    default: prevent things such as shared folders from working properly. If you see
    default: shared folder errors, please make sure the guest additions within the
    default: virtual machine match the version of VirtualBox you have installed on
    default: your host and reload your VM.
    default: 
    default: Guest Additions Version: 4.2.0
    default: VirtualBox Version: 5.1
==> default: Mounting shared folders...
    default: /vagrant => /home/ciprian
ciprian@linux-3uc4:~> vagrant destroy
    default: Are you sure you want to destroy the 'default' VM? [y/N] y
==> default: Forcing shutdown of VM...
==> default: Destroying VM and associated drives...
ciprian@linux-3uc4:~> 
```



