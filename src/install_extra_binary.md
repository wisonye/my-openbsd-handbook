# Install extra binary

- `bat`, `lf`, `ripgrep`, `git`, `xclip`

    ```bash
    doas pkg_add bat lf ripgrep git xclip
    ```

    - `xclip`

        If you want to copy content from file to system clipboard and can paste
        into browser, then you need to copy like this:

        ```bash
        cat ~/temp/reddit-post.txt | xclip -selection clipboard
        ```

        Now, you can `Ctrl+V` to paste it to browser.

        </br>


    - Config git

        ```bash
        git config --global user.name "Wison Ye"
        git config --global user.email "wisonye@gmail.com"
        git config --global pull.rebase false
        ```

        Generate the SSH key and add it to ssh agent:

        ```bash
        #
        # Gnerate new ssh key pair
        #
        ssh-keygen -t rsa -C "wisonye@gmail.com"

        #
        # Make sure to run in `ksh` or `bash`, just not `fish`!!!
        #
        eval "$(ssh-agent)"
        ssh-add ~/.ssh/id_rsa

        #
        # Print the public key and copy it, then you can add it
        # to your github
        #
        cat ~/.ssh/id_rsa.pub
        ```

        </br>


- `fish`

    ```bash
    doas pkg_add fish
    su
    echo "$(which fish)" >> /etc/shells
    exit

    chsh -s $(which fish)
    ```

    Re-login to take effect.

    One thing about running command by `fish`:

    `fish` will load `~/.config/fish/config.fish` before running any command,
    that takes time. It's ok for opening terminal to run `fish`, but it's NOT OK
    for running one-shot command. Here is the case:

    ```bash
    time fish --private --command "ls -lht"

    # total 18616
    # -rw-------  1 wison  wison   9.0M Jan 28 15:11 alacritty.core
    #
    # ________________________________________________________
    # Executed in  154.91 millis    fish           external
    #    usr time  130.00 millis    0.00 micros  130.00 millis
    #    sys time   10.00 millis    0.00 micros   10.00 millis
    ```

    As you see it's pretty slow which cost `154.91ms`!!!

    But if you use `--no-config` to run the command, then it won't load the
    default fish config, that will be a lot more faster than before:

    ```bash
    Time fish --private --no-config --command "ls -lht"

    # Total 18616
    # -rw-------  1 wison  wison   9.0M Jan 28 15:11 alacritty.core
    #
    # ________________________________________________________
    # Executed in   28.03 millis    fish           external
    #    usr time    0.00 millis    0.00 micros    0.00 millis
    #    sys time   20.00 millis    0.00 micros   20.00 millis
    ```

    Same command but only takes `28.03ms`, that's huge difference!!!

    </br>

