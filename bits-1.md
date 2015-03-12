bits #1
=======

## Mp3 in a terminal

We're gonna use [Mplayer], so get
one by doing like
    % [[ which mplayer ]] && 
    % sudo zypper in mplayer 
    % sudo apt-get install in mplayer 
    % sudo rpm -i mplayer 

    % cat >> ~/.bashrc
    play() { mplayer "$1" -volume 68 -noconsolecontrols &>/dev/null; & }
    }
    playl() {
      { mplayer -volume 68 -noconsolecontrols -playlist "$1" &>/dev/null; } &
    }
    Ctrl^D
    % source ~/.bashrc


## Scanning via shortcut

IrfanView and hooked up scanner
fire up CMD by pressing [Win+R]
and typing *cmd*

    % cd %USERPROFILE%\Desktop 
    % copy con scan.bat
    @echo off
    cd %USERPROFILE%\Desktop
    start "" "C:\Program Files (x86)\IrfanView\i_view32.exe" /scanhidden /dpi=(300,300) /gray /convert=%1%.jpg
    Ctrl^D
    % rundll32.exe appwiz.cpl,NewLinkHere .

locate that BAT and add %RANDOM%

## ls when cd

make a shell function

    % d() { cd "$@" && ls -CF; }
    % d /tmp


(pj)

