# Debian 10 Install Steps and History
## General tips and commands
### System logs
`$ sudo dmesg`
### Bash
#### View command history
`$ ~/.bash_history` (inspect contents)
### Aptitude
#### Kill any install processes to remove dpkg lock
`sudo killall dpkg`
#### Fix package issues (may need to remove dpkg lock first)
`sudo dpkg --configure -a`
#### Find missing package/s based on missing files
`apt-file search [path_segment/s]` (example: `apt-file search bin/designer`)
### Files
#### Print list of file/directories sorted by filesize/s
`$ du -h --max-depth=1 | sort -hr`
### Gpg
#### Display generated keys and show their UIDs
`$ gpg --list-secret-keys --keyid-format=long`
or for shorted IDs
`$ gpg --list-keys --keyid-format short`
### Links
#### View the full path of a symlink
`$ readlink -f [symlink_name]`
(an alternative in Debian 10 is 'realpath', however it is not available in all linux distributions)
### Checksums
#### Checking sha256 sums
`$ sha256sum <file>`
### Venv
#### Entering venvs made easy
`$ nano ~/.bashrc` (add the line `alias [alias_name]="{command} [command args]"`)
### Pinentry settings
#### List available prompt types
`$ ls /usr/bin | grep pinentry`
#### Change default prompt type


Once you've saved the file and re-opened bash, you can type `[alias_name]` to execute `{command} [command args]`

For example, adding the line `alias voluxDE="source /home/denver/github/volux/venv_testing/bin/activate` would allow you to enter the venv 'venv_testing' just by typing `voluxDE`.

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

Install keybase (removed since, do not install)
<!-- - `$ cd ~/Downloads/`
- `$ curl --remote-name https://prerelease.keybase.io/keybase_amd64.deb`
- `$ sudo apt install ./keybase_amd64.deb` (note: this will add the keybase repository automatically)
- `$ run_keybase` -->

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

In `sudo nvidia-settings` ([original tutorial](https://www.techticity.com/howto/how-to-fix-nvidia-vsync-on-linux-with-proprietary-drivers/))
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

Install postman (API testing thing)
- `$ snap install postman`

Uninstall postman because how the hell do you use it
- `$ snap remove postman`

Install Cura 4.3.0 as AppImage
- Download from https://ultimaker.com/software/ultimaker-cura
- Put the AppImage in ~/Applications/AppImages
- Make it executable (from CLI, in directory, do `$ chmod +x Ultimaker_Cura-4.3.0.AppImage`)
- Double-click that bad boy to launch.
- Install icon (for .desktop file)
	- Download the file "cura-128.png" from the [github page](https://github.com/Ultimaker/Cura), place it in `~/Applications/AppImages/icons/cura-128.png`
	- `$ ln -s /home/denver/Applications/AppImages/icons/cura-128.png /home/denver/.icons/cura-128.png` (this lets you enter "cura-128.png" instead of a full path to the file)
- Create a .desktop file for cura
	- `$ nano ~/Applications/AppImages/desktop-files/cura.desktop`
		- Place the following contents into it (modified version of the one on github):
		```.desktop
		[Desktop Entry]
		Name=Cura (AppImage)
		Name[de]=Ultimaker Cura
		Name[nl]=Ultimaker Cura
		GenericName=3D Printing Software
		GenericName[de]=3D-Druck-Software
		GenericName[nl]=3D-printsoftware
		Comment=Cura converts 3D models into paths for a 3D printer. It prepares your print for maximum accuracy, minimum printing time and good reliability with many extra features that make your print come out great.
		Comment[de]=Cura wandelt 3D-Modelle in Pfade für einen 3D-Drucker um. Es bereitet Ihren Druck für maximale Genauigkeit, minimale Druckzeit und guter Zuverlässigkeit mit vielen zusätzlichen Funktionen vor, damit Ihr Druck großartig wird.
		Comment[nl]=Cura converteert 3D-modellen naar paden voor een 3D printer. Het bereidt je print voor om zeer precies, snel en betrouwbaar te kunnen printen, met veel extra functionaliteit om je print er goed uit te laten komen.
		Exec=cura %F
		TryExec=cura
		Icon=cura-128.png
		Terminal=false
		Type=Application
		MimeType=model/stl;application/vnd.ms-3mfdocument;application/prs.wavefront-obj;image/bmp;image/gif;image/jpeg;image/png;text/x-gcode;application/x-amf;application/x-ply;application/x-ctm;model/vnd.collada+xml;model/gltf-binary;model/gltf+json;model/vnd.collada+xml+zip;
		Categories=Graphics;
		Keywords=3D;Printing;Slicer;
		StartupWMClass=cura.real
		```
	- `$ ln -s /home/denver/Applications/AppImages/desktop-files/cura.desktop /home/denver/.local/share/applications/cura.desktop`
- Make executable anywhere (and make .desktop file work)
	- `$ sudo ln -s /home/denver/Applications/AppImages/Ultimaker_Cura-4.3.0.AppImage /usr/local/bin/cura`

Install RPCS3 as AppImage (aka. rpcs3)
- ***add instructions***
- Put in ~/Applications/AppImages
- Make it executable

### DANGER - READ INSTRUCTIONS, DON'T DELETE YOUR BLOODY ROOT
Remove all old folder structure under ~/home
- It should only be ~/home/denver, so we'll delete all the stuff I screwed up transfering from Manjaro
- ***WARNING: RUN THE FOLLOWING UNDER /home!!!***
- ***DO NOT DO THIS IN THE ROOT DIRECTORY IN CASE YOU SCREW IT UP***
sudo mv /home/home /home/o_home
sudo rm -rf /home/o_home
sudo rm -rf /home/bin
sudo rm -rf /home/boot
sudo rm -rf /home/dev
sudo rm -rf /home/desktopfs-pkgs.txt
sudo rm -rf /home/etc
sudo rm -rf /home/lib /home/lib64
sudo rm -rf /home/lost+found
sudo rm -rf /home/mnt
sudo rm -rf /home/opt
sudo rm -rf /home/proc
sudo rm -rf /home/root
sudo rm -rf /home/rootfs-pkgs.txt /home/run /home/sbin
sudo rm -rf snap
sudo rm -rf srv sys usr var
sudo rm -rf tmp

Install PCSX2 via Flatpak
- `$ flatpak install flathub net.pcsx2.PCSX2`

Install and configure etckeeper (version control for /etc)
- `$ sudo apt install etckeeper`
- `$ cd /etc`
- `$ sudo etckeeper init`
- `$ sudo nano /etc/etckeeper/etckeeper.conf` (for config settings, no changes required but here if needed prior to next step) (this might take a second)
- `$ sudo etckeeper commit "First commit of my /etc directory"`

Update Nvidia driver from v418.74 to v430.50 ([original guide](https://linuxconfig.org/how-to-install-nvidia-driver-on-debian-10-buster-linux) - not strictly followed)
- Download latest "long lived branch version" from [here](https://www.nvidia.com/en-us/drivers/unix/).
- Unplug second monitor (just in-case, it's broken stuff in the past)
- Check the readme for driver version 430.50 [here](http://us.download.nvidia.com/XFree86/Linux-x86_64/430.50/README/installdriver.html)
- `$ systemctl set-default multi-user.target` ("Reboot to multi-user runlevel. This will disable the GUI user after reboot")
- `$ systemctl reboot` (reboot system)
- change default keyboard layout back to qwerty with `$ dpkg-reconfigure keyboard-configuration`
- `$ sudo apt remove nvidia-driver`
- `$ sudo apt autoremove`
- `$ sudo reboot`
- `$ sudo apt purge nvidia.` (uninstall anything nvidia related and associated config files)
- reboot and test if anything still installed (looks good!)
... (bunch of other stuff, check paper)
- `$ `
- sudo apt install nvidia-settings ***wait don't do that***
- `$ systemctl set-default graphical.target`

Install Wine (fingers crossed) ([tutorial used](https://linuxhint.com/installing_wine_debian_10/))
- `$ sudo apt update`
- `$ sudo apt install wine wine64 wine32 winbind winetricks`
- when prompted, enable WINS server stuff

Install Real-time Corrupter (WINE)
- Install mono (complete) ([tutorial used](https://www.mono-project.com/download/stable/#download-lin-debian))
		- `$ sudo apt install apt-transport-https dirmngr gnupg ca-certificates` (all of these dependency packages were already installed)
		- `$ sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys 3FA7E0328081BFF6A14DA29AA6A19B38D3D831EF`
		- `$ echo "deb https://download.mono-project.com/repo/debian stable-buster main" | sudo tee /etc/apt/sources.list.d/mono-official-stable.list`
		- `$ sudo apt update`
		- `$ sudo apt install mono-complete` (heads up, it's ~500mb)
		***IN REFLECTION, I MIGHT HAVE GONE ABOUT THIS WRONG, YOU MAY JUST NEED THE STEPS BELOW***
- Install wine-mono ([instructions used](https://askubuntu.com/questions/841847/mono-package-for-wine-is-not-installed))
		- Download wine-mono-4.9.3.msi from [here](http://dl.winehq.org/wine/wine-mono/)
		- `$ wine64 uninstaller`
		- select 'install' from the GUI
		- select the downloaded .msi package
		- press "ok"
- Run RTC
		- navigate to the directory containing RTC_Launcher.exe (should be ~/Applications/Windows/real-time_corruptor)
		- `$ wine RTC_Launcher.exe`
		- uh oh it's gotten very complicated.

Update all python3 pip/pypi related stuff
- `$ pip3 install setuptools wheel pip --upgrade`

Uninstall postman
- `$ snap remove postman`

Install volux in a virtual environment (can be used to make other venv held packages accessible outside of venv)
- `$ cd ~/Applications`
- `$ python3 -m venv volux`
- `$ source volux/bin/activate`
- `$ pip install volux`
- `$ sudo ln -s /home/denver/Applications/volux/bin/volux /usr/local/bin/volux`
- Finished! You can now access the virtual environments /bin/volux from your own command-line! (simply type `volux` anywhere)

Install Vim
- `$ sudo apt install vim`

Install font: _'Hack'_
- `$ sudo apt install fonts-hack`

Change prompt back to askpass (why it recently changed to TTY-only I do not know. Update? It's still the same window for GPG prompts)
- `$ sudo apt install ssh-askpass-gnome ssh-askpass`

Force usage of fullscreen ssh auth prompts instead of cli prompts ([info source](https://unix.stackexchange.com/questions/460237/gnome-keyring-daemon-sometimes-not-asking-for-passphrase-need-to-provide-it-via?noredirect=1))
- `$ /usr/bin/gnome-keyring-daemon --start --components=pkcs11` (find path to GUI prompt)
- `/run/user/1000/keyring/ssh` (output I recieved)
- `$ export SSH_AUTH_SOCK="/run/user/1000/keyring/ssh"` (export the SSH_AUTH_SOCK env variable to gnome-keyring-daemon)

***add fstab changes***

Install Lutris
- `$ echo "deb http://download.opensuse.org/repositories/home:/strycore/Debian_9.0/ ./" | sudo tee /etc/apt/sources.list.d/lutris.list`
- `$ wget -q https://download.opensuse.org/repositories/home:/strycore/Debian_9.0/Release.key -O- | sudo apt-key add -`
- `$ sudo apt-get update`
- `$ sudo apt-get install lutris`

Install Java Runtime Environment
- `$ sudo apt install default-jre`

Install MultiBootUSB
- Download MultiBootUSB from [here](https://github.com/mbusb/multibootusb/releases) (or [here](http://multibootusb.org/page_download/) if you'd rather use the site than repo)
- Move .deb file to ~/Applications/PACKAGES/
- `$ sudo apt install ~/Applications/PACKAGES/python3-multibootusb_9.2.0-1_all.deb`

Install VMWare Workstation 15.5.1 Player for Linux 64-bit
- Download from [here](https://my.vmware.com/en/web/vmware/free#desktop_end_user_computing/vmware_workstation_player/15_0)
- move to ~/Applications/PACKAGES
- navigate to ~/Applications/PACKAGES
- `$ sudo bash ./VMware-Player-15.5.1-15018445.x86_64.bundle`

Install graphviz for pycallgraph2:
- `$ sudo apt install graphviz`

Install gephi for pycallgraph2:
- Download from [here](https://gephi.org/)
- Extract folder to ~/Applications/APPS
- Install Java 8 (requirement) ("Installing OpenJDK 8" in [this guide](https://linuxize.com/post/install-java-on-debian-10/))
	- `$ sudo apt update`
	- `$ sudo apt install apt-transport-https ca-certificates wget dirmngr gnupg software-properties-common`
	- `$ wget -qO - https://adoptopenjdk.jfrog.io/adoptopenjdk/api/gpg/key/public | sudo apt-key add -`
	- `$ sudo add-apt-repository --yes https://adoptopenjdk.jfrog.io/adoptopenjdk/deb/`
	- `$ sudo apt update`
	- `$ sudo apt install adoptopenjdk-8-hotspot`
	- `$ java -version` (verify the installation by checking the Java version)
	- you should get this output from the last command:
	```
	openjdk version "1.8.0_212"
	OpenJDK Runtime Environment (AdoptOpenJDK)(build 1.8.0_212-b04)
	OpenJDK 64-Bit Server VM (AdoptOpenJDK)(build 25.212-b04, mixed mode)
	```

- `$ sudo apt install libcanberra-gtk-module libcanberra-gtk3-module` (optional dependency)
- `$ sudo ln -s ~/Applications/APPS/gephi-0.9.2/bin/gephi /usr/local/bin/gephi` (make it accessible from CLI)

Install Gource because it's cool as heck
- `$ sudo apt install gource`

Install bash-doc (bash documentation)
- `$ sudo apt install bash-doc`

Install Docker Engine - Community ([guide used](https://docs.docker.com/install/linux/docker-ce/debian/))
- Add repository
		- `$ sudo apt update`
		- `$ sudo apt install apt-transport-https ca-certificates curl gnupg2 software-properties-common` (install/check requirements for apt install over HTTPS)
		- `$ curl -fsSL https://download.docker.com/linux/debian/gpg | sudo apt-key add -` (add docker's GPG key)
		- `$ sudo apt-key fingerprint 0EBFCD88` (check key fingerprint is _9DC8 5822 9FC7 DD38 854A E2D8 8D81 803C 0EBF CD88_)
		- `$ sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/debian $(lsb_release -cs) stable"` (add source)
- Install from repo
		- `$ sudo apt update`
		- `$ sudo apt-get install docker-ce docker-ce-cli containerd.io`
- Test installation
		- `$ sudo docker run hello-world` (verify docker installed correctly)

Install flake8 linter for atom
- `$ apm install linter-flake8 --verbose`

Install flake8 (for use in linters)
- `$ cd ~/Applications`
- `$ python3 -m venv flake8`
- `$ source flake8/bin/activate`
- `$ pip install flake8`
- `$ deactivate`
- `$ sudo ln -s /home/denver/Applications/flake8/bin/flake8 /usr/local/bin/flake8` (add to path)

Install hunspell en-au
- `$ sudo apt install hunspell-en-au`

Install black plugin for atom
- `$ apm install python-black --verbose`

Install black (for use in atom)
- `$ cd ~/Applications`
- `$ python3 -m venv black`
- `$ source black/bin/activate`
- `$ pip install black`
- `$ deactivate`
- `$ sudo ln -s /home/denver/Applications/black/bin/black /usr/local/bin/black` (add to path)

Install portaudio19-dev
- `$ sudo apt install portaudio19-dev`

Install RPCS3 as AppImage + get source
- Download/*install* AppImage
	- Download latest AppImage from [here](https://rpcs3.net/download)
	- Place the AppImage in `~/Applications/AppImages` (make sure it's executable)
	- `$ sudo ln -s /home/denver/Applications/AppImages/rpcs3-v0.0.7-9177-2290c389_linux64.AppImage /usr/local/bin/rpcs3`
- Clone the repo for desktop assets
	- `$ cd ~/github`
	- `$ git clone git@github.com:RPCS3/rpcs3.git`
- Link .desktop file
	- `$ ln -s home/denver/github/rpcs3/rpcs3/rpcs3.desktop /home/denver/.local/share/applications/rpcs3.desktop`
- Link the icon file
	- `$ ln -s /home/denver/github/rpcs3/rpcs3/rpcs3.svg /home/denver/.icons/rpcs3.svg`

Fix PS3 controller for RPCS3 ([original info](https://wiki.rpcs3.net/index.php?title=Help:Controller_Configuration))
- `$ sudo nano /etc/udev/rules.d/99-ds3-controllers.rules`
	- Paste in these contents:
	```
	# DualShock 3 over USB
	KERNEL=="hidraw", ATTRS{idVendor}=="054c", ATTRS{idProduct}=="0268", MODE="0666"

	# DualShock 3 over Bluetooth
	KERNEL=="hidraw*", KERNELS=="*054C:0268*", MODE="0666"
	```
- `$ sudo udevadm control --reload-rules`
- Reconnect/replug your controller (if this fails to work, reboot)

Install KiCad via Flatpak
- `$ flatpak install flathub org.kicad_pcb.KiCad`

Add volux .desktop files
- `$ ln -s /home/denver/github/volux/volux_launch.desktop ~/.local/share/applications/volux_launch.desktop`
- `$ ln -s /home/denver/github/volux/volux_launch_lightshow.desktop ~/.local/share/applications/volux_launch_lightshow.desktop`

Install kdenlive for video editting (RIP IH Discord)
- `$ sudo apt install kdenlive`

Install tilda (drop-down terminal alternative)

```
sudo apt install tilda
```

Install atom packages

```
apm install tidy-markdown
```

Install NVM (node version manager)

```
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.35.1/install.sh | bash
```

-   reopen terminal

```
nvm install 12.8.1
```

Install atom packages

```
apm install fast-eslint
apm install emmet
```

Install Teamviewer (adds custom repo during install)

- Download the .deb from [here](sudo apt install teamviewer_15.0.8397_amd64.deb)
- Create ~/Applications/packages_deb (`touch ~/Applications/packages_deb`)
- Move .deb file to ~/Applications/packages_deb
- `sudo apt install ~/Applications/packages_deb/teamviewer_15.0.8397_amd64.deb` (don't worry, it will add an entry in /etc/apt/sources.list.d/teamviewer.list)

Install atom packages

```
apm install rainbow-csv bracket-colorizer tablr
```

Install sqlitebrowser

```
sudo apt install sqlitebrowser
```

Install ide-python for atom

```
apm install ide-python
python3 -m venv python-language-server
source python-language-server/bin/activate
pip install 'python-language-server[all]'
sudo ln -s /home/denver/Applications/python-language-server/bin/pyls /usr/local/bin/pyls
```

-   Lastly in the settings for the ide-python atom package, change the python executable path to be `/home/denver/Applications/python-language-server/bin/python3`

Install gTile gnome extension

Install mesa-vulkan-drivers in attempts to fix VR

```
sudo apt install mesa-vulkan-drivers libvulkan1 libvulkan1:i386
```

I JUST WANT STEAM VR TO NOT CRASH 49/50 TIMES I LAUNCH IT

```
sudo apt remove nvidia-driver
sudo apt autoremove
```

Install unstable Nvidia driver

```
/etc/init.d/gdm3 stop
```

-   ***(remaining instructions missing!)***

Enable h264_nvenc encoding with ffmpeg ***(WARNING, FRUITLESS)***

-   install nv-codec-headers
```
git clone https://git.videolan.org/git/ffmpeg/nv-codec-headers.git
cd nv-codec-headers && sudo make install && cd -
```

-   clone ffmpeg source
```
cd ~/Downloads
git clone https://git.ffmpeg.org/ffmpeg.git
```

<!-- -   Install CUDA toolkit (nevermind)
```
sudo apt install nvidia-cuda-toolkit
sudo apt remove nvidia-cuda-toolkit
``` -->

<!-- -   install CUDA with docker ([original instructions](https://github.com/NVIDIA/nvidia-docker))
```
sudo docker pull nvidia/cuda
distribution=$(. /etc/os-release;echo $ID$VERSION_ID)
curl -s -L https://nvidia.github.io/nvidia-docker/gpgkey | sudo apt-key add -
curl -s -L https://nvidia.github.io/nvidia-docker/$distribution/nvidia-docker.list | sudo tee /etc/apt/sources.list.d/nvidia-docker.list
sudo apt update
sudo apt install nvidia-container-toolkit
sudo systemctl restart docker
sudo docker run --gpus all nvidia/cuda:9.0-base nvidia-smi
``` -->

-   Uninstall manually installed nvidia driver (using Windows for VR now anyway)

	-   ensure this stuff is uninstalled

		```
		sudo apt remove nvidia-driver nvidia-cuda-toolkit
		```

	-   uninstall manually installed driver (use same file you downloaded to install it, should still be in ~/Downloads)

		```
		sudo bash NVIDIA-Linux-x86_64-440.44.run --uninstall
		```

-   Install stable nvidia driver

	-   if you want to double-check it's nvidia-driver you'll need
		```
		sudo apt install nvidia-detect
		nvidia-detect
		```

	-   install stable driver
		```
		sudo apt install nvidia-driver
		```

	-   reboot

-   Install CUDA toolkit

	```
	sudo apt install nvidia-cuda-toolkit
	```

-   compile ffmpeg with special arguments

	-   enter the root of the cloned repo
	```
	cd ~/Downloads/ffmpeg
	```

  -   install requirements
	```
	sudo apt-get update -qq && sudo apt-get -y install \
	  autoconf \
	  automake \
	  build-essential \
	  cmake \
	  git-core \
	  libass-dev \
	  libfreetype6-dev \
	  libsdl2-dev \
	  libtool \
	  libva-dev \
	  libvdpau-dev \
	  libvorbis-dev \
	  libxcb1-dev \
	  libxcb-shm0-dev \
	  libxcb-xfixes0-dev \
	  pkg-config \
	  texinfo \
	  wget \
	  zlib1g-dev
	```

	-   make some directories
	```
	mkdir -p ~/ffmpeg_sources ~/bin
	```

	- install NASM
	```
	sudo apt-get install nasm
	```

	- install Yasm
	```
	sudo apt-get install yasm
	```

	- install libx264-dev
	```
	sudo apt-get install libx264-dev
	```

	- install libx265-dev
	```
	sudo apt-get install libx265-dev libnuma-dev
	```

	- install libvpx-dev
	```
	sudo apt-get install libvpx-dev
	```

	- install libfdk-aac-dev
	```
	sudo apt-get install libfdk-aac-dev
	```

	- install libmp3lame-dev
	```
	sudo apt-get install libmp3lame-dev
	```

	- install libopus-dev
	```
	sudo apt-get install libopus-dev
	```

	- compile ffmpeg with special flags
	```
	cd ~/ffmpeg_sources && \
	wget -O ffmpeg-snapshot.tar.bz2 https://ffmpeg.org/releases/ffmpeg-snapshot.tar.bz2 && \
	tar xjvf ffmpeg-snapshot.tar.bz2 && \
	cd ffmpeg && \
	PATH="$HOME/bin:$PATH" PKG_CONFIG_PATH="$HOME/ffmpeg_build/lib/pkgconfig" ./configure \
	  --prefix="$HOME/ffmpeg_build" \
	  --pkg-config-flags="--static" \
	  --extra-cflags="-I$HOME/ffmpeg_build/include" \
	  --extra-ldflags="-L$HOME/ffmpeg_build/lib" \
	  --extra-libs="-lpthread -lm" \
	  --bindir="$HOME/bin" \
	  --enable-gpl \
	  --enable-libass \
	  --enable-libfdk-aac \
	  --enable-libfreetype \
	  --enable-libmp3lame \
	  --enable-libopus \
	  --enable-libvorbis \
	  --enable-libvpx \
	  --enable-libx264 \
	  --enable-libx265 \
	  --enable-cuda-sdk \
	  --enable-cuvid \
	  --enable-nvenc \
	  --enable-nonfree \
	  --enable-libnpp \
	  --extra-cflags=-I/usr/local/cuda/include \
	  --extra-ldflags=-L/usr/local/cuda/lib64 \
	  --enable-nonfree && \
	PATH="$HOME/bin:$PATH" make -j 10 && \
	make install && \
	hash -r
	```

-   ***AND AFTER ALL THAT IT DOESN'T WORK, AWESOME.*** (requires version >435 to use nvenc but can't install nvidia-cuda-toolkit unless I'm using driver version 418, would install manually however nvidia doesn't officially support Debian for cuda-toolkit and thus there's no package to manually install)

Install VSCodium (VS Code without telemetry)

```
wget -qO - https://gitlab.com/paulcarroty/vscodium-deb-rpm-repo/raw/master/pub.gpg | sudo apt-key add -
echo 'deb https://gitlab.com/paulcarroty/vscodium-deb-rpm-repo/raw/repos/debs/ vscodium main' | sudo tee --append /etc/apt/sources.list.d/vscodium.list
sudo apt update && sudo apt install codium
```

Uninstall VS Code

```
sudo snap remove code
```

Install mongodb server ([steps](https://docs.mongodb.com/manual/tutorial/install-mongodb-on-debian/))

```
wget -qO - https://www.mongodb.org/static/pgp/server-4.2.asc | sudo apt-key add -
echo "deb http://repo.mongodb.org/apt/debian buster/mongodb-org/4.2 main" | sudo tee /etc/apt/sources.list.d/mongodb-org-4.2.list
sudo apt update
sudo apt install mongodb-org
```

-   start mongodb server
```
sudo service mongod start
```

Install tmux
```
sudo apt install tmux
```

Install chromium
```
sudo apt install chromium
```

Install pipenv (buckle in, this might get slightly confusing)
```
python3 -m venv ~/Applications/pipenv
source ~/Applications/pipenv/bin/activate
pip install pipenv
deactivate
sudo ln -s ~/Applications/pipenv/bin/pipenv /usr/local/bin/pipenv
```

Install pyaudio
```
sudo apt install python3-pyaudio
```

Install node-red via snap (actually not using yet)

- Install

	```
	snap install node-red
	```

- Firewall rule

	```
	sudo ufw allow from 127.0.0.1 port 1880 to 127.0.0.1 port 1880
	```

Install gnome-boxes

```
sudo apt install gnome-boxes
```

Install rust via script

```
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
```

Install GNU Plot

```
sudo apt install gnuplot
```

Install hyperfine via Cargo (hyperfine is used for benchmarking execution times)

```
cargo install hyperfine
```

Installed apt upgrades

Uninstall metasploit

```
sudo apt remove metasploit-framework
```

Download Slic3r as AppImage

- move AppImage into ~/Applications/

Uninstall lutris via apt (original repo is dead)

```bash
sudo apt remove lutris
sudo rm /etc/apt/sources.list.d/lutris.list /etc/apt/sources.list.d/lutris.list.save
```

Errors:
> Err:19 http://download.opensuse.org/repositories/home:/strycore/Debian_9.0 ./ Release> &nbsp;  404  Not Found [IP: 195.135.221.134 80]

> E: The repository 'http://download.opensuse.org/repositories/home:/strycore/Debian_9.0 ./ Release' no longer has a Release file.

Remove any lutris reminants

```bash
sudo apt update
sudo apt upgrade
sudo apt autoremove
```

Fix apt note during upgrading (unsupported arch for mongodb)

```bash
sudo nano /etc/apt/sources.list.d/mongodb-org-4.2.list
```

- change this line
	> `deb http://repo.mongodb.org/apt/debian buster/mongodb-org/4.2 main`
- to this (add architecture as _[arch=amd64]_)
	> `deb [arch=amd64] http://repo.mongodb.org/apt/debian buster/mongodb-org/4.2 main`

```bash
sudo apt update
```

Fix firefox _STILL_ not being updated

- despite trying to `sudo apt update`, `sudo apt upgrade`, `sudo apt full-upgrade` and `sudo apt dist-upgrade`'ing, firefox still won't update from 60.8.0esr-1~deb10u1 to 68.6.0esr-1~deb10u1

- will try marking it manually installed

	```bash
	sudo apt install firefox-esr
	```

- not luck

- **backed up tabs and stuff using firefox sync**

- going to uninstall and reinstall

	```bash
	sudo apt remove firefox-esr
	```

- wtf is going on, `sudo apt info firefox-esr` says 60.8.0esr-1~deb10u1 is latest, despite an `sudo apt update`??

- rebooting

- what the actual fuck

	> ```
	> denver@prion-debian10:~$ sudo apt install firefox-esr
	> Reading package lists... Done
	> Building dependency tree       
	> Reading state information... Done
	> Suggested packages:
	>   fonts-stix | otf-stix fonts-lmodern
	> E: Can't find a source to download version '60.8.0esr-1~deb10u1' of 'firefox-esr:amd64'
	> ```

- holy crap it was pinned by apt-listbugs! (seen in /etc/apt/preferences.d/apt-listbugs)

- remove the following content from `/etc/apt/preferences.d/apt-listbugs` (it is the entire content in this instance):

	```
	Explanation: Pinned by apt-listbugs at 2019-09-07 10:50:11 +1000
	Explanation:   #933757: Firefox-esr FTBFS "failed to open: /sbuild-nonexistent/.cargo/.package-cache"
	Package: firefox-esr
	Pin: version 60.8.0esr-1~deb10u1
	Pin-Priority: 30000
	```

- check it worked

	```
	sudo apt update
	sudo apt info firefox-esr
	```

- oh thank god it did

- update stuff and try again

	```
	sudo apt update
	sudo apt install firefox-esr
	```

- oh it seems to have worked, thank god for that (it even restored my tabs!)

- reboot just to be sure everything is working as intended

- looks good, think it's all fixed now

Do a sudo apt upgrade (2020/04/21-11:25) (24h time)

```
denver@prion-debian10:~$ sudo apt list --upgradable
Listing... Done
adoptopenjdk-8-hotspot/buster 8u252-b09-2 amd64 [upgradable from: 8u242-b08-2]
codium/unknown 1.44.2-1587206561 amd64 [upgradable from: 1.43.2-1585083818]
git-man/stable,stable 1:2.20.1-2+deb10u3 all [upgradable from: 1:2.20.1-2+deb10u2]
git/stable 1:2.20.1-2+deb10u3 amd64 [upgradable from: 1:2.20.1-2+deb10u2]
keybase/unknown 5.4.0-20200416162659.e81aee5eaf amd64 [upgradable from: 5.3.1-20200320154633.3e235215b3]
mongodb-org-mongos/buster 4.2.6 amd64 [upgradable from: 4.2.5]
mongodb-org-server/buster 4.2.6 amd64 [upgradable from: 4.2.5]
mongodb-org-shell/buster 4.2.6 amd64 [upgradable from: 4.2.5]
mongodb-org-tools/buster 4.2.6 amd64 [upgradable from: 4.2.5]
mongodb-org/buster 4.2.6 amd64 [upgradable from: 4.2.5]
virtualbox-6.0/unknown 6.0.20-137117~Debian~buster amd64 [upgradable from: 6.0.18-136238~Debian~buster]
```

- `sudo apt update`
- `sudo apt upgrade`

Install virt-manager

```
sudo apt install virt-manager
```

Install Cataclysm: Dark Days Ahead via snap store

```
snap install cataclysm
```

Uninstall keybase (heavily based on [these instructions](https://github.com/keybase/client/issues/7897#issuecomment-612992685))

- close keybase window, don't log out or exit keybase
- disconnect network
- kill all processes related to 'keybase' or 'kbfs'
- _"First, as described above"_
  - _"uninstall"_
	```
	sudo apt purge keybase
	sudo apt autoclean
	```
  - _"Remove user data at the risk of losing unrecoverable keys"_
  	```
  	rm -rf ~/.local/share/keybase ~/.config/keybase
  	```
  - _"Delete empty directory in root"_
  	```
  	sudo fusermount -u /keybase # I needed to do this first
  	sudo rm -r /keybase
  	```
- _"Then manually remove from package management"_
  - _"Remove from apt list"_
	```
	sudo rm /var/lib/apt/lists/prerelease.keybase*
	sudo rm /etc/apt/sources.list.d/keybase.list
	sudo rm /etc/apt/sources.list.d/keybase.list.save # step I did
	```
  - _"Untrust the key"_
	```
	apt-key list  # To get the following key
	sudo apt-key del 656D16C7
	```
- _"Finally, just to be sure..."_
  - reconnect network briefly for next step
	```
	sudo apt update
	```
  - disconnect network again
  - finish off with this stuff
	```
	sudo apt autoremove
	sudo apt autoclean
	sudo apt clean
	sudo dpkg --configure -a
	sudo apt -f install # I did this because I don't like apt/apt-get
	sudo apt-get -f install
	```
  - reboot

Update apt packages (19/05/2020)

```
sudo apt update
sudo apt upgrade
```

Disable middle-click paste in Firefox (was annoying in figma)

- navigate to about:config
- set the following to false
  - middlemouse.contentLoadURL (disables behaviour outside text-boxes) (was already false)
  - middlemouse.paste (text-boxes only) (was set true before change)

Update apt packages (23/05/2020)

```bash
sudo apt update
sudo apt upgrade
sudo apt autoremove # no changes
sudo apt autoclean # no changes
```

Update pcsx2 flatpak

```bash
flatpak update net.pcsx2.PCSX2
```

Update apt packages (24/06/2020)

```bash
sudo apt update
sudo apt upgrade
sudo apt autoremove # no changes
sudo apt autoclean # no changes
```

Update flatpaks (26/06/2020)

```bash
flatpak update com.spotify.Client
flatpak update tv.kodi.Kodi
flatpak update org.kicad_pcb.KiCad
flatpak uninstall --unused
```

Install GNU IceCat

- download icecat logo from [here](https://www.gnu.org/software/gnuzilla/icecat-128.png)
- `mkdir ~/Applications/.icons/`
- copy `icecat-128.png` to `~/Applications/.icons/`
- `ln -s /home/denver/Applications/.icons/icecat-128.png /home/denver/.icons/icecat-128.png`
- download icecat from GNU website
- extract to `~/Applications/icecat`
- create `~/.local/share/applications/icecat.desktop` with the following contents
  - ```toml
	[Desktop Entry]
	Type=Application
	Name=IceCat        
	GenericName=IceCat
	Comment=GNU IceCat browser                        
	Exec=/home/denver/applications/icecat/icecat 
	Icon=icecat-128.png
	#StartupWMClass=processing-app-Base
	```

Uninstall TeamViewer

```bash
sudo apt purge teamviewer
```

Update apt packages (24/06/2020)

```bash
sudo apt update
sudo apt upgrade
sudo apt autoremove # removed: qml-module-qtquick-dialogs qml-module-qtquick-privatewidgets
sudo apt autoclean # no changes
```

- then rebooted

Install syncthing

```bash
sudo apt install syncthing
```

Allow syncthing through ufw

```bash
sudo ufw allow syncthing
```

Clean up left-overs from lutris

```bash
rm -rf /home/denver/.cache/lutris
rm -rf /home/denver/.config/lutris
rm -rf /home/denver/lutris
rm -rf /home/denver/.local/share/lutris
rm -rf /home/denver/.local/share/applications/wine-extension-chm.desktop
rm -rf /home/denver/.local/share/applications/wine-extension-gif.desktop
rm -rf /home/denver/.local/share/applications/wine-extension-hlp.desktop
rm -rf /home/denver/.local/share/applications/wine-extension-htm.desktop
rm -rf /home/denver/.local/share/applications/wine-extension-ini.desktop
rm -rf /home/denver/.local/share/applications/wine-extension-jfif.desktop
rm -rf /home/denver/.local/share/applications/wine-extension-jpe.desktop
rm -rf /home/denver/.local/share/applications/wine-extension-msp.desktop
rm -rf /home/denver/.local/share/applications/wine-extension-pdf.desktop
rm -rf /home/denver/.local/share/applications/wine-extension-png.desktop
rm -rf /home/denver/.local/share/applications/wine-extension-rtf.desktop
rm -rf /home/denver/.local/share/applications/wine-extension-txt.desktop
rm -rf /home/denver/.local/share/applications/wine-extension-url.desktop
rm -rf /home/denver/.local/share/applications/wine-extension-vbs.desktop
rm -rf /home/denver/.local/share/applications/wine-extension-xml.desktop
```

Try integrating Volux with systemd

```bash
sudo rm -r /usr/local/bin/volux
sudo cp /home/denver/github/volux/voluxtilevis.service /etc/systemd/user
sudo cp /home/denver/github/volux/voluxbar.service /etc/systemd/user
sudo cp /home/denver/github/volux/dist/voluxtilevis /usr/local/bin
sudo cp /home/denver/github/volux/dist/voluxbar /usr/local/bin
systemctl --user disable voluxtilevis
systemctl --user enable voluxbar
```

Install Discord as flatpak (friend using Debian claimed they don't have memory leak issues I've been experiencing)

```bash
flatpak install discord
```

Uninstall Discord as snap (leave flatpak version)

```bash
sudo snap remove discord
```

Install apt-file (view contents of .deb files)

```bash
sudo apt update
sudo apt install apt-file
sudo apt-file update
```

Install v4l2loopback (create virtual camera inputs)

```bash
sudo apt install v4l2loopback-dkms
sudo modprobe v4l2loopback
```

Install obs-v4l2sink

- Download [obs-v4l2sink.deb](https://github.com/CatxFish/obs-v4l2sink/releases/download/0.1.0/obs-v4l2sink.deb)
- Put obs-v4l2sink.deb into ~/Applications/PACKAGES/

```bash
sudo apt install /home/denver/Applications/PACKAGES/obs-v4l2sink.deb
```

- Didn't work, at least it's not working without a reboot :\

Install obs-studio (snap version)

```bash
sudo snap install obs-studio
```
- [work-around for driver nvidia driver issue](https://github.com/snapcrafters/obs-studio/issues/68#issuecomment-670948068)
	```bash
	SOURCE="/usr/lib/x86_64-linux-gnu"
	DEST="/snap/obs-studio/current/usr/lib/x86_64-linux-gnu" # may need to change this
	sudo mount --bind $SOURCE/libGL.so.1 $DEST/libGL.so.1
	sudo mount --bind $SOURCE/libEGL.so.1 $DEST/libEGL.so.1.0.0
	sudo mount --bind $SOURCE/libGLX.so.0 $DEST/libGLX.so.0
	sudo mount --bind $SOURCE/nvidia/current/libGLX_nvidia.so.418.152.00 $DEST/libGLX_mesa.so.0 # needed to change numbers on end of first path
	```

Allow access to /dev/ttyUSB devices

```bash
sudo adduser $USER dialout
```

- reboot

Install screen to use REPL on ESP32

```bash
sudo apt install screen
```

Install PCSX2 as flatpak (leave other installed)

```bash
flatpak install flathub net.pcsx2.PCSX2
```

- might still need to create a .desktop file, but might appear after reboot anyway

Install kdenlive as flatpak (leave apt version installed for now)

```bash
flatpak install flathub org.kde.kdenlive
```

Update system packages (2020/09/24)

```bash
sudo apt update
```

Uninstall VirtualBox

- Open Virtual Box
- Navigate through the UI to File > Preferences > Extensions
- Uninstall any installed extensions (none in this instance)
- Check you've either backed-up or deleted all virtual machines at "~/VirtualBox VMs/"
- Check the output of virtualbox-* isn't overzealous
```bash
sudo apt list | grep virtualbox-*
```

- Purge virtualbox packages
```bash
sudo apt purge virtualbox-*
```

- Remove any remaining virtual machines
```bash
rm -r "VirtualBox VMs"/
```

- Clear any remaining configuration files
```bash
rm -rf /home/denver/.config/VirtualBox
```

- Check for any remaining processes/services
```bash
sudo ps aux | grep -i "vbox"
```

- Kill any "ghost" processes that appear to be relevant
```bash
sudo pkill VBox*
```

- Remove VirtualBox repository
```bash
sudo rm /etc/apt/sources.list.d/virtualbox.list /etc/apt/sources.list.d/virtualbox.list.save
```

- Autoremove any unnecessary packages
```bash
sudo apt autoremove
```

Resolve apt issue

- Update nvidia-docker signiture (was invalid) and run apt update
```bash
curl -s -L https://nvidia.github.io/nvidia-docker/gpgkey | sudo apt-key add -
sudo apt update
```

Upgrade apt packages

```bash
sudo apt update
sudo apt upgrade
```

Install Blender as Flatpak

```bash
flatpak install flathub org.blender.Blender
```

Install [xpadneo](https://github.com/atar-axis/xpadneo) and pair Xbox One controller

- Install
	```bash
	cd /home/denver/Applications
	sudo apt-get install dkms linux-headers-`uname -r` # already had both
	cd xpadneo
	```
- Connect (from xpadneo readme)
	```bash
	sudo bluetoothctl
	```
	```
	[bluetooth]# scan on
	```
    - wait until all available devices are listed (otherwise it may be hard to identify which one is the gamepad)
    - push the connect button on upper side of the gamepad, and hold it down until the light starts flashing fast
    - wait for the gamepad to show up in bluetoothctl, remember the address (e.g. C8:3F:26:XX:XX:XX)
	```
    [bluetooth]# pair <MAC>
    [bluetooth]# trust <MAC>
    [bluetooth]# connect <MAC>
	```
	- The <MAC> parameter is optional if the command line already shows the controller name
    - You know that everything works fine when you feel the gamepad rumble ;)

Reinstall Spotify via flatpak (cache is huge and can't clear it from app)

```bash
flatpak uninstall com.spotify.Client
```

- delete any large files/folders where it appears safe to do so under ~/.var/app/com.spotify.Client

```bash
flatpak install flathub com.spotify.Client
```

Enable sharing to recieve files over bluetooth (Cancelled)
- [Debian guide](https://wiki.debian.org/BluetoothUser#GNOME_topics)
- Settings > Sharing > File Sharing
  - Add password + enable using password
  - Enable one or more networks
- NEVERMIND: it looks like it's completely broken/abandoned on Debian 10
- Re-disable sharing
- Just use SyncThing instead

Install bpytop and alias it over htop

```bash
cd /opt
sudo su # DANGER ZONE!!!
python3 -m venv bpytop
source bpytop/bin/activate
pip install bpytop
deactivate
exit # Back to (relative) safety
sudo ln -s /opt/bpytop/bin/bpytop /usr/local/bin/
echo 'alias htop="bpytop"' >> /home/denver/.bash_aliases
```

Add debian backports for latest gamemode release

```bash
sudo nano /etc/apt/sources.list.d/backports.list
```
- add the follwing contents to `backports.list`:
    ```
	deb https://deb.debian.org/debian buster-backports main
	```
```bash
sudo apt update
```

Install gamemode from debian backports

```bash
sudo apt -t buster-backports install gamemode
```

Install gamemode gnome shell extension - **FAILED ATTEMPT**

- Visit https://extensions.gnome.org/extension/1852/gamemode/
- Download extension
  - Gnome version = 3.30
  - Extension version = 3
- Install extension manually
    ```bash
    unzip /home/denver/Downloads/gamemodechristian.kellner.me.v3.shell-extension.zip -d /home/denver/.local/share/gnome-shell/extensions/gamemode@christian.kellner.me/
    ```
- Restart gnome shell
  - Press Alt + F2
  - Type 'r' and press enter
- Patch config menu breaking because gnome 3.30 isn't supported (despite the metadata)
  - Note the following steps are through my own trial-and-error
  ```bash
  cp /usr/share/gnome-shell/extensions/alternate-tab@gnome-shell-extensions.gcampax.github.com/convenience.js /home/denver/.local/share/gnome-shell/extensions/gamemode@christian.kellner.me/
  ```
  - Edit source of `prefs.js`
    - Add the following line under `const Me = ExtensionUtils.getCurrentExtension();`
    ```js
	const Convenience = Me.imports.convenience; // Added by me
	```
	- Replacing each of the following instances like so:

    | Find                                | Replace                          |
    | ----------------------------------- | -------------------------------- |
    | `ExtensionUtils.initTranslations()` | `Convenience.initTranslations()` |
    | `ExtensionUtils.getSettings()`      | `Convenience.getSettings()`      |

  - Open tweak tool
  - Navigate to extensions > gamemode
  - Click the cog
  - Although the extension is still broken, settings menu works now!
- Remove extension
  ```bash
  cd /home/denver/.local/share/gnome-shell/extensions
  rm -rf gamemode\@christian.kellner.me/
  ```
- Restart gnome shell
  - Press Alt + F2
  - Type 'r' and press enter

Configure Noita on Steam to use gamemode + GL performance flags
- Go into Steam
- Find Noita
- Right-click, select "Properties..."
- Go to "General" > "SET LAUNCH OPTIONS..."
- Add the following: `__GL_THREADED_OPTIMIZATION=1 __GL_SHADER_DISK_CACHE=1 gamemoderun %command%`
- Press enter
- Close the window, etc.
- Launch the game
- 60 FPS, wooo!

Install [xow](https://github.com/medusalix/xow) for offical Xbox controller dongle support
- Install dependencies
  ```bash
  sudo apt install libusb-1.0-0-dev libusb-1.0-0
  ```
- Install xow
  ```bash
  cd /home/denver/Applications
  git clone https://github.com/medusalix/xow
  cd xow
  make BUILD=RELEASE
  sudo make install
  sudo systemctl enable xow
  sudo systemctl start xow
  ```
- Reboot

Install OpenSpades as flatpak

```bash
flatpak install flathub jp.yvt.OpenSpades
```

Install RetroArch as flatpak

```bash
flatpak install flathub org.libretro.RetroArch
```

Install Okular as flatpak

```bash
flatpak install flathub org.kde.okular
```

Update apt packages

```bash
sudo apt update
```

Install FileZilla

```bash
sudo apt install filezilla
```

Update tzdata package

```bash
sudo apt --only-upgrade install tzdata
```

Update NVIDIA drivers from backports (for RPCS3)

- change /etc/apt/sources.list.d/backports.list from this:
	```
	deb https://deb.debian.org/debian buster-backports main
	```
- to this
	```
	deb https://deb.debian.org/debian buster-backports main contrib non-free
	```

```bash
sudo apt update
sudo apt install linux-headers-amd64
sudo apt install -t buster-backports nvidia-driver
```

Install RPCS3 as flatpak

```bash
flatpak install flathub net.rpcs3.RPCS3
```

Reinstall Discord as flatpak

```bash
flatpak uninstall com.discordapp.Discord
flatpak install flathub com.discordapp.Discord
```

~~Download Freetube AppImage~~ (cancelled)

- Download `FreeTube-0.9.2.AppImage` from https://github.com/FreeTubeApp/FreeTube/releases
- Copy it to ~/Applications/AppImages
- Make it executable
  ```bash
  chmod +x /home/denver/Applications/AppImages/FreeTube-0.9.2.AppImage
  ```
- Double-click to use
- Getting sandbox error, giving up
- Delete `FreeTube-0.9.2.AppImage`

Install FreeTube as flatpak

```bash
flatpak install flathub io.freetubeapp.FreeTube
```

Install drawio as snap
```bash
sudo snap install drawio
```

Uninstall drawio as snap (my shell theme borks the UI)
```bash
sudo snap remove drawio
```

Download drawio as AppImage **(reverted)**
- Download from [here](https://github.com/jgraph/drawio-desktop/releases/download/v13.9.9/draw.io-x86_64-13.9.9.AppImage)
- Move to ~/Applications/AppImages/
- Make executable
- Delete draw.io-x86_64-13.9.9.AppImage (Fails to start due to sandbox stuff)

Install sshfs

```bash
sudo apt install sshfs
```

Update flatpak packages (all suceeded except spotify update, even after retry)

```bash
flatpak update
```

Install /e/ OS easy installer

```bash
sudo snap install easy-installer --channel=latest/beta
```

Install FreeCAD as flatpak

```bash
flatpak install flathub org.freecadweb.FreeCAD
```

Install Spotify-TUI as Snap

```bash
snap install spt
```

Install spotifyd

- Get binary from [here](https://github.com/Spotifyd/spotifyd/releases/download/v0.3.0/spotifyd-linux-full.tar.gz)
- Download it to `~/Applications/spotifyd-0.3.0/spotifyd-linux-full.tar.gz`
- Extract binary to same folder
- `cp ~/Applications/spotifyd-0.3.0/spotifyd /usr/bin/spotifyd`

Install secret-tool

```bash
sudo apt install libsecret-tools
```

Configure spotifyd

- Store password in keyring
  - `secret-tool store --label='spotifyd' application rust-keyring service spotifyd username <your-username>`
- `mkdir -p ~/.config/spotifyd`
- `nano ~/.config/spotifyd/spotifyd.conf`
	```toml
	[global]
	username = "<your-username>"
	use_keyring = true
	#use_mpris = true
	backend = alsa
	#device = "<your-audio-device>" # use aplay -L for a list of devices
	#mixer = "PCM"
	#volume_controller = "alsa"
	device_name = "spotifyd_debian10"
	bitrate = 320
	#cache_path = "<absolute-path-to-cache-dir>"
	no_audio_cache = true
	volume_normalisation = true
	normalisation_pregain = 0
	device_type = "speaker"
	```
- Install service file for systemd
  - `curl https://raw.githubusercontent.com/Spotifyd/spotifyd/master/contrib/spotifyd.service -o ~/.config/systemd/user/spotifyd.service`
- Start service and enable at login
  - `systemctl --user start spotifyd.service`
  - `systemctl --user enable spotifyd.service`

Install Boxtron (v0.5.4) manually (instructions from [here](https://github.com/dreamer/boxtron#installation-using-tarball-for-a-single-user))

1. Install Debian dependencies

	```bash
	sudo apt install dosbox inotify-tools timidity fluid-soundfont-gm
	```

2. Download and unpack tarball to `compatibilitytools.d` directory (create one if it does not exist):

	```bash
	cd ~/.local/share/Steam/compatibilitytools.d/ || cd ~/.steam/root/compatibilitytools.d/
	curl -L https://github.com/dreamer/boxtron/releases/download/v0.5.4/boxtron.tar.xz | tar xJf -
	```

3. Start/restart Steam.
4. In game properties window select "Force the use of a specific Steam Play
   compatibility tool" and select "Boxtron (native DOSBox)".

Install stretchly via snap + apt requirements

```bash
snap install stretchly
sudo apt install libappindicator1
sudo sysctl kernel.unprivileged_userns_clone=1 # needed to start stretchly w/o rebooting first
```

- Create or modify `/etc/sysctl.d/00-local-userns.conf`
  - Add the following line:
	```conf
	kernel.unprivileged_userns_clone=1
	```

Uninstall Slack (via snap)

```bash
snap remove slack
```

Install Rotki via AppImage
- Download from here: https://github.com/rotki/rotki/releases/download/v1.14.2/rotki-linux_x86_64-v1.14.2.AppImage
- Put in ~/Applications/AppImages
- Make executable
- Run it

Update apt packages

```bash
sudo apt update
sudo apt upgrade
```

Download Ledger Live as AppImage

- download from here: https://download-live.ledger.com/releases/latest/download/linux
- put in ~/Applications/AppImages
- make executable
- execute!

Debug ledger connection

- `wget -q -O - https://raw.githubusercontent.com/LedgerHQ/udev-rules/master/add_udev_rules.sh | sudo bash`
- working now!

Uninstall unused flatpaks

```bash
flatpak uninstall --unused
```

Update Discord flatpak

```bash
flatpak update com.discordapp.Discord
```

Update flatpaks

```bash
flatpak update
```

Autoremove apt packages

```bash
sudo apt autoremove
sudo apt autoclean
```

Uninstall gog-galaxy-wine and parsec snap

```bash
snap remove gog-galaxy-wine
sudo snap remove parsec
```

[2021/04/27] Download latest ledger live appimage, drop into ~/AppImages, make executable, ran it

[2021/05/25] Update Discord flatpak

```bash
flatpak update com.discordapp.Discord
```

[2021/05/26] Update Discord flatpak (again, lol)

```bash
flatpak update com.discordapp.Discord
```

[2021/05/31] Install obs-studio as flatpak

```bash
flatpak install flathub com.obsproject.Studio
```

[2021/07/02] update apt packages

```bash
sudo apt update
sudo apt upgrade
```

[2021/07/12] install wireless driver

```bash
# build
cd ~/github
git clone https://github.com/gnab/rtl8812au.git
cd rtl8812au/
make
# test
sudo insmod 8812au.ko
# -- test the wireless adapter now works, then continue
# install
sudo cp 8812au.ko /lib/modules/$(uname -r)/kernel/drivers/net/wireless
sudo depmod
# dkms
sudo apt install build-essential dkms # won't install anything if entire install history has been followed
sudo make dkms_install
echo 8812au | sudo tee -a /etc/modules
```

[2021/07/12] install OpenVPN + configure Mullvad stuff ([reference](https://mullvad.net/en/help/linux-openvpn-installation/))
```bash
sudo apt install openvpn network-manager-openvpn network-manager-openvpn-gnome
# when prompted about outstading bug by apt-listbugs, type y and hit enter
mkdir ~/Config
```

- Get VPN config files from https://mullvad.net/en/account/#/openvpn-config/?platform=android
  1. Platform - Android/Chrome OS
  2. Location - All countries - All Cities
  3. Download ZIP archive, put it in ~/Config
  4. Extract contents of ZIP archive into same directory as the archive (~/Config)
  5. Navigate to settings > network > VPN > + (add)
  6. Select "Import from file..."
  7. Select ~/Config/mullvad_config_android/mullvad_au_all.ovpn
  8. Set the username to your mullvad account number (check PW manager)
  9. Set the password to "m"
  10. Click "add"

```bash
sudo service network-manager restart
```

- Enable VPN
  1. Go to settings > network > VPN
  2. Enable mullvad_au_all VPN toggle

[2021/07/13] Adjust firewall for LIFX on new WiFi
```bash
sudo ufw allow from 10.0.0.0/24 port 56700
sudo ufw delete allow from 192.168.20.0/24 port 56700
```

[2021/08/12] Update PCSX2 flatpak

```bash
flatpak update net.pcsx2.PCSX2
```

[2021/08/18] update apt packages

```bash
sudo apt update
sudo apt upgrade
```

[2021/08/18] install krita

```bash
sudo apt install krita
```

[2021/08/28] update pipenv and pipenv venv's pip + setuptools + wheel versions

```bash
cd ~/Applications/pipenv
source bin/activate
# ENSURE-ENSURE ENSURE ENSURE YOU'RE IN THE VENV FIRST!
# Check with pip -V and pip3 -V!!!
pip install --upgrade pip setuptools wheel
pip install --upgrade pipenv
deactivate
# ENSURE, ENSURE, ENSURE YOU'VE LEFT THE VENV!!
```

[2021/10/07] update packages

```bash
sudo apt update
sudo apt upgrade
```

[2021/10/08] update packages + flatpaks

```bash
flatpak update
sudo apt update
sudo apt upgrade
flatpak uninstall --unused
flatpak update
```

[2021/11/08] install mattermost flatpak, uninstall after running (crashes)

```bash
flatpak install flathub com.mattermost.Desktop
flatpak uninstall com.mattermost.Desktop
```

[2021/11/08] download mattermost as linux executable

- Download `https://releases.mattermost.com/desktop/5.0.1/mattermost-desktop-5.0.1-linux-x64.tar.gz`
- Verify sha256 checksum `c040a619cb9c014059bcfb9b207854dd034028cef4c169917c48ddda6650d866`
- Extract `mattermost-desktop-5.0.1-linux-x64.tar.gz` to `~/Applications/mattermost`
- Make .desktop file executable
- Execute .desktop file from terminal

[2021/11/17] download + install yt-dlp because youtube-dl speeds are borked

```bash
cd ~/Applications
python3 -m venv yt-dlp
cd yt-dlp
source bin/activate
pip install yt-dlp
deactivate
```

[2021/11/17] make stretchly start on boot (and fix launching from system search)

```bash
ln -s /var/lib/snapd/desktop/applications/stretchly_stretchly.desktop ~/.config/autostart/stretchly_stretchly.desktop
rm ~/.config/autostart/stretchly_stretchly.desktop # realized that their packaged .desktop file doesn't work (crashes)
```

- Create derivative of icon with band-aid over it to distinguish between the packaged version and mine
  - Stretchly icon can be found under `/snap/stretchly/6/meta/gui/icon.png`
- Write own .desktop file that works (using derivative icon)
  - Stretchly .desktop file can be found under `/var/lib/snapd/desktop/applications/stretchly_stretchly.desktop`

```bash
mkdir -p ~/Applications/PATCHES/stretchly
cp /var/lib/snapd/desktop/applications/stretchly_stretchly.desktop ~/Applications/PATCHES/stretchly/
```

- Copy derivative icon to `~/Applications/PATCHES/stretchly/icon_fixed.png`
- Replace contents of `~/Applications/PATCHES/stretchly/stretchly_stretchly.desktop` with the following:

```
[Desktop Entry]
Type=Application
Name=Stretchly
GenericName=Break Reminder
Comment=The break time reminder app
Icon=/home/denver/Applications/PATCHES/stretchly/icon_fixed.png
Exec=/snap/stretchly/current/stretchly %u
Terminal=false
StartupWMClass=Stretchly
Categories=Utility;
```

- Create symlink to new .desktop file

```bash
ln -s ~/Applications/PATCHES/stretchly/stretchly_stretchly.desktop ~/.local/share/applications
```

- WOOOOOO Launching from gnome system search works, icon and all! (may take a sec to update)
- Copy .desktop file to autostart Stretchly!

```bash
cp ~/Applications/PATCHES/stretchly/stretchly_stretchly.desktop ~/.config/autostart/
chmod -x ~/.config/autostart/stretchly_stretchly.desktop # don't make executable, all other files weren't executable either
```

[2021/11/17] remove keybase autostart entry

```bash
rm ~/.config/autostart/keybase_autostart.desktop
```
