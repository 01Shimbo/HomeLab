
CIFS mount
1. Create the CIFS service file:
```
sudo nano /etc/systemd/system/mount-data.service
```
```
[Unit]
Description=Mount Data CIFS Share
After=network.target
Wants=network.target

[Service]
Type=oneshot
RemainAfterExit=yes
TimeoutStartSec=60
ExecStart=/usr/bin/bash -c '\
  for i in {1..30}; do \
    if ping -c1 -W1 192.168.1.54 >/dev/null 2>&1; then \
      mount -t cifs //192.168.1.54/data /mnt/ -o credentials=mnt/.SMBcredentials,vers=3.0 && exit 0; \
    fi; \
    sleep 2; \
  done; \
  echo "NAS not reachable after 60 seconds"; \
  exit 1'
ExecStop=/bin/umount /mnt/
Restart=on-failure
RestartSec=10

[Install]
WantedBy=multi-user.target
```
2. Reload and enable:
```
sudo systemctl daemon-reload
sudo systemctl enable mount-data.service
sudo systemctl start mount-data.service
sudo systemctl status mount-data.service
```

NFS mount
```
sudo nano /etc/systemd/system/mount-media.service
```
```
[Unit]
Description=Mount Media NFS Share
After=network.target
Wants=network.target

[Service]
Type=oneshot
RemainAfterExit=yes
TimeoutStartSec=60
ExecStart=/usr/bin/bash -c '\
  for i in {1..30}; do \
    if ping -c1 -W1 192.168.1.54 >/dev/null 2>&1; then \
      mount -t nfs -o nfsvers=3,proto=tcp,port=2049 192.168.1.54:fileshare /var/data && exit 0; \
    fi; \
    sleep 2; \
  done; \
  echo "NAS not reachable after 60 seconds"; \
  exit 1'
ExecStop=/bin/umount /var/data
Restart=on-failure
RestartSec=10

[Install]
WantedBy=multi-user.target
```
```
sudo systemctl daemon-reload
sudo systemctl enable mount-media.service
sudo systemctl start mount-media.service
sudo systemctl status mount-media.service\
```
