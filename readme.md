

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
4. Failed to skip sart setting of new system, need to boot and set up username/passwd/timezone/hostname by a monitor
5. IP Set Up, edit '/etc/dhcpcd.conf' on the SD card (boot) for a static IP [necessary for the MATRIX **soy**  only]. For the raspberrypi zero attached with camera, you only need to change the host name as "Rasperry-Order", Order is numberic
<pre>
static ip_address=192.168.31.123/24
static routers=192.168.31.1
static domain_name_servers=192.168.31.1
</pre>

6. SSH login in  via `ssh seed@192.168.31.101` (192.168.31.123) when you and the raspberry both under the wifi named "WIFI"
7. other setting ...
	a. camera script
	<pre>
	#!/bin/bash

	DATE=$(date +"%Y-%m-%d_%H%M")
	Host=`hostname`
	Time=`echo $Host | sed 's/.*-//'`

	raspistill -vf -hf -o ~/Documents/${Host}.${DATE}.jpg

	sleep ${Time}m #sleep for unload in turn

	scp  ~/Documents/${Host}.${DATE}.jpg   soy@192.168.31.123:/home/soy/Documents/Pic/ #IP or hostname raspberrypi

	## Get data transfer status and remove it if transfered to The MATRIX
	status=$?
	[ $status -eq 0 ] && rm ~/Documents/${Host}.${DATE}.jpg

	</pre>
	b. crontab -e `* */1 * * *  sh ~/Documents/Pic/work.sh` #every 1h conduct this script


#### II. Clone the Rasperry Pi OS across different Rasperry Pis
1. image the disk with [Disk Utility on Mac](https://gallaugher.com/make-a-copy-of-a-raspberry-pi-sd-card-mac/), rename the .cdr file to an iso file.
2. [Raspberry Pi -- Cloning Using SD Card Copier](https://linuxhint.com/how-to-clone-a-raspberry-pi-sd-card/)
   `Menu` >> `Accessions` >> `SD Card Copier`  [suggested]

#### III. 	In each clone, sets below:
1. Set up for ssh/scp w/o password
1)  `ssh-keygen -t rsa` generate the key (*default press press press*)
2)  add the public key (.ssh/id_rsa.pub) to **MATRIX** (file: .ssh/authorized_keys)
BINGO! ssh username@HOSTNAME (example, soy@raspberrypi) or username@IP
