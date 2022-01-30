# Enable `SoftUpdate` to increase the write performance

https://www.openbsd.org/faq/faq14.html#SoftUpdates

`doas vi /etc/fstab` and then add `softdep` flag after `rw` like below:

```bash
12033150e68e50fb.b none swap sw
12033150e68e50fb.a / ffs rw,softdep 1 1
12033150e68e50fb.l /home ffs rw,softdep,nodev,nosuid 1 2
12033150e68e50fb.d /tmp ffs rw,softdep,nodev,nosuid 1 2
12033150e68e50fb.f /usr ffs rw,softdep,nodev 1 2
12033150e68e50fb.g /usr/X11R6 ffs rw,softdep,nodev 1 2
12033150e68e50fb.h /usr/local ffs rw,softdep,wxallowed,nodev 1 2
12033150e68e50fb.k /usr/obj ffs rw,softdep,nodev,nosuid 1 2
12033150e68e50fb.j /usr/src ffs rw,softdep,nodev,nosuid 1 2
12033150e68e50fb.e /var ffs rw,softdep,nodev,nosuid 1 2
```

Reboot to take effect.

</br>

