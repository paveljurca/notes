bits #0
=======

## Dropbox as a backup

The service is free up to 2GB so given
that my most important stuff is just
text files, I'm more than backed.

So you update a file, it updates on Dropbox.
You (accidentally) delete a file, it *stays* on Dropbox!
This is what I would call *the* bonus ;)

First install [Dropbox](https://www.dropbox.com/install?os=lnx),
second make a [Cron file](http://tldp.org/LDP/lame/LAME/linux-admin-made-easy/using-cron.html).

    % su -
    % pushd /etc/cron.daily/
    % cat > bckup
    #!/bin/sh
    cp --archive --update /home/pavel/txt/ /home/pavel/Dropbox
    cp --archive --update /home/pavel/code/ /home/pavel/Dropbox
    Ctrl^D
    % chown pavel:pavel bckup 
    % chmod +x bckup

## Sleep mode from CLI

Yes, [pm-utils](http://www.google.com/search?q=pm+utils) handle that.
    
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
you have to swap the boot flag (via [fdisk](http://www.debian.org/releases/slink/sparc/fdisk.txt)
[*command* **a**]) between your partitions
but [Grub2](http://www.gnu.org/software/grub/) has
no clue about your Windows there.

You should create the entry by opening `/etc/grub.d/40_custom`
and write something like

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
to change tabs for 4 spaces, use `M` to comment out lines
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
    command -range=% M :<line1>,<line2>s/^/#/
    command -range=% MM :<line1>,<line2>s/^#//
    map ,m :s/^/#/g<CR>
    map ,mm :s/^#//g<CR>
    filetype indent on
    syntax on
    colorscheme peachpuff
    highlight LineNr ctermfg=238
    Ctrl^D

![Vim](d/vim.png)
(gnome-terminal)

* [Vim for Perl devs](http://mamchenkov.net/wordpress/2004/05/10/vim-for-perl-developers/)
* [vi reference page](http://www.kichwa.com/quik_ref/vi_ref.html)
* [Learn Vim Progressively](http://yannesposito.com/Scratch/en/blog/Learn-Vim-Progressively/)
* [vi-improved](http://vi-improved.org/)

## Sublime setup

First get [Sublime](http://www.sublimetext.com/3),
second get [package manager](http://sublime.wbond.net/installation). 

Then via the package manager install [SublimeLinter3](http://github.com/SublimeLinter/SublimeLinter3)
and [SublimeCodeIntel](http://github.com/SublimeCodeIntel/SublimeCodeIntel).
If doing [Perl](http://www.perl.org) you may try [SublimeLinter-perl](http://github.com/oschwald/SublimeLinter-perl)
or even [ModernPerl](https://github.com/Blaizer/ModernPerl-sublime) highlighter.

Additionaly get a nice [theme](http://colorsublime.com/)
or a [font](http://font.ubuntu.com/) (which extract
somewhere at `/usr/local/share/fonts/truetype/Ubuntu`).

Finally set preferences..

    % pushd ~/.config/sublime-text-3/Packages/User/
    % cp Preferences.sublime-settings Preferences.sublime-settings.bak
    % cat > Preferences.sublime-settings
    {
    "auto_complete": false,
    "bold_folder_labels": true,
    "caret_style": "phase",
    "close_windows_when_empty": false,
    "color_scheme": "Packages/User/Darkside.tmTheme",
    "default_line_ending": "unix",
    "find_selected_text": true,
    "fold_buttons": false,
    "font_face": "Ubuntu Mono",
    "font_size": 14.0,
    "highlight_line": true,
    "highlight_modified_tabs": true,
    "hot_exit": false,
    "ignored_packages":
    [
        "Vintage"
    ],
    "line_numbers": true,
    "line_padding_bottom": 0,
    "line_padding_top": 0,
    "rulers":
    [
        65
    ],
    "show_encoding": true,
    "show_full_path": true,
    "show_line_endings": true,
    "tab_size": 4,
    "theme": "Default.sublime-theme",
    "translate_tabs_to_spaces": true,
    "trim_trailing_white_space_on_save": true,
    "word_wrap": "true",
    "wrap_width": 65
    }
    Ctrl^D

And in theme file `$HOME/.config/sublime-text-3/Packages/User/Darkside.tmTheme`
you may colorize variables by locating the `Variable` section

    <string>Variable</string>
    <key>scope</key>
    <string>variable, punctuation.definition.var.perl</string>
    <key>settings</key>
    <dict>
       <key>fontStyle</key>
       <string></string>
       <key>foreground</key>
       <string>#eab700</string>
    </dict>

![Sublime](d/sublime.png)

To determine the `scope` just highlight
a variable/keyword/function/whatever and
press `Ctr+Alt+Shift+P`, it shows up in a status bar.


