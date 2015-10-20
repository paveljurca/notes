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


