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
persistently; you may wish to specify the `r,w,x` bits or [effective](https://doc.opensuse.org/documentation/html/openSUSE_121/opensuse-security/cha.security.acls.html#sec.security.acls.handle.defacl.eff) IDs, i.e. `dmask` the new files

    /dev/sda1            /mnt/data            ntfs       uid=1000,gid=1000,dmask=027,fmask=137              0 2

## Inverted commas right

To change `"Buy Windows," said Steve.` into `„Kupujte Windows,“ řekl Steve.`,
write it actually like this `@Kupujte Windows,@@ řekl Steve.` and then substitute

    % perl -pi.bak -e 's/@@/“/g' file.txt
    % perl -pi -e 's/@/„/g' file.txt

## Windows 8/10 upgrade and OEM partition

So you did a clean install instead of an [upgrade](http://windows.microsoft.com/en-us/windows-10/media-creation-tool-install). **Warning** The following is a rough outline, always have backup and now what you type in!

1. Boot [Live CD](https://help.ubuntu.com/community/LiveCD#Reasons_for_Using_a_LiveCD_Session) and in [Gparted](https://apps.ubuntu.com/cat/applications/gparted/) toggle off the filesystem `hidden` [flag](http://www.linux.org/threads/gparted-partition-and-filesystem-flags.8112/) on a partition (usually) called `RESERVED`, it's (usually) the last one
2. Start Windows in a repair mode with command line, and `X:\> dism.exe Dism /apply-image /imagefile:G:\RecoveryImage\install.wim /index:1 /applydir:C:\`
3. Backup boot configuration data `X:\> bcdedit /export D:\BCD.BAK`
4. Remove the current entry `X:\> bcdedit /delete {current}`
5. Create boot files `X:\> bcdboot c:\windows /s c: /f ALL`
6. Create the boot entry `X:\> bootrec /rebuildbcd`
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


