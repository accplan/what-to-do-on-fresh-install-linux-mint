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
6. fix freeze games (including unity games) when wine application is losing focus:
    - Open Regedit with `wine regedit`
    - Go to HKEY_CURRENT_USER\Software\Wine\X11 Driver, creating Key "X11 Driver" if it does not exist
    - Create a new String entry named "UseTakeFocus" with value "N"
  
7. run `systemd-analyze blame` to see which parts of systemd slows down the boot process. You probably want to disable this: `systemctl disable NetworkManager-wait-online.service` 

8. disable expensive tumblerd thumbnailers in `/etc/xdg/tumbler/tumbler.rc` (ex.: PDF, epub), or disable thumbnailing in the file manager altogether (Thunar, Edit -> Preferences, Show Thumbnails -> Never)

9. fix choppiness of virt-manager with virtio 3d acceleration enabled: run virt-manager like this: `vblank_mode=0 virt-manager` (optionally can edit startup icon: `bash -c "vblank_mode=0 virt-manager"`). Source: https://github.com/virt-manager/virt-manager/issues/228#issuecomment-1802768955
