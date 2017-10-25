bits #0
=======

## Dropbox as a backup

The service is free up to 2GB so given
that my most important stuff is just
text files, I'm more than backed.

You update a file, it updates on Dropbox.
You delete a file, it *stays* on Dropbox!

First install [Dropbox](https://www.dropbox.com/install?os=lnx),
second make a [Cron file](http://tldp.org/LDP/lame/LAME/linux-admin-made-easy/using-cron.html).

    % su -
    % pushd /etc/cron.daily/
    % cat > backup
    #!/bin/sh
    cp --archive --update /home/pavel/txt/ /home/pavel/Dropbox
    cp --archive --update /home/pavel/code/ /home/pavel/Dropbox
    Ctrl^D
    % chown pavel:pavel bckup 
    % chmod +x backup

## Sleep mode from CLI

[pm-utils](http://www.google.com/search?q=pm+utils) handle that
    
    % echo "alias asleep='sudo /usr/sbin/pm-suspend'" >> ~/.bashrc
    % source ~/.bashrc
    % asleep

You might need to adjust the [sudoers file](http://help.ubuntu.com/community/Sudoers#Shutting_Down_From_The_Console_Without_A_Password)
to have it *password-less*:

    Cmnd_Alias SHUTDOWNS = /sbin/shutdown, /sbin/poweroff, /sbin/reboot, /usr/sbin/pm-suspend
    pavel ALL=(ALL) NOPASSWD: SHUTDOWNS

## Grub2 boot entry

If you have a dual-boot and installed GNU/Linux first
and Windows second; you also have a problem. Not only
you have to swap a boot flag (via [fdisk](http://www.debian.org/releases/slink/sparc/fdisk.txt)) between your partitions.
But [Grub2](http://www.gnu.org/software/grub/) still has
no clue about your Windows there.

Create the boot entry by opening `/etc/grub.d/40_custom` and add

    menuentry 'windows7' {
    set root='hd0,msdos3'
    chainloader +1
    }

then

    % pushd /boot/grub2/
    % cp grub.cfg grub.cfg.bak
    % grub2-mkconfig -o grub.cfg

Have a look at the `/etc/grub.d/30_os-prober` file
particularly those `--class windows` lines.

p.s. [penguin splash screen](https://en.opensuse.org/SDB:Animated_penguin_GRUB_splash_screen)

## Vim setup

The following forces [Vim](https://github.com/tpope/vim-sensible)
to change tab for 4 spaces, use `t` to comment out lines
or to check [Perl](http://github.com/vim-perl/vim-perl) [syntax](https://github.com/spicyjack/public/blob/master/notes/vim_plugins.md) on `:make`

    % cat >> ~/.vimrc
    set number
    set shiftwidth=4
    set softtabstop=4
    set shiftround
    set expandtab
    set t_Co=256
    set autoindent
    set wrap
    set showmatch
    " :make
    set makeprg=perl\ -c\ %\ $*
    set errorformat+=%m\ at\ %f\ line\ %l\.
    set errorformat+=%m\ at\ %f\ line\ %l
    set autowrite
    " color complex things
    let perl_extended_vars=1
    let perl_include_pod=1
    " quit instead of Ex mode
    map Q :q
    " toggle comment
    map t :s/^/#/g<CR>
    map tt :s/^#//g<CR>
    filetype indent on
    syntax on
    colorscheme peachpuff
    highlight LineNr ctermfg=238
    Ctrl^D

[printscreen](d/vim.png)

Please find the updated version on [my github](https://github.com/paveljurca/foo/blob/master/nix/dotfiles/vimrc).

* [Vim plugin to highlight variables in Perl](https://github.com/pjcj/vim-hl-var)
* [Vim for Perl devs](http://mamchenkov.net/wordpress/2004/05/10/vim-for-perl-developers/)
* [vi reference page](http://www.kichwa.com/quik_ref/vi_ref.html)
* [Learn Vim Progressively](http://yannesposito.com/Scratch/en/blog/Learn-Vim-Progressively/)
* [Mastering Vim](https://twitter.com/MasteringVim)
* [How to boost your Vim productivity](http://sheerun.net/2014/03/21/how-to-boost-your-vim-productivity/)
* [Little less sensible yet great vim defaults](https://github.com/sheerun/vimrc)
* [vi-improved](http://vi-improved.org/)
* [The Ultimate vimrc](https://github.com/amix/vimrc)

p.s. [How To Vimrc](http://dougblack.io/words/a-good-vimrc.html)

## Sublime setup

First get [Sublime](http://www.sublimetext.com/3),
second get a [package manager](http://sublime.wbond.net/installation). 

Then via the package manager install [SublimeLinter3](http://github.com/SublimeLinter/SublimeLinter3)
and [SublimeCodeIntel](http://github.com/SublimeCodeIntel/SublimeCodeIntel).
If doing [Perl](http://www.perl.org) you may try [SublimeLinter-perl](http://github.com/oschwald/SublimeLinter-perl)
or even [ModernPerl](https://github.com/Blaizer/ModernPerl-sublime) highlighter.

Additionaly get a nice [theme](http://colorsublime.com/) or a [font](http://font.ubuntu.com/).
The font usually goes at `/usr/local/share/fonts/truetype/Ubuntu`.

Finally set preferences at `~/.config/sublime-text-3/Packages/User/Preferences.sublime-settings`
or change colors of a *scope* like `punctuation.definition.var.perl`. To determine that scope
highlight the source keyword and press `Ctr+Alt+Shift+P`. Watch the status bar.

I made it to a [gist](https://gist.github.com/paveljurca/82ec6154c0cde29e22b4). Please see the source there.

[printscreen](d/sublime.png)

* [Colour schemes for a variety of editors](https://github.com/daylerees/colour-schemes)


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


bits #2
=======

## Bell in a shell

I'm going crazy with a system speaker and the shell's ability to ring the bell
every time I've pressed one too many `BACKSPACE` or so

    % echo 'set bell-style none' >> $HOME/.inputrc

* [Readline](http://ss64.com/bash/syntax-inputrc.html)

## /sbin/route

Being connected to [RJ-45](https://en.wikipedia.org/wiki/Ethernet) and [WiFi](https://en.wikipedia.org/wiki/Hotspot_%28Wi-Fi%29)
at once may cause a routing table to mess up, i.e. two networks, two gateways.

So look up a "primary" `Iface` and mark its [gateway default](https://en.wikipedia.org/wiki/Default_gateway)
where `10.0.0.138` is a gateway to the Internet

/* @todo screenshot */

    % sudo route  # print the table
    % sudo route add default gw 10.0.0.138

## fstab is like Diskpart's assign letter

/etc/[fstab](http://www.linfo.org/etc_fstab.html) lets you mount a given partition
persistently. Plus you may specify the `r,w,x` bits or [effective](https://doc.opensuse.org/documentation/html/openSUSE_121/opensuse-security/cha.security.acls.html#sec.security.acls.handle.defacl.eff) IDs, i.e. `dmask` dirs and `fmask` files

    /dev/sda1            /mnt/data            ntfs       uid=1000,gid=1000,dmask=027,fmask=137              0 2

## PowerPoint 2013 changes display setup

* for 2016 use `..\Office\16.0\..`
* for 2013 use `..\Office\15.0\..`

PowerPoint now breaks mirroring of displays because it's got [Enhanced Presenter View](https://support.office.com/en-us/article/View-your-speaker-notes-privately-while-delivering-a-presentation-on-multiple-monitors-ccfa1894-e3ef-4955-9c3a-444d58248093) by default. So after a first slideshow ever it resets the display mode to extended, i.e. Microsoft tells you what you want [sic]. But there's a registry key `RestoreTopology` to clean up the mess

create `C:\powerpnt_restore_topology.vbs`
    
    Set objShell = WScript.CreateObject("WScript.Shell")
    objShell.RegWrite "HKCU\Software\Microsoft\Office\15.0\PowerPoint\Options\RestoreTopology", 1, "REG_DWORD"

create `hklm_run_restore_topology.reg` and run it
    
    Windows Registry Editor Version 5.00

    [HKEY_LOCAL_MACHINE\Software\Microsoft\Windows\CurrentVersion\Run]
    "powerpnt_restore_topology"="C:\\powerpnt_restore_topology.vbs"

logoff

* [PowerPoint 2013 Changes display setup](https://answers.microsoft.com/en-us/office/forum/office_2013_release-powerpoint/powerpoint-2013-changes-display-setup/3ca470a0-d896-4577-b724-a360b6bf1c4e?auth=1)
* [How to add subkeys by using a .reg file](https://support.microsoft.com/en-us/kb/310516)

p.s. [disable presenter view](https://social.technet.microsoft.com/Forums/office/en-US/a91dea3b-9c88-4d2b-9545-e48c3055a16c/powerpoint-2013-presenter-view-is-a-big-issue?forum=officeitpro) for good

## Windows 8 to Windows 10 upgrade and OEM partition

So you did a clean install instead of an [upgrade](http://windows.microsoft.com/en-us/windows-10/media-creation-tool-install). **Warning** The following is a rough outline, always have a backup and know what you type in..

1. Boot [Live CD](https://help.ubuntu.com/community/LiveCD#Reasons_for_Using_a_LiveCD_Session) and in [Gparted](https://apps.ubuntu.com/cat/applications/gparted/) toggle off the filesystem `hidden` [flag](http://www.linux.org/threads/gparted-partition-and-filesystem-flags.8112/) on a partition (usually) called `RESERVED`, it's (usually) the last one
2. Start Windows in a repair mode with command line, and `X:\> dism.exe Dism /apply-image /imagefile:G:\RecoveryImage\install.wim /index:1 /applydir:C:\`
3. Backup boot configuration data `X:\> bcdedit /export D:\BCD.BAK`
4. Remove the current entry `X:\> bcdedit /delete {current}`
5. Create boot files `X:\> bcdboot c:\windows /s c: /f ALL`
6. Create a boot entry `X:\> bootrec /rebuildbcd`
7. reboot

* [DISM Image Management Command-Line](https://technet.microsoft.com/en-us/library/hh825258.aspx)
* [Clean install without install disc?](http://www.eightforums.com/installation-setup/18494-clean-install-windows-8-without-install-disc-3.html)
* [Fix and Customize Boot for Windows](https://chaoliu12.wordpress.com/2013/01/31/fix-and-customize-boot-for-windows/)
* [bcdboot.exe](http://ss64.com/nt/bcdboot.html)
* [Windows Recovery Environment](https://technet.microsoft.com/en-us/library/hh825221.aspx)
* [Repairing the Windows Recovery Environment](http://www.terabyteunlimited.com/kb/article.php?id=587)
* [REAgentC Command-Line](https://technet.microsoft.com/en-us/library/hh825204.aspx)
* [Capture and Apply Windows, System, and Recovery Partitions](https://technet.microsoft.com/en-us/library/hh825041.aspx)
* [Windows Image File Boot](https://technet.microsoft.com/en-us/library/dn594399.aspx?f=255&MSPPError=-2147217396)


bits #3
=======

https://blogs.technet.microsoft.com/yongrhee/2009/09/11/lpmuihow-to-add-ime-keyboards-using-the-unattend-xml/

https://support.microsoft.com/en-us/help/2764405/how-to-automate-regional-and-language-settings-in-windows-vista--windo

http://rzander.azurewebsites.net/how-to-change-the-welcome-screen-language-in-win10/


## Windows: add keyboard layout on the fly

__First__ thought was to use [dism.exe](https://technet.microsoft.com/en-us/library/dd744360%28v=ws.10%29.aspx)
to update the Windows image online

    dism.exe /Online /Set-InputLocale:0407:00000407

or to "Package the German keyboard layout for subsequent delivery and installation"
with [The Microsoft Keyboard Layout Creator](https://msdn.microsoft.com/en-us/goglobal/bb964665).  
But these options are overkill and of course need you to be Admin to do that.

__Second__ thought was to add a registry key `HKCU\Keyboard Layout\Preload\3`
with a target [keyboard ID](https://technet.microsoft.com/en-us/library/dd744319%28WS.10%29.aspx).
But then the user has to re-login, or you would create `%LOGONSERVER%\NETLOGON\Scripts\logonscript.vbs`
to add the German keyboard layout for all (or some based on your AD Group Policy) at startup

    Set objShell = WScript.CreateObject("WScript.Shell")
    objShell.RegWrite "HKCU\Keyboard Layout\Preload\3", "00000407", "REG_SZ"

This sounds reasonable but server admins are usually not in favor of layering new stuff.

So the __third__ thought was to add a keyboard layout on a user request, a Desktop shortcut.
Windows XP had [Unattend.txt](https://support.microsoft.com/en-us/kb/289125) but since
[Windows Vista](https://technet.microsoft.com/en-us/library/cc721887%28WS.10%29.aspx)
we have [Multilingual User Interface](https://msdn.microsoft.com/en-us/goglobal/bb964650)
and [XML](http://superuser.com/a/356206)

    <gs:GlobalizationServices xmlns:gs="urn:longhornGlobalizationUnattend">
    <gs:UserList>
    <gs:User UserID="Current"/>
    </gs:UserList>
    <gs:InputPreferences>
    <!-- The German keyboard layout -->
    <gs:InputLanguageID Action="add" ID="0407:00000407" Default="true"/>
    </gs:InputPreferences> 
    </gs:GlobalizationServices>

Save it as `keyboard_DE.xml` and create a shortcut to `control intl.cpl,, /f:"keyboard_DE.xml"`.


## Windows display switch 

***

TODO https://getpocket.com/a/queue/list/display

***

setup jednotlivých grafických driverů je pro Windows 10 držen ve větvi
HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\GraphicsDrivers.

Tam je ale jen možné pro každý monitor definovat max/min rozlišení, otočení nebo scaling displeje.

Rozšíření nebo zrcadlení displeje drží hodnota Attach.ToDesktop ve větvi
HKEY_CURRENT_CONFIG/System/CurrentControlSet/Control/VIDEO/,
která ale ve Windows 10 dál neexistuje.

ŘEŠENÍM je tak od >=Windows 7 utilitka DisplaySwitch.exe!

Namísto z Registrů je lépe volat DisplaySwitch
až na závěr, tzn. jako startup položku.

Navíc je to potřeba volat jako zástupce
v kontextu uživatele,
tzn. spouštět DisplaySwitch.exe přímo,
ne jako (pod)proces z BAT souboru;
jinak pouze zobrazí nabídku pro přepnutí displeje,
avšak sám už ho nepřepne.

1. Make a shortcut to `DisplaySwitch.exe /clone`
2.
  ```
  @setlocal enableextensions
  @set cwd=%~dp0
  @copy "%cwd%DisplaySwitch.lnk" "%systemdrive%\ProgramData\Microsoft\Windows\Start Menu\Programs\Startup\."
  ```
3. Run as administrator
