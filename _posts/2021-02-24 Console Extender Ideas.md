---
layout: post
title: Ivanti Endpoint Manager + Console Extender Ideas
subtitle: Extend the Context-Menu of EPM with needed features
cover-img: /assets/posts/210111_1/cover-red-plain.png
share-img: /assets/posts/210111_1/61e76ffa78954c85a0973580da07b9fd.png
thumbnail-img:  /assets/posts/210111_1/61e76ffa78954c85a0973580da07b9fd.png
tags: [Console, EPM]
published: true
---
2021-02-24 Console Extender Ideas
To be able to run the commands from any console I added all Scripts and binaries to a share on the core-server.

# Prepare the share

- C:\Program Files\LANDesk\ManagementSuite\ldlogon
- ..\MyFiles\bin
- ..\MyFiles\scripts

I used one for binary-files and one for scripts
![1c2926a56ef57aaa4292d97bf92507b1.png](/assets/posts/210224_1//922cd741ce734565810d5fd057497255.png)

## C$ as Admin
I work (like I should) without local admin-rights for my and other computers, but sometimes I need to troubleshoot some stuff on the computers of our users. So I looked for a way to run a Connect to C$ with admin-credentials.

I found [FreeCommanderXE](https://freecommander.com/de/downloads-2/) and runas.exe as good working solution.

#### Prepare Freecomander
Put a copy of the FreeCommanderXE-Program folder in the bin-directory
![fb1cf7e866585a90853bb966b2a92f67.png](/assets/posts/210224_1//76b37f16961644818ce1c22935b233e3.png)

#### Prepare runas.exe
And create a batch file for **runas.exe** 
![1a81ab6168741ef3bc8d336d0056020f.png](/assets/posts/210224_1//d981f2c7dc4b4648b5ae130ce5636fca.png)

![524ee40a974877fad0ce075ef4a9779a.png](/assets/posts/210224_1//5a31b50bd0844979b373687dfa8e7161.png)

```batch
::"\\MyCORE.local\ldlogon\MyFiles\Scripts\runas_user_bin.cmd"
SET BIN=%1
SET USERN=%2
echo Starting %BIN% as %USERN%

runas /user:%USERN% /savecred %BIN%
pause
```

#### Prepare the EPM Console
In Endpoint Manager -> Console Extender: point to the runas-script
![069546a2395aff9fe33fa89fe265b3e9.png](/assets/posts/210224_1//33935ce2d73a4ec38639c461ada16d3c.png)
```
Executable:
\\\MyCore.local\ldlogon\MyFiles\scripts\runas_user_bin.cmd 

Command Line:
\\MyCore.local\ldlogon\MyFiles\bin\FreeCommanderXE\FreeCommander.exe /R=\\<Computer.Device Name>.<Computer.Domain Name>\C$" domain\administrator
```

#### Use it
Select a device and Start it via:
![ee7900f5a4a0c95daf2ea70f4c708274.png](/assets/posts/210224_1//040e5106ee024eb99427c8b0aec2d943.png)


## CMD as Admin$
Sometimes it is helpfull for me to get directly on to the commandline of a users-device. So I utilize my admin-credentials and [psexec.exe](https://docs.microsoft.com/en-us/sysinternals/downloads/psexec) for the task.

#### Prepare the EPM Console
In Endpoint Manager -> Console Extender: point to the runas-script

```
Executable:
\\MyCore.local\ldlogon\MyFiles\scripts\runas_user_bin.cmd 

Command Line:
"cmd /c \\MyCore.local\ldlogon\MyFiles\bin\psexec.exe -s \\<Computer.Device Name>.<Computer.Domain Name> cmd" domain\admin-user
```
![dc9928c298a6ddcb87fb7375c38e2392.png](/assets/posts/210224_1//48fcfb3af9a34bad9bf08e41c00b57e5.png)


