---
external help file: PSITPro5_Core.xml
online version: http://go.microsoft.com/fwlink/p/?linkid=289607
schema: 2.0.0
---

# Remove-Module
## SYNOPSIS
Removes modules from the current session.

## SYNTAX

### UNNAMED_PARAMETER_SET_1
```
Remove-Module [-ModuleInfo] <PSModuleInfo[]> [-Force] [-Confirm] [-WhatIf]
```

### UNNAMED_PARAMETER_SET_2
```
Remove-Module [-Name] <String[]> [-Force] [-Confirm] [-WhatIf]
```

### UNNAMED_PARAMETER_SET_3
```
Remove-Module [-FullyQualifiedName] <ModuleSpecification[]> [-Confirm] [-WhatIf]
```

## DESCRIPTION
The Remove-Module cmdlet removes the members of a module, such as cmdlets and functions, from the current session.

If the module includes an assembly (.dll), all members that are implemented by the assembly are removed, but the assembly is not unloaded.

This cmdlet does not uninstall the module or delete it from the computer.
It affects only the current Windows PowerShell session.

## EXAMPLES

### Example 1: Remove a module
```
PS C:\>Remove-Module -Name "BitsTransfer"
```

This command removes the BitsTransfer module from the current session.

### Example 2: Remove all modules
```
PS C:\>Get-Module | Remove-Module
```

This command removes all modules from the current session.

### Example 3: Remove modules by using the pipeline
```
PS C:\>"FileTransfer", "PSDiagnostics" | Remove-Module -Verbose
VERBOSE: Performing operation "Remove-Module" on Target "filetransfer (Path: 'C:\Windows\system32\WindowsPowerShell\v1.0\Modules\filetransfer\filetransfer.psd1')". 
VERBOSE: Performing operation "Remove-Module" on Target "Microsoft.BackgroundIntelligentTransfer.Management (Path: 'C:\Windows\assembly\GAC_MSIL\Microsoft.BackgroundIntelligentTransfer.Management\1.0.0.0__31bf3856ad364e35\Microsoft.BackgroundIntelligentTransfe
r.Management.dll')".
VERBOSE: Performing operation "Remove-Module" on Target "psdiagnostics (Path: 'C:\Windows\system32\WindowsPowerShell\v1.0\Modules\psdiagnostics\psdiagnostics.psd1')". 
VERBOSE: Removing imported function 'Start-Trace'. 
VERBOSE: Removing imported function 'Stop-Trace'. 
VERBOSE: Removing imported function 'Enable-WSManTrace'. 
VERBOSE: Removing imported function 'Disable-WSManTrace'. 
VERBOSE: Removing imported function 'Enable-PSWSManCombinedTrace'. 
VERBOSE: Removing imported function 'Disable-PSWSManCombinedTrace'. 
VERBOSE: Removing imported function 'Set-LogProperties'. 
VERBOSE: Removing imported function 'Get-LogProperties'. 
VERBOSE: Removing imported function 'Enable-PSTrace'. 
VERBOSE: Removing imported function 'Disable-PSTrace'. 
VERBOSE: Performing operation "Remove-Module" on Target "PSDiagnostics (Path: 'C:\Windows\system32\WindowsPowerShell\v1.0\Modules\psdiagnostics\PSDiagnostics.psm1')".
```

This command removes the BitsTransfer and PSDiagnostics modules from the current session.

The command uses a pipeline operator (|) to send the module names to Remove-Module.
It uses the Verbose common parameter to get detailed information about the members that are removed.

The Verbose messages show the items that are removed.
The messages differ because the BitsTransfer module includes an assembly that implements its cmdlets and a nested module with its own assembly.
The PSDiagnostics module includes a module script file (.psm1) that exports functions.

### Example 4: Remove a module by using ModuleInfo
```
PS C:\>$a = Get-Module BitsTransfer
PS C:\> Remove-Module -ModuleInfo $a
```

This command uses the ModuleInfo parameter to remove the BitsTransfer module.

## PARAMETERS

### -Force
Indicates that this cmdlet removes read-only modules.
By default, Remove-Module removes only read-write modules.

The ReadOnly and ReadWrite values are stored in AccessMode property of a module.

```yaml
Type: SwitchParameter
Parameter Sets: UNNAMED_PARAMETER_SET_1, UNNAMED_PARAMETER_SET_2
Aliases: 

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -FullyQualifiedName
Specifies the fully qualified names of modules to remove.

```yaml
Type: ModuleSpecification[]
Parameter Sets: UNNAMED_PARAMETER_SET_3
Aliases: 

Required: True
Position: 1
Default value: None
Accept pipeline input: True(ByValue)
Accept wildcard characters: False
```

### -ModuleInfo
Specifies the module objects to remove.
Enter a variable that contains a module object (PSModuleInfo) or a command that gets a module object, such as a Get-Module command.
You can also pipe module objects to Remove-Module.

```yaml
Type: PSModuleInfo[]
Parameter Sets: UNNAMED_PARAMETER_SET_1
Aliases: 

Required: True
Position: 1
Default value: None
Accept pipeline input: True (ByValue)
Accept wildcard characters: False
```

### -Name
Specifies the names of modules to remove.
Wildcard characters are permitted.
You can also pipe name strings to Remove-Module.

```yaml
Type: String[]
Parameter Sets: UNNAMED_PARAMETER_SET_2
Aliases: 

Required: True
Position: 1
Default value: None
Accept pipeline input: True (ByValue)
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

### System.String, System.Management.Automation.PSModuleInfo
You can pipe module names and module objects to Remove-Module.

## OUTPUTS

### None
This cmdlet does not generate any output.

## NOTES

## RELATED LINKS

[Get-Module](2cccd4c4-9a21-4c77-b691-984ee57242e1)

[Import-Module](af616c24-e122-4098-930e-1e3ea2080ade)

[about_Modules](3be86334-7efa-4ccd-952e-54afe47977a2)
