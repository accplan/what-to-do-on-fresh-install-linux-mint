# what-to-do-on-fresh-install-linux-mint

1. disable `Ctrl+Alt+Backspace` (or the system is going to logout if you press it accidentally. Happens very often):
   add file `/etc/X11/xorg.conf.d/99-dontzap.conf` with the following content:
   ```
   Section "ServerFlags"
    Option "DontZap" "true"
   EndSection
   ```

2. add `vm.swappiness=2` (or other small number, in percent of total RAM) to `/etc/sysctl.conf` (fixes problems with swap)
