jfly/dotfiles
=============

I bought a new laptop, so it felt like it's finally time to do this.

I'm using the excellent [dotbot](https://github.com/anishathalye/dotbot) to
manage everything. Just git clone, and run the `./install` script!

# Directions for fresh Arch install

- `echo "blacklist pcspkr" > /etc/modprobe.d/nobeep.conf` - disable system beep
- `pacman -S vim && mv /usr/bin/vi /usr/bin/vi.bak && ln -s /usr/bin/vim /usr/bin/vi` - install and set up vim as default

- `pacman -S sudo && visudo` - install and configure sudo
- `useradd -m -G wheel -s /bin/bash jeremy && passwd jeremy` - create user and set their password

- `pacman -S git python` - install dependencies to install jfly/dotfiles
- `git clone https://github.com/jfly/dotfiles.git && cd dotfiles && ./install`

## Audio
- https://aur.archlinux.org/packages/asoundconf - http://unix.stackexchange.com/a/146297
- Headphone noise is due to power_save mode - https://bbs.archlinux.org/viewtopic.php?pid=1554497#p1554497
- TODO: use pulseaudio instead of just alsa
- Install [pasystray](https://aur.archlinux.org/cgit/aur.git/snapshot/pasystray.tar.gz)
- `pacman -S pamixer`

## kaladin specific

- `echo "options psmouse proto=imps" > /etc/modprobe.d/psmouse.conf` - http://natalian.org/archives/2015/02/18/Archlinux_on_a_Lenovo_X1C3/
  - Note: not needed with newer kernels.
- https://bbs.archlinux.org/viewtopic.php?pid=1492564#p1492564
~/thirdrepos/downgrader @kaladin> cat /proc/sys/net/core/wmem_max
212992
~/thirdrepos/downgrader @kaladin> echo 83886080 > /proc/sys/net/core/wmem_max
bash: /proc/sys/net/core/wmem_max: Permission denied
~/thirdrepos/downgrader @kaladin> sudo bash -c "echo 83886080 > /proc/sys/net/core/wmem_max"
~/thirdrepos/downgrader @kaladin> # http://www.linuxquestions.org/questions/linux-networking-3/sendmsg-no-buffer-space-available-334631/
~/thirdrepos/downgrader @kaladin> cat /proc/sys/net/core/wmem_max83886080
~/thirdrepos/downgrader @kaladin> 


## Power stuff
- Install from AUR: `https://aur.archlinux.org/packages/laptop-mode-tools/`
- `pacman -S acpi acpid ethtool wireless_tools`
- `systemctl enable laptop-mode`
- Edit /etc/laptop-mode/laptop-mode.conf accordingly (ttps://push.cx/2015/dual-booting-arch-linux-on-lenovo-x1-carbon-3rd-gen suggests changing LM_BATT_MAX_LOST_WORK_SECONDS)


## Setting up x11 and xmonad
- `pacman -S xorg-server xorg-xinit xorg-xsetroot xorg-xmodmap xorg-xmessage xorg-xrandr xorg-xrdb xmonad xmonad-contrib xmobar feh roxterm wmname network-manager-applet openssh alsa-utils scrot xclip numlockx xvkbd xsel`
- [trayer-srg](https://aur.archlinux.org/packages/trayer-srg-git/)
- Install the appropriate [video card driver](https://wiki.archlinux.org/index.php/xorg#Driver_installation)
- [dmenu2](https://aur.archlinux.org/packages/dmenu2/)

## AUR utils
- `pacman -S wget base-devel`

## Install volnoti
- Install [hcchu/volnoti](https://github.com/hcchu/volnoti#new-options-in-this-fork) from github. [volnoti](https://aur.archlinux.org/packages/volnoti) doesn't have the features needed for volnoti-brightness.
- Install bc.

## Fonts
- `pacman -S ttf-liberation ttf-bitstream-vera`
- Install [ttf-google-fonts-git](https://aur.archlinux.org/packages/ttf-google-fonts-git/) from the AUR

## Setting up wireless with network manager
- `pacman -S networkmanager network-manager-applet`
- `systemctl enable NetworkManager.service && systemctl start NetworkManager.service`

## wrk
- `pacman -S mosh networkmanager-vpnc`

## Misc
- [Generate ssh keys](https://help.github.com/articles/generating-ssh-keys/)
- `pacman -S ctags && sudo npm install -g git://github.com/ramitos/jsctags.git` - for vim tagbar plugin
- `pacman -S the_silver_searcher` - for faster ctrl+p in vim
- `timedatectl set-ntp true` - enable time sync
- Install [google-chrome](https://aur.archlinux.org/packages/go/google-chrome/google-chrome.tar.gz) from the AUR.
- Prevent autosuspend of usb mouse: https://fitzcarraldoblog.wordpress.com/2013/02/26/how-to-prevent-a-usb-mouse-auto-suspending-in-linux-when-a-laptops-power-supply-is-disconnected/
- [byzanz](https://aur.archlinux.org/packages/byzanz/)
  - [xrectsel](https://aur.archlinux.org/packages/xrectsel/)
- Set default browser:
  - ~ @kaladin> xdg-settings set default-web-browser chromium.desktop
    xdg-settings: $BROWSER is set and can't be changed with xdg-settings
  - ~ @kaladin> unset BROWSER
  - ~ @kaladin> xdg-settings set default-web-browser chromium.desktop
  - ~ @kaladin> xdg-mime query default text/html
    chromium.desktop
- Add `"detachKeys": "ctrl-^,q"` to `~/.docker/config.json`


## Lenovo specific
- Fix trackpoint middle button scroll by creating a `/etc/X11/xorg.conf.d/20-trackpoint.conf` as per https://wiki.archlinux.org/index.php/Lenovo_ThinkPad_T410.
- Enable [synaptics touchpad](https://wiki.archlinux.org/index.php/Touchpad_Synaptics)
    - `pacman -S xf86-input-synaptics`

## TODO
- When switching monitors, change DPI and update running applications
  - http://unix.stackexchange.com/questions/12613/is-there-a-way-to-find-all-x-resources-an-application-uses
  - backup plan: chrome://settings/search#zoom
