---
external help file: PSITPro5_Management.xml
online version: http://go.microsoft.com/fwlink/p/?linkid=293907
schema: 2.0.0
---

# Restore-Computer
## SYNOPSIS
Starts a system restore on the local computer.

## SYNTAX

```
Restore-Computer [-RestorePoint] <Int32> [-Confirm] [-WhatIf]
```

## DESCRIPTION
The Restore-Computer cmdlet restores the local computer to the specified system restore point.

Restore-Computer restarts the computer.
The restore is completed during the restart operation.

System restore points and Restore-Computer are supported only on client operating systems, such as Windows 7, Windows Vista, and Windows XP.

## EXAMPLES

### Example 1: Restore the local computer
```
PS C:\>Restore-Computer -RestorePoint 253
```

This command restores the local computer to the restore point that has sequence number 253.

### Example 2: Restore the local computer with confirmation
```
PS C:\>Restore-Computer -RestorePoint 255 -Confirm
Confirm
Are you sure you want to perform this action? 
Performing operation "Restore-Computer" . 
[Y] Yes  [A] Yes to All  [N] No  [L] No to All  [S] Suspend  [?] Help (default is "Y"):
```

This command restores the local computer to the restore point that has sequence number 255.
It uses the Confirm parameter to prompt the user before actually performing the operation.

### Example 3: Restore a computer and check the status
```
PS C:\>Get-ComputerRestorePoint
PS C:\> Restore-Computer -RestorePoint 255
PS C:\> Get-ComputerRestorePoint -LastStatus
```

These commands run a system restore and then check its status.

The first command uses Get-ComputerRestorePoint to get the restore points on the local computer.

The second command restores the computer to the restore point with sequence number 255.

The third command uses the LastStatus parameter of Get-ComputerRestorePoint cmdlet to check the status of the restore operation.
Because Restore-Computer forces a restart, this command would be entered after the computer restarts.

## PARAMETERS

### -RestorePoint
Specifies the sequence number of the restore point. 
To find the sequence number, use the Get-ComputerRestorePoint cmdlet.
This parameter is required.

```yaml
Type: Int32
Parameter Sets: (All)
Aliases: SequenceNumber,SN,RP

Required: True
Position: 1
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -Confirm
Prompts you for confirmation before running the cmdlet.Prompts you for confirmation before running the cmdlet.

```yaml
Type: SwitchParameter
Parameter Sets: (All)
Aliases: 

Required: False
Position: Named
Default value: False
Accept pipeline input: False
Accept wildcard characters: False
```

### -WhatIf
Shows what would happen if the cmdlet runs.
The cmdlet is not run.Shows what would happen if the cmdlet runs.
The cmdlet is not run.

```yaml
Type: SwitchParameter
Parameter Sets: (All)
Aliases: 

Required: False
Position: Named
Default value: False
Accept pipeline input: False
Accept wildcard characters: False
```

## INPUTS

### None
You cannot pipe input to this cmdlet.

## OUTPUTS

### None
This cmdlet does not generate any output.

## NOTES
* To run a Restore-Computer command on Windows Vista and later versions of the Windows operating system, open Windows PowerShell by using the Run as administrator option.
* This cmdlet uses the Windows Management Instrumentation (WMI) SystemRestore class.

## RELATED LINKS

[Checkpoint-Computer](9ef7dd97-dbd9-43de-8988-9ab85e7827ad)

[Disable-ComputerRestore](06c5d9de-8a14-449c-b13b-c6793297e3fe)

[Enable-ComputerRestore](47fd013a-d03b-487d-8c7b-17e93f038d1f)

[Get-ComputerRestorePoint](3afe67e8-56bd-4505-b7f6-b822143a28d5)

[Restart-Computer](ba50f64c-866e-4315-91c7-0ce16b44c47e)
