[![Build status](https://ci.appveyor.com/api/projects/status/g8n7j59gkdvk3efe/branch/master?svg=true)](https://ci.appveyor.com/project/PowerShell/xdisk/branch/master)

#xDisk has been deprecated and replaced by [xStorage](http://github.com/powershell/xstorage)

#xDisk

The** xDisk** module is a part of the Windows PowerShell Desired State Configuration (DSC) Resource Kit, which is a collection of DSC Resources.
This module contains the **xDisk and xWaitforDisk** resources.
These resources enable you to wait for a disk to become available and then initialize, format, and bring it online using PowerShell DSC.

**NOTE:** This resource follows a process to detect the existance of a RAW disk, initialize the disk, create a volume of maximum size, and then format the new volume.
Before beginning that operation, the disk is marked 'Online' and if it is set to 'Read-Only', that property is removed.
While this is intended to be non-destructive, as with all expiremental resources the scripts contained should be thoroughly evaluated and well understood before implementing in a production environment or where disk modifications could result in lost data.

**All of the resources in the DSC Resource Kit are provided AS IS, and are not supported through any Microsoft standard support program or service.
The "x" in xDisk stands for experimental**, which means that these resources will be **fix forward** and monitored by the module owner(s).

## Resources

**xDisk** resource has following properties:

*   **DiskNumber**: Specifies the identifier for which disk to modify.
*   **DriveLetter**: Specifies the preffered letter to assign to the disk volume.


**xWaitforDisk** resource has following properties:

*   **DiskNumber**: Specifies the identifer for which disk to wait for.
*   **RetryIntervalSec**: Specifies the number of secods to wait for the disk to become available.
*   **Count**: The number of times to loop the retry interval while waiting for the disk.


## Versions

### 1.0

*   Initial release with the following resources 
    *   xDisk
    *   xWaitforDisk

## Examples 

### Wait for disk 2 to become available, and then make the disk available as a new formatted volume.


```powershell
Configuration DataDisk
{
    
    Import-DSCResource -ModuleName xDisk
 
    Node localhost
    {
        xWaitforDisk Disk2
        {
             DiskNumber = 2
             RetryIntervalSec = 60
             Count = 60
        }
        xDisk GVolume
        {
             DiskNumber = 2
             DriveLetter = 'G'
        }
    }
}
 
DataDisk -outputpath C:\DataDisk
Start-DscConfiguration -Path C:\DataDisk -Wait -Force -Verbose
```


## Contributing
Please check out common DSC Resources [contributing guidelines](https://github.com/PowerShell/DscResource.Kit/blob/master/CONTRIBUTING.md).
