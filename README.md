# Backup of Raspberry PI Homebridge

## Installing Service
Create file
```bash
sudo nano /etc/default/homebridge-{name}
```
with the following contents:
```bash
HOMEBRIDGE_OPTS=-U /home/pi/.homebridge-{name}
# DEBUG=*
```

Create another file
```bash
sudo nano /etc/systemd/system/homebridge-{name}.service
```
with the following contents
```bash
[Unit]
Description=Homebridge-{name}
After=syslog.target network-online.target

[Service]
Type=simple
User=homebridge
EnvironmentFile=/etc/default/homebridge-{name}
ExecStart=homebridge $HOMEBRIDGE_OPTS
Restart=on-failure
RestartSec=10
KillMode=process

[Install]
WantedBy=multi-user.target
```

Then run these commands:
```bash
sudo chmod -R 0777 /home/pi/.homebridge-{name}
sudo systemctl daemon-reload
sudo systemctl enable homebridge-{name}
sudo systemctl start homebridge-{name}
```

## Cron Reboot
```bash
sudo nano /home/pi/reboot.sh
```
with the following contents:
```bash
reboot
```
then run 
```bash
sudo chmod -R 0777 /home/pi/reboot.sh
```
then add it to cron with
```bash
crontab -e
```
and add line
```bash
# 4am every Sunday
0 4 * * 0  /home/pi/reboot.sh
```
and then verify changes with
```bash
crontab -l
```

## Commands
Restarting service:
```bash
sudo systemctl restart homebridge-{name}.service
```

Logs:
```bash
journalctl -f -u homebridge-{name}.service
```

Running Homebridge manually:
```bash
homebridge -U ~/.homebridge-{name}/ -Q -R -D
```