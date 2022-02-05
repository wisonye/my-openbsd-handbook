# Basic

### How to enable or disable `PF` at boot time

`PF` is enabled by default in `/etc/rc.local`, you can disable it like this:

```bash
doas rcctl disable pf
```

</br>

### How to enable and disable `PF` after boot time

```bash
# Eable
doas pfctl -e

# Disable
doas pfctl -d
```

</br>

### How to load `/etc/pf.conf`

```bash
# Load the pf.conf file
pfctl -f  /etc/pf.conf

# Parse the file, but don't load it
pfctl -nf /etc/pf.conf
```

</br>



### How to show info

```bash
# pfctl -sr     # Show ruleset
# pfctl -ss     # Show state table
# pfctl -si     # Show info: filter stats and counters
# pfctl -sa     # Show all: everything it can show
```

</br>


