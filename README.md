# tse-linux-workshop

This guide will walk you through how to take a barebones Arch Linux installation
and customize it to your liking. It'll also go over a bit of kernel compilation.

## VirtualBox Setup

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

## Recommended System Update

Once you log in, you should probably update the package list as well as any outdated
packages.

Like most other standard Linux distributions, Arch Linux maintains a list of packages that
have been installed on the system thus far, as well as what versions of these packages
are present. Updating the system is thus a three-part process:

* First, we have to fetch an updated package list. The Arch Linux maintains a number of
package list servers, also known as "mirrors", that list all known Arch Linux packages
and their current versions. When you update, the first thing your Linux distribution will
do is hit the closest, active mirror to it. Then, for each of your packages, it'll compare
your version of the package with the most recent version. If they're different, then your
distribution will know that it needs to fetch a new binary for that package.
* Once your distribution has an updated package list, it'll go through each of the outdated
packages and fetch a new binary from the Arch Linux mirrors. `pacman`, Arch Linux's package
manager, will keep these in a temporary cache on your system.
* Once all updated packages have been fetched, Arch Linux will decompress and install each
one.

The cool thing is that you can do all of this with a single command. Try running:
```
sudo pacman -Syu
```

Remember: the superuser password is the login password for tse. This may also take some time 
to complete, depending on how recently I uploaded the OVA file.

## Tiling Window Manager Installation

## Terminal Emulator Installation

## Desktop Status Bar Installation

## Shell Installation

## Tiling Window Manager Customization

## Terminal Emulator Customization

## Desktop Status Bar Customization

## Shell Customization

## Kernel Compilation
