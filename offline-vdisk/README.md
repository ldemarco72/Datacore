<pre>
La documentazione specifica del comando (Set-DcsLogicalDiskAccess) si trova qui:
https://docs.datacore.com/SSV-WebHelp/dc_cmdlet_ref_gde_10psp7u2.htm#CommandDetails_IDRC0YOTLFOCWGVQTRJBXZRHHRI0KKTDB2FLBDHBQMXF4OGBHNYTJ

Sostanzialmente possiamo ricavare le informazioni necessarie sui vDisk in questo modo:

> Get-DcsVirtualDisk | select Alias,Size,ScsiDeviceIdString,Id,Type | ft -auto

In questo modo avremo il Datacore ID del vDisk che ci servirà per portarlo offline e il rispettivo ScsiDeviceID per identificarlo meglio.

Sample output:
Alias            Size   ScsiDeviceIdString               Id                                            Type
-----            ----   ------------------               --                                            ----
areteocr001      1 GB   60030D90B777E2017E26E641EE6107F6 5fc378cf1015485c951f45c575f12e5c MultiPathMirrored
aretevoting001   1 GB   60030D906645E501F4F567B876958DF4 aef3f984a68742e788b7195cf8928e78 MultiPathMirrored
areteredo001_1   25 GB  60030D90F28CF701F16D09836FD69589 66c24c4178cd47a78914146b5f38e3a7       NonMirrored

Per portare offline il vDisk "aretevoting001":

> Set-DcsLogicalDiskAccess -LogicalDisk 66c24c4178cd47a78914146b5f38e3a7 -AccessEnabled false


L'azione è immediata e si può controllare lo stato in questo modo:

> Get-DcsLogicalDisk -virtualDisk 66c24c4178cd47a78914146b5f38e3a7

Sample output:
DeletePending                : False
StreamDiskId                 :
RetentionTime                : 0
StreamSize                   : 0 B
StreamState                  : NotPresent
StreamDiskDataProtectionRole : None
SequentialStorage            : False
PoolId                       : C46FF449-3003-466C-A356-346241565C3C:{4c5d469b-1f97-11e7-80c3-1866daf59ef1}
VolumeIndex                  : 6
MinQuota                     : 0 B
MaxQuota                     : 0 B
TierAffinity                 : {1, 2, 3}
StorageName                  : sail12credo002_2
ReclamationState             : Idle
InReclamation                : False
DataStatus                   : UpToDate
PresenceStatus               : Present
Size                         : 25 GB
SectorSize                   : 512 B
MappingName                  : V.{4c5d469b-1f97-11e7-80c3-1866daf59ef1}-00000006
DiskStatus                   : Online
Virtualized                  : True
ClientAccessRights           : Offline
MirrorAccessDisabled         : True
Failure                      : Healthy
VirtualDiskId                : 243f1755df374fd9ab26d98da7c6d9c0
DiskRole                     : First
ServerHostId                 : C46FF449-3003-466C-A356-346241565C3C
IsMapped                     : True
Protected                    : True
Replacing                    : False
TrunkVirtualDiskId           :
SequenceNumber               : 137834
Id                           : d31f765c-6e3d-463e-8333-053e4c1ecb29
Caption                      : sail12credo002_2 on ccs-sdssrv102
ExtendedCaption              : sail12credo002_2 on ccs-sdssrv102
Internal                     : False

La voce interessante è "ClientAccessRights" che normalmente sarebbe in modalità "ReadWrite".

</pre>