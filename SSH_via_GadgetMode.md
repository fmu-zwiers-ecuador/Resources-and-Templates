# SSH into Raspberry Pi via USB Gadget Mode

This guide explains how to set up a Raspberry Pi Zero/Zero W/Zero 2 W to use USB gadget mode and connect to it via SSH over USB. This is useful when no Wi-Fi or Ethernet is available.

---

## Requirements

- Raspberry Pi Zero, Zero W, or Zero 2 W
- Micro-USB cable (data-capable, not just charging)
- Computer running macOS, Linux, or Windows
- Latest Raspberry Pi OS image (Lite recommended)

---

## 1. Prepare the SD Card

1. Flash Raspberry Pi OS to the SD card using Raspberry Pi Imager.
2. After flashing, mount the boot partition of the SD card on your computer.

---

## 2. Enable SSH

Create an empty file named `ssh` (no extension) in the boot partition:

```bash
touch /Volumes/boot/ssh    # macOS/Linux
```

On Windows: create a new text file, delete its contents, and rename it to `ssh` (no extension).

---

## 3. Enable USB Gadget Mode

1. Open `config.txt` in the boot partition and add this line at the end:

```
dtoverlay=dwc2
```

2. Open `cmdline.txt` (single line file) and after `rootwait`, add:

```
modules-load=dwc2,g_ether
```

Make sure the file is still a single line.

---

## 4. Connect Pi to Computer

1. Safely eject the SD card, insert it into the Raspberry Pi.
2. Plug the Pi into your computer using the USB data port (the port labeled "USB", not "PWR IN").
3. Wait ~1–2 minutes for the Pi to boot.

---

## 5. Find the Pi's Network Interface

On your computer, check for a new USB Ethernet interface:

### macOS:
```bash
ifconfig
```
Look for `enX` or `usbmodem` type device.

### Linux:
```bash
ip addr
```
Look for `usb0`.

### Windows:
Open Network Connections and check for a new USB Ethernet/RNDIS adapter.

---

## 6. Assign an IP Address (if needed)

Usually, the Pi will appear as `raspberrypi.local` via mDNS.

If not, manually assign your computer an IP:

- **macOS/Linux**: Set interface (e.g. `en5`) to static IP `192.168.7.1`.
- **Windows**: Set IPv4 for the RNDIS adapter to `192.168.7.1`.

The Pi will use `192.168.7.2`.

---

## 7. SSH into the Pi

Default credentials:
- **User**: `pi`
- **Password**: `raspberry`

Connect using:

```bash
ssh pi@raspberrypi.local
```

or (if `.local` doesn't resolve):

```bash
ssh pi@192.168.7.2
```

---

## 8. Optional: Configure Hostname

To make it easier to find the Pi in the future:

```bash
sudo raspi-config
```

- Go to System Options → Hostname
- Set a unique name, e.g. `beamnode1`.

Then connect with:

```bash
ssh pi@beamnode1.local
```

---

## Troubleshooting

- **No new interface appears**: Ensure you used the correct USB port on the Pi (not the PWR IN port). Check that `dtoverlay=dwc2` and `modules-load=dwc2,g_ether` were added correctly.
- **Cannot resolve .local**: Install Bonjour (Windows) or Avahi (Linux).
- **SSH connection refused**: Ensure the `ssh` file exists in the boot partition.

---

## References

- Raspberry Pi Gadget Mode Docs
- Raspberry Pi Imager
