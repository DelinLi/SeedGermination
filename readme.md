

#### I. Set Up of Raspberry Pi (3B/4B/Zero)

1. [Raspberry Pi Imager](https://www.raspberrypi.com/software/) install the Raspberry Pi OS (32 bit) "a port of Debian Bullseye with the Raspberry Pi Desktop Released 2022-04-04"
2. Wifi Set UP, create `wpa_supplicant.conf` on the SD card (boot) and edit:
<pre>
country=CN # 2-digit country code
ctrl_interface=DIR=/var/run/wpa_supplicant GROUP=netdev
network={
	ssid="WifiName"  ##wifi name
	psk="Seed2022" ##wifi passwd SeedGerm2022
  key_mgmt=WPA-PSK
}
</pre>
3. Configure Enable
  a. Enable `ssh`,  add an empty file named "ssh" -- `sudo touch ssh` to the SD card
  b. Enable `camera`, edit the `config.txt`
  <pre>
start_x=1             # essential
gpu_mem=128           # less possibly induce errors
disable_camera_led=0  # 1 if you don't want the camera led to glow
  </pre>
4. Failed to skip sart setting of new system, need to boot and set up username/passwd/timezone by a monitor
5. IP Set Up, edit '/etc/dhcpcd.conf' on the SD card (boot) for a static IP .
<pre>
static ip_address=192.168.31.10X/24
static routers=192.168.31.1
static domain_name_servers=192.168.31.1
</pre>

6. SSH login in  via `ssh seed@192.168.31.101` (192.168.31.10X, X=1 is set up here) when you and the raspberry both under the wifi named "WIFI"
7. other setting ...
	a. crontab -e
	b. camera script

II. Clone the Rasperry Pi OS across different Rasperry Pis
image the disk with [Disk Utility on Mac](https://gallaugher.com/make-a-copy-of-a-raspberry-pi-sd-card-mac/), rename the .cdr file to an iso file. Then

III.
