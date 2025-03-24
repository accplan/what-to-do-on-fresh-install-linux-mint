# what-to-do-on-fresh-install-linux-mint

1. disable `Ctrl+Alt+Backspace` (or the system is going to logout if you press it accidentally. Happens very often):
   add file `/etc/X11/xorg.conf.d/99-dontzap.conf` with the following content:
   ```
   Section "ServerFlags"
    Option "DontZap" "true"
   EndSection
   ```

2. add `vm.swappiness=2` (or other small number, in percent of total RAM) to `/etc/sysctl.conf` (fixes problems with swap)

3. install .net 4.5, 4.7.2 on wine 64-bit prefix: https://www.reddit.com/r/wine_gaming/comments/8r6low/guide_how_to_install_net_45_on_64bit_prefixes/

4. run shell scripts from thunar:
```sh
xfconf-query --channel thunar --property /misc-exec-shell-scripts-by-default --create --type bool --set true
```
