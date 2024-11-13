# Printing packages for OpenWrt

This is a [package feed] aiming at providing a complete printing stack
for OpenWrt and enable AirPrint for Cups. This does require the Avahi daemon for
AirPrint functionality. It is based loosely on Vladdrako/openwrt-printing-packages.

Notably it has:
- Cups 2.4.11
- Ghostscript 10.04.0
- Gutenprint 5.3.4-2024-06-25T13-20-6665bfaa
- lcms 2.15
- Cairo 1.17.4
- libjbigkit 2.1
- libpcre 8.41
- OpenPrinting's cups-filters 1.28.15
- poppler 0.90.0
- qpdf 11.6.3

[package feed]: http://wiki.openwrt.org/doc/devel/feeds

[timesys.com]: http://repository.timesys.com/buildsources/g/ghostscript/

### To use this feed,

- add this repository to /etc/opkg/customfeeds.conf to use a prebuilt package for aarch64_cortex-a53

```
src/gz printing https://raw.githubusercontent.com/jastheace/cups-aarch64_cortex-a53/refs/heads/main
```

- set up your router to use [external storage] for its root file
  system, as these packages require more than a 100 MB of space.

[external storage]: http://wiki.openwrt.org/doc/howto/extroot

- set up a [cross-compilation environment]
[cross-compilation environment]: http://wiki.openwrt.org/doc/devel/crosscompile

- add this line to your `feeds.conf` or `feeds.conf.default`

```
src-git printing https://github.com/jastheace/openwrt-printing-packages.git
```

- to compile everything in this feed you should use the script `setup-buildsystem.sh` or some variation of those commands.

- copy compiled packages to your router (copy the whole directory as you need the files used to index the packages)

```
scp -r ./bin/$ARCH/packages root@openwrt.lan:/storage/printer/packages/
```

- add local package source to the opkg configuration `/etc/opkg.conf` with

```
src/gz printing file:/storage/printer/packages
```

- see `opkg-install-printing-packages.sh` to see a suggestion of what to install.

- tested against *Attitude Adjustment* (because that is what I have installed...).

- Avahi is notified of printers added to Cups, and these will appear as *Air Printer*'s in iOS devices.

### opkg - https://github.com/jastheace/cups-aarch64_cortex-a53/tree/main
