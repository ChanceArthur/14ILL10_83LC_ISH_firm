# Updated Intel ISH firmware for Fedora 43 on Lenovo Yoga 9i 2-in-1 (14ILL10)

(ver. **Dec 8 2025**)

Updated Intel ISH firmware, originally provided by Lenovo as a [Windows driver for the Yoga 9i 2-in-1 (14ILL10)](https://pcsupport.lenovo.com/us/en/products/laptops-and-netbooks/yoga-series/yoga-9-2-in-1-14ill10/downloads/ds572874-intel-integrated-sensor-hub-driver-for-windows-11-64-bit-yoga-9-2-in-1-14ill10), extracted and repackaged as an RPM for Fedora 43. It fixes tablet/laptop conversion and autorotation.

## Overview

Firmware was extracted from the driver executable as `ishS_MEU_aligned.bin` and is installed as `/usr/lib/firmware/intel/ish/ish_lnlm.bin`. The existing xz-compressed upstream version is left in place and automatically ignored while the new firmware is present. After installation, `initramfs` needs to be rebuilt and the system rebooted to load the new firmware.

## Installation

Choose the method appropriate for your Fedora installation.

### Atomic (rpm-ostree)

```
$ sudo rpm-ostree install -C /path/to/14ILL10_83LC_ISH_firm.rpm
$ sudo rpm-ostree initramfs --enable
$ sudo systemctl reboot
```

### Non-atomic (dnf)

```
$ sudo dnf install /path/to/14ILL10_83LC_ISH_firm.rpm
$ sudo dracut -f
$ sudo systemctl reboot
```

### Non-atomic (manual copy)

Extract the provided RPM and copy the firmware file:

```
$ sudo cp /path/to/ish_lnlm.bin /usr/lib/firmware/intel/ish/ish_lnlm.bin
$ sudo dracut -f
$ sudo systemctl reboot
```

## Verify installation

You can confirm that the firmware loaded correctly without converting or rotating the device:

```
$ sudo dmesg | grep ish_
```

You should see something like the following:

```
[    2.104817] intel_ish_ipc 0000:00:12.0: ISH loader: load firmware: intel/ish/ish_lnlm.bin
[    2.131492] intel_ish_ipc 0000:00:12.0: ISH loader: firmware loaded. size:526848
[    2.131494] intel_ish_ipc 0000:00:12.0: ISH loader: FW base version: 5.8.0.7720
[    2.131495] intel_ish_ipc 0000:00:12.0: ISH loader: FW project version: 1.0.6.12644
```

## Removal

When working firmware has been made available upstream, this repository will no longer be necessary.

### Atomic (rpm-ostree)

```
$ sudo rpm-ostree uninstall 14ILL10_83LC_ISH_firm
$ sudo rpm-ostree initramfs --disable
$ sudo systemctl reboot
```

### Non-atomic (dnf)

```
$ sudo dnf uninstall 14ILL10_83LC_ISH_firm
$ sudo dracut -f
$ sudo systemctl reboot
```

### Non-atomic (if manually copied)

```
$ sudo rm /usr/lib/firmware/intel/ish/ish_lnlm.bin
$ sudo dracut -f
$ sudo systemctl reboot
```
