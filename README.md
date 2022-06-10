# OneShot for Termux with vibro/sound notification

## [For sound notification line #1007 must be edited by yourself "audio/file"](https://github.com/jevgenyonegin/OneShot_Termux_light_mod/blob/0d220cf7c127fd4f844149aeea85e5f414220979/oneshot.py#L1007)
Termux: nano -l +1007 oneshot.py > make changes > Ctrl+o > Ctrl+x

## This method not working on MIUI roms (or maybe some others), so there is another way to at least play sound
```
pkg install play-audio
```
Edit line [#1007](https://github.com/jevgenyonegin/OneShot_Termux_light_mod/blob/0d220cf7c127fd4f844149aeea85e5f414220979/oneshot.py#L1007) replacing "termux-media-player play" with "play-audio"

As example:
```
proc = subprocess.Popen('play-audio knock.ogg', shell=True)
```
# Overview
**OneShot** performs [Pixie Dust attack](https://forums.kali.org/showthread.php?24286-WPS-Pixie-Dust-Attack-Offline-WPS-Attack) without having to switch to monitor mode.
# Features
 - [Pixie Dust attack](https://forums.kali.org/showthread.php?24286-WPS-Pixie-Dust-Attack-Offline-WPS-Attack);
 - integrated [3WiFi offline WPS PIN generator](https://3wifi.stascorp.com/wpspin);
 - [online WPS bruteforce](https://sviehb.files.wordpress.com/2011/12/viehboeck_wps.pdf);
 - Wi-Fi scanner with highlighting based on iw;
# Requirements
 - Python 3.6 and above;
 - [Wpa supplicant](https://www.w1.fi/wpa_supplicant/);
 - [Pixiewps](https://github.com/wiire-a/pixiewps);
 - [iw](https://wireless.wiki.kernel.org/en/users/documentation/iw).
# Setup

## [Termux](https://f-droid.org/en/packages/com.termux/)
Please note that root access is required.  

**Installing requirements**
 ```
 pkg install -y root-repo
 pkg install -y git tsu python wpa-supplicant pixiewps iw
 ```
**Getting OneShot**
 ```
 git clone --depth 1 https://github.com/jevgenyonegin/OneShot_Termux_mod OneShot
 ```
#### Running
 ```
 sudo python OneShot/oneshot.py -i wlan0 --iface-down -K
 ```

# Usage
```
 oneshot.py <arguments>
 Required arguments:
     -i, --interface=<wlan0>  : Name of the interface to use

 Optional arguments:
     -b, --bssid=<mac>        : BSSID of the target AP
     -p, --pin=<wps pin>      : Use the specified pin (arbitrary string or 4/8 digit pin)
     -K, --pixie-dust         : Run Pixie Dust attack
     -B, --bruteforce         : Run online bruteforce attack

 Advanced arguments:
     -d, --delay=<n>          : Set the delay between pin attempts [0]
     -w, --write              : Write AP credentials to the file on success
     -F, --pixie-force        : Run Pixiewps with --force option (bruteforce full range)
     -X, --show-pixie-cmd     : Alway print Pixiewps command
     --vuln-list=<filename>   : Use custom file with vulnerable devices list ['vulnwsc.txt']
     --iface-down             : Down network interface when the work is finished
     -l, --loop               : Run in a loop
     -v, --verbose            : Verbose output
     --pbc                    : Run WPS push button connection
 ```

## Usage examples
Start Pixie Dust attack on a specified BSSID:
 ```
 sudo python3 oneshot.py -i wlan0 -b 00:90:4C:C1:AC:21 -K
 ```
Show avaliable networks and start Pixie Dust attack on a specified network:
 ```
 sudo python3 oneshot.py -i wlan0 -K
 ```
Launch online WPS bruteforce with the specified first half of the PIN:
 ```
 sudo python3 oneshot.py -i wlan0 -b 00:90:4C:C1:AC:21 -B -p 1234
 ```
## Troubleshooting
#### "RTNETLINK answers: Operation not possible due to RF-kill"
 Just run:
```sudo rfkill unblock wifi```
#### "Device or resource busy (-16)"
 Try disabling Wi-Fi in the system settings and kill the Network manager. Alternatively, you can try running OneShot with ```--iface-down``` argument.
#### The wlan0 interface disappears when Wi-Fi is disabled on Android devices with MediaTek SoC
 Try run the following:
```
sudo chmod 644 /dev/wmtWifi
sudo sh -c 'echo 1 > /dev/wmtWifi'
```
# Acknowledgements
## Special Thanks
* `rofl0r` for initial implementation;
* `drygdryg` for developing OneShot
* `Monohrom` for testing, help in catching bugs, some ideas;
* `Wiire` developing Pixiewps;
* `eda-abec` for vulwsc updates.
