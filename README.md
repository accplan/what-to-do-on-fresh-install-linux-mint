# what-to-do-on-fresh-install-linux-mint

1. disable `Ctrl+Alt+Backspace` (or the system is going to logout if you press it accidentally. Happens very often):
   add file `/etc/X11/xorg.conf.d/99-dontzap.conf` with the following content:
   ```
   Section "ServerFlags"
    Option "DontZap" "true"
   EndSection
   ```

2. add `vm.swappiness=2` (or other small number, in percent of total RAM) to `/etc/sysctl.conf` (fixes problems with swap)

3. run shell scripts from thunar:
   ```sh
   xfconf-query --channel thunar --property /misc-exec-shell-scripts-by-default --create --type bool --set true
   ```
4. remove integrated microphones chinese spies place in their mini pcs:
   create a file `sudo nano /etc/modprobe.d/snd_hda_intel.conf` with the following contents:
   ```
   install snd_hda_intel /bin/true
   ```
   this technique called "fake install".
   The driver name potentially can be different. To find out the actual driver name use `lspci -k`
   
