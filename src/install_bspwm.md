# Install bspwm window manager

- Install

    ```bash
    doas pkg_add bspwm sxhkd alacritty feh polybar dmenu
    ```

    After the user logs in from `xenodm`, the `/etc/X11/xenodm/Xsession`
    script checks whether there is a `$HOME/.xsession` script and call it
    if it exists. That's the file that we can place all personal X
    window settings and start the `bspwm` window manager:

    ```bash
    # Localization
    echo "export LC_CTYPE=\"en_US.UTF-8\"" >> ~/.xsession

    # Start bspwm
    echo "bspwm" >> ~/.xsession
    ```

    </br>


- Tricky things

    Plz make sure the `chmod +x ~/.config/bspwm/bspwmrc`!!!

    Plz make sure the `chmod +x ~/.config/bspwm/bspwmrc`!!!

    Plz make sure the `chmod +x ~/.config/bspwm/bspwmrc`!!!

    Otherwise, the `bspwm` runs but can't load the configuration file, as
    that configuration `it's a shell script`!!!! It can't run without the
    `+x` attribute!!!

    </br>


- Let `xsession` run `bspwm`

    After restoring your `bspwm` and `sxhkd` configuration file, add the
    following content to `~/.xsession`:

    ```bash
    export SXHKD_SHELL='/bin/sh'
    export LC_CTYPE="en_US.UTF-8"


    # ------------------------------------------------------------------
    # Open keyboard symbol output for debugging purpose if your hotkey
    # not correctly.
    # ------------------------------------------------------------------
    # xev > ~/temp/xev.log &


    # ------------------------------------------------------------------
    # Open the default terminal for debugging purpose if you can't
    # run your window manager correctly.
    # ------------------------------------------------------------------
    # alacritty &

    # ------------------------------------------------------------------
    # Start window manager
    # ------------------------------------------------------------------
    bspwm -c ~/.config/bspwm/bspwmrc 2>~/.bspwm.err >~/.bspwm.out
    ```

    Check the `~/.bspwm.err` if it doesn't run correctly.

    </br>


    Restart `xendo` display manager to take effect
    ```bash
    doas rcctl restart xenodm
    ```

    </br>

- How to debug if window manger doesn't run correctly

    After user loing success, `xenodm` will run `/etc/X11/xenodm/Xsession`
    and it will run your `$HOME/.xsession` (if exists).

    If error happen or you can't login, for example, the window
    manager you set in `$HOME/.xsession` can't run correctly,
    you can checkout the `$HOME/.xsession-errors` to figure out
    what's happening:)

    </br>

    **Important tips:**

    _**If you can't login into X and it said `forbidden`, then you need to
    double-check that whether your default shell is in the `/etc/shells` or
    not!!!**_

    </br>


- Copy custom font to `~/.fonts`

    You need to copy the custom font to `~/.fonts` folder and run
    the following commands:

    ```bash
    mkdir ~/.fonts
    cp -rvf /home/wison/my-shell/backup/nerd-font-patched-sauce-code-pro/* ~/.fonts/

    # Update font cache
    doas fc-cache

    # Restart xendo display manager to take effect
    doas rcctl restart xenodm
    ```

    For more details, run `man fonts` and read the `INSTALLING FONTS`
    section.

    </br>
