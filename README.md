# NOTE: This repository is no longer maintained.
Feel free to fork and use my efforts towards noXland
if you find value in it.  


# Native experimental Wayland session for KISS 🌿

Tests done with Intel graphics. Eudev dependency for Wlroots!  
Compositors are build with suid bit. This seems to be commonly used next to  
logind but is of course not 100% ideal.   
If you have experience with `elogind`, dont hesitate to jump in. With [KISS-kde](https://github.com/dilyn-corner/KISS-kde#kiss-kde)  
there is already an attempt to pave the way for it towards KISS. This might,  
altough much more complex, the sanest way to launch any wlroots based compositor.  
For this repo I envision a solution with the least graphical frills possible.


NOTE: Due to problems with the latest release of `wf-config` and `wayfire`, they   
both are packaged as a git version.


## NoXland

NoXland is an approach to provide a wayland only session with as many  
dependencies of X ditched as possible.  
While `libxkbcommon` and `xkeyboard-config` are the minimum requiered for a  
graphical base system, they are also sufficent for qt based browsers in commu-   
nity and the ones mentioned here, without deficiencies in performance.  
Webkit2gtk based browser _can_ get along with the aforementioned, but the  
performance is very poor. Therefore, mesa can be build with `libglvnd` to provide  
the missing opengl functionality. This pulls in libX11 and some others. The  
package count is still less than a conventional build.   
Furthermore gtk+3 can be build without the X11 backend(gdkx.h). Wyeb and surfer  
work without it. Vimb not.  
Firefox needs the X11 gtk+3 backend and opengl(`libglvnd`). X dependencies can  
be ditched, but the requierements are high.  
  
Note: Some games requiere libX11 and opengl.  
  
  
### Steps to reproduce  
First, while you might have to rebuild stuff more than once, install ccache.  
Following the motto "It is easier to add stuff than to remove", I recommend  
a system reset and start from scratch. This way you have no unwelcomed packages  
to build against and error messages will point you to the right places to look  
at. Check after each build if there is stuff which can be removed. I also suggest  
to compare the above packages to the official ones. Mesa is provided with  
`libglvnd` and can be forked towards your own needs.  

Note:The following expects  that `libxkbcommon` and `xkeyboard-config` are  
     installed.  

Qt5 is the easiest to build. Just remove everything with a "x" in the depends-  
file and there should be no problem towards qt5-webengine. Falkon requieres the  
configure flag `-DNO_X11=ON`. Qt5-x11extras is not needed.  

Gtk+3 without X11 backend is also easy to build. See the package above. However,  
to enable it, `--enable-x11-backend` has to be explicitly set.  
The actual depends file will list: `libX11` `libXau` `libXext` `libXi` `libxcb`.  
There is more X requiered at build time that will be orphaned afterwards.

For Firefox, I was able to remove `libXinerama` `libXxf86vm` `libxshmfence` at  
buildtime.


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

## Links
- [Awesome Wayland](https://github.com/natpen/awesome-wayland) - A curated list of Wayland code and resources
- [mvi](https://github.com/occivink/mpv-image-viewer) - View images in mpv 
