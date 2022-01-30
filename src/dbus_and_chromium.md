# `dbus` and `Chromium`

I prefer to use `Chromium` as the default browser, as it performs very well
and open source and secure (it uses `openbsd pledge unveil`). You need to
install `dbus` and use `dbus-run-session chrome` to launch. Otherwise, you
will see error in terminal (if you start there) and it start very slow
(as can't connect to `dbus`):

```bash
doas pkg_add dbus chromium
```

After that:

```bash
#
# Enable `messagebus`, as Chromium need `dbus` to run. If you run chromium
# without `dbus`, a lot more issues and laggy.
#
doas rcctl enable messagebus
doas rcctl start messagebus

# Start in dbus session
dbus-run-session chrome &
```

For security reason, go to `chrome://settings`, then `security and privacy`,
`Privacy Sandbox` at the bottom, then make sure turn off `Privacy Sandbox
trials` and `FLoc`.

But if you really care about the privacy, then you can install `iridium` instead,
it's the `chromium` hardened build version!!!

</br>
