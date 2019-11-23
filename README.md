# Debian 10 Install Steps and History
## General tips and commands
### Bash
#### View command history
`$ ~/.bash_history (inspect contents)`
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

Install RPCS3 as AppImage
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
