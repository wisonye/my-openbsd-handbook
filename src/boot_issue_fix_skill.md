# Boot issue fix skill and experience

- Black screen during booting process and never comes back

    Usually happens on `amdgpu` driver with the unsupported Graphic Card. Jump
    to the end to see how to disable gpu driver.

    </br>

- Keyboard not working during the `boot -c`

    When something wrong, you need to run `boot -c` to go into boot config before
    continuing the booting process. But in some cases, your keyboard doesn't
    work after going into the `User Kenerl Config` prompt, it will show a tiny
    rectangle area like below (and keep refreshing the content inside that
    rectangle area):

    [my video recording](https://drive.google.com/file/d/1Iq2skUDZFMYqYKYJGiAhVm7EAu0VRX7d/view?usp=sharing)

    [Another screenshot](https://ibb.co/QchqhtY)

    So how to solve that?

    - Use `boot hd0a:/bsd.rd` to boot the kenerl Ram Disk from your OpenBSD boot disk

    - Try to mount the OpenBSD boot disk, mount the `/` and `/usr`

        Before mounting, you need to know the disk name by running:

        ```bash
        sysctl hw.disknames

        #hw.disknames=sd0:,rd0:XXXXX,sd1:YYYY
        ```

        So the `sd0` if the first disk (but not OpenBSD style), `rd0` is the ram
        disk itself, and the `sd1` should be your OpenBSD boot disk, as it has
        the unique disk label.

        Then the next you need to figure out which partition related to `/` and
        `/usr` by running:

        ```bash
        disklabel sd1
        ```

        If you see `disklabel: /dev/rsd1: No such file or directory`, then you
        need to run the following command to create the device special file:

        ```bash
        cd /dev
        sh ./MAKEDEV sd1
        ```

        Now you can run `disklabel sd1` again to list all parititions to mount.

        For mounting the `/`, it usually at the first partition if you use auto
        partition when installing OpenBSD:

        ```bash
        mkdir /mnt-sd1a
        mount /dev/sd1a /mnt-sd1a
        ```

        </br>

    - Modify the boot config permanently

        ```bash
        config -ef /bsd
        ```

        You can disable the gpu driver like `disable amdpug` (if you know what
        the case), then `quit` with saving.

        For more command details, read `man boot_config`.

        </br>


