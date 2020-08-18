# Random Notes

## Ubuntu 20.04 disk performance

- Initial with default external USB 3.0 4TB drive FS (fat) on Raspberry Pi 4 (2GB)

```
root@home-0:/mnt/foo# dd oflag=direct  if=/dev/zero of=./tmp.file5 oflag=direct bs=128k count=1
1+0 records in
1+0 records out
131072 bytes (131 kB, 128 KiB) copied, 0.00239885 s, 54.6 MB/s
root@home-0:/mnt/foo# dd oflag=direct  if=/dev/zero of=./tmp.file5 oflag=direct bs=128k count=1
1+0 records in
1+0 records out
131072 bytes (131 kB, 128 KiB) copied, 0.00170114 s, 77.0 MB/s
root@home-0:/mnt/foo# dd oflag=direct  if=/dev/zero of=./tmp.file5 oflag=direct bs=128k count=1
1+0 records in
1+0 records out
131072 bytes (131 kB, 128 KiB) copied, 0.00140573 s, 93.2 MB/s
root@home-0:/mnt/foo# dd oflag=direct  if=/dev/zero of=./tmp.file5 oflag=direct bs=128k count=1
1+0 records in
1+0 records out
131072 bytes (131 kB, 128 KiB) copied, 0.00218718 s, 59.9 MB/s

/dev/sda2:
 Timing cached reads:   1602 MB in  2.00 seconds = 801.54 MB/sec
 Timing buffered disk reads: 408 MB in  3.01 seconds = 135.50 MB/sec
root@home-0:/mnt/foo# hdparm -Tt /dev/sda2

/dev/sda2:
 Timing cached reads:   1622 MB in  2.00 seconds = 811.78 MB/sec
 Timing buffered disk reads: 408 MB in  3.00 seconds = 135.95 MB/sec
```

- ZFS no encryption

```
root@home-0:/tank/test# dd oflag=direct  if=/dev/zero of=./tmp.file oflag=direct bs=128k count=1; rm ./tmp.file
1+0 records in
1+0 records out
131072 bytes (131 kB, 128 KiB) copied, 0.00111683 s, 117 MB/s
root@home-0:/tank/test# dd oflag=direct  if=/dev/zero of=./tmp.file oflag=direct bs=128k count=1; rm ./tmp.file
1+0 records in
1+0 records out
131072 bytes (131 kB, 128 KiB) copied, 0.00173008 s, 75.8 MB/s
root@home-0:/tank/test# dd oflag=direct  if=/dev/zero of=./tmp.file oflag=direct bs=128k count=1; rm ./tmp.file
1+0 records in
1+0 records out
131072 bytes (131 kB, 128 KiB) copied, 0.00120526 s, 109 MB/s
root@home-0:/tank/test# dd oflag=direct  if=/dev/zero of=./tmp.file oflag=direct bs=128k count=1; rm ./tmp.file
1+0 records in
1+0 records out
131072 bytes (131 kB, 128 KiB) copied, 0.00122289 s, 107 MB/s



```

- DD WRITE/READ SPEED

```
dd if=/dev/zero of=./tmp.file oflag=direct bs=128k count=10000
sysctl vm.drop_caches=3; dd if=./tmp.file of=/dev/null bs=128k count=10000

```

### fat

- write = 142, 147, 160 MB/s
- read = 134, 137, 133 MB/s

### zfs default

- read = 120, 131, 127 MB/s
- write = 69.2, 50.0, 56.0 MB/s

### zfs native encrypted

- write = 31.6, 32.0, 32.3 MB/s
- read = 31.3, 30.7, 30.3 MB/s

### ext 4

- write = 125, 127, 131 MB/s
- read = 139, 137, 138 MB/s

### xfs

- write = 133, 132, 135 MB/s
- read = 143, 142, 142 MB/s

### btrfs

- write = 134, 134, 130 MB/s
- read = 136, 142, 142 MB/s

#### ext 4 encrypted (Luks)

- TBD
