for AMD Instinct GPUs enable XNACK: https://niconiconi.neocities.org/tech-notes/xnack-on-amd-gpus/

# Enabling XNACK

To enable XNACK in the Linux kernel, the kernel must be up-to-date with HMM support. Then, it can enabled by two methods: a boot-time kernel argument amdgpu.noretry=0 by putting this argument in the bootloader configuration, alternatively, passing the amdgpu kernel module parameter noretry=0 by declaring options amdgpu noretry=0 in /etc/modprobe.d.

The Linux kernel documentation says:

`noretry (int): Disable XNACK retry in the SQ by default on GFXv9 hardware. On ASICs that do not support per-process XNACK this also disables retry page faults. (0 = retry enabled, 1 = retry disabled, -1 auto (default))`

Then, before running an HIP program, the environmental variable HSA_XNACK=1 must be set. It may be a good idea to put export HSA_XNACK=1 in your shell profile.

# Compiler Options

To further increase performance, it’s possible to specifically target AMD GPUs with the assumption that XNACK is always enabled during compile time. To implement this, add the suffix :xnack+ to your target name. For example, instead of targeting gfx906, you can instead target gfx906:xnack+.

It’s worth noting that the object code compilewd with xnack+ does not work on GPUs without XNACK. Thus, if you decide to do this, you may want to compile another version without xnack as fallback.

Personally, I didn’t notice a difference in performance of my own test code.

According to LLVM’s documentation:

`If specified, generate code that can only be loaded and executed in a process that has a matching setting for XNACK replay. If not specified for code object V2 to V3, generate code that can be loaded and executed in a process with XNACK replay enabled. If not specified for code object V4 or above, generate code that can be loaded and executed in a process with either setting of XNACK replay.`

`XNACK replay can be used for demand paging and page migration. If enabled in the device, then if a page fault occurs the code may execute incorrectly unless generated with XNACK replay enabled, or generated for code object V4 or above without specifying XNACK replay. Executing code that was generated with XNACK replay enabled, or generated for code object V4 or above without specifying XNACK replay, on a device that does not have XNACK replay enabled will execute correctly but may be less performant than code generated for XNACK replay disabled.`

According to GCC’s documentation:

`-mxnack=on -mxnack=off -mxnack=any`

`Compile binaries suitable for devices with the XNACK feature enabled, disabled, or either mode. Some devices always require XNACK and some allow the user to configure XNACK. The compiled code must match the device mode. At present this option is a placeholder for support that is not yet implemented.`
