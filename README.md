# Debian 10 Install Steps and History
## General tips and commands
### Bash
#### View command history
`$ ~/.bash_history (inspect contents)`
### Gpg
#### Display generated keys and show their UIDs
`$ gpg --list-secret-keys --keyid-format=long`
## Commands and History
- Realise I made my Manjaro installation partion the /home mount for Debian 10... whoops ***WE NEED TO ADDRESS THIS LATER***
- `$ su`
- `$ sudo adduser denver sudo`
- reboot

- add contrib and non-free to sources
- `$ sudo apt update`
- `$ sudo apt install nvidia-detect`
- install package recommended by nvidia-detect (this case was `$ sudo apt install nvidia-driver`)
- say "okay" to nouveau warning
- reboot

---- note: oh thank god that's so much less laggy ----

---- note: from now on referring to "fresh debian 9 install steps" repo for setting up the basics

- connect PS3 controller via USB
- confirm prompt for connection
- test at https://html5gamepad.com/ (it's working perfectly out-of-the-box, god bless Debian)
- unplug USB
- press home button and test bluetooth support is working automatically (it is thank god)

- Gnome settings > Online Accounts (Add any relevant accounts)
- Gnome settings > Privacy > Location Services (turn on)
- Gnome settings > Sharing (enable)
- Gnome settings > Sharing > Computer Name (change this if it's not clear enough)
- Gnome settings > Devices > Displays > Night Light (turn it on)
- Gnome settings > Devices > Keyboard > "+" [scroll way down] (add these: Ctrl + Alt + T = gnome-terminal; Super + E = nautilus -w)
- Gnome settings > Details > Date & Time > Time Format (change to AM / PM)

- Tweak tool settings > Appearance > Themes > Applications (change to Adwaita-dark)

- Install gnome extensions
	- install drop down terminal
- Configure gnome extensions
	- [wip, refer to original install step repo at https://github.com/DrTexxOfficial/fresh-debian-install-steps for answers now]

- `$ sudo apt install apt-listbugs apt-listchanges`
- `$ sudo apt install snapd git gparted`

Configure flatpak
- `$ sudo apt install flatpak`
- `$ sudo apt install gnome-software-plugin-flatpak`
- `$ flatpak remote-add --if-not-exists flathub https://flathub.org/repo/flathub.flatpakrepo`
- reboot

Install some flatpak apps
- `$ flatpak install flathub io.atom.Atom` (Atom Text Editor)
- `$ flatpak install flathub com.spotify.Client` (Spotify)

Install and configure steam
- `$ sudo dpkg --add-architecture i386` (enable 32bit package support)
- `$ sudo apt update`
- ensure (or be sure) that your graphics driver is installed and that non-free is in your /etc/apt/sources.list file
- `$ sudo apt install steam`
- `$ sudo apt install nvidia-driver-libs-i386` (_"For full functionality (including Vulkan), also install the libraries listed as Recommends in the nvidia-driver-libs-i386 package"_)
- reboot
- login to steam and add a new library at ~/SteamLibrary-4TB
- delete ~/SteamLibrary-4TB/steamapps folder
- mount the 4TB drive by clicking it in nautilus and entering password as required
- `$ ln -s /media/denver/DEN_HDD_4TB/Steam-Linux/steamapps ~/SteamLibrary-4TB/steamapps`
- relaunch steam

- learn the hard way that Beat Hazard 2 will not launch via any version of Proton if it is installed on the 4TB using this method, however it will work if the install files are moved to ~/.steam/debian-installation/steamapps (moved via steam's game properties GUI with "MOVE INSTALL FOLDER..." under the "LOCAL FILES" tab). Is this the case for all proton games? Is it all proton versions? Further testing required.
- [UPDATE 2019-JUL-07-10:54] Thumper also won't work with any version of Proton if it is installed on the 4TB drive with this method (yet Linux native games have been fine), it may be an issue with installing Windows related dependencies (I only saw the DirectX install and such work fine, but only when the game had been installed on ~/.steam/steam/steamapps. The DirectX install and other windows dependencies did not ever appear when launching games installed on the 4TB drive, shortly followed by a crash.

- create a new repo for this file on github.com
- `$ mkdir ~/github`
- `$ cd ~/github`
- brief intermission, configure ssh for git and github.com ([more info](https://help.github.com/en/articles/connecting-to-github-with-ssh))
- `$ ssh-keygen -t rsa -b 4096 -C "<github-private-commit-email>"` (generate a new key)
- `$ eval "$(ssh-agent -s)"` (start the ssh-agent in the background)
- `$ ssh-add ~/.ssh/id_rsa` (_"Add your SSH private key to the ssh-agent"_)
- `$ sudo apt install xclip` (install xclip for copying the SSH key to clipboard)
- `$ xclip -sel clip < ~/.ssh/id_rsa.pub` (copy SSH key to clipboard to later paste at github.com)
- head to your settings and paste in the new ssh key
- okay back to the repo for this file
- `$ git clone git@github.com:DrTexxOfficial/debian-10-install-history.git`
- more fun git configuration before we commit and push
- `$ git config --global user.email "<github-private-commit-email>"`
- `$ git config --global user.name "DrTexxOfficial"`

Beat Hazard 2 Open Mic workaround ***(WORK IN PROGRESS)***
- `$ sudo apt install pavucontrol`
- `$ pactl load-module module-combine-sink`
- now open pavucontrol and change beat hazard 2's output to be the new device
- at the bottom of pavucontrol, change "Show:" to be "All Streams"
- Increase the volume of the new device
- Under the playback tab of pavucontrol, change the input of Spotify to be "Simultaneous output to Built-in Audio Analog Stereo"
- Under the Recording tab of pavucontrol, change the input sink to be "Monitor Source of Simultaneous output to Built-in Audio Analog Stereo"
- to later remove the device, to remove the device: `$ pactl unload-module module-combine-sink`
- NOTE: be sure to mute _playback_ of the new device, otherwise you'll get ugly double-audio issues (don't mute it's input!)

Get python3 stuff set-up
- `$ sudo apt install python3-venv` (venv for py3 virtual environments)
- `$ python3 -m pip install pip --upgrade`
- `$ sudo apt-get install python3-tk python3-xlib python3-dbus libasound2-dev`
- `$ sudo apt-get install python3-dev`

Install atom via apt instead of flatpak
- `$ sudo apt install wget`
- `$ wget -qO - https://packagecloud.io/AtomEditor/atom/gpgkey | sudo apt-key add -`
- `$ sudo sh -c 'echo "deb [arch=amd64] https://packagecloud.io/AtomEditor/atom/any/ any main" > /etc/apt/sources.list.d/atom.list'`
- `$ sudo apt update`
- `$ sudo apt install atom`
- `$ apm install script`
- _ensure all settings have been duplicated!_
- `$ flatpak uninstall io.atom.Atom`

Install htop
- `$ sudo apt install htop`

Install TriggerCMD v1.0.1 ***NOT GOTTEN WORKING YET, NECCESSARY?***
- install curl
- `$ sudo apt install curl`
- install npm
- `$ sudo apt install nodejs npm --upgrade`
- enable legacy support for tray icons
- install this gnome extension to get legacy tray icon support: https://extensions.gnome.org/extension/1031/topicons/
- install triggercmd
- `$ sudo apt install ~/Applications/PACKAGES/triggercmdagent_1.0.1_all.deb`
- go to https://www.triggercmd.com/user/computer/create and copy your token
- `$ triggercmdagent`
- paste in your token when prompted
- `$ sudo /usr/share/triggercmdagent/app/src/installdaemon.sh`
- Now you should see your Raspberry Pi in your TRIGGERcmd account.
- reboot
- Edit /root/.TRIGGERcmdData/commands.json to add or remove commands.

Get bluetooth headphones working properly
- (refer to instructions in https://github.com/drtexxofficial/fresh-debian-install-steps)
- note: only thing that didn't work as normal is that the file to be deleted - `/var/lib/gdm3/.config/systemd/user/sockets.target.wants/pulseaudio.socket` - doesn't exist at this location. ***will need to look into further if pulseaudio still begins at startup in the same fashion, what does all this mean? Still not 100% sure honestly***

Install Project M
- install requirements: `$ sudo apt install libsdl2-2.0.0` ***DON'T USE OFTEN, USUALLY KODI VISUALIZER INSTEAD; FEEL FREE TO UNINSTALL.***
- Download files

Install Kodi
- `$ sudo apt install kodi`

Install Suru++ 30 Barcelona icon theme
- follow instuctions found [here](https://github.com/gusbemacbe/suru-plus/wiki/Installing-the-stable-version-with-CLI) (install in home for GTK)

- `$ sudo update-desktop-database` (updates database for .desktop files, store custom made ones in ~/.local/share/applications)

Add en_US.UTF-8 locale for Source Engine based games
- `$ sudo dpkg-reconfigure locales`
- press space to add locales to list (add en_US.UTF-8)
- press tab to highlight 'ok' and then press enter
- set default locale as Australian (en_AU.UTF-8)
- press enter to continue
- done!

Install discord via flatpak
- `$ flatpak install com.discordapp.Discord`

- `$ sudo apt update`
- `$ sudo apt upgrade`

Reinstall discord via snap (instead of flatpak)
- nevermind the flatpak version is still broken (21-Jul-2019)
- `$ flatpak uninstall com.discordapp.Discord`
- `$ snap install discord`

Install Slack via Snap (flatpack is unofficial)
- `$ snap install slack --classic`

Install tor and torbrowser
- `$ sudo apt install tor` (for logging into Facebook because they've already got enough of my data)
- Download torbrowser from [here](https://www.torproject.org/download/) and put the contents in it's own folder in ~/Applications (in this instance, the folder was already called tor-browser_en-US)
- navigate to ~/Applications/tor-browser_en-US
- `$ ./start-tor-browser.desktop` (this must be run once before the .desktop file will function as intended or is even worth linking)
- `$ ln -s ~/Applications/tor-browser_en-US/start-tor-browser.desktop start-tor-browser.desktop` (double check that this path is accurate!)
- You should now be able to type "tor" into your desktop search and launch the tor browser from there (in the event of issues try running `$ sudo update-desktop-database`)

Install youtube-dl
- `$ snap install youtube-dl`

Install Arc theme for Debian
- `$ sudo apt install arc-theme`
- Open tweak tool and change the following settings:
- Appearance > Themes > Applications > "Arc-Dark"
- Appearance > Themes > Shell > "Arc-Dark"

Install qbittorrent
- `$ sudo apt install qbittorrent`

Install Skype via gnome-software (it's a flatpak) (primarily for emergency calls using Skype credit)

Download Balena Etcher AppImage and put in ~/Applications/AppImages

Install v programming language compiler
```
git clone https://github.com/vlang/v
cd v
make
```

`sudo ln -s [path to V repo]/v /usr/local/bin/v`

Install vscode via Snap
`snap install vscode --classic`

Install mercurial (so we can get OpenTyrian and build it from source)
`sudo apt install mercurial`

Download, build and run OpenTyrian
- `$ hg clone https://bitbucket.org/opentyrian/opentyrian`
- `$ sudo apt install libsdl-dev libsdl-net1.2-dev` (used for Simple DirectMedia Layer, which I believe appears to be graphics related)
copy the 'data' folder from [this file](http://www.camanis.net/opentyrian/releases/opentyrian-2.1.20130907-w32-bundle.zip) into the same directory as OpenTyrian (looking like this: [root OpenTyrian folder]/data)
- navigate to the opentyrian folder
`$ make`
- Done! Enjoy OpenTyrian on Linux Debian 10 Buster!

Install Zoom for video conferencing
`$ flatpak install flathub us.zoom.Zoom`

Install OBS studio
`$ sudo apt install ffmpeg`
`$ sudo apt install obs-studio`

Install VirtualBox ([original guide](https://www.virtualbox.org/wiki/Linux_Downloads))
- create `/etc/apt/sources.list.d/virtualbox.list`
- make the contents this: `deb https://download.virtualbox.org/virtualbox/debian buster contrib`
- `$ wget -q https://www.virtualbox.org/download/oracle_vbox_2016.asc -O- | sudo apt-key add -` (download and register Oracle public-key)
- `$ sudo apt update`
- `$ sudo apt install virtualbox-6.0`

Install net-tools for ifconfig command ***just using `ip addr` works, is this necessary?***
- `$ sudo apt install net-tools`

Install zenmap for network topology scanning
- `$ sudo apt install zenmap`

Install metasploit
- `$ curl https://raw.githubusercontent.com/rapid7/metasploit-omnibus/master/config/templates/metasploit-framework-wrappers/msfupdate.erb > msfinstall && chmod 755 msfinstall && ./msfinstall`

Enable alternatetab extension in tweak tool

Install Kodi via flatpak
- `$ flatpak install flathub tv.kodi.Kodi`

Install netflix addon in Kodi (Leia onwards only, needs DRM support) ([original guide](https://forum.kodi.tv/showthread.php?tid=329767))
- `$ pip3 install --user pycryptodomex`
- download the [netflix repository](https://github.com/castagnait/repository.castagnait/raw/master/repository.castagnait-1.0.0.zip)
- install the addon from within Kodi
- install Netflix from the Netflix Addon Repository
- configure with username and password
- install any required dependencies when prompted
- done!

Install flatpak-builder for development of flatpaks
- `$ sudo apt install flatpak-builder`

Add network printers to PC
- `$ sudo apt install task-print-server`
- add the printer in gnome settings

Uninstall Zoom
- `$ flatpak remove zoom`

Install UFW (Uncomplicated Firewall)
- `$ sudo apt install ufw`
- `$ sudo ufw enable`

Setup GPG keys for github
- [generating GPG keys](https://help.github.com/en/articles/generating-a-new-gpg-key)
- [adding GPG keys to GitHub account](https://help.github.com/en/articles/adding-a-new-gpg-key-to-your-github-account)
- [associating GPG key with git](https://help.github.com/en/articles/telling-git-about-your-signing-key)

- add key to git
	- `$ git config --global user.signingkey {KEY-ID}`

- To add your GPG key to your bash profile, paste the text below:
```
$ test -r ~/.bash_profile && echo 'export GPG_TTY=$(tty)' >> ~/.bash_profile
$ echo 'export GPG_TTY=$(tty)' >> ~/.profile
```
- sign all commits automatically
	- `$ git config --global commit.gpgsign true`
- fix usage of old username
	- `$ git config --global user.name DrTexx`
	- `$ git config --global user.email {github-noreply-email}`

Install Dropbox (later uninstalled, see further down for alternative install method)
- `$ sudo apt install nautilus-dropbox`
- `$ sudo apt install python-gpg` (optional package for verification) ***dropbox actually uses python-gpgme, however it is depreciated in Debian 10. This isn't a necessary install***
- Run Dropbox

Install keybase
- `$ cd ~/Downloads/`
- `$ curl --remote-name https://prerelease.keybase.io/keybase_amd64.deb`
- `$ sudo apt install ./keybase_amd64.deb` (note: this will add the keybase repository automatically)
- `$ run_keybase`

Migrate to official VSCode Snap
- `$ snap remove vscode`
- `$ snap install code --classic`

Install some requirements for V (graphics dependencies)
- `$ sudo apt install libglfw3 libglfw3-dev libfreetype6-dev libssl-dev`

Install dependencies for audio-visualiser-python
- `$ sudo apt install libjpeg-dev zlib1g-dev`

Install 2 to 3 (convert python2 to python3 syntax)
- `$ sudo apt install 2to3`

Install dependencies for PyAudio package
- `$ sudo apt install portaudio19-dev python-all-dev`

Install audacity
- `$ sudo apt install audacity`

Reinstall dropbox via different means
- `$ sudo apt remove nautilus-dropbox`
- download the latest source [here](https://linux.dropbox.com/packages/) (https://linux.dropbox.com/packages/)
- install requirements for building dropbox from source
- `$ sudo apt install gnome-common libnautilus-extension-dev python3-gi python3-docutils`
- extract the source and navigate into the directory
- `$ ./configure`
- `$ make`
- `$ make install`
- install a requirement for dropbox installer itself
- `$ sudo apt install python3-gpg`
- `$ ./dropbox start -i`
- install the proprietary binaries for dropbox when instructed by the installer

Uninstall dropbox because they broke selective sync
- ***INSTRUCTIONS TODO*** (basically opposite of above installs using apt)

Install pCloud
- download AppImage and move it to ~/Applications/AppImages
- make pcloud executable
- run pcloud

Install GIMP
- `$ sudo apt install gimp`

In nvidia-settings ([original tutorial](https://www.techticity.com/howto/how-to-fix-nvidia-vsync-on-linux-with-proprietary-drivers/))
- x server display configuration
		- advanced...
		- enable "force full composition pipeline"
		- "Save to X Configuration File" to make permanent ***I didn't do this***
- x screen 0
		- OpenGL settings
				- sync to Vblank = False
				- allow flipping = False

Turn a microphone into an output stream using pactl (loopback module)
- `$ pactl load-module module-loopback`
