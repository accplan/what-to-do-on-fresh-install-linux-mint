1.  The pci-stub driver is no longer built into the kernel. Add this single line to the 'etc/modules-load.d/modules.conf':

```
pci-stub
```

Then run `sudo update-initramfs -u`

Then add `pci-stub.ids=0000:0000,0001:0001` to the kernel parameters

2. to list devices and assigned drivers run:

```
driverctl -v list-devices
```

3. to assign a driver to the device run:

```
driverctl set-override 0000:03:00.0 amdgpu # or vfio-pci
```
