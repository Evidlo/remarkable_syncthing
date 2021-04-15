# Syncthing on reMarkable

A guide for setting up Syncthing on a reMarkable.

## Installation

0. Install syncthing on the host machine and get the computer's id.  You must also have [Entware](http://github.com/evidlo/remarkable_entware) installed on the reMarkable.

1. Install syncthing on the reMarkable and start up the configuration GUI.

``` bash
opkg install syncthing
syncthing -gui-address "http://0.0.0.0:8888" -no-restart

```

2. Browse to the reMarkable Syncthing interface at `http://10.11.99.1:8888`

3. Add folders to sync.  I chose `/home/root/books` for syncing documents to [Plato](http://github.com/darvin/plato).  `/usr/share/remarkable/` and `/home/root/.local/share/remarkable/xochitl/` may also be of interest.

4. Add a remote device (your computer).  Syncthing may automatically detect your computer's id.  Under the `Sharing` tab, enable `Auto Accept` and check the folder you previously created.

5. Browse to `http://localhost:8080` and wait for a prompt to connect to the reMarkable and accept.  This took about 30s to appear.

6. Wait for another prompt to receive the shared folder and accept.  Verify that folder syncing works.

7. Copy `syncthing.service` to `/etc/systemd/system/` then start and enable the service

``` bash
wget https://raw.githubusercontent.com/Evidlo/remarkable_syncthing/master/syncthing.service -O /etc/systemd/system/syncthing.service
systemctl daemon-reload
systemctl start syncthing
systemctl enable syncthing
```
## Caveats

- At least one of the devices must be publically accessible on TCP 22000.  Alternatively, if at least one of the devices can access the other on local intranet, you can add another discovery method via `[Remote Device] > Edit > Advanced > Addresses`.. eg: `tcp://192.168.1.123:22000`

- WiFi needs to be on an connected for this to work, so xochitl should be running.
