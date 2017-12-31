## About

This repository includes PKGBUILD file to build and install emacs-mozc helper function in order to integrate Emac and fcitx-mozc.

This will be useful if you already have fcitx-mozc in your system and add Emacs plugin for mozc in ArchLinux.

Original souces are in : https://aur.archlinux.org/packages/emacs-mozc/ 

## Install

Fetch code

    git clone https://github.com/utky/emacs-mozc-only.git

Build and install

    cd emacs-mozc-only
    yaourt -Pi .

Successfully installed

    $ yaourt -Q emacs-mozc
    local/emacs-mozc 2.20.2673.102-2 (mozc-im)

## Maintanance

I will not add fix to this file unless I find some problem with newer version emacs, mozc in future.

