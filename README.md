# linux-notes

Various notes about working with Linux for group computers. 

## Flatpak and Appimage

It is good practice to use ```sudo ...``` or root access sparingly or not at all. Most applications can be installed as a flatpak via [Flathub](https://flathub.org/home). Flatpak is a packaging format that allows software installation that is (1) independent of the linux distribution, (2) sanboxed, (3) runs applications in user space, (4) are curated from a central repository like Flathub, and (5) are frequently automatically updated. Combined this reduces security risks and improves system stabilty. You may need to enable Flathub as a valid repository.

```bash
[use@host ~]$ flatpak remote-add --if-not-exist
s flathub https://flathub.org/repo/flathub.
flatpakrepo
```

An alternative for running software in user space are Appimage packages. Appimage packages are binary packages that also run independendent of the distribution. However, Appimages are often downloaded directly from the source (e.g. developer website) and this results in a slightly higher security risk. Appimages posted on GitHub and of popular software are generally safe to use. Examples of Appimages are [belanaEtcher](https://www.balena.io/etcher/) or [neovim](https://github.com/neoim/nevim/releases). Furthermore Appimages do not automatically update, so the onus is on you to check for new versions.

To run an Appimage you need to download the file and change the permission to execute

```bash
[user@host ~]$ chmod a+x nvim.appimage
```

You can the run the file

```bash
[user@host ~]$ ./nvim.appimage
```

## User Space Executables

Linux places global executables (for all users) in ```/usr/bin``` or ```/usr/local/bin```.
Check that the following is in your ```.bashrc``` file

```bash
# User specific environment
if ! [[ "$PATH" =~ "$HOME/.local/bin:$HOME/bin:" ]]
then
    PATH="$HOME/.local/bin:$HOME/bin:$PATH"
fi
export PATH
```

Files places in ```/home/$USER/.local/bin/``` are then executable from the command line.

## Julia Install (local)

Download the latest binary tree executable from [https://julialang.org/downloads/](https://julialang.org/downloads/)

```bash
[user$host ~]$ https://julialang-s3.julialang.org/bin/linux/x64/1.7/julia-1.7.3-linux-x86_64.tar.gz
```

Then moce/extract it in your desired location. I have all my downloaded Software in ```/home/$USER/Documents/Software/```

```bash
[user@host ~]$ mkdir -p /home/$USER/Documents/Software/
[user@host ~]$ mv julia-1.7.3-linux-x86_64.tar.gz /home/$USER/Documents/Software/
[user@host ~]$ cd /home/$USER/Documents/Software
[user@host Software]$ tar xvfz julia-1.7.3-linux-x86_64.tar.gz 
[user@host Software]$ ln -s /home/$USER/Documents/Software/julia-1.7.3-linux-x86_64/julia-1.7.3/bin/julia /home/$USER/.local/bin/julia
```

**Note: If you are updating Julia to a new version you must first delete the old soft link**

```bash
[user@host ~]$ rm ~/.local/bin/julia
```

Test that julia works

```bash
[user@host ~]$ julia
              _
   _       _ _(_)_     |  Documentation: https://docs.julialang.org
  (_)     | (_) (_)    |
   _ _   _| |_  __ _   |  Type "?" for help, "]?" for Pkg help.
  | | | | | | |/ _` |  |
  | | |_| | | | (_| |  |  Version 1.7.3 (2022-05-06)
 _/ |\__'_|_|_|\__'_|  |  Official https://julialang.org/ release
|__/                   |

julia>

```

## Enable Drag-Drop for Chrome installed via Flatpak/Flathub

Flatpak apps are sanboxed. Drag and drop requires that the flatpak app has permission to interact with the file system. Run this to enable drag and drop. 

```bash
[user@host ~]$ flatpak override --user --filesystem=home com.google.Chrome
```

It should be possible now to drag an image from the file system to google slides.

