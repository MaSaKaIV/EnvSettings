# EnvSettings Memo
An overview of the environment about Windows and Archlinux Duealboot is given below.

## Setup DualBoot
Basically, it follows URL1 and URL2. For installation notes, see the section below.

[URL1:Archlunux Intall Guide](https://wiki.archlinux.org/title/installation_guide)

[URL2:Archlinux and Windows Dualboot JP](https://wiki.archlinux.jp/index.php/Windows_%E3%81%A8_Arch_%E3%81%AE%E3%83%87%E3%83%A5%E3%82%A2%E3%83%AB%E3%83%96%E3%83%BC%E3%83%88)

### Notes
* As mentioned in URL2, if you install Windows first in a UEFI environment, the efi partition already exists, so you should not create a new partition on Archlinux, but mount /mnt/boot/efi on the existing efi partition.
* In my case, after installing os-prober, GRUB_DISABLE_OS_PROBER was not listed in /etc/default/grub, so I edited the status of GRUB_DISABLE_OS_PROBER in /usr/bin/grub-mkconfig.

## Setup instructions on Archlinux
**My Custom List**
| Category | Mine |
|:-:|:-:|
| Video Driver | xf86-video-intel |
| Window Manager | i3 |
| Display Manager | LightDM |
| Launcher | rifo |
| status bar | i3blocks |

Network connection using a wireless LAN.
```sh
$ systemctl enable NetworkManager
$ systemctl start NetworkManager
```

Add main user.
```sh
$ useradd -d /home/USERNAME -s /bin/bash -m USERNAME
$ passwd USERNAME
$ usermod -aG wheel USERNAME
$ EDITOR=vi visudo
/etc/sudoers
- # %wheel ALL=(ALL) ALL
+ %wheel ALL=(ALL) ALL
```

Edit /etc/pacman.conf for colourising the output of pacman.
```sh
$vi /etc/pacman.conf
/etc/pacmand.conf
- #Color
+ Color
+ ILoveCandy
```

Reboot then login to main user.

Update package data server 
```sh
$ sudo pacman -Syu
```

Install shell you wanna use then change login shell.
And set optional settings in zshrc.
Finally, log in to the shell.
``` sh 
$ sudo pacman -S zsh 
$ chsh -s $(which zsh)
$ vi ~/.zshrc
~/.zshrc
+ autoload -U compinit && compinit
+ PROMPT='%n@%m:%~ %# '
+ export EDITOR=vi

$ exec zsh -l
```

Intall git for getting yay.
```sh
$ sudo pacman -S git
```

Clone yay-bin then built it.
```sh
$ mkdir ~/pacsrc
$ git clone https://aur.archlinux.org/yay-bin.git
$ makepkg -si
```

Intall fonts for xorg.
```sh
$ sudo pacman -s noto-fonts noto-fonts-cjk noto-fonts-emoji noto-fonts-extra
$ yay -S ttf-ricty
```

Intall display server.
```sh
$ sudo pacman -S xorg-server
```

Intall to the keyboat layout you use.
And change to HOMELANGUAGE in all of system locale
```sh
$ sudo localectl set-x11-keymap jp
$ sudoedit /etc/locale.conf
/etc/locale.conf
- LANG=en_US.UTF-8
+ LANG=ja_JP.UTF-8
```

Intall display manager then Enable LightDM.
And Install lightdm-theme.
```sh
$ sudo pacman -S lightdm lightdm-gtk-greeter
$ sudo systemctl enable lightdm
$ sudo yay -S lightdm-webkit-theme-osmos
```

Intall window manager.
```sh
$ sudo pacman -S i3
```

Intall terminal emulator and launcher for i3.
Then, Custom rofi settings.
```
$ sudo pacman -S cool-retro-term rofi
$ mkdir ~/.config/rofi
$ vi ~/.config/rofi/config.rasi
~/.config/rofi/config.rasi
configuration {
    theme: "slate-mod";
    modi: "combi,drun,run,window";
    combi-modi: "drun,run,window";
    show-icons: true;
}

$ vi ~/.config/rofi/slate-mod.rasi
~/.config/rofi/slate-mod.rasi
* {
    background-color: #282C33;
    border-color: #2e343f;
    text-color: #8ca0aa;
    spacing: 0;
    width: 640px;
}

inputbar {
    border: 0 0 1px 0;
    children: [prompt, entry];
}

prompt {
    padding: 16px;
    border: 0 1px 0 0;
}

textbox {
    background-color: #2e343f;
    border: 0 0 1px 0;
    border-color: #282C33;
    padding: 8px 16px;
}

entry {
    padding: 16px;
}

listview {
    cycle: false;
    margin: 0 0 -1px 0;
    scrollbar: false;
}

element {
    border: 0 0 1px 0;
    padding: 8px;
}

element-text {
    background-color: inherit;
    text-color: inherit;
    padding: 0 0 0 8px;
}

element selected {
    background-color: #2e343f;
}

element-icon {
    size: 24px;
    background-color: inherit;
}
```

enable to call rofi from i3
```sh
$ vi ~/.config/i3/config
~/.config/i3/config
- bindsym $mod+d exec --no-startup-id dmenu_run
+ bindsym $mod+d exec --no-startup-id "rofi -show combi"
```

Change status bar from i3status to i3blocks
```sh
$ mkdir ~/.config/i3blocks
$ vi ~/.config/i3blocks/config
~/.config/i3blocks/config
separator_block_width=16

[load average]
command=uptime | awk -F': ' '{print $2}'
interval=5

[time]
command=date '+%m/%d(%a) %H:%M'
interval=1
```

Make script for Show to check for package updates.
```sh
$ vi ~/.config/i3blocks/check-updates.sh
~/.config/i3blocks/check-upates.sh
#!/bin/sh

# Count number of updates
updatenum=$({ checkupdates; paru -Qua } | wc -l)

# Print
case $updatenum in
  0) echo 'System is up to date' ;;
  1) echo "$updatenum update available" ;;
  *) echo "$updatenum updates available" ;;
esac

$ chmod +x ~/.config/i3blocks/check-updates.sh
```

Add new settings in i3blocks config
```sh
$ vi ~/.config/i3blocks/config
~/.config/i3blocks/config
separator_block_width=16

+ [updates]
+ command=./check-updates.sh
+ interval=3600

[load average]
command=uptime | awk -F': ' '{print $2}'
interval=5

[time]
command=date '+%m/%d(%a) %H:%M'
interval=1
```

Change to use i3blocks instead of i3status
```sh
bar {
-        status_command i3status
+        status_command i3blocks
}
```

Set fonts for xorg.
```sh
$ sudoedit /etc/fonts/local.conf
/etc/fonts/locale.conf
<?xml version="1.0"?>
<!DOCTYPE fontconfig SYSTEM "fonts.dtd">
<fontconfig>
  <alias>
    <family>serif</family>
    <prefer>
      <family>Noto Serif</family>
      <family>Noto Serif CJK JP</family>
    </prefer>
  </alias>
  <alias>
    <family>sans-serif</family>
    <prefer>
      <family>Noto Sans</family>
      <family>Noto Sans CJK JP</family>
    </prefer>
  </alias>
  <alias>
    <family>monospace</family>
    <prefer>
      <family>Noto Sans Mono</family>
      <family>Noto Sans Mono CJK JP</family>
    </prefer>
  </alias>
</fontconfig>

$ fc-cache -f
```

Intall Web brawser
```sh
$ sudo pacman -S chromium
```

Intall your HOMELANGUAGE
```sh
$ sudo pacman -S fcitx fcitx-mozc fcitx-im fcitx-configtool
$ vi ~/.xprofile
~/.xprofile
export GTK_IM_MODULE=fcitx
export QT_IM_MODULE=fcitx
export XMODIFIERS=@im=fcitx
fcitx
```

Intall Sound Server
```sh
$ sudo pacman -S pulseaudio pavucontrol
```

Set wallpapaer.
You just open image file then Specify the wallpaper you want to use.
```sh
$ sudo pacman -S archlinux-wallpaper
$ sudo pacman -S feh
$ feh /usr/share/backgrounds/archlinux
$ vi ~/.xprofile
sh ~/.fehbg
```

```※xorg-apps instead of xorg-server-utils 2021/10/31```
