# Package cleanup suggestions

Written on 2026-10-27 from the `pkglists` history and a search of the dotfiles. The Arch laptop itself was not inspected, so nothing here is proven. Check each item on the laptop before removing it.

## What happened on 2026-10-24

* Five auto-commits changed `primary/pacman.txt` (now `personal/pacman.txt`). The list went from 269 to 220 packages: 50 removed, 1 added.
* Added: `fastfetch`, which replaces `neofetch`.
* Probably replaced, not forgotten: `neofetch` (now `fastfetch`), `swayidle` (now `hypridle`, which the config uses), and `hyprlang`, `hyprutils`, `hyprgraphics`, `aquamarine`, `hyprwire`, `hyprtoolkit`, `hyprland-protocols`. The last seven are Hyprland's own libraries and most likely became dependencies, so they stopped being listed without being uninstalled.
* Real removals, about 40: `thunderbird`, `geary`, `vlc`, `libreoffice-fresh`, `gnumeric`, `meld`, `wezterm`, `lazydocker`, `mcfly`, `hashcat`, `john`, `rarcrack`, `cmake`, `meson`, `python-pip`, `python-setuptools`, `dialog`, `dog`, `speedtest-cli`, `net-tools`, `wdisplays`, `wlr-randr`, `wtype` and others.
* No config files in the dotfiles repos belong to the removed apps, so there is nothing stale to clean up.
* None of the 50 came back later.

## The list is regenerated from the laptop

* `update-generated-pkgs` rewrites the list from what is installed (`-Qqe`). Manual edits to the list are overwritten the next time it runs on the laptop.
* After the 2026-10-24 run, `sddm` (removed from the lists earlier) and `bitwarden-cli` (it conflicts with `nodejs`, so the Bitwarden CLI now comes from npm) are listed again.
* To remove a package for good: uninstall it on the laptop with `paru -Rns <package>`, then let the list regenerate.

## How the candidates were found

* Every package in `personal/pacman.txt` (220) was searched for in `voidrice`, `universal` and the nvim config.
* 87 are mentioned somewhere. 133 are not.
* Not mentioned does not mean unused: kernel, firmware, fonts, `openssh` and `networkmanager` are fine. It only narrows the search.

## Batch 1: most likely forgotten extras

* `cheese`: webcam app. The `preview-webcam` script already covers it.
* `cmatrix`: scrolling Matrix screen saver.
* `prismlauncher`: Minecraft launcher.
* `dbeaver`: SQL client.
* `gnome-multi-writer`: writes an ISO to several USB sticks at once.
* `nginx`: web server, probably left from a past project.
* `stress`: CPU and memory stress test.
* `speech-dispatcher`: speech synthesis service, usually pulled in as a dependency.
* `urlview`: URL picker for mail.
* `xdg-ninja`: checks `$HOME` for stray dotfiles.
* `pkgstats`: sends your package list to Arch.
* `ntp`: superseded by `systemd-timesyncd`.

## Next batches: overlaps worth a look

* Network stack: `iwd`, `networkmanager`, `dhcpcd`, `dnsmasq`. NetworkManager can use `iwd` as its Wi-Fi backend and does its own DHCP, so `dhcpcd` and `dnsmasq` may be unneeded.
* Power: `tlp`, `throttled`, `powertop`. `powertop` auto-tuning can fight `tlp`. `throttled` is for the ThinkPad throttling fix in `todo.md`, so keep it if that is still wanted.
* Archive tools: `7zip`, `atool`, `file-roller`. Three tools for one job.
* Disk tools: `gparted` and `gnome-disk-utility` overlap.
* Previews and thumbnails: `tumbler`, `ffmpegthumbnailer`, `epub-thumbnailer-git`, `docx2txt`, `odt2txt`, `catdoc`, `mediainfo`, `tesseract`, `tesseract-data-jpn`, `tesseract-data-osd`. Keep only what `lf` previews or a script really calls.
* Audio plugins for EasyEffects: `calf`, `lsp-plugins`, `zam-plugins`, `mda.lv2`. Often only one suite is used.
* Keyring and login: `gnome-keyring`, `seahorse`, `pam-gnupg`.
* Monitoring: `bmon`, `nethogs`, `mtr`, `procs`, `dust`, `ncdu`. Pick the ones you open.
* Hyprland extras: `hyprlauncher` (you use rofi), `hyprsysteminfo`, `hyprpicker`, `hyprland-guiutils`, `hyprwayland-scanner` (a build tool).
* Dev tools: `go`, `rust-analyzer`, `tree-sitter-cli`, `ctags`, `base-devel`, `dkms`, `linux-headers`. `dkms` and `linux-headers` are only needed if a package builds a kernel module.
* Editors: `neovim-nightly-bin` next to `neovim`, and `ex-vi-compat`.
* Other: `geoclue` is needed by `darkman`, so keep it. `sddm` was replaced by starting Hyprland from tty1, so it should go.

## How to check on the Arch laptop

* Oldest installs first, to find things from years ago: `expac --timefmt='%Y-%m-%d' '%l\t%n' $(pacman -Qeq) | sort | head -n 60`
* Biggest first: `expac -H M '%m\t%n' $(pacman -Qeq) | sort -rh | head -n 30`
* Orphaned dependencies: `pacman -Qtdq`
* What needs a package: `pacman -Qi <package> | grep 'Required By'`
* Remove with its unused dependencies and config: `paru -Rns <packages>`
* Regenerate the lists afterwards with `update-generated-pkgs`.

## Appendix: the 133 packages the dotfiles never mention

Descriptions come from the official repository databases and the AUR.

### Command-line tools and dev

* `7zip` (extra): File archiver for extremely high compression
* `atool` (extra): A script for managing file archives of various types
* `base-devel` (core): Basic tools to build Arch Linux packages
* `bitwarden-cli` (extra): The command line vault
* `bmon` (extra): Portable bandwidth monitor and rate estimator
* `calf` (extra): LV2 plug-in suite
* `catdoc` (extra): A convertor for Microsoft Word, Excel, PowerPoint and RTF Files to text
* `claude-code` (AUR): An agentic coding tool that lives in your terminal
* `cmatrix` (extra): A curses-based scrolling 'Matrix'-like screen
* `ctags` (extra): Generates an index file of language objects found in source files
* `dkms` (extra): Dynamic Kernel Modules System
* `docx2txt` (extra): Recovers text from DOCX files, with good formatting
* `downgrade` (AUR): Bash script for downgrading one or more packages to a version in your cache or the A.L.A
* `dust` (extra): A more intuitive version of du in rust
* `entr` (extra): Run arbitrary commands when files change
* `epub-thumbnailer-git` (AUR): EPUB e-books thumbnailer for file managers that make use of python-pillow
* `ex-vi-compat` (extra): The ex and vi commands based on vim's compatibility modes
* `fastfetch` (extra): A feature-rich and performance oriented neofetch like system information tool
* `fd` (extra): Simple, fast and user-friendly alternative to find
* `file-roller` (extra): Create and modify archives
* `geoclue` (extra): Modular geoinformation service built on the D-Bus messaging system
* `git-delta` (extra): Syntax-highlighting pager for git and diff output
* `glow` (extra): Command-line markdown renderer
* `gnome-disk-utility` (extra): Disk Management Utility for GNOME
* `gnome-keyring` (extra): Stores passwords and encryption keys
* `gnome-multi-writer` (extra): Write an ISO file to multiple USB devices at once
* `go` (extra): Core compiler tools for the Go programming language
* `gparted` (extra): A Partition Magic clone, frontend to GNU Parted
* `gst-plugin-pipewire` (extra): Multimedia graph framework - pipewire plugin
* `hyprlauncher` (extra): A multipurpose and versatile launcher / picker for Hyprland
* `hyprpicker` (extra): A wlroots-compatible Wayland color picker that does not suck
* `hyprwayland-scanner` (extra): A Hyprland implementation of wayland-scanner, in and for C++
* `iwd` (extra): Internet Wireless Daemon
* `jq` (extra): Command-line JSON processor
* `lf` (extra): A terminal file manager inspired by ranger
* `lsp-plugins` (extra): Collection of open-source plugins
* `man-db` (core): A utility for reading man pages
* `mda.lv2` (extra): A port of the MDA VST plugins to LV2
* `mediainfo` (extra): Supplies technical and tag information about media files (CLI interface)
* `mtr` (extra): Combines the functionality of traceroute and ping into one tool (CLI version)
* `ncdu` (extra): Disk usage analyzer with an ncurses interface
* `neovim-remote` (AUR): Support --remote and friends for Neovim
* `nethogs` (extra): A net top tool which displays traffic used per process instead of per IP or interface
* `nginx` (extra): Lightweight HTTP server and IMAP/POP3 proxy server
* `notmuch` (extra): Notmuch is not much of an email program
* `odt2txt` (extra): extracts the text out of OpenDocument Texts
* `os-prober` (extra): Utility to detect other OSes on a set of drives
* `pacman-contrib` (extra): Contributed scripts and tools for pacman systems
* `pam-gnupg` (AUR): Unlock GnuPG keys on login
* `perl-file-mimeinfo` (extra): Determine file type, includes mimeopen and mimetype
* `pkgstats` (extra): Submit a list of installed packages to the Arch Linux project
* `prismlauncher` (extra): Minecraft launcher with ability to manage multiple instances
* `procs` (extra): A modern replacement for ps written in Rust
* `rofi-emoji-git` (AUR): A Rofi plugin for selecting emojis
* `rsync` (extra): A fast and versatile file copying tool for remote and local files
* `rust-analyzer` (extra): Rust compiler front-end for IDEs
* `sddm` (extra): QML based X11 and Wayland display manager
* `seahorse` (extra): GNOME application for managing PGP keys
* `speech-dispatcher` (extra): High-level device independent layer for speech synthesis interface
* `stress` (extra): A tool that stress tests your system (CPU, memory, I/O, disks)
* `tesseract` (extra): An OCR program
* `tesseract-data-jpn` (extra): Tesseract OCR data (jpn)
* `tesseract-data-osd` (extra): Tesseract OCR data (osd)
* `tree-sitter-cli` (extra): CLI tool for developing, testing, and using Tree-sitter parsers
* `tumbler` (extra): Thumbnail service implementing the thumbnail management D-Bus specification
* `urlview` (AUR): A curses URL parser for text files
* `wev` (extra): A tool for debugging wayland events on a Wayland window, analagous to the X11 tool xev
* `wl-clipboard` (extra): Command-line copy/paste utilities for Wayland
* `xdg-ninja` (AUR): A shell script which checks your $HOME for unwanted files and directories
* `zenity` (extra): Display graphical dialog boxes from shell scripts

### Desktop apps and themes

* `adw-gtk-theme` (extra): Unofficial GTK 3 port of the libadwaita theme
* `adwaita-qt6` (AUR): A style to bend Qt6 applications to look like they belong into GNOME Shell
* `bc` (extra): An arbitrary precision calculator language
* `breeze` (extra): Artwork, styles and assets for the Breeze visual style for the Plasma Desktop
* `cheese` (AUR): Take photos and videos with your webcam, with fun graphical effects
* `dbeaver` (extra): Free universal SQL Client for developers and database administrators (community edition)
* `ffmpegthumbnailer` (extra): Lightweight video thumbnailer that can be used by file managers
* `gtk3` (extra): GObject-based multi-platform GUI toolkit
* `hyprland-guiutils` (extra): Hyprland GUI utilities
* `hyprsysteminfo` (AUR): A tiny qt6/qml application to display information about the running system
* `imagemagick` (extra): An image viewing/manipulation program
* `neovim-nightly-bin` (AUR): Fork of Vim aiming to improve user experience, plugins, and GUIs (nightly build)
* `nwg-look` (extra): GTK settings editor adapted to work on wlroots-based compositors
* `perl-image-exiftool` (extra): Read and write EXIF information
* `pipewire-jack` (extra): Low-latency audio/video router and processor - JACK replacement
* `qt5-wayland` (extra): Provides APIs for Wayland
* `transmission-cli` (extra): Fast, easy, and free BitTorrent client (CLI tools, daemon and web client)
* `wf-recorder` (extra): Screen recorder for wlroots-based compositors such as sway
* `zam-plugins` (extra): Collection of audio plugins for high-quality processing
* `zathura-pdf-mupdf` (extra): PDF support for Zathura (MuPDF backend) (Supports PDF, ePub, and OpenXPS)

### Hardware, drivers, power

* `bluez-utils` (extra): Development and debugging utilities for the bluetooth protocol stack
* `fprintd` (extra): D-Bus service to access fingerprint readers
* `fwupd` (extra): Simple daemon to allow session software to update firmware
* `intel-media-driver` (extra): Intel Media Driver for VAAPI - Broadwell+ iGPUs
* `intel-ucode` (extra): Microcode update files for Intel CPUs
* `linux-firmware` (core): Firmware files for Linux - Default set
* `mesa-utils` (extra): Essential Mesa utilities
* `ntfs-3g` (extra): NTFS FUSE driver
* `pipewire-alsa` (extra): Low-latency audio/video router and processor - ALSA configuration
* `powertop` (extra): A tool to diagnose issues with power consumption and power management
* `sof-firmware` (extra): Sound Open Firmware
* `throttled` (extra): Workaround for Intel throttling issues in Linux
* `tlp` (extra): Linux Advanced Power Management
* `vulkan-intel` (extra): Open-source Vulkan driver for Intel GPUs
* `whois` (extra): Intelligent WHOIS client

### Libraries and runtimes

* `fcitx5-gtk` (extra): Fcitx5 gtk im module and glib based dbus client library
* `hyprcursor` (extra): The hyprland cursor format, library and utilities
* `lib32-mesa` (multilib): Open-source OpenGL drivers - 32-bit
* `lib32-vulkan-intel` (multilib): Open-source Vulkan driver for Intel GPUs - 32-bit
* `libnotify` (extra): Library for sending desktop notifications
* `libpulse` (extra): A featureful, general-purpose sound server (client library)
* `libva-utils` (extra): Intel VA-API Media Applications and Scripts for libva
* `linux-headers` (core): Headers and scripts for building modules for the Linux kernel
* `mpv-mpvacious` (AUR): Adds mpv keybindings to create Anki cards from movies and TV shows
* `nuspell` (extra): Fast and safe spellchecking C++ library
* `perl-compress-raw-lzma` (AUR): Low-Level Perl Interface to lzma compression library
* `poppler` (extra): PDF rendering library based on xpdf 3.0

### System, boot, network

* `dhcpcd` (extra): DHCP/ IPv4LL/ IPv6RA/ DHCPv6 client
* `dnsmasq` (extra): Lightweight, easy to configure DNS forwarder and DHCP server
* `dosfstools` (core): DOS filesystem utilities
* `efibootmgr` (core): Linux user-space application to modify the EFI Boot Manager
* `gvfs-mtp` (extra): Virtual filesystem implementation for GIO - MTP backend (Android, media player)
* `networkmanager` (extra): Network connection manager and user applications
* `openssh` (core): SSH protocol implementation for remote login, command execution and file transfer
* `zram-generator` (extra): Systemd unit generator for zram devices

### Input method and spelling

* `aspell` (extra): A spell checker designed to eventually replace Ispell
* `fcitx5-configtool` (extra): Configuration Tool for Fcitx5
* `fcitx5-mozc` (extra): Fcitx5 Module of A Japanese Input Method for Chromium OS, Windows, Mac and Linux (the Open Sour
* `ntp` (extra): Network Time Protocol reference implementation

### Fonts

* `noto-fonts` (extra): Google Noto TTF fonts
* `ttf-jetbrains-mono-nerd` (extra): Patched font JetBrains Mono from nerd fonts library
* `ttf-ubraille` (AUR): Unicode Braille font
