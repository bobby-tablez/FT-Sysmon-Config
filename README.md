# FT-sysmon-config: Yet another Sysmon config import file

This is a Microsoft Sysinternals Sysmon configuration file template whis has been forked from [SwiftOnSecurity](https://github.com/SwiftOnSecurity/sysmon-config)

As a threat researcher, I run a lot of malware for the sake of gathering telemetry and TTPs. When I'm unable to detect a TTP using the default SwiftOnSecurity config import file, a change to the config here and the change is documented below. This repo is a collection of changes based on the detection of current and active malware TTPs. 

It's important to note that some of the changes may increase logging on the host, when compared to the original sysmon config file that this was forked from. 

Used as the default Sysmon configuration file in [Enable-all-the-Logs!](https://github.com/bobby-tablez/Enable-All-The-Logs)

## Feel free to contribute!
If you've got any changes you'd like to make, please fork this repo, and submit a PR with your changes. 

## Changes:
Below are a list of changes made since forking
### EVENT ID 1: (PROCESS CREATE)
- Removed Conhost exclusion
### EVENT ID 7: (IMAGE LOAD)
- DLL loading from '\appdata\local'
- DLL loading from '\appdata\roaming'
- DLL loading from '\Downloads\'
- DLL loading from 'Non "C:\" drives'
### EVENT ID 11 (FILE CREATE)
- MS Office templates directory (Emotet)
- File events in MS Office Templates Directory (Emotet)
- File events in 'C:\ProgramData' directory (Common for malware stagers)
- Include 'C:\ProgramData'
- Include 'C:\Windows\Tasks'
- Include 'C:\Windows\Fonts'
- Include 'C:\Windows\Temp'
- Include '.lnk'
- Include '.url'
- Exclude '\WerFault.exe'
###  EVENT ID 12 & 13 & 14 (REGISTRY MODIFICATION)
- Include 'reg.exe'
- Include 'regedit.exe'
- Include 'wscript.exe'
- Include 'cscript.exe'
- Include 'powershell.exe', 'powershell_ise.exe','pwsh.exe' (flags changes from New-ItemProperty module)
- Include 'C:\Windows\Temp'
- Include 'C:\Windows\Fonts'
- Include 'C:\Users\Public'
- Include '\appdata\local\'

###  EVENT ID 17 & 18 (NAMED PIPES)
- Include '\msagent_'
- Include '\mojo.5688.8052.'
- Include '\ntsvcs'
- Include '\DserNamePipe'
- Include '\SearchTextHarvester'
- Include '\scerpc'
- Include '\mypipe-f'
- Include '\mypipe-h'
- Include '\windows.update.manager'
- Include '\win_svc'
- Exclude 'CompatTelRunner.exe'

## EVENT ID 26 (FILE DELETE DETECTED)
- Enabled
- Exclude 'CompatTelRunner.exe'
- Exclude 'WerFault.exe'


## Use ##
### Install ###
Run with administrator rights
~~~~
sysmon.exe -accepteula -i ft-sysmonconfig-export.xml
~~~~

### Update existing configuration ###
Run with administrator rights
~~~~
sysmon.exe -c ft-sysmonconfig-export.xml
~~~~

### Uninstall ###
Run with administrator rights
~~~~
sysmon.exe -u
~~~~

**[See other forks of this configuration](https://github.com/SwiftOnSecurity/sysmon-config/network)**

## Required actions ##

### Prerequisites ###
Highly recommend using [Notepad++](https://notepad-plus-plus.org/) to edit this configuration. It understands UNIX newline format and does XML syntax highlighting, which makes this very understandable. I do not recommend using the built-in Notepad.exe.

### Customization ###
You will need to install and observe the results of the configuration in your own environment before deploying it widely. For example, you will need to exclude actions of your antivirus, which will otherwise likely fill up your logs with useless information.

The configuration is highly commented and designed to be self-explanatory to assist you in this customization to your environment.

### Design notes ###
This configuration expects software to be installed system-wide and NOT in the C:\Users folder. Various pieces of software install themselves in User directories, which are subject to extra monitoring. Where possible, you should install the system-wide version of these pieces of software, like Chrome. See the configuration file for more instructions.
