# Change the VirtualBox resolution

- First step, do some settings on the host side:

    ```bash
    # You can show the VM instance info by running this:
    VBoxManage showvminfo OpenBSD-7| bat

    # Show the extra data settings
    VBoxManage getextradata OpenBSD-7| bat
    ```

    Then you need to shutdown the VM and set the custom resolution like this:

    ```bash
    # For new MacBookPro
    VBoxManage setextradata OpenBSD-7 \
        CustomVideoMoel 3072×1920x32

    # For 2019 iMac
    VBoxManage setextradata OpenBSD-7 \
        CustomVideoMoel 5120x2880x32


    # Query to confirm
    VBoxManage getextradata OpenBSD-7

    # Key: CustomVideoMoel, Value: 3072×1920
    # Key: GUI/LastCloseAction, Value: PowerOff
    # Key: GUI/LastNormalWindowPosition, Value: 296,178,630,371
    # Key: GUI/LastScaleWindowPosition, Value: 582,238,640,480
    # Key: GUI/ScaleFactor, Value: 1.75
    ```

    </br>

- Second step, do some settings on the vm instance side:

    Restart the VM and login to it,

    Restart X to take effect:

    ```bash
    doas rcctl restart xenodm
    ```

    </br>

