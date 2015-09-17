bits #1
=======

## Mp3 in a terminal

..with [Mplayer](https://www.mplayerhq.hu/DOCS/HTML/en/MPlayer.html)

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
    % rundll32.exe appwiz.cpl,NewLinkHere .

In that wizard locate our scan.bat
and append it with `%RANDOM%`

![scan.bat](d/scanner.png)

## ls when cd

Just make a shell function

    % j() { cd "$@" && ls -CF; }
    % j /tmp

## .config/autostart/terminal.desktop

Even with X server and a [window manager](https://www.gnome.org/)
running, one may still like the idea of having a terminal
right from the startup. This way in Gnome

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

If this has ever happened to you,
you also know that this exhibits
with every SSH output. And it's
a **client** side problem.

Although
you [cannot](https://superuser.com/questions/485569/how-to-disable-sendenv-variables-set-in-ssh-config-from-ssh-config)
get away with a dotfile, you can
still suppress it globally in a 
`/etc/ssh/ssh_config` file. Just
comment out those pesky `SendEnv`
lines.

Or simply use [env](https://wiki.archlinux.org/index.php/Environment_variables)

    % env LANG=C ssh realname@hostname

## Package Manager

To install Firefox on Windows you have to google it, verify it, download it and run it.
To do the same on GNU/Linux you just install it because the system already knows where to look for.
Linux has [package manager](https://en.wikipedia.org/wiki/Package_manager) and [repositories](http://packman.links2linux.org/).

openSUSE `zypper in firefox`, or Debian `apt-get install firefox`, or Fedora `rpm -i firefox`

@todo SCREENSHOT `zypper lr -uP`


