# How to config boot

`man boot.conf` with detail information, here is something that you should know
about:

- `/etc/boot.conf`

    That's boot config file, but if you put something wrong there, you can hold
    down the `Control` to skip that config processing.

    </br>

    By default, timeout set to `5` seconds, you can add the following settings
    to reduce it:

    ```bash
    set timeout 1
    ```

    </br>


- Custom boot

    In the boot prompt UI, you can start to type boot command to stop the auto boot, below is your
    option:

    ```bash
    # Causes the kernel to ask for the root device to use.
    boot -a

    # Causes the kernel to go into boot_config(8) before performing
    # autoconf(4) procedures. In that user config mode, you can enable
    # or disable # the parituclar driver/module etc.
    boot -c

    # Causes the kernel to boot single-user. Then you can reset the root
    # password when needed
    boot -s
    ```

    </br>


    If you want to boot the ram disk kernel (same thing like the install media),
    then you can boot like this:

    ```bash
    #
    # `hd0` the first deteced device, usually you can see all device names in
    # the boot UI.
    #
    # `a` means the first partition.
    #
    # `/bsd.rd` - Ram disk kernel
    #
    boot hd0a:/bsd.rd
    ```

    </br>

    One more thingn about the `boot -c` (boot into user config), you can run
    `doas config -ef /bsd` to do the same thing (modify boot configuration,
    diable/enable driver, etc) and save back to `/bsd`. After done, you can
    choose `exit` (exit without saving) or `quit` (exit with saving change.).

    For more command details, read `man boot_config`.

    </br>
