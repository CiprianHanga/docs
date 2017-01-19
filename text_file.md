## Some Linux annoyances

### After distro upgrade, X won't start

#### Temporary Solution:

- in the boot screen, choose Advanced options
- choose the last: boot from a previous snapshot
- choose one (I chosed the oldest, first one after install)
- if successfull, when logged in open a root term and type:

```bash
snapper rollback
shutdown -r now
```

- the system will reboot and (hopefully) get into the new snapshot as default
- Note: the apps installed after that snapshot will need to be installed again (it will preserve the settings though)
- maybe rolling back to a more recent snapshot should be a better solution?


### mplayer

- search for the mvp package with YaST2
    - also get the smvp package (frontend for MVP)

### qt (needed for various packages, and good to have for development anyway)

- go to http://wiki.qt.io/
- choose your install method from list (for me: Install Qt5 on OpenSUSE)
- follow the steps

- add the repo from http://download.opensuse.org/repositories/KDE:/Qt5/<choose your release>



