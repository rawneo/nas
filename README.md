# Script for NAS setup on Raspberry Pi 5

```
$ sudo apt update
$ sudo apt full-upgrade
```

## PCIe update ##
```
$ sudo vi /boot/firmware/config.txt
```
> Add following lines
```
[all]
dtparam=pciex1
dtparam=pciex1_gen=3
```

## For oled display ##
```
$ sudo apt install i2c-tools
$ sudo usermod -a -G i2c,spi,gpio pi
$ sudo apt install python3-dev python3-pip python3-numpy libfreetype6-dev libjpeg-dev build-essential
$ sudo apt install libsdl2-dev libsdl2-image-dev libsdl2-mixer-dev libsdl2-ttf-dev libportmidi-dev
$ git clone https://github.com/rm-hull/luma.examples.git luma
$ cd luma
$ sudo apt install -y python3-virtualenv
$ virtualenv venv
$ source venv/bin/activate
$ python3 -m pip install -e .
$ pip install psutil

```
$ vi nas.py
> Copy the nas.py code here. Save and Exit

## Test the python script
```
$ python3 nas.py
```

### Create Service file
```
$ sudo vi /etc/systemd/system/nas.service

```
> Add following lines
```
[Unit]
Description=NAS Display service
After=network.target

[Service]
Type=simple
ExecStart=/home/pi/luma/venv/bin/python3 /home/pi/luma/nas.py
Restart=always
RestartSec=5
TimeoutSec=60
RuntimeMaxSec=infinity
PIDFile=/tmp/nas.pid

[Install]
WantedBy=multi-user.target
```
> Save and Exit

```
$ sudo chmod 644 /etc/systemd/system/nas.service
$ sudo systemctl daemon-reload
$ sudo systemctl enable nas.service 
$ sudo systemctl start nas.service 
$ sudo systemctl status nas.service
```

## To Debug
```
$ journalctl -u nas.service -f
```
