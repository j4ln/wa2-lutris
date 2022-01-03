![Game DVD Cover](/images/cover.jpg)
# White Album 2 on Linux
This guide covers installing the DVD extended edition of [White Album 2](https://leaf.aquaplus.jp/product/wa2cc/) via Lutris to easily get the game working on Linux.

The game can be legally purchased from the following sites:
- [Amazon JP (Physical)](https://www.amazon.co.jp/dp/B0788BRGN3) - Offers global shipping
- [DMM (Digital)](https://dlsoft.dmm.co.jp/detail/aquap_0019/)


## Requirements
- White Album 2 Extended Edition as Disc (+ DVD drive) or ISO's
- A Vulkan capable GPU, and [Driver](https://github.com/doitsujin/dxvk/wiki/Driver-support) for DXVK Support


## Installation
### Enable Japanese locale
Check if it's already enabled with: `locale -a | grep ja`

If you see something like the following proceed to the next step
```
ja_JP
ja_JP.eucjp
ja_JP.ujis
ja_JP.utf8
japanese
japanese.euc
```

Otherwise generate/install the locale, and install a supported Japanese font. This will vary by distribution. A few are listed below.
- Arch:
	```
	sudo sed -i 's/^#ja_JP.UTF-8 UTF-8/ja_JP.UTF-8 UTF-8/' /etc/locale.gen
	sudo locale-gen
	sudo pacman -S otf-ipafont noto-fonts-cjk
	```
- Ubuntu:
	```
	sudo locale-gen ja_JP.{UTF-8,EUC-JP}
	sudo apt install -y fonts-{takao,mona,monapo}
	```
- Fedora:
	```
	sudo dnf groupinstall -y 'Japanese Support'
	sudo dnf install -y hanazono-fonts mona-*-fonts
	```


### Install Lutris and Wine dependencies
These steps will heavily depend on your Linux distribution. Lutris has great documentation covering these which I've linked against each.
1. [Lutris](https://lutris.net/downloads/)
2. [Wine Dependencies](https://github.com/lutris/docs/blob/master/WineDependencies.md)
3. [Drivers](https://github.com/lutris/docs/blob/master/InstallingDrivers.md) (needed for DXVK Support)


### Install 32-bit GStreamer
Without these dependencies the in game videos won't play, and throw an error.
- Arch:
	```
	sudo pacman -S lib32-gst-plugins-good
	yay -S lib32-gst-plugins-ugly lib32-gst-libav
	```
- Ubuntu:
	```
	sudo apt install -y gstreamer1.0-{plugins-{good,ugly},libav}:i386
	```
- Fedora:
	```
	sudo dnf install -y gstreamer1-{plugins-{good,ugly},libav}.i686
	```


### Using CDemu (if using ISO's)
[CDemu](https://wiki.archlinux.org/title/CDemu) can emulate the existence of a DVD drive, and is needed to be used if your installing with ISO images as opposed to using a physical DVD drive.
- Install CDemu, refer online for steps that cover your distribution
- Once installed check CDemu status with `cdemu status`
- If there's a single device listed add another with `cdemu add-device`
- Load the install images to both devices
	```
	cdemu load 0 /path/to/WA2_EE_1.iso
	cdemu load 1 /path/to/WA2_EE_2.iso
	```
- Mount both of these disks


### Running the Lutris installer
- Via Lutris | Pending moderation, use the below steps for now
- Local with the install script
	1. Download the install script [white-album-2-extended-edition-dvd.yml](/white-album-2-extended-edition-dvd.yml)
	2. Open a terminal in the directory where the file was downloaded to
	3. Install the script under Lutris with `lutris -i white-album-2-extended-edition-dvd.yml`

During installation leave the install directory as default.

After Disc 1 has finished installing, a prompt will appear asking for Disc 2 to be inserted. Insert Disc 2 and ensure you can see it's contents in your file manager before pressing OK on the prompt to continue.

If you installed the game with CDemu, make sure to unload the discs afterwards
```
cdemu unload 0
cdemu unload 1
cdemu remove-device
```


## Troubleshooting
### Game Crashes on start up
This may be due to one of the discs data being misread during installation. Try removing the game, and reinstalling it again. It might also be worth to dump both the discs as ISO's and load them with CDemu instead. If you still are getting this issue after retrying, please open an issue with any applicable logs from lutris.


### After Disc 1, the installer is unable to continue with Disk 2
To resolve this I had to reconnect my external disc reader after switching the discs so that I could mount disc 2, and see it's files correctly before pressing OK on the continue prompt.

If you are using an internal disc reader where this isn't possible you can instead dump both the discs as ISO's and load them in with CDemu before starting the installation. See the below troubleshooting heading for more on this approach.


### Disc Check Error Dialog
![Disc Check Error Dialog](/images/disc-check-error.png)  
This error may occur when the game installer is ran from an mounted ISO. Ensure your mounting the installer ISO's with [CDemu](https://wiki.archlinux.org/title/CDemu).


## Credit
[Todokanai Translations](https://todokanaitl.github.io/) team for releasing the english patch along with providing instructions for installing and patching the [game with wine](https://github.com/TodokanaiTL/wa2-wine).

[TheMoeWay's guide covering Visual novels on Linux](https://learnjapanese.moe/vn-linux/).