# Innovative Applications with Bluetooth® Mesh

## Support Material and Sample Codes

Copyright (C) 2018 Intel

### Foreword

The code samples located here are written to support Intel's ["Innovative
Applications with Bluetooth® Mesh"](https://2018.hackjunction.com/challenges/applications-with-bluetooth-mesh)
challenge at [Junction 2018](https://2018.hackjunction.com/).

Intel provides an [experimental branch of bluez stack](https://github.com/intel/Junction2018/tree/master)
for the Junction participants and anyone else interested in the leading edge of
Bluetooth Mesh development.

The samples are meant to give ideas for developers. They shall not be considered
fully completed, production ready projects.

Happy Hacking!

## Setup

We can host 8 teams. Each team will get a NUC with Linux and 4 reel boards. 

NUC: linux, meshd and python sample

Reel Board: Zephyr, BT mesh app with capabilities to send / receive text messages, blink led and read tempereture. 

We have our own WiFi where you join (`jnct-ble5-mesh`). You should use ssh to connect to the NUC. For example `ssh team1`

### Support Materials

#### How to Build BlueZ with mesh support

```bash
git clone https://github.com/intel/Junction2018.git
cd Junction2018
./bootstrap-configure --disable-android --disable-systemd --disable-midi --disable-obex --disable-avrcp --disable-cups --disable-network --disable-a2dp --disable-hid --disable-hog
```

Also 
```bash
git clone https://git.kernel.org/pub/scm/libs/ell/ell.git
git reset --hard 0.13
make && make install
```

If you are on Ubuntu you may need to fix `mesh/dbus.c` by hand.

### Samples



