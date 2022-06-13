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

Driver installation:
```
sudo apt install -y raspberrypi-kernel-headers bc build-essential dkms git numlockx

mkdir ~/src
cd ~/src
git clone https://github.com/morrownr/8812au-20210629.git
cd ~/src/8812au-20210629

./cmode-on.sh # Enable concurrent mode for wifi repeater

./ARM_RPI.sh # On ARM 32 bit
sudo ./install-driver.sh # install driver with dkms
```
Useful docs here: https://github.com/morrownr/8812au-20210629/tree/main/docs
