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
        you select it when installing OpenBSD), it includes the `gmake` and
        `clang` and `clang++`which you don't need to install.

        ```bash
        #
        # Make sure back to `ksh` (just in case `fish` can't handle some
        # script)
        #
        ksh

        doas pkg_add llvm py3-llvm gas

        # Set the env vars
        export CC=clang
        export CXX=clang++
        ```

        </br>

    - <del>Download the [LTS version](https://github.com/nodejs/node/releases) and build it</del>

        <del>

        ```
        mkdir ~/temp && cd ~/temp
        wget https://github.com/nodejs/node/archive/refs/tags/v14.18.3.tar.gz
        tar xzvf v14.18.3.tar.gz
        cd node-14.18.3
        ```

        </del>

        <del>Because the official node still not support `OpenBSD`, so this way will</del>
        <del>fail at the end!!!</del>

        </br>

    - Use the work around build version

        Here is the [building issue for OpenBSD](https://github.com/nodejs/node/issues/41224)

        Then we need to git clone his `fix-openbsd-build` branch for compiling
        `Node 16LTS` on `OpenBSD`:

        ```bash
        git clone --branch fix-openbsd-build https://github.com/localscope/node.git node-16-openbsd
        cd node-16-openbsd
        ```

        </br>


    - Configure and compile

        Plz make sure `node-16-openbsd` is in the partition that contains the
        `wxallowed` mount flag, otherwise, compile will fail!!!

        Usually, `/usr/local` partition already enabled `wxallowed`. If you're
        compiling in `/home` partition and you will install `node` to `/usr/local`
        , then you can enable the `wxallowed` to `/home` partition temporary by
        running the following command:

        ```bash
        doas mount -uo wxallowed /home
        ```

        </br>

        ```bash
        ./configure
        gmake -j4
        doas gmake install
        ```

        </br>

        Then remove all temporary stuff

        ```
        cd ..
        rm -rf node-16-openbsd
        ```

        </br>

- Finally, install `TypeScript` LSP support

    ```bash
    doas npm install -g typescript typescript-language-server
    ```

    Global module will be installed to `/usr/local/lib/node_modules/`

    </br>

