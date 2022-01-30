# Customize xendom and retina resolution

`/etc/X11/xenodm/` holds all the `xenodm` files, and `/etc/X11/xenodm/xenodm-config`
defines all config files to use. They will be loaded or executed in order
like below:

- `/etc/X11/xenodm/Xservers` to start a `X` server (for rendering).

- `/etc/X11/xenodm/Xsetup_0` to setup the screen for the `xlogin` widget,
you can put screen settings here, like: screen background color or `dpi`
(but NOT the resolution!!!). This script runs as `root`.

- `/etc/X11/xenodm/Xresources` affects the user login window, customize login
UI here.

    The `Xresources` file is loaded onto the display as a resource database
    using `xrdb(1)`.  As the authentication widget reads this database before
    starting up, it usually contains parameters for that widget:

    ```bash
    xlogin*login.translations: #override\
            <Key>F1: set-session-argument(failsafe) finish-field()\n\
            <Key>Return: set-session-argument() finish-field()
    xlogin*borderWidth: 3
    xlogin*greeting: CLIENTHOST
    #ifdef COLOR
    xlogin*greetColor: CadetBlue
    xlogin*failColor: red
    #endif
    ```

    For more detail explaination for all `xlogin*XXX` var info, just read
    it from `man xenodm`, it's super detail info there:)

    </br>

- `/etc/X11/xenodm/Xstartup` runs as `root` after user login.

- `/etc/X11/xenodm/Xsession` runs as the login user.

    It will load and add the ssh key in `$HOME/.ssh/` if exists. Then it
    will load `~/.xsession` if exists. Usually, put the user specified
    settings and start window manger in `~/.xession`. If `~/.xsession`
    and `~/.Xresources` exists at the same time, then only `~/.xsession`
    will be used, that's why you won't see `~/.Xresources` be executed.

    </br>

If you found `xenodm` runs not correctly, you can check the `/var/log/xenodm.log`
to figure out what's wrong with your settings. It only contains the last run log,
no history log.

</br>
