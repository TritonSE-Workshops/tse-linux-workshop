# tse-linux-workshop

This guide will walk you through how to take a barebones Arch Linux installation
and customize it to your liking. It'll also go over a bit of kernel compilation.

## VirtualBox Setup

Before we begin, you must have **at least 6-7 GB of free disk space** available.

The first step is to download the OVA file for the VirtualBox. This is basically
just a compressed version of the virtual hard drive along with some metadata. 
It's a 2GB file, so the download might take some time.

https://drive.google.com/open?id=1CR3ot6dPG2KJt3ZMXwKF24Q2Syy4mi-S

After you've acquired the OVA file, you can open VirtualBox. Go to the top left
of the corner and click on "Import Applicance". It should prompt you to look
for the OVA file you just downloaded. Just follow the steps as it walks you
through the import process.

Once you've imported and started the virtual machine successfully, you can
go ahead and delete the OVA file because it won't be needed anymore.

## Logging In

The login credentials are listed below:

* Username: tse
* Password: .

Note that the password is literally just a period. This should
make it easy to switch to superuser access.

After you login, you should be greeted with a bash shell. Type in

```
echo $SHELL
```

It should print "/bin/bash". Try `$XDG_SESSION_TYPE` too; it should be "tty".

## Required Packages

The following packages should already be present on your system. Most of this guide
will focus on how to set them up.
* xorg
* i3
* rxvt-unicode
* zsh
* polybar
* asp

If, for some reason, you find that one of these packages isn't installed, you can do so
with:
```
yay -S <PACKAGE NAME>
```

For reference, you can update the entire system using:
```
yay -Syu
```

However, I wouldn't recommend doing this, because it could take a long time, depending on
how many packages have been updated since I last ran the command.

## Swapping Shells

First, let's stop using the bash shell. Instead, let's try switching to the fish shell.
fish should already be installed on your machine, so try running it with:

```
fish
```

You should notice some immediate improvements. There is now color highlighting, and
there is also tab-based autocompletion for both commands and directories. Try typing
in "bas" and pressing TAB. It should allow you to cycle through the various commands
prefixed with "bas" including "bash". If you press ENTER

Now, let's replace bash with fish for our default shell. Type in:

```
chsh
```

When it prompts for the location of your new shell, use `/bin/fish`.

Now, log out by pressing CTRL-D twice. The first CTRL-D will log you out of fish
and back into your original bash shell. The second CTRL-D will log you out of
your bash shell back into the login screen. When you relog, you should spawn
a fish shell instead of a bash shell.

## Tiling Window Manager Customization

## Terminal Emulator Customization

## Desktop Status Bar Customization

## Kernel Compilation

This last step is intended to teach you about how Arch Linux handles kernel compilation.

The Arch Build System (ABS) is a system for taking source code and compiling it into
binary packages that `yay` or `pacman` can install. Most packages will provide a PKGBUILD
file that specifies how their source code is compiled into a package (similar to a Makefile).
For packages in the official Arch Linux repositories, you can find Git repositories containing
their PKGBUILDs using a tool called `asp`. We're going to look at the Git repository for Arch's
**linux** package, which, as you might imagine, contains the Linux PKGBUILD.

First, run:

```
asp update linux
```

This will cause `asp` to pull the Linux PKGBUILD repository from the Internet and store it
somewhere on your computer (in some file cache).

Now, run:

```
cd ~
asp checkout linux
```

You should see the **linux** directory if you run `ls`. Try running `tree linux`. You should get:

```
linux
├── repos
│   ├── core-i686
│   │   ├── 90-linux.hook
│   │   ├── config.i686
│   │   ├── config.x86_64
│   │   ├── linux.install
│   │   ├── linux.preset
│   │   └── PKGBUILD
│   ├── core-x86_64
│   │   ├── config
│   │   ├── PKGBUILD
│   │   └── sphinx-workaround.patch
│   └── testing-x86_64
│       ├── config
│       ├── PKGBUILD
│       └── sphinx-workaround.patch
└── trunk
    ├── config
    ├── PKGBUILD
    └── sphinx-workaround.patch

5 directories, 15 files
```

There are two main subdirectories of note, **repos** and **trunk**.
1. **repos** contains the PKGBUILD for Linux, separated by assembly language. Arch Linux suports i686 and x86_64, which is why you see core-1686 and core-x86_64. Your machine is running x86_64, so we're going to use **linux/repos/core-x86_64**.
2. The **trunk** is used by developers for staging/testing purposes. It's not super relevant to our needs.

Now, let's inspect the x86_64 PKGBUILD. Run:
```
cd linux/repos/core-x86_64
vim PKGBUILD
```

At the top, you'll see a couple of variables. PKGBUILD's are really just retextured bash
scripts, so all of the syntax is the same. Some variables of note:
* pkgbase=linux <sub>The name of the package</sub>
* pkgver=5.6.11.arch1 <sub>The version of Linux we're using</sub>
* makedepends=(...) <sub>The packages that the Linux package depends on</sub>
* validpgpkeys=(...) <sub>Used to verify the integrity of source files</sub>
* sha256sums=(...) <sub>The checksums for all of the files we're compiling.</sub>

Scrolling down, there are two functions of note, standardized by the PKGBUILD system:
* prepare() { ... } <sub>Applies patches to Linux, generates a default config</sub>
* build() { ... } <sub>Runs Linux Makefile build commands</sub>

Now, let's actually use the PKGBUILD to compile Linux. Inside linux/repos/core-x86_64, run:

```
MAKEFLAGS="-j1" makepkg -sc
```

The `-s` flag tells `makepkg` to install any missing dependencies (syncdeps), and the `-c` flag tells `makepkg` to clean up after producing a valid binary package. The MAKEFLAGS option is passed to the Linux Makefile, and in turn, "-j1" specifies that Linux should use 1 processor in the compilation phase.

Linux takes a long time to compile, and it will take even longer running on a virtual machine with only one thread compiling. The PKGBUILD will also clone the entire Linux source tree (which is huge), so it's probably not going to finish in your lifetime. Exit out using CTRL-C.
