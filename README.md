# kicksecure-live-build - Building Kicksecure-like images with live-build

WARNING: UNOFFICIAL, USE AT YOUR OWN RISK. If security or usability is a concern, please download Kicksecure from https://kicksecure.com. This repository exists for development purposes only.

This is a live-build configuration tree for building a Kicksecure-like image. I say Kicksecure-like because for one this is not official, and for two it generates an ISO that differs from the official Kicksecure ISO in multiple substantial ways (some of these differences are an unavoidable result of the build tooling in use, others are because of bugs yet to be worked out). Notably CPU microcode is NOT yet included, thus significantly reducing the security of this image. This will be fixed soon.

* All packages are downloaded at builds time from official repositories. Packages are not compiled from source *yet*.
* Due to the use of dracut as an initramfs, you must build and use [a fork of live-build](https://salsa.debian.org/ArrayBolt3/live-build/-/tree/arraybolt3/lb-dracut?ref_type=heads) in order to build this image. You will need to use the `arraybolt3/lb-dracut` branch. The fork is a combination of the upstream live-build code along with the code from https://salsa.debian.org/live-team/live-build/-/merge_requests/353 and some fixes I put in place to make things work. Ultimately all of this will eventually be upstreamed into live-build if at all possible.
* You will need to build patched versions of [desktop-config-dist](https://github.com/ArrayBolt3/desktop-config-dist/tree/arraybolt3/live-build) and [live-config-dist](https://github.com/ArrayBolt3/live-config-dist/tree/arraybolt3/live-build) and place the resulting deb files from these builds under `config/packages.chroot` for the build to function properly. These packages are NOT shipped in the configuration tree for security reasons (I don't want to ship binary blobs built on my machine). The changes to these will eventually be upstreamed if at all possible. The links go to the corresponding git repositories, you will want to use the `arraybolt3/live-build` branch for both of them.
* Debian Sid is the only supported build operating system. You can install it as a chroot underneath Debian 12 or whatever OS you're currently using, then install the fork of live-build into the chroot and do your building from there.

Once you have your customized deb packages in place and your build environment set up, the actual build can be done by issuing the following commands while in the root of the git repo:

    lb config
    sudo lb build

This should eventually spit out a a number of build artifacts, including the ISO file itself, `live-image-amd64.hybrid.iso`. You can flash this to a USB drive and boot it, or run it in a virtual machine by running `qemu-system-x86_64 -enable-kvm -m 4G -smp 2 -cdrom live-image-amd64.hybrid.iso`.

## TODO

* Ensure that apt updates, upgrades, installs work
* Integrate with derivative-maker
* Get live-build patches merged upstream
