# Control services

`System daemons` (or `services`) are started, stopped and controlled by the
rc(8) script via `rc.d(8)`. Here is the offical [link](https://www.openbsd.org/faq/faq10.html#rc)

```bash
# List services
rcctl ls all|failed|off|on|started|stopped [daemon]

# Start, stop or restart services
rcctl [-df] check|reload|restart|stop|start [daemon]

# Enable or disable when booting
rcctl disable|enable|order [daemon]
```

`[daemon]` is the service name which you can found that in `/etc/rc.d` folder

All services enable or not are configure in `/etc/rc.conf`, super simple:)

But plz **DO NOT** modify the `/etc/rc.conf`, just keep that to default settings,
as it will be changed when upgrading to new version. So, for your
personal customized service, just use `rcctl` to save them into `/etc/rc.conf.local`!!!

</br>

