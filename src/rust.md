# Rust

Upgrade BSD to `current` branch snapshot for getting the higher version `rust`.

</br>

- Install `rust`

    - If already installed the older version before ugrading, then remove them

        ```bash
        doas pkg_delete rust rust-clippy rust-gdb rust-rustfmt
        ```

        </br>


    - Install the latest version

        ```bash
        doas pkg_add rust rust-clippy rust-gdb rust-rustfmt
        ```

        Make sure you add the `~/.cargo/bin` to `PATH`!!!

        </br>

- Error about  `openssl`

    The `openssl-sys` crate always be a tricky problem to compile. Usually,
    the core issue is missing the correct env var settings.

    For example, if you see the error like below when installing
    `cargo-make`, that means the default os preinstalled `openssl` is NOT
    the right version for this crate to compile.

    So, you have to install the acceptable version to match the compile
    condition.

    ```bash
    This crate is only compatible with OpenSSL (version 1.0.1 through 1.1.1, or 3.0.0), or LibreSSL 2.5
        through 3.4.1, but a different version of OpenSSL was found. The build is now aborting
        due to this version mismatch.
    ```

    So, how to solve that?

    - Install the acceptable version:

        ```bash
        doas pkg_add openssl

        # Ambiguous: choose package for openssl
        #         0: <None>
        #         1: openssl-1.0.2up4
        #         2: openssl-1.1.1m
        #         3: openssl-3.0.1
        ```

        Pick the correct version the error asks for.

        </br>

    - Add the following env vars

        When compiling the `openssl-sys` crate, there is a `build/main.rs`
        to control how to get the necessary env vars to find the `include`
        and `lib` location. If you like to figure out how it works, you can
        open the file to have a look (just replace the `0.9.71` to the
        version number to what you saw in the error)

        `.cargo/registry/src/github.com-1ecc6299db9ec823/openssl-sys-0.9.71/build/main.rs`


        Basically, you need to set the `OPENSSL_LIB_DIR` and `OPENSSL_INCLUDE_DIR`
        correctly. The detail is [here](https://docs.rs/openssl/latest/openssl/#manual)

        For example, when I installed the `openssl-1.1.1m`, the bin file
        and location will look like below:

        ```bash
        # Executable
        /usr/local/bin/eopenssl11

        # Lib folder
        /usr/local/include/eopenssl11/openssl/

        # Include folder
        /usr/local/lib/eopenssl11

        # Pkg file
        /usr/local/lib/pkgconfig/eopenssl11.pc
        /usr/local/lib/pkgconfig/libecrypto11.pc
        /usr/local/lib/pkgconfig/libessl11.pc
        ```

        You can run `pkg_info -L openssl` to list all of those. Yes, it's
        crazy, it calls `eopenssl`, that's why you fail to compile!!!

        For my case, I need to set the following env vars:

        ```bash
        # Fish shell
        set --export OPENSSL_LIB_DIR /usr/local/lib/eopenssl11
        set --export OPENSSL_INCLUDE_DIR /usr/local/include/eopenssl11/openssl

        # If you're using bash shell
        export OPENSSL_LIB_DIR=/usr/local/lib/eopenssl11
        export OPENSSL_INCLUDE_DIR=/usr/local/include/eopenssl11/openssl
        ```

        </br>

    - Add the symbol link to replace the preinstalled `openssl`

        - Backup old version and make new symbol link

            ```bash
            doas mv /usr/bin/openssl /usr/bin/openssl_bak
            doas ln -s /usr/local/bin/eopenssl11  /usr/bin/openssl

            # Test it, should show the installed version
            openssl version
            # OpenSSL 1.1.1m  14 Dec 2021
            ```

            </br>

        - To avoid the default `pkg-config` mess up the include path

            I guess, the `pkg-config` will feed the sys include path and lib
            path to the `openssl-sys` building script by default, at least
            that's how I fail to compile. As `/usr/lib/pkgconfig/openssl.pc`
            will point to the `/usr/include/openssl` (the old version),
            and the `build/expando.c` will include the sys header like this:

            ```c++
            #include <openssl/opensslv.h>
            #include <openssl/opensslconf.h>
            ```

            That will override your `OPENSSL_INCLUDE_DIR` settings, that's
            why you still fail to compile even installed the different version
            !!!

            So, that's why you need to hide the old version and create symbol
            link to the new version:

            ```bash
            doas mv /usr/include/openssl /usr/include/openssl_bak
            doas ln -s /usr/local/include/eopenssl11/openssl /usr/include/openssl
            ```

            </br>

        From now on, you should already solve the `openssl` compile error:)

        </br>


    A tips for the `cargo-make` crate, you can add the following option to
    bypass the `TLS` (default) feature, then you won't face the `openssl`
    problem:

    ```bash
    cargo install --no-default--features cargo-make
    ```

    Here is the related [comment](https://github.com/sagiegurari/cargo-make/issues/614#issuecomment-991066510)

    </br>


- Error about installing `cargo-watch`

    ```bash
    error[E0432]: unresolved import `nix::sys::socket::sockopt::PeerCredentials`
        --> /home/wison/.cargo/registry/src/github.com-1ecc6299db9ec823/zbus-1.9.2/src/connection.rs:163:44
        |
    163 |         use nix::sys::socket::{getsockopt, sockopt::PeerCredentials};
        |                                            ^^^^^^^^^^^^^^^^^^^^^^^^ no `PeerCredentials` in `sys::socket::sockopt`

    error[E0432]: unresolved import `nix::sys::socket::sockopt::PeerCredentials`
        --> /home/wison/.cargo/registry/src/github.com-1ecc6299db9ec823/zbus-1.9.2/src/azync/connection.rs:180:44
        |
    180 |         use nix::sys::socket::{getsockopt, sockopt::PeerCredentials};
        |                                            ^^^^^^^^^^^^^^^^^^^^^^^^ no `PeerCredentials` in `sys::socket::sockopt`

    For more information about this error, try `rustc --explain E0432`.
    error: could not compile `zbus` due to 2 previous errors
    ```

    `Freebsd` issue is [here](https://github.com/watchexec/cargo-watch/issues/184)

    Also, a known issue in [zbus](https://gitlab.freedesktop.org/dbus/zbus/-/issues/141)
    as well. So, for `OpenBSD`, it seems no luck at this moment:)

    </br>

- Install useful rust binary

    ```bash
    cargo install du-dust cargo-make cargo-cache
    ```

    After that, run the following command to re-claim some disk space when
    needed:

    ```bash
    cargo cache --autoclean
    ```

    </br>

