bits #2
=======

## Bell in a shell

I'm going crazy with a system speaker and the shell's ability to ring the bell
every time I've pressed one too many `BACKSPACE` or so

    % echo 'set bell-style none' >> $HOME/.inputrc

* [Readline](http://ss64.com/bash/syntax-inputrc.html)

## /sbin/route

Being connected to [RJ-45](https://en.wikipedia.org/wiki/Ethernet) and [WiFi](https://en.wikipedia.org/wiki/Hotspot_%28Wi-Fi%29)
at once may cause the routing table to "blow out" (two DHCP servers and two gateways).

So look up a "primary" `Iface` and mark its gateway [default](https://en.wikipedia.org/wiki/Default_gateway)

/* @todo screenshot */

    % sudo route  # print the table
    % sudo route add default gw 10.0.0.138

## fstab

/etc/[fstab](http://www.linfo.org/etc_fstab.html) lets you mount a given partition
persistently; you may wish to specify the `r,w,x` bits or [effective](https://doc.opensuse.org/documentation/html/openSUSE_121/opensuse-security/cha.security.acls.html#sec.security.acls.handle.defacl.eff) IDs,
i.e. `dmask` the new files

    /dev/sda1            /mnt/data            ntfs       uid=1000,gid=1000,dmask=027,fmask=137              0 2

## Inverted commas right

To change `"Buy Windows," said Bill.` into `„Kupujte Windows,“ řekl Bill.`,
write it actually like this `@Kupujte Windows,@@ řekl Bill.` and then run

    % perl -pi.bak -e 's/@@/“/g' file.txt
    % perl -pi -e 's/@/„/g' file.txt

But sure you know better ways :-)


