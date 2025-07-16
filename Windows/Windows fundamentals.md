We can use [[RDP]] to login
`┌──(namura㉿namu)-[~]
└─$ xfreerdp3 /u:administrator /p:letmein123! /v:10.10.164.178 `

## File system
NTFS is known as a journaling file system. In case of a failure, the file system can automatically repair the folders/files on disk using information stored in a log file. This function is not possible with FAT.
Another feature of NTFS is **Alternate Data Streams** ( **ADS** ). Every file has at least one data stream ( `$DATA` ), and ADS allows files to contain more than one stream of data. From a security perspective, malware writers have used ADS to hide data.

## windows\system32 folder
Environment variables store information about the operating system environment. This information includes details such as the operating system path, the number of processors used by the operating system, and the location of temporary folders

## User accounts control (UAC)
`lusrmgr.msc` gives the local user manager window. 
https://learn.microsoft.com/en-us/windows/security/application-security/application-control/user-account-control/how-it-works

