# Hide Update from Windows Update

Hide KB updates by completing the following steps with elevated privileges on affected Windows nodes in PowerShell.

1. Install the PowerShell `PSWindowsUpdate` module by running `Install-Module PSWindowsUpdate`
2. Show the available updates by running `Get-WindowsUpdate`
3. To hide an update, run `Hide-WindowsUpdate -KBArticleID <KBNUMBER>` where KBNUMBER is the KB update you’d like to hide. For example, `Hide-WindowsUpdate -KBArticleID KB5007206`
4. To refresh the list of updates, reboot.