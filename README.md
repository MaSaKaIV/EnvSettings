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
1. Settings of Network
2. Settings of Mirror
3. Create user and group
4. Add _user_ to sudoers
5. sudo Pacman -Syyu
6. Font Settings
* xorg : noto-fonts
* terminal : ttf-ricty
8. Settings of Window Manager

| Category | Mine |
|:-:|:-:|
| Video Driver | xf86-video-intel |
| Window Manager | i3 |
| Display Manager | LightDM |
| Launcher | rifo |
| status bar | i3blocks |

```※xorg-apps instead of xorg-server-utils 2021/10/31```

7. FireWall Settings

```Set Unmute with amixer```

8. Input Device

1. sudo pacman -S zsh
2. chsh -s $(which zsh)
3. vi ~/.zshrc
4. ~/.zshrc
autoload -U compinit && compinit
PROMPT='%n@%m:%~ %# '
export EDITOR=nano
5. exec zsh -l
6. sudoedit /etc/pacman.conf
- #Color
+ Color
6. sudo pacman -S git
8. mkdir ~/src
9. git clone https://aur.archlinux.org/yay-bin.git
10. makepkg -si
11. sudo pacman -s noto-fonts noto-fonts-cjk noto-fonts-emoji noto-fonts-extra
12. sudo pacman -S ttf-ricty
