# what-to-do-on-fresh-install-linux-mint

1. disable `Ctrl+Alt+Backspace` (or the system is going to logout if you press it accidentally. Happens very often):
   add file `/etc/X11/xorg.conf.d/99-dontzap.conf` with the following content:
   ```
   Section "ServerFlags"
    Option "DontZap" "true"
   EndSection
   ```

2. add `vm.swappiness=2` (or other small number, in percent of total RAM) to `/etc/sysctl.conf` (fixes problems with swap)

3. enable the ability to run shell scripts from thunar:
   ```sh
   xfconf-query --channel thunar --property /misc-exec-shell-scripts-by-default --create --type bool --set true
   ```
4. remove integrated microphones chinese spies place in their mini pcs:
   create a file `sudo nano /etc/modprobe.d/snd_hda_intel.conf` with the following contents:
   ```
   install snd_hda_intel /bin/true
   ```
   this technique is called "fake install".
   The driver name potentially can be different. To find out the actual driver name use `lspci -k`
   
5. mount temp directory on a bigger disk if root disk is small. Add this entry to /etc/fstab:
   ```
   /media/user/bigger-drive/tmp   /tmp   none   bind   0   0
   ```
   Symbolic link will not work for apt: it will complain that it doesn't have enough permissions to create temporary files.
