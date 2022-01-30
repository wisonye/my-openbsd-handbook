# Wubi Pinyin support

Install `fcitx`:

```bash
doas pkg_add fcitx fcitx-config-tool
```

</br>

After that, add the following settings into `~/.xsession`:

```bash
export GTK_IM_MODULE=fcitx
export QT_IM_MODULE=fcitx
export XMODIFIERS="@im=fcitx"
```

</br>


Then enable `wbpy:True` (WuBi + PinYin) in the `~/.config/fcitx/profile`, it's in
the same line start with `EnabledIMLList=`:

```
wbpy:True,
```

</br>

Lauch `fcixt` in `~/.config/bspwm/bspwmrc`:

```bash
# Chinese input method
pkill fcitx
fcitx > ~/.fcitx.log &
```

</br>


By default, `Ctrl+space` switch input method, `Shift` switch US and `WubiPinYin` when current input method is `WubiPinYin`.

Bug: Can't type in browser, have no idea yet.


