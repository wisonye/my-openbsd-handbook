# How to upgrade to `current` version

- The versions you need to know:

    There are three flavors of OpenBSD:

    - `release`: The version of OpenBSD shipped every six months.

    - `current`: The development branch. Every six months, `-current` is tagged and
    becomes the next `-release`.

    - `stable`: The `-release` branch, plus patches found on the errata page. When
    very important fixes are made to `-current`, they are backported to the supported
    `-stable` branches.

    </br>

- Before upgrading

    Before you upgrade, plz make sure to check available disk space in `/usr`.
    Verify that the `/usr` partition has a size of at least 1.1G. With less space
    the upgrade may fail and you should consider reinstalling the system instead.

    The easy way to find out is running the following command:

    ```bash
    df -h | grep "/usr"

    # /dev/wd0f      2.3G    1.7G    481M    79%    /usr
    ```

    Removed to free some space before the upgrade.

    </br>

- Do the auto upgrade by running `sysupgrade`

    If you prefer to run interactive upgrade or manual upgrade, plz read from
    [here](https://www.openbsd.org/faq/upgrade70.html).

    ```bash
    doas sysupgrade -s
    ```

    It will reboot after done.

    </br>

- After upgrading

    Loign and run the following commands:

    ```bash
    # Upgrade all installed packages (some packages you have to re-install)
    # `-U`: Update dependencies if required before installing the new package(s).
    # `-u`: update all installed packages.
    doas pkg_add -Uu

    # After upgrading the sets, the system will reboot with the upgraded kernel
    # and run `sysmerge` during boot. In some cases, configuration files cannot
    # be modified automatically. So we can run it manually to make sure:)
    doas sysmerge

    # Or you can run in `diff mode`, then you're able to interact with it to do
    # merging under your control.
    # doas sysmerge -d
    ```

    </br>


- Finally, `sysclean` is optional

    Finally, optional to handle the outdated files or libraries by using `sysclean`:

    ```bash
    # Install it
    doas pkg_add sysclean

    # List all outdated packages
    doas sysclean -p

    # /usr/X11R6/lib/libXfixes.so.6.0 scrot-1.6
    # /usr/lib/libc++.so.8.0  py3-msgpack-0.6.2p2v0
    # /usr/lib/libc++abi.so.5.0       py3-msgpack-0.6.2p2v0
    # /usr/lib/libc++abi.so.5.0       ripgrep-12.1.0
    # /usr/lib/libcrypto.so.47.0      wget-1.21.2
    # /usr/lib/libssl.so.50.0 wget-1.21.2
    ```

    Then just remove them and install the new version if needed. For example:

    ```bash
    # Uninstall them
    # doas pkg_delete wget ripgrep py3-neovim py3-msgpack screenfetch scrot

    # Now run it again, should got nothing output:
    # doas sysclean -p

    # Then re-install again if you needed
    # doas pkg_add ripgrep wget py3-neovim py3-msgpack screenfetch
    ```

    </br>
