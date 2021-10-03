### About

*linux-browser-installer* is a Bourne shell script to install Linux versions of
the Chrome, Brave or Vivaldi browsers under FreeBSD into a Linux (Ubuntu Focal) chroot.
They allow you to use web services like *Netflix*, *Prime Video*, or *Spotify*
which require [Widevine](https://en.wikipedia.org/wiki/Widevine).
The script is based on the excellent [Howto](https://forums.freebsd.org/threads/linuxulator-how-to-run-google-chrome-linux-binary-on-freebsd.77559/) by @[patovm04](https://github.com/patovm04).

If not defined otherwise, Ubuntu Focal (`$ubuntu_version`) is installed under
`/compat/ubuntu` (`$chroot_path`). A modified version of FreeBSD's linux rc-script
(`rc.d/ubuntu`) is used to start the *linuxulator*, and to mount the chroot's
filesystems.

This is a forked version to unbreak 14.0-CURRENT.

**WARNING:** Due to a segfault with `gpg` on 14, package verification is
disabled. This is **not** very secure and you should keep this in mind if you
want to use this repository.

Vivaldi also has to download via HTTP because of the Let's Encrypt certificate
encryption fiasco.

### System requirements

FreeBSD 12.2-RELEASE or higher

### Please Note

You can't run different Linux chroots at the same time. If you want to run
CentOS-based applications under `/compat/linux`, you have to set
`sysctl compat.linux.emul_path=/compat/linux`, and start the linux rc-script
(`service linux onestart`). Depending on which chroot you intend to use by
default, set either (not both) `linux_enable="YES"` or `ubuntu_enable="YES"`
in `/etc/rc.conf`.

### Usage
#### Install Chrome, Brave or Vivaldi browser

````
# ./linux-browser-installer install chrome
````

and/or

````
# ./linux-browser-installer install brave
````

and/or

````
# ./linux-browser-installer install vivaldi
````

If the chroot is not existing yet, it will be created first.

Run `/usr/local/bin/linux-chrome`, `/usr/local/bin/linux-brave`
or `/usr/local/bin/linux-vivaldi` to start your installed browser.

#### Deinstall Chrome, Brave or Vivaldi browser

````
# ./linux-browser-installer deinstall chrome
````

and/or

````
# ./linux-browser-installer deinstall brave
````

and/or

````
# ./linux-browser-installer deinstall vivaldi
````

This command deinstalls the browser, and removes its wrapper
scripts from `/usr/local/bin` and `$chroot_path/bin` along with its
desktop file.

#### Create chroot

````
# ./linux-browser-installer chroot create
````

#### Upgrade software installed in the chroot

````
# ./linux-browser-installer chroot upgrade
````

#### Delete chroot

````
# ./linux-browser-installer chroot delete
````

Before deleting the entire chroot under `$chroot_path`, this command
unmounts all the chroot's filesystems, deletes the rc script, and removes its
variable(s) from `/etc/rc.conf`.

#### Update symlinks
- - -

**Note**: Symlinks to files outside the chroot will not work when `chroot`'ing.

- - -

##### For icons

````
# ./linux-browser-installer symlink icons
````

This command updates the symlinks from `$prefix/share/icons` to
`$chroot_path/usr/share/icons`. Use this after installing new icons
to make them available to applications in the chroot.

##### For themes
````
# ./linux-browser-installer symlink themes
````

This command updates the symlinks from `$prefix/share/themes` to
`$chroot_path/usr/share/themes`. Use this after installing new themes
to make them available to applications in the chroot.

#### Delete working files from current directory
````
# ./linux-browser-installer clean
````
