# Neovim

By default, OpenBSD 7.0 only include `neovim 0.5 release`. After upgrading,
you can have `neovim 0.6.0`. But if you want install the latest release,
plz follow the steps below. More detail info is from 
[here](https://github.com/neovim/neovim/wiki/Building-Neovim#build-prerequisites).

```bash
# Install the necessary dependencies
doas pkg_add gmake cmake libtool unzip autoconf-2.69p3 automake-1.15.1 curl gettext-tools wget
```

You might see some errors like this:

```bash
# Can't find autoconf-2.69p2
# Can't find automake-1.15p0
```

If that happens, it means the package version you're asking is not exists,
just run the command below to find the nearest version to replace it:

```bash
pkg_info -Q automake
pkg_info -Q autoconf
```

</br>


Export the compilation env vars:

```bash
# Make sure back to `ksh` (just in case `fish` can't handle some script)
ksh

# Set the env vars
export AUTOCONF_VERSION=2.69
export AUTOMAKE_VERSION=1.15
```

</br>


Then clone the release version you want from [here](https://github.com/neovim/neovim/releases)
and build it:

```bash
# Download and untar it
mkdir ~/temp && cd ~/temp
wget https://github.com/neovim/neovim/archive/refs/tags/v0.6.1.tar.gz
tar xzvf v0.6.1.tar.gz
cd neovim-0.6.1

# Build the third-party dependencies
mkdir .deps && cd .deps
cmake ../third-party/
gmake

# Build neovim and install
cd ..
mkdir build && cd build
cmake -DCMAKE_BUILD_TYPE=Release ..
gmake

# Install
doas gmake install

# Remove temp stuff
cd ../.. && rm -rf neovim-0.6.1
```

</br>

Finally, install pythong support:

```bash
doas pkg_add py3-neovim
```

</br>
