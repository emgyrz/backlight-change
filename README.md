# backlight-change

The script for manipulating (set/increase/decrease/store/resume) the brightness (exact value or percent) of the screen using `/sys/class/backlight` device files.

`backlight-change --help` output:
```sh

Usage: backlight-change [VALUE] [arguments] [command]

The VALUE can be specified as a percentage or an exact value.
If it is prefixed with - or +, the brightness will be reduced or increased based on the current value. Otherwise, the specified value will be set.

Arguments:
  -h, --help        Show this message and exit
  -d DEV_NAME       Specify the device to work with. By default first device in /sys/class/backlight/ is used
  -f FILE_NAME      Specify the file to save the value to and from which to take the value. By default - /home/mz/.backlight_stored_value

Commands:
  store             Store current brightness value
  resume            Set current brightness value from stored value

Examples: backlight-change 50%
          backlight-change +10% -d amdgpu_bl0
          backlight-change -33
          backlight-change store
          backlight-change resume -f ~/.config/backlight_stored_value

```

## Install

Put the `backlight-change` file somewhere where your `$PATH` variable points. For example, `~/.local/bin/`

```sh
cd /tmp &&\
    git clone --depth=1 https://github.com/emgyrz/backlight-change.git backlight-change &&\
    cd $_ &&\
    cp ./backlight-change ~/.local/bin/ &&\
    echo Done
```

#### It will be good 
...to grant the permission to user

For `systemd` users from [ArchWiki](https://wiki.archlinux.org/title/backlight#ACPI):

> By default, only root can change the brightness by this method. To allow users in the video group to change the brightness, a udev rule such as the following can be used (Logging out/Rebooting may be necessary to changes take effects):
>
> `/etc/udev/rules.d/backlight.rule`
>
> `ACTION=="add", SUBSYSTEM=="backlight", RUN+="/bin/chgrp video $sys$devpath/brightness", RUN+="/bin/chmod g+w $sys$devpath/brightness"`

Or run same commands sometime on system boot. E.g. `openrc` user can create file in `/etc/local.d/` with `.start` postfix. E.g.

`/etc/local.d/backlight_rights.start`
```sh
#!/bin/sh

/bin/chgrp video /sys/class/backlight/*/brightness
/bin/chmod g+w /sys/class/backlight/*/brightness
```
___

...and setup key bindings. i3 config example
```sh

bindsym XF86MonBrightnessUp exec --no-startup-id backlight-change +5%
bindsym XF86MonBrightnessDown exec --no-startup-id backlight-change -5%

```



## License
MIT




