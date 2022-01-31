# Package management

The best documentation is [here](https://www.openbsd.org/faq/faq15.html#Intro)

- How to use mirror

    ```bash
    #
    # Use mirror, replace the `7.0` and `amd64` to your OpenBSD version and your
    # CPU architecture!!!
    #
    export PKG_PATH=https://mirror.fsmg.org.nz/pub/OpenBSD/7.0/packages/amd64/

    #
    # If you've already upgraded to the `current` snapshot, so you have to use
    # the `snapshot` version, not the `7.0`. Otherwise, you still get the older
    # version!!!
    #
    # You can add a `-v` option to see the `PKG_PATH` settings when using `pkg`
    # command.
    #
    # For example: `pkg_info -v -Q rust`
    #
    export PKG_PATH=https://mirror.fsmg.org.nz/pub/OpenBSD/snapshots/packages/amd64/
    ```

    </br>


- Basic usage:

    ```bash
    # Search package (to install)
    pkg_info -Q SERACH_KEYWORD_HERE


    # Search package that already installed
    pkg_info -a | grep SERACH_KEYWORD_HERE


    # List installed package file list (install to where)
    pkg_info -L INSTALLED_PACKGE_NAME_HERE


    # Query package info (after you know the full packge name)
    pkg_info PACKAGE_NAME_HERE


    # Install
    doas pkg_add PACKAGE_NAME_HERE


    # Remove
    doas pkg_delete PACKAGE_NAME_HERE


    # Remove unused dependencies
    doas pkg_delete -a


    # Check consistency of installed packages (to find the broken
    # installed packages)
    pkg_check
    ```
    </br>

- How to search package more faster?

    By default, `pkg_info -Q` **ALWAYS** will search online (with your mirror)
    to get back the result, but that's the slow way. The better way to use
    `pkglocatedb`:

    ```bash
    doas pkg_add pkglocatedb
    ```

    </br>

    Then you can search like this:

    ```bash
    pkglocate noevim
    ```

    It will search all condition includes `package name, path, filename`, but
    sometimes you only want to search for the `package name` (to install), that's
    the downside for using `pkglocate`.

    </br>


- Update all installed packages (after upgrading to current snapshot)

    ```bash
    #
    # Upgrade all installed packages (some packages you have to
    # re-install)
    #
    # `-U`: Update dependencies if required before installing the new
    # package(s).
    # `-u`: update all installed packages.
    #
    # `-v`: verbose mode
    #
    doas pkg_add -Uuv
    ```

    </br>

- How to export installed packge list to another newly installed OpenBSD

    ```bash
    pkg_info -mz | teel ~/installed_pkg_list.txt
    ```

    After copying to the new installed OpenBSD, you can install them like this:

    ```bash
    doas pkg_add -l installed_pkg_list
    ```

    </br>

