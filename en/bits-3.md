bits #3
=======

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


