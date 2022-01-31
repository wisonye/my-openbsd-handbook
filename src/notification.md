# Notification

- Install

    ```bash
    doas pkg_add dunst
    ```

    </br>

- Add configuration
    
    Copy example configuration file to your `~/.config`:

    ```bash
    cp -rvf /usr/local/share/dunst ~/.config/
    ```

    Then add your customization settings there.

    </br>


- Make it starts with `bspwm`

    ```bash
    # Notification server
    pkill dunst
    dunst -config ~/.config/dunst/dunstrc &
    ```

    </br>

- How to test it

    ```bash
    #
    # Low priority
    # No app name (use `notify-send` itself)
    # No title
    #
    notify-send --urgency low \
        "Battry is too low, plz plugin the charger NOW!!!"

    #
    # Normal priority
    # No app name
    # With progress bar
    # With title and body
    #
    notify-send \
        --hint int:value:80 \
        "Database recovery" \
        "Almost finish ......"

    #
    # `dunstify` is the same command with `notify-send`
    #
    dunstify \
        --hints int:value:90 \
        "Database recovery" \
        "Almost finish ......"

    #
    # Critical priority
    # With app name
    # With title and body
    #
    notify-send --app-name "Battry checker" \
        --urgency critical \
        "Battery almost die" \
        "Battry is too low, plz plugin the charger NOW!!!"
    ```

    </br>

- How to close the notification window

    - Based on the config file, it disappear after `30` seconds.

    - Mouse left click should close the current, but I didn't that works.

    - Mouse right click close all, it works for me.

    - Mouse middle click to run the action if exists (e.g. Open URL)

    </br>


