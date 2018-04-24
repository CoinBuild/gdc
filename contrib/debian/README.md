
Debian
====================
This directory contains files used to package gdcd/gdc-qt
for Debian-based Linux systems. If you compile gdcd/gdc-qt yourself, there are some useful files here.

## gdc: URI support ##


gdc-qt.desktop  (Gnome / Open Desktop)
To install:

	sudo desktop-file-install gdc-qt.desktop
	sudo update-desktop-database

If you build yourself, you will either need to modify the paths in
the .desktop file or copy or symlink your gdcqt binary to `/usr/bin`
and the `../../share/pixmaps/gdc128.png` to `/usr/share/pixmaps`

gdc-qt.protocol (KDE)

