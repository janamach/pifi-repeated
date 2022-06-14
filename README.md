# pifi-repeated: setting up RPi with TP-Link T4U (rtl88x2bu)

The information below was collected here:

https://github.com/morrownr/88x2bu-20210702.git

https://github.com/swkim01/waveshare-dtoverlays

https://spotpear.com/index/study/detail/id/141.html

When using 4" LCD touchscreen may not behave properly. To fix this, the following worked for me:
```
# With jessie (xy swap):
xinput set-prop "ADS7846 Touchscreen" "Coordinate Transformation Matrix" 0 1 0 1 0 0 0 0 1
```
```
# With bullseye (y inverted):
xinput set-prop "ADS7846 Touchscreen" "Coordinate Transformation Matrix" 1 0 0 0 -1 1 0 0 1
```
To enable numlock on boot, do this:
```
sudo apt install -y numlockx
echo "numlockx on" >> .profile
```

Disable the native wifi and bluetooth by adding the following to config.txt:

```
dtoverlay=pi3-disable-wifi
dtoverlay=pi3-disable-bt
```


Driver installation:
```
sudo apt update
sudo apt install -y raspberrypi-kernel-headers bc build-essential dkms git numlockx dnsmasq hostapd

mkdir ~/src
cd ~/src
git clone https://github.com/morrownr/88x2bu-20210702.git

cd ~/src/88x2bu-20210702

./cmode-on.sh # Enable concurrent mode for wifi repeater

./ARM_RPI.sh # On ARM 32 bit
sudo ./install-driver.sh # install driver with dkms
```
Then follow this instruction:
https://pimylifeup.com/raspberry-pi-wifi-extender/

with `nohook wpa_supplicant` in `step 6`.

## Issues

After following the instruction, `Failed to start hostapd.service: Unit hostapd.service is masked.` is possible. Then do:

```
sudo systemctl unmask hostapd
```

`hostapd` and `dnsmasq` do not seem to start on boot on Raspberry Pi 3, workaround:
```
crontab -e
```
and

```
@reboot sleep 10 && sudo service hostapd start
@reboot sleep 20 && sudo service dnsmasq start
```
