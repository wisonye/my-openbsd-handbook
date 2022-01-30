# How to reset root passwd

- Boot into single user mode

    ```bash
    # disk: hd0+ hd1+
    # >> OpenBSD/amd64 BOOT 3.33
    boot> boot -s
    ```

    </br>

- Mount the `/` and `/usr` then run `passwd`

    ```bash
    #
    # Both / and /usr will need to be mounted read-write.
    #
    fsck -p / && mount -uw /
    fsck -p /usr && mount /usr


    #
    # You already have root privileges from being in single user mode,
    # so you will not be prompted to provide the current password.
    #
    passwd
    ```

    After that, you can either pressing `CTRL-D` to resume the normal boot
    process to multi-user mode or by entering `reboot`.

    </br>


