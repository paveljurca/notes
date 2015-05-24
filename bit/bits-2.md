bits #2
=======

## Bell in the shell

I'm going crazy with my system speaker
and a shell's ability to ring the bell
every time I've pressed one too many `BACKSPACE`*s* or so

    % echo 'set bell-style none' >> $HOME/.inputrc

* [Readline](http://ss64.com/bash/syntax-inputrc.html)

## /sbin/route

Being connected to LAN and WiFi simultaneously
may cause one's packets don't find
the way out (because another DHCP server
confused one's route table). Just look up a proper
one `Iface` and its subnet, then add the gateway

    % sudo route
    % sudo route add default gw 10.0.0.138

## fstab

/etc/[fstab](http://www.linfo.org/etc_fstab.html)
lets you mount a given partition persistently;
you may wish to specify the `r,w,x` bits or effective IDs,
i.e. `dmask` new files created

    /dev/sda1            /mnt/data            ntfs       uid=1000,gid=1000,dmask=027,fmask=137              0 2

## listing of *nix programms

I've found useful for maintainance..

