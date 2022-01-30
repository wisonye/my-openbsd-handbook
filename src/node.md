# Node

- Install pre-built binary

    ```bash
    doas pkg_add node
    ```

    </br>

- Build from source

    By default, OpenBSD 7.0 only comes with `NodeJS 12`, if you like to install any
    version you want, plz follow the steps below. More info from [here](https://github.com/nodejs/node/blob/master/BUILDING.md).

    - Install the necessary dependencies

        `OpenBSD` comes with all default compliation tools in `comp70` set (if
        you select it when installing OpenBSD), it includes the `gmake` which
        you don't need to install.

        ```bash
        #
        # Make sure back to `ksh` (just in case `fish` can't handle some
        # script)
        #
        ksh

        #
        # Choose `8.4` version for both `gcc` and `g++`
        #
        doas pkg_add gcc g++ llvm py3-llvm gas

        #
        # After installation, there is NO `gcc` and `g++` binary exists!!!
        # As it names `egcc` and `eg++`, that's why you need to set the
        # env vars below before running `./configure`
        #
        Set the env vars
        export CC=egcc
        export CXX=eg++
        ```

        </br>

    - Download the [LTS version](https://github.com/nodejs/node/releases) and build it

        ```
        mkdir ~/temp && cd ~/temp
        wget https://github.com/nodejs/node/archive/refs/tags/v14.18.3.tar.gz
        tar xzvf v14.18.3.tar.gz
        ```

        </br>

    - Configure and compile

        ```
        cd node-14.18.3
        ./configure
        ```

        If you see this error:

        ```bash
        # ERROR: Did not find a new enough assembler, install one or build
        # with
        #       --openssl-no-asm.
        #        Please refer to BUILDING.md
        ```

        Then plz use `./configure --openssl-no-asm` instead

        </br>

        Buil and install

        ```bash
        gmake
        doas gmake install
        ```

        </br>

        Then remove all temporary stuff

        ```
        cd ..
        rm -rf node-14.18.3
        ```

        </br>

- Finally, install `TypeScript` LSP support

    ```bash
    doas npm install -g typescript typescript-language-server
    ```

    </br>

