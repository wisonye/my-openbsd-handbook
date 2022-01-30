# Disable the xconsole

The `xconsole` program displays messages which are usually sent to
`/dev/console`. It shows as a black box on the bottom-right that you can
see it in the display manager UI.

Actually you can use `echo` to send any message to the `xconsole` like below:

```bash
doas echo "Hey:)"  > /dev/console
```

Or you can actually use `vi` or `vim` to edit some text content and save it
to `xconsole` like this:

```bash
# After saving, all contents will show into `xconsole`
vi /dev/console
```

</br>



For disabling it, just comment one line in `/etc/X11/xenodm/Xsetup_0`:

```bash
# ${exec_prefix}/bin/xconsole -geometry 480x130-0-0 -daemon -notify -verbose -fn fixed -exitOnFail
```

After saving the change, relogin (if you're in X) or restart the `xenodm`
service instead to take effect.

```bash
doas rcctl restart xenodm
```

</br>
