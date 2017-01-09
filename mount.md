## Mounting Partitions and External Drives in openSUSE

### NTFS Partitions

- ATM configuration:
    - C:\ located at ```/dev/sda2```
    - D:\ located at ```/dev/sda5```
    - F:\ located at ```/dev/sdc1```
    - M:\ located at ```/dev/sdb1```

- command format: ```mount -t ntfs-3g   /dev/sdb1   /path_to/mount_point```
- run these:

```bash
sudo mount -t ntfs-3g /dev/sda5 /run/media/ciprian/D
sudo mount -t ntfs-3g /dev/sdb1 /run/media/ciprian/F
sudo mount -t ntfs-3g /dev/sdc1 /run/media/ciprian/M
```

- add these to your fstab file (```$ sudo vim /etc/fstab```)

```bash
/dev/sda5   /run/media/ciprian/D    ntfs-3g defaults    0 0
/dev/sdb1   /run/media/ciprian/F    ntfs-3g defaults    0 0
/dev/sdc1   /run/media/ciprian/M    ntfs-3g defaults    0 0
```

