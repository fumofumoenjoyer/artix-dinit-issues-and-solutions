# artix-dinit-issues-and-solutions
Some issues and solutions i found designed for absolute noobs like me.

## Cant run trasditional systemd services
You need to create a custom service or get a package for the service you want for example:
```
sudo pacman -S sddm-dinit
```
This package adds an sddm service on this directory ```/etc/dinit.d/sddm```

dinit services dont have an extension.
the sddm service looks like this:

```
type            = process
command         = /usr/bin/sddm
smooth-recovery = true
depends-on      = logind
depends-on      = sddm-pre
depends-on      = login.target

```
This is an example of my custom [nvidia-dinit service](https://github.com/fumofumoenjoyer/nvidia-service) which i use to set custom clock speeds and power limits on my GPU when i log in since the nvidia-smi doesnt keep the config:
```
type            = scripted
command         = /usr/bin/sh /usr/local/sbin/nvidia.sh
smooth-recovery = true
logfile         = /var/log/dinit/nvidia-dinit.log
depends-on      = login.target
```
This service is a scripted type, which means that its a oneshot, which means it runs once everytime i login or boot (depending on the config)


## Thunar doesnt detect gvfs:
I noticed thunar didnt show up my removable media or the trash folder, after some websearches i stumbled upon this error message on Thunar's preferences

![image](https://github.com/user-attachments/assets/90491653-2117-4d6f-aac5-92b479d301f3)

This is most probably because dinit cant enable the dbus service as an user, to fix this install this:
```
sudo pacman -S turnstile turnstile-dinit
```
Then enable the turnstiled service like this:
```
dinitctl enable turnstiled
```
Relogin or Reboot and check the output of ```dinitctl list```
```
[[+]     ] boot
[{+}     ] system
[{+}     ] dbus
```
It it looks like that the issue should be fixed.

## No emoji fonts
Follow this repo: https://github.com/androlabs/emoji-archlinux


## This is it for now, i'll be back.
