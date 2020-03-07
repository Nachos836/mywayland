# Native Wayland and root-less Sway for KISS 🌿
```
Vimb (Webkit2gtk and Gtk+3)works very well.
Surf does not work due the lack of Xprop?
Falkon (QT5) works quite well(performance) but crashes now and then + buggy right-click. Ok to use.
Firefox works good but is flickery on resizing. I cant force all the hardwareacceleration like in X.
Fork Firefox and add 'ac_add_options --enable-default-toolkit=cairo-gtk3-wayland' to mozzconfig.
There is a launcher with Wayland specific variables. Tested with ESR and 72.0.1.
SDL games Crispy Doom, Doomretro and Xonotic working well. Sauerbraten does not work.
Tests done with udev and Intel graphics.
```

## Compatibility variables
```
BEMENU_BACKEND=wayland
SDL_VIDEODRIVER=wayland
MOZ_ENABLE_WAYLAND=1
QT_QPA_PLATFORM=wayland-egl
QT_WAYLAND_FORCE_DPI=physical
```

## Wayland communication socket
```
if test -z "${XDG_RUNTIME_DIR}"; then
    export XDG_RUNTIME_DIR=/tmp/${UID}-runtime-dir
    if ! test -d "${XDG_RUNTIME_DIR}"; then
        mkdir "${XDG_RUNTIME_DIR}"
        chmod 0700 "${XDG_RUNTIME_DIR}"
    fi
fi
```

## Migration
```
Starting off a fresh xorg-less KISS, installation *should* be as easy as
'kiss b sway && kiss i sway' without any further intervention.

However when you come over from Xorg KISS, some rebuilds may be rquired
in order to pickup Wayland:
gtk+3, intel-vaapi-driver, libav, mesa, mpv, sdl*, webkit2gtk. Maybe more.
Packages which dont pickup Wayland automtically are added with modified buildfiles.
Make sure to have KMS enabled.
```
