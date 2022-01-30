# Solve the MacBookPro Wifi NIC issues

After installing the OpenBSD to `MacBookPro`, the first issue will be `NO Network`!!!

And you will see the following output from the `dmesg`:

```bash
bwfm0: failed loadfirmware of file brcmfmac43602-pcie.bin
```

That's saying: `Open BSD knows you're using the BCM43602 PCIE Wifi NIC, but no
driver`, even the nic configuration file is there: `/etc/hostname.bwfm0` and the config
settings is correct.

`bwfm0` is the wifi NIC name, maybe is different with yours, but just replace your wifi
NIC is fine.

Usually OpenBSD will check and install the missing firmware on the
first boot, but it won't happen without existing network!!!


- So, how to solve that?

    Download the entire `firmware/7.0` driver folder from [here](http://firmware.openbsd.org/firmware/7.0/bwfm-firmware-20200316.1.2p2.tgz)
    manually and then copy to a `MS-DOS` format USB and plugin to your MacBookPro.

    Then run the following command to figure out which disk and partition should
    mount into:

    - First, list all connected disks

        ```bash
        sysctl | grep hw.disknames

        #hw.disknames=sd0:xxxxxx,sd1: ,sd2:
        ```

        So, the `sd0` should be the OpenBSD root disk, and maybe the `sd1` is the
        USB.

    - Try to list the `sd1` disk partition

        ```bash
        disklabel sd1

        # 16 partitions:
        # #     size    offset  fstype
        #  c:   xxxx         0  unused
        #  i:   xxxx      2048  MSDOS
        ```

        Yup, that's the `MSDOS` USB disk and the partition `i` is the partition
        we need to mount.

        </br>

    Now, we can update the firemware from the local path:

    ```bash
    # mount the USB to `/mnt`, `i` is the parition you got
    # from the previous output
    mount /dev/sd1i /mnt

    #
    # Update the firmware locally, for example the folder path in USB is:
    # `/fw_update/7.0`
    #
    fw_update -p /mnt/fw_update/7.0
    ```

    Reboot to take effect.

    </br>

- Speed issues

    Even Wifi works but still have the slow speed problem, only for around
    `80 ~ 100Mbps` max download but faster upload. You will notice that is
    very slow compare to what you can have in your local LAN speed on same
    Wifi network.

    I've already try:

    ```bash
    # List all supported mode for the NIC and driver
    ifconfig bwfm0 media

    # bwfm0: flags=808843<UP,BROADCAST,RUNNING,SIMPLEX,MULTICAST,AUTOCONF4> mtu 1500
    #         lladdr ac:bc:32:8d:ae:95
    #         index 1 priority 4 llprio 3
    #         groups: wlan egress
    #         media: IEEE802.11 autoselect mode 11ac (VHT-MCS0 mode 11ac)
    #         status: active
    #         ieee80211: nwid JI-LE-SHI-JIE chan 36 bssid 34:58:40:ca:f8:bc -54dBm wpakey wpaprotos wpa2 wpaakms psk wpaciphers ccmp wpagroupcipher ccmp
    #         supported media:
    #                 media autoselect
    #                 media autoselect mediaopt hostap
    #                 media autoselect mode 11a
    #                 media autoselect mode 11a mediaopt hostap
    #                 media autoselect mode 11b
    #                 media autoselect mode 11b mediaopt hostap
    #                 media autoselect mode 11g
    #                 media autoselect mode 11g mediaopt hostap
    #                 media autoselect mode 11n
    #                 media autoselect mode 11n mediaopt hostap
    #                 media autoselect mode 11ac
    #                 media autoselect mode 11ac mediaopt hostap
    #         inet 192.168.1.105 netmask 0xffffff00 broadcast 192.168.1.255
    ```

    And then try `11n` and `11ac` (the default mode):

    ```bash
    doas ifconfig bwfm0 media autoselect mode 11ac
    doas /etc/netstart bwfm0
    ```

    After NIC reconnected to Wifi (get IP address), speed issue still there.
    So I think maybe the `bwfm` driver issue.

    Also, I confirmed that it's the same issue happens on the `Gigabit` ethernet port as well!!!

    </br>
