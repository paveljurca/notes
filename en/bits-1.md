bits #1
=======

## Mp3 in a terminal's background

We're gonna use [Mplayer](https://www.mplayerhq.hu/DOCS/HTML/en/MPlayer.html).
You should obtain it via your favorite [package manager](https://en.wikipedia.org/wiki/Package_manager)
at a no time. But make sure to have a proper [repository](http://packman.links2linux.org/) first ;)

    % play() { mplayer "$1" -volume 68 -noconsolecontrols &>/dev/null; & }
    % play nyan-cat.mp3
    % //carry on

## Scanning via a desktop shortcut

Have IrfanView installed and of course
a connected one scanner. Then fire up [CMD](https://en.wikipedia.org/wiki/Cmd.exe)
by pressing [ Win+R ] and type *cmd*

    % cd %USERPROFILE%\Desktop 
    % copy con scan.bat
    @echo off
    cd %USERPROFILE%\Desktop
    start "" "C:\Program Files (x86)\IrfanView\i_view32.exe" /scanhidden /dpi=(300,300) /gray /convert=%1%.jpg
    Ctrl^D
    % rundll32.exe appwiz.cpl,NewLinkHere .

In that wizard locate our scan.bat
and append it with *%RANDOM%*

![SCANNER shortcut](d/scanner.png)

## ls when cd

Just make a shell function like this:

    % j() { cd "$@" && ls -CF; }
    % j /tmp

## .config/autostart/terminal.desktop

Even with X server and a [window manager](https://www.gnome.org/)
running, one may still like the idea of having a terminal
right from the startup. This way you would do it in Gnome:

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

source: [Desktop Entry Specification](https://developer.gnome.org/desktop-entry-spec/)

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
comment out the pesky `SendEnv`
lines.

Or simply use [env](https://wiki.archlinux.org/index.php/Environment_variables)

    % env LANG=C ssh realname@hostname


