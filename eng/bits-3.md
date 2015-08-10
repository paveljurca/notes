bits #3
=======

*nix quick ref

## system

    uname -a                                      # OS info
    cat /etc/*release                             # OS info
    lsb_release -a                                # OS info
    dmesg                                         # /var/log/messages
    chkconfig                                     # check daemons
    service --status-all                          # list services
    uptime                                        # machine uptime
    ldd /bin/bash                                 # library depend
    lsmod                                         # loaded modules
    cat /proc/cpuinfo                             # cpu info
    free -m                                       # memory info
    pkill                                         # regex kill by name
    pgrep                                         # regex grep PID by name
    ps aux                                        # cur processes
    pstree -p                                     # process tree with PIDs

## file system

    ls -tr                                        # list by time
    ls *.txt|wc -l|xargs echo "txt "              # execute args
    find ./ -type d -exec chmod 775 {}\;          # change dir permissions
    rm -Rf ./.??*                                 # remove * and hidden ones
    chattr +i file                                # immutable on the file
    df -h                                         # disk usage
    du -h                                         # file usage
    stat                                          # file system info
    lsof                                          # list open files
    fuser -m /mnt/data                            # processes using the mount point

## network

    ip a sh                                       # list Ifaces
    ifconfig eth0 inet 192.168.0.1/24 up          # bring eth0 up
    iptables -t nat -L                            # NAT table
    curl ifconfig.me                              # show the WAN IP
    route add default gw 10.0.0.138               # gateway
    mtr 89.24.181.250                             # network diag
    bing 10.0.0.34 10.0.0.138                     # bandwidth tester
    nmap -v -A google.com                         # network explore
    nmap -v -sn 10.0.0.0/24                       # subnet scan
    iwlist wlan0 scanning                         # APs scan
    tcpdump -i wlan0                              # packets on given interface
    tcpdump port 80                               # sniff port 80
    lsof -nPi tcp:80                              # processes using TCP port 80
    netstat -anp --inet                           # which port which process
    arp                                           # MAC <-> IP address mapping
    dig                                           # DNS lookup

## run at

    at -f backup.sh -v 10:00                      # run once
    atq                                           # list at queue
    atrm                                          # remove at
    echo '0,20,40 * * * * yes'|crontab            # run every 20 minutes

## var

    $0                                            # shell or script name
    "$PWD"                                        # cur dir
    $SECONDS                                      # sec from uptime
    $MACHTYPE                                     # architecture
    $PIPESTATUS                                   # $? return code
    $PPID                                         # parent PID
    $HISTCMD                                      # size of $HISTFILE
    (IFS=":x:";echo $( </etc/passwd))             # field separator

## misc

    tput sgr 0 1                                  # underline console
    tput bold                                     # bold console
    tput sgr0                                     # reset to defaults
    echo $( </etc/passwd)                         # content as a single line
    ls /s{bin,elinux}                             # bash expansion
    $(date +%A)                                   # day in a week
    man hier                                      # file system hierarchy
    cat logs/access_log|uniq                      # unique lines
    kill -9 `ps -dN|grep tty2|cut -d\  -f1`       # force log off on tty2
    rbash                                         # restricted shell
    chroot                                        # change root dir
    echo stressed|rev                             # reverse
    exiftool song.mp3                             # show meta data
    shopt                                         # shell options
    watch -n1 "date '+%D%n%T'|figlet -k"          # fancy timer
    setterm -blength 0                            # turn off the bell
    stty -echo                                    # disable console output
    stty echo                                     # enable console output


