bits #1
=======

## Mp3 in a terminal

by [Mplayer](https://www.mplayerhq.hu/DOCS/HTML/en/MPlayer.html)

    % play() { mplayer "$1" -volume 68 -noconsolecontrols &>/dev/null; & }
    % play nyan-cat.mp3
    % //carry on

## IrfanView scan

`Win+R` and [CMD](https://en.wikipedia.org/wiki/Cmd.exe)

    % cd %USERPROFILE%\Desktop 
    % copy con scan.bat
    @echo off
    cd %USERPROFILE%\Desktop
    start "" "C:\Program Files (x86)\IrfanView\i_view32.exe" /scanhidden /dpi=(300,300) /gray /convert=%1%.jpg
    Ctrl^D

Finally create a scan.bat shortcut with `%RANDOM%` at the end.

    % rundll32.exe appwiz.cpl,NewLinkHere .

![scan.bat](d/scanner.png)

## ls when cd

To list content when changing directory one can do

    % j() { cd "$@" && ls -CF; }
    % j /tmp

## .config/autostart/terminal.desktop

Even with X server and a [window manager](https://www.gnome.org/)
one may still like the idea of having a terminal
right from the startup

    % cat > $HOME/.config/autostart/terminal.desktop
    [Desktop Entry]
    Type=Application
    Exec=/usr/bin/gnome-terminal
    Hidden=false
    NoDisplay=false
    X-GNOME-Autostart-enabled=true
    Name=terminal
    Comment=#!
    Ctrl^D

* [Desktop Entry Specification](https://developer.gnome.org/desktop-entry-spec/)

## SSH SendEnv

![$LC_ALL](d/lc-all.png)

If this has ever happened to you, you also know that this exhibits
with every SSH output. It's a **client side** problem.

Although you [cannot](https://superuser.com/questions/485569/how-to-disable-sendenv-variables-set-in-ssh-config-from-ssh-config)
get away with a dotfile, you can still suppress it globally in
`/etc/ssh/ssh_config` file, i.e. comment out the pesky `SendEnv` lines.

Or simply use [env](https://wiki.archlinux.org/index.php/Environment_variables)

    % env LANG=C ssh realname@hostname

## Package Manager

To install Firefox on Windows you have to google this, check that, download this and run that.  
To do the same on GNU/Linux you just install it because system knows who to ask. Linux has a [package manager](https://en.wikipedia.org/wiki/Package_manager) and [repositories](http://packman.links2linux.org/), i.e. checked SW lists where to look for.

openSUSE `zypper in firefox`, or Debian `apt-get install firefox`, or Fedora `rpm -i firefox`

@todo SCREENSHOT `zypper lr -uP`

Or you can always [compile it from the source](http://tldp.org/LDP/LG/current/smith.html) or just execute it, i.e. Sublime Text or youtube-dl.

* [Makefile](https://en.wikipedia.org/wiki/Makefile)


