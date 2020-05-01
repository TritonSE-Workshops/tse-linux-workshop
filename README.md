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

## System Update

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

## Installing Required Packages

Now, we're gonna get to installing everything we'll need. Run the following command:
```
sudo pacman -S i3 xorg rxvt-unicode zsh
```

If you get any lines prompting for a selection, just press enter. You shouldn't have
to worry about installing specific packages in a group.

The last step is to install polybar. Polybar isn't found in the main Arch Linux
package list unfortunately, so if we did `sudo pacman -S polybar`, it would actually
give us an error indicating that polybar can't be found. Instead, you should run:
```
yay -S polybar
```

Press enter for any prompts that you may get. You may also get some 404's from mirrors
if they aren't able to be reached but eventually you should land on a working one.

### So ... why doesn't `pacman` know what polybar is?

The `pacman` package manager is designed to communicate with a network of mirrors who
maintain the **official** Arch Linux repositories for selectively chosen, extremely
popular packages. i3, xorg, urxvt, and zsh are all examples of very widely used packages,
so they are officially maintained by the top Arch Linux brass. For more details,
see https://wiki.archlinux.org/index.php/Official_repositories.

Unfortunately, while polybar is a popular and fairly well-known tool, it simply hasn't
reach the popularity or maintainability that is necessary for it to be included in the
official repositories. In fact, many packages similar to polybar also share the same fate.
The reasons for their exclusion could be complex, or it simply could be that they aren't
as well-maintained as they should be, or it could be that a package is slated for inclusion
in the official repository in the far future. In general however, it is difficult for
a package to be included in the official archives because it has to be trusted by a wide
network of developers, each of whom must devote time to scrutinizing and maintaining it.
This is actually the same problem that other distributions face (namely Ubuntu and Debian).

However, just because a package isn't popular doesn't mean it is useless. A select few users
might actually depend on the package. But if it's not in the official repositories, how do these
users acquire the package?

The beauty of Arch Linux is that it provides an elegant solution to this problem. Arch Linux
provides a secondary, unofficial repository called the **AUR or Arch User Repository**. The
AUR is structured exactly like the official repositories, only that any user can upload
a package! If a user uploads a package, they become the "maintainer" for that package and
are responsible for uploading new versions, fixing any build errors, etc. Packages on the
AUR are still vetted for malware, both by humans and anti-virus software, but they aren't
scrutinized as heavily as packages on the official repository, so be careful when downloading
random packages. Fortunately, polybar is a relatively popular package, even on the AUR, so
it will have a lot of attention given to it to make sure it is ok: https://aur.archlinux.org/packages/?O=0&K=polybar.

As a user, you can sign up on the Arch forums and actually vote for packages that you like.
Packages that have a high vote count (e.g. high popularity) are personally reviewed by the
official repository maintainers for potential inclusion. This is how packages are transitioned
from the unofficial AUR to the official Arch repositories.

The one problem is that the AUR is unreachable by `pacman`. `pacman` only knows about the official
Arch repositories and not the AUR, which is intentional, because the AUR is considered more unsafe.
This is why we use what's called an "AUR helper" to download packages from the AUR. In this case,
the AUR helper we are using is called `yay`, which is what I personally use. `pikaur` is also another
solid choice. A list of AUR helpers can be found here (there are many, each with their own flaws,
benefits, design choices, etc.): https://wiki.archlinux.org/index.php/AUR_helpers. 

Another cool thing about AUR helpers is that they usually wrap `pacman`. The AUR is specifically set up
such that packages that appear on the official repositories don't appear on the AUR. This means that AUR
helpers usually ping the official repositories first for checking if a package exists and only then
do they go to the AUR. You could use an AUR helper to essentially do anything that `pacman` does, from
installing official packages to upgrading your entire system. In fact, the commands are the exact same!
We could have done:

```
yay -Syu
yay -S i3 xorg rxvt-unicode zsh
```

and it would have done the exact same thing as `pacman`. 

## Tiling Window Manager Customization

## Terminal Emulator Customization

## Desktop Status Bar Customization

## Shell Customization

## Kernel Compilation
