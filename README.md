### service:
```
#/etc/systemd/system/fix-mouse-lag.service

[Unit]
Description=run root script at boot/wake to fix mouse lag
Before=bluetooth.service

[Service]
Type=oneshot
RemainAfterExit=yes
ExecStart=/usr/bin/sleep 2
ExecStart=/usr/local/bin/fix-mouse-lag.sh

[Install]
WantedBy=bluetooth.service
```


### script:
```
#!/bin/sh

#/usr/local/bin/fix-mouse-lag.sh
#script run by fix-mouse-lag.service at boot/wake

echo 0 > /sys/kernel/debug/bluetooth/hci0/conn_latency
echo 6 > /sys/kernel/debug/bluetooth/hci0/conn_min_interval
echo 7 > /sys/kernel/debug/bluetooth/hci0/conn_max_interval

```
### Run

```
sudo chown root:root /etc/systemd/system/fix-mouse-lag.service
sudo chmod a+rx /usr/local/bin/fix-mouse-lag.sh
sudo systemctl enable fix-mouse-lag.service --now
```
