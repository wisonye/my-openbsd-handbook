# Config `doas`

```bash
# Run it as root
su

# Create and add rules to `/etc/doas.conf`
vi /etc/doas.conf

permit persist keepenv setenv { PATH } wison as root
```

Replace `wison` to your user name.

For more info, read it from `man 5 doas.conf`.

Btw, `/etc/examples` folder holds all OpenBSD configuration file examples!!!

Btw, `/etc/examples` folder holds all OpenBSD configuration file examples!!!

Btw, `/etc/examples` folder holds all OpenBSD configuration file examples!!!

</br>
