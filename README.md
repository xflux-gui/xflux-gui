FLUXGUI DOES NOT WORK ON MOST MODERN SYSTEMS
======================

Fluxgui doesn't work on most modern systems, and there are no plans to fix it. You probably want to use [Redshift](http://jonls.dk/redshift/) via `redshift-gtk` instead. If you're curious, see:
* [#27](https://github.com/xflux-gui/fluxgui/issues/27) for why `fluxgui` probably won't work on your system and how to test if it can.
* [#113](https://github.com/xflux-gui/fluxgui/issues/113) for a possible way forward for the project.


f.lux indicator applet
======================
_Better lighting for your computer_

The f.lux indicator applet `fluxgui` is an indicator applet to control
`xflux`, an application that makes the color of your computer's
display adapt to the time of day: warm at night, and like sunlight
during the day. Reducing blue light exposure in the evening can help
you fall asleep at night. See https://justgetflux.com/research.html
for more details.

This project -- https://github.com/xflux-gui/fluxgui -- is only
concerned with the `fluxgui` indicator applet program, not with the
underlying `xflux` program the indicator applet controls. The `xflux`
program is responsible for actually changing the color of your
screen. See https://justgetflux.com/linux.html for more information
about `xflux`.

`xflux` is downloaded automatically when installing `fluxgui`. Simply
run `fluxgui` in your terminal after installation to open the applet.
You can also easily configure the applet to auto-start on login.

Install Instructions
--------------------

### Only Python 3 is Supported

The `fluxgui` is only known to work with Python 3.

### Ubuntu PPA Package Manager Install

To install via apt:

```bash
sudo add-apt-repository ppa:nathan-renniewaldock/flux
sudo apt-get update
sudo apt-get install fluxgui
```
See [ubuntuhandbook.org instructions](http://ubuntuhandbook.org/index.php/2016/03/install-f-lux-in-ubuntu-16-04/) for more details.

#### For Ubuntu 20.04 LTS and above
While `sudo apt-get update` there is an error [#144](https://github.com/xflux-gui/fluxgui/issues/144)
```console
E: The repository 'http://ppa.launchpad.net/nathan-renniewaldock/flux/ubuntu focal Release' does not have a Release file. 
```
To solve:
1. `sudo vim /etc/apt/sources.list.d/nathan-renniewaldock-ubuntu-flux-focal.list`
2. Replace `focal` to `bionic` and save it.
3. Repeat [above](#ubuntu-ppa-package-manager-install) mentioned steps from line 2. 


If you have trouble with the PPA version try the manual install below.

### Fedora Package Manager Install

There is no Fedora package provided yet. Please use [Manual Install](#manual-install) below.

### Manual Install

To install manually you first install the dependencies using your package manager, and then install `fluxgui` using the provided `setup.py`. The manual install can be done locally or system wide.

#### Install Dependencies Using Package Manager

##### Ubuntu/Debian

Partial list of Python 3 dependencies (after the uprgrade to GTK+ 3 in PR #112. If you discover the correct deps, please submit a PR):

```bash
sudo apt-get install python3-pexpect python3-distutils gir1.2-appindicator3-0.1 gir1.2-gtk-3.0
```

Out of date Python 2 dependencies; the remaining Python 3 deps should be similar:

```bash
sudo apt-get install git python-appindicator python-xdg python-pexpect python-gconf python-gtk2 python-glade2 libxxf86vm1  libcanberra-gtk-module
```

##### Fedora/CentOS

WARNING: these dependencies may be out of date after the uprgrade to GTK+ 3 in PR #112. If you discover the correct deps, please submit a PR.

```bash
sudo yum install git python-appindicator python2-pyxdg python3-pexpect gnome-python2-gconf pygtk2 pygtk2-libglade
```

#### Install `fluxgui`

There are separate instructions in the code below for installing system wide and for installing locally in your user directory; choose one.

```bash
# Download fluxgui
cd /tmp
git clone "https://github.com/xflux-gui/fluxgui.git"
cd fluxgui
./download-xflux.py

# EITHER install system wide
sudo ./setup.py install --record installed.txt

# EXCLUSIVE OR, install in your home directory
#
# The fluxgui program installs
# into ~/.local/bin, so be sure to add that to your PATH if installing
# locally. In particular, autostarting fluxgui in Gnome will not work
# if the locally installed fluxgui is not on your PATH.
./setup.py install --user --record installed.txt
       
# Run flux
fluxgui
```

### Manual Uninstall

If you manually installed instead of using package manager, you can uninstall
by making `setup.py` tell you where it installed files and then
removing the installed files.

```bash
# EITHER uninstall globally
#
# The 'installed.txt' is generated when you install. Reinstall first if you
# as described above if you don't have an 'installed.txt' file.
sudo xargs rm -vr < installed.txt
sudo glib-compile-schemas "$(dirname "$(grep apps.fluxgui.gschema.xml installed.txt)")"

# EXCLUSIVE OR uninstall in your home directory
xargs rm -vr < installed.txt
glib-compile-schemas "$(dirname "$(grep apps.fluxgui.gschema.xml installed.txt)")"
```

License
-------

The `fluxgui` applet is released under the [MIT License](https://github.com/xflux-gui/fluxgui/blob/master/LICENSE). The underlying `xflux` program that actually controls the screen color is closed source.

Developing
----------

### Coding Style

Try to stick to the same coding style that is already used in the file you are editing.
In particular, don't change the style of code you're not already editing for some other
reason. Style changes create noise in the Git history and make the `git blame` output
misleading. When reviewing a PR, the maintainers want to focus on the logical changes
introduced by your code, and extraneous style changes make that harder.

### Running `fluxgui` Without Installing

When working on `fluxgui`, you can use
```bash
cd <path to your fluxgui.git clone>
# You only need to download xflux once.
./download-xflux.py
glib-compile-schemas .
GSETTINGS_SCHEMA_DIR=`pwd` PATH=`pwd`:$PATH PYTHONPATH=`pwd`/src:$PYTHONPATH ./fluxgui
```
to test your local copy of `fluxgui` without installing anything.

### Change Logs, Versions, Releases

Note changes in `./debian/changelog`.

Use version `<ver>~pre` until ready to release a version. When
releasing a version remove the `~pre` suffix from the version strings
and commit, copying the changelog changes for the current release into
the commit message. Then `git tag -a v<ver>`, using the commit msg for
the tag annotation. Finally, create another commit with the new `<next
version>~pre` version strings and changelog entry.

When releasing the version string needs to be changed in
`debian/changelog` and `setup.py`, and the release dates needs to be
added in `debian/changelog`.
