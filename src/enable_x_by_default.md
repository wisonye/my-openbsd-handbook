# Enable X by default

The recommended way to run X is with the `xenodm` display manager. It offers
some important security benefits over the traditional `startx` command:

```bash
rcctl enable xenodm
rcctl start xenodm
```

In `OpenBSD` you got a few `getty`, the `getty5`(Ctrl + Alt + F5) is the X.

</br>
