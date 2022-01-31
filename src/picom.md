# Picom compositor

- Install

    ```bash
    doas pkg_add picom
    ```

    </br>


- Add it to your window manager start script

    ```bash
    # Picom compositor
    #
    # All `--shadow-XXX` options only take effect after `--shadow` sets!!!
    #
    pkill picom
    picom \
        # --shadow \
        # --shadow-radius 10 \
        # --shadow-opacity 0.8 \
        # --shadow-offset-x -8 \
        # --shadow-offset-y -8 \
        # --shadow-red 0.67 \
        # --shadow-green 0.90 \
        # --shadow-blue 0.99 \
        # --frame-opacity 0.8
        --backend glx \
        --blur-background &
    
    ```

    </br>


