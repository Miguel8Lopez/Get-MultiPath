pdate# Get-MultiPath. 
Check if ESXi multipath is homogeneous in your infrastructure

## SYNOPSIS

  Get HBA information and FC and iSCSI Multipath information from all ESXi hosts in -Server and -Location (optional)

![Image of Multipath Intro](/images/multipath_intro.JPG)


## DESCRIPTION

  The script return:
1. A table with the number of paths on each HBA
![Image of Multipath Table](/images/multipath_table.JPG)

2. .csv files with a list of paths, HBAs and SATP policy.
  1. Config.Stg.HBAs.csv
  2. Config.Stg.MultiPath.csv
  3. Config.Stg.SATP.csv


  The csv files could be imported to MS Excel and convert to Pivot Tables for further analisys. Here are some examples:
* Get HBA Vendor, Model, VID, DID, SVID, SDID, Driver, Firmware, Speed, status, etc on each ESXi host
![Image of Multipath HBAs](/images/multipath_hbas.JPG)

* Check if multipath configuration (SATP and PSP) is homogeneous on ESXi hosts:
![Image of Multipath HBAs](/images/multipath_paths.JPG)

![Image of Multipath HBAs](/images/multipath_paths2.JPG)


* Check path state (active, standby, dead)
![Image of Multipath HBAs](/images/multipath_status.JPG)


* Check IOPS Limit and IOPS Type in Round Robin paths
![Image of Multipath HBAs](/images/multipath_iops.JPG)


* Check if LUNs are presented to ESXi hosts with the same LUN ID
![Image of Multipath HBAs](/images/multipath_lunid.JPG)


* Get the sum of active/passive preferred/not preferred paths on each SAN Port and how they are distributed on SAN Targets
![Image of Multipath HBAs](/images/multipath_sanports.JPG)


* Check if ESXi hosts have the same default PSP Policy for each SATP
![Image of Multipath HBAs](/images/multipath_satp.JPG)


Just one change is made to ESXi hosts: start/stop TSM-SSH Service to get HBA Firmware.
-VCusername credential needs 'Host.Config.NetService' permission on vCenter Server to start and stop TSM-SSH Service
If -ESXiusername is not provided, the script won't try to start/stop TSM-SSH Service nor get HBA Firmware



## Data Analisys

When scritp execution is finished, three .csv files are written to c:\Temp (you can change this editing $cLibrary).

You can use the .xlsx attached to import these files. Simply change the three data connections with your 3 csv files.

Select "Connections" command from Data menu.
![Image of Multipath HBAs](/images/multipath_excel01.JPG)

Edit each connection to point to your .csv files.
![Image of Multipath HBAs](/images/multipath_excel02.JPG)

Complete the "Text Import Wizard"
![Image of Multipath HBAs](/images/multipath_excel03.JPG)![Image of Multipath HBAs](/images/multipath_excel04.JPG)![Image of Multipath HBAs](/images/multipath_excel05.JPG)


Every time you run the script, just open the .xlsx file, run “Refresh All” from Data menu and refresh the pivot tables.


## PARAMETERS

.PARAMETER Server
vCenter Server or ESXi host for which you want to retrieve the MultiPath information.



### PARAMETER Location
Datacenter or Folder where the ESXi hosts are located in the inventory.



### PARAMETER VCusername
Username to access vCenter Server Service.


Required privilege 'Host.Config.NetService' ("Host / Configuration / Security Profile and Password") to start and stop TSM-SSH service.

If TSM-SSH service is started in your ESXi hosts, 'Host.Config.NetService' privilege is not needed.


If you want to run this script as a Scheduled Task, you can use these functions to store credentials securely:

http://powershell.com/cs/blogs/tips/archive/2014/03/28/exporting-and-importing-credentials-in-powershell.aspx


### PARAMETER ESXiusername
Username with SSH access to ESXi. Either local or Domain username ("domain\ESX Admins" member)


If you want to run this script as a Scheduled Task, you can use these functions to store credentials securely

http://powershell.com/cs/blogs/tips/archive/2014/03/28/exporting-and-importing-credentials-in-powershell.aspx



### PARAMETER HBAonly
When $true, only HBA information is gathered.


## EXAMPLES

### EXAMPLE
Get the Multipath configuration from "Datacenter-A" ESXi hosts members on "MyvCenterServer.MyDomain.com"

Current user session is used to athenticate to vCenter Server Service.

HBA Firmware version is not gathered.

Get-MultiPath -Server MyvCenterServer.MyDomain.com -Location Datacenter-A

### EXAMPLE

Get the Multipath configuration from all registered ESXi hosts.

vCenter Server Service and ESXi credentials will be asked.

Get-MultiPath -Server MyvCenterServer.MyDomain.com -VCusername MyDomain\VCreadonly -ESXiusername MyDomain\ESXiAdmin

### EXAMPLE

Get the Multipath configuration from all registered ESXi hosts.

vCenter Server Service and ESXi credentials will be asked.


Get-MultiPath -Server MyvCenterServer.MyDomain.com -VCusername MyDomain\VCreadonly -ESXiusername MyDomain\ESXiAdmin -HBAonly:True


## NOTES

LinkedId: Miguel8Lopez


Thanks to:
* alanrenouf‌
* LucD‌
* http://www.virtu-al.net/
* http://www.lucd.info/
* http://www.powershell.com/
* http://www.vmdude.fr/
* http://www.powershelladmin.com/
