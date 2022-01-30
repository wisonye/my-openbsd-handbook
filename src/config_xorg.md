# Config Xorg

Here are the man pages for all information:

```bash
man Xserver
man Xorg
man xorg.conf
```

important config file:

`/etc/X11/xinit/xinitrc`

</br>

- Enable GPU configuration

    All GPU example config files you can found in `/usr/X11R6/share/X11/xorg.conf.d/`

    Just copy one of them to `/etc/X11/xorg.conf.d`

    How to confirm what driver should be used to?

    ```bash
    # Search in the `dmesg`
    dmesg | grep wsdisplay

    # wsdisplay0 at inteldrm0 mux 1: console (std, vt100 emulation), using wskbd0
    ```

    Or

    ```bash
    dmesg | grep intel

    # inteldrm0 at pci0 dev 2 function 0 "Intel Iris Pro Graphics 5200" rev 0x08
    # drm0 at inteldrm0
    ```

    Or you can use `fw_update` to confirm, as it deteced your hardware and
    download the particular firemware automatic:

    ```bash
    doas fw_update

    # fw_update: added none; updated intel,inteldrm; kept bwfm,vmm
    ```

    That means use `intel` driver and `inteldrm0` is the device Identifier.

    </br>

    MacBooPro 2015 use `Iris(TM) Pro Graphics: 5200/6200/P6300`, that means
    use `intel` driver (Intel integrated graphics chipsets).

    Create `/etc/X11/xorg.conf.d/20-intel.conf` with the following settings:

    ```bash
    Section "Device"
        Identifier  "inteldrm0"
        Driver      "intel"
        Option      "TearFree" "true"
    EndSection
    ```

    restart to take effect.

    </br>

- Font scale issues

    If you see font too small on Rentia screen or any screen, then Add the
    following settings to `~/.Xdefaults`:

    ```bash
    Xft.dpi:192
    ```

    </br>


    Or try any one settings below to see which one fits your screen:

    `Xft.dpi` controls the font scale like below:
    
    - `96` - base
    - `120` - scale 25%
    - `144` - scale 50%
    - `168` - scale 75%
    - `192` - scale 100%

    Restart X to take effect!
    
    </br>

- Resolution

    How to get and change the screen resolution:

    ```bash
    # Query current resolution and supported resolution
    # `+` means preferred mode
    # `*` means current mode
    xrandr --query

    # Screen 0: minimum 8 x 8, current 2880 x 1800, maximum 32767 x 32767
    # eDP1 connected primary 2880x1800+0+0 (normal left inverted right x axis y axis) 331mm x 207mm
    #    2880x1800     59.99*+
    #    2880x1620     59.96    59.97
    #    2560x1600     59.99    59.97
    #    2560x1440     59.96    59.95
    #    2048x1536     60.00
    ```

    How to set it?

    ```bash
    xrandr --output eDP1 --mode 2560x1440
    ```
    `eDP1` is the connected screen to set, then follow by the valid mode (resolution).

    </br>
