# Home-AssistantConfig
Backup and Step-by-Step configuration of my Home-Assistant.

## Hardware
1x Raspberry Pi 3<br/>
1x Phillips Hue Bridge<br/>
2x Philips Hue White A19 60W Smart Bulb

## Installation
1. Download the [Raspbian Lite] image.<br/>
2. Use [Etcher] to flash the image into the SD card.<br/>
3. Open the `boot` partition and create a new file `ssh`. With no content.<br/>
4. Insert SD card to Raspberry Pi and turn it on.

[Hassbian Image]: https://github.com/home-assistant/pi-gen/releases/tag/untagged-8e6074a6a6dbf5ed52ca
[Etcher]: https://etcher.io/
[Raspbian Lite]: https://www.raspberrypi.org/downloads/raspbian/

### Expand the Filesystem, change Time Zone and Password
SSH to your system as the user `pi` and run:
```
$ sudo raspi-config
```

### Updating the Host Operating System
SSH to your system as the user `pi` and run:
```
$ sudo apt-get update
$ sudo apt-get upgrade
```

### Install Pi-Hole
```
$ sudo curl -sSL https://install.pi-hole.net | bash
```

### Install MPD and MPC
```
$ sudo apt-get install mpd mpc
```

### Install Hassbian-Config
```
$ wget https://github.com/home-assistant/hassbian-scripts/releases/download/v0.9.1/hassbian-scripts_0.9.1.deb
$ sudo dpkg -i hassbian-scripts_0.9.1.deb
$ sudo apt --fix-broken install
$ sudo dpkg -i hassbian-scripts_0.9.1.deb
```

### Install Home Assistant
```
$ sudo hassbian-config install homeassistant
```

### Updating Home Assistant
SSH to your system as the user `pi` and run:
```
$ sudo hassbian-config upgrade homeassistant
```

#### Technical Details
* Home Assistant is installed in a virtual Python environment at `/svr/homeassistant/`
* Home Assistant will be started as a service run by the user `homeassistant`
* The configuration is located at `/home/homeassistant/.homeassistant`

## Installing Samba
`$ sudo hassbian-config install samba`

## Installing DuckDNS
`$ sudo hassbian-config install duckdns`

## Configuration Backup to GitHub
#### Step 1: Installing and Initializing Git
Install the Git package on your Home Assistant server:
```
$ sudo apt-get update
$ sudo apt-get install git
```
#### Step 2: Creating `.GITIGNORE`
Create a `.gitignore` file using `Notepad++` editor and place it in the root of your Home Assistant configuration directory: `<config dir>/.gitignore`.
```
# Example .gitignore file for your config dir
*
!*.yaml
!.gitignore
secrets.yaml
known_devices.yaml
```
#### Step 3: Preparing your Home Assistant Directory for GitHub
```
$ sudo -u homeassistant -H -s
$ cd /home/homeassistant/.homeassistant/
$ git init
$ git config user.email "you@example.com"
$ git config user.name "Your Name"
$ git add .
$ git commit
```
#### Step 4: Your initial commit to GitHub
```
$ git remote add origin https://github.com/username/Home-AssistantConfig
$ git pull --allow-unrelated-histories origin master
$ git push -u origin master
```
You will be asked to enter your GitHub username and password.

### Keep GitHub Updated
```
$ sudo -u homeassistant -H -s
$ cd /home/homeassistant/.homeassistant/
$ git add .
$ git status
$ git commit -m "COMMIT MESSAGE"
$ git push origin master
```
