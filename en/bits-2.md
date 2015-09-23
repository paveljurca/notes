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
persistently; you may wish to specify the `r,w,x` bits or [effective](https://doc.opensuse.org/documentation/html/openSUSE_121/opensuse-security/cha.security.acls.html#sec.security.acls.handle.defacl.eff) IDs, i.e. `dmask` dirs and `fmask` files

    /dev/sda1            /mnt/data            ntfs       uid=1000,gid=1000,dmask=027,fmask=137              0 2

## PowerPoint 2013 changes display setup

PowerPoint now breaks mirroring of displays because it's got [Enhanced Presenter View](http://www.indezine.com/products/powerpoint/learn/powerpoint-2013/enhanced-presenter-view-ppt2013.html) by default. So after a first slideshow ever it resets the display mode to extended, i.e. Microsoft tells you what you want [sic]. But there's a registry key `RestoreTopology` to tell PowerPoint to clean up after itself

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

p.s. [disable presenter view](https://social.technet.microsoft.com/Forums/office/en-US/a91dea3b-9c88-4d2b-9545-e48c3055a16c/powerpoint-2013-presenter-view-is-a-big-issue?forum=officeitpro) completely

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


