When you've finally mainlined your device, you might be looking for a different userspace than pmOS (no hard feelings).

Well, you can easily make use of Librem 5 / Pinephone / anything aarch64 (or arm32 if you're crazy..) images (provided you managed to get DRM (i.e. mdss on qcom) up)!

Here's how to do it:

```
* Download and extract/mount the image on your PC

* Run "readlink sbin/init" and replace the symlink with a copy of the binary (otherwise the image might not boot)

* Open "/etc/fstab" and remove partitions other than the rootfs (most of the time you just want to delete /boot, without that the init system will hang and ultimately fail due to the partition not being mounted)

* Extract the ramdisk from /boot and include it in your device's boot image (for example with "abootimg -u boot.img -r ramdisk.img")

* Format your userdata partition (well.. you can use whatever partition you want (even on the uSD if you managed to get it working), but userdata is the biggest one)

* "adb push" or "dd" the rootfs onto your userdata partition (if you use dd then you will likely want to resize the filesystem, some distros do this automagically but some don't)

* Add a root parameter to your kernel cmdline, either via defconfig or DTS:

eMMC: "root=/dev/mmcblk[0-9]p[0-9]+" (ex. /dev/mmcblk1p76 on XA2)

UFS: "root=/dev/sd[a-z][0-9]+" (ex. /dev/sda1; just like the SATA drives on your PC)

* Boot and keep your fingers crossed :)
```

If your GPU isn't supported in the bundled mesa you might want to either wait for the distro to update (or kindly ask the maintainers to do so) or compile it yourself. If you're crazy you might also fake the GPU ID that the kernel driver passes to userspace, but that's definitely not advised.
