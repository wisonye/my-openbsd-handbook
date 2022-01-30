# Advanced Power Management (APM)

- Enable the `APM daemon`

    ```bash
    # Enable APM daemon with `-L` power save mode (for latop)
    doas rcctl enable apmd
    doas rcctl set apmd status on
    doas rcctl set apmd flags -L
    ```

    </br>

- Use `apm` (Advanced Power Managment Control Program)

    ```bash
    # Display external charger, `0` disconnteced, `1` connected
    apm -a


    # Display the battery status.  0 means high, 1 means low, 2 means
    # critical, 3 means charging, 4 means absent, and 255 means
    # unknown`0` high
    apm -b


    # Display battery in percentage or minus
    apm -l
    apm -m


    # Put the system into stand-by (light sleep) state
    apm -S
    ```

    </br>

