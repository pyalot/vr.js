# Experimental Oculus Rift USB Driver

This implements a simple, hacky Oculus Rift dev kit driver in pure Javascript using the chrome.usb APIs. It only runs on Chrome, and only within a Packaged App.

## Preparing the Device

Unfortunately the Rift exposes itself as a HID device and operating systems prevent Chrome from using them. Hopefully future versions of Chrome will expose a HID API that allows easy access to these devices, but for now you must disable the built-in operating system HID control of the device. Once you do this the Rift will not be usable by the official Rift SDK until you disable the hacks.

No reboot is required to install/uninstall the hack, so if you want to experiment with this driver you can install the hack, test it out, and uninstall before using native Oculus SDK applications. In the future this hopefully won't be required.

### Windows

I've been unable to find a way to treat the Rift as a generic USB device. Until Chrome has a native HID API it may remain inaccessible. If you know a workaround please let me know!

### OS X

A codeless kernel extension is required to disable the system HID drivers from taking control of the Rift and preventing its use in Chrome. This extension is safe as it contains no code and is specified to only the Rift dev kit.

Installing:

* Execute `sudo ./experimental/usb-drivers/hacks/install-usb-hack.sh`

Uninstalling:

* Execute `sudo ./experimental/usb-drivers/hacks/uninstall-usb-hack.sh`

### Linux

A custom device rule that acts to unbind the Rift from the USB HID driver every time its plugged in is required.

Installing:

* Execute `sudo ./experimental/usb-drivers/hacks/install-usb-hack.sh`

Uninstalling:

* Execute `sudo ./experimental/usb-drivers/hacks/uninstall-usb-hack.sh`

## Preparing the Demo

* Execute `./experimental/usb-driver/setup.sh` to copy files.
* Load the `experimental/usb-driver` unpacked extension from chrome://extensions
* Launch!

## Limitations

The chrome.usb APIs (and all of the chrome.* APIs) are implemented very inefficiently right now. THe actual time spent in Javascript while using this driver is miniscule and the overhead from the APIs dwarfs it by orders of magnitude. Hopefully work can be done to optimize the IPC occurring during each API call, as well as adding APIs that more accurately model the use case of USB (for example, overlapping interrupt reads to prevent the round-trip stalls).

The Chrome API's only expose raw USB devices and there is no API for HID devices (such as the Rift). This is why the hacks above are required. My hope is that Chrome will expose HID devices through chrome.usb (or some other API) to allow this driver to work on all platforms without any work by the user.

## Ideas

* Refactor and mock out a chrome.usb.hid API, use that instead of random HID code.