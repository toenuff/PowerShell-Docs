---
external help file: PSITPro5_Utility.xml
online version: http://go.microsoft.com/fwlink/p/?linkid=293954
schema: 2.0.0
---

# Enable-PSBreakpoint
## SYNOPSIS
Enables the breakpoints in the current console.

## SYNTAX

### UNNAMED_PARAMETER_SET_1
```
Enable-PSBreakpoint [-Id] <Int32[]> [-PassThru] [-Confirm] [-WhatIf]
```

### UNNAMED_PARAMETER_SET_2
```
Enable-PSBreakpoint [-Breakpoint] <Breakpoint[]> [-PassThru] [-Confirm] [-WhatIf]
```

## DESCRIPTION
The Enable-PSBreakpoint cmdlet re-enables disabled breakpoints.
You can use it to enable all breakpoints, or you can specify breakpoints by submitting breakpoint objects or breakpoint IDs.

A breakpoint is a point in a script where execution stops temporarily so that you can examine the instructions in the script.
Newly created breakpoints are automatically enabled, but you can disable them by using the Disable-PSBreakpoint cmdlet.

Technically, this cmdlet changes the value of the Enabled property of a breakpoint object to True.

Enable-PSBreakpoint is one of several cmdlets designed for debugging Windows PowerShell scripts.
For more information about the Windows PowerShell debugger, see about_Debuggers.

## EXAMPLES

### Example 1: Enable all breakpoints
```
PS C:\>Get-PSBreakpoint | Enable-PSBreakpoint
```

This command enables all breakpoints in the current console.
You can abbreviate the command as gbp | ebp.

### Example 2: Enable breakpoints by ID
```
PS C:\>Enable-PSBreakpoint -Id 0, 1, 5
```

This command enables breakpoints with breakpoint IDs 0, 1, and 5.

### Example 3: Enable a disabled breakpoint
```
PS C:\>$B = Set-PSBreakpoint -Script "sample.ps1" -Variable Name
PS C:\> $B | Disable-PSBreakpoint -PassThru
AccessMode : Write
Variable   : Name
Action     : 
Enabled    : False
HitCount   : 0
Id         : 0
Script     : C:\ps-test\sample.ps1
ScriptName : C:\ps-test\sample.ps1 PS C:\>$B | Enable-PSBreakpoint -PassThru
AccessMode : Write
Variable   : Name
Action     : 
Enabled    : True
HitCount   : 0
Id         : 0
Script     : C:\ps-test\sample.ps1
ScriptName : C:\ps-test\sample.ps1
```

These commands re-enable a breakpoint that has been disabled.

The first command uses the Set-PSBreakpoint cmdlet to create a breakpoint on the Name variable in the Sample.ps1 script.
Then, it saves the breakpoint object in the $B variable.

The second command uses the Disable-PSBreakpoint cmdlet to disable the new breakpoint.
It uses a pipeline operator (|) to send the breakpoint object in $B to the Disable-PSBreakpoint cmdlet, and it uses the PassThru parameter of Disable-PSBreakpoint to display the disabled breakpoint object.
This lets you verify that the value of the Enabled property of the breakpoint object is False.

The third command uses the Enable-PSBreakpoint cmdlet to re-enable the breakpoint.
It uses a pipeline operator (|) to send the breakpoint object in $B to the Enable-PSBreakpoint cmdlet, and it uses the PassThru parameter of Enable-PSBreakpoint to display the breakpoint object.
This lets you verify that the value of the Enabled property of the breakpoint object is True.

The results are shown in the following sample output.

### Example 4: Enable breakpoints using a variable
```
PS C:\>$B = Get-PSBreakpoint -Id 3, 5
PS C:\> Enable-PSBreakpoint -Breakpoint $B
```

These commands enable a set of breakpoints by specifying their breakpoint objects.

The first command uses the Get-PSBreakpoint cmdlet to get the breakpoints and saves them in the $B variable.

The second command uses the Enable-PSBreakpoint cmdlet and its Breakpoint parameter to enable the breakpoints.

This command is the equivalent of Enable-PSBreakpoint -Id 3, 5.

## PARAMETERS

### -Breakpoint
Specifies the breakpoints to enable.
Enter a variable that contains breakpoint objects or a command that gets breakpoint objects, such as a Get-PSBreakpoint command.
You can also pipe breakpoint objects to Enable-PSBreakpoint.

```yaml
Type: Breakpoint[]
Parameter Sets: UNNAMED_PARAMETER_SET_2
Aliases: 

Required: True
Position: 1
Default value: None
Accept pipeline input: True (ByValue)
Accept wildcard characters: False
```

### -Id
Enables breakpoints that have the specified breakpoint IDs.
The default value is all breakpoints.
Enter the IDs or a variable that contains the IDs.
(You cannot pipe IDs to Enable-PSBreakpoint.) To find the ID of a breakpoint, use the Get-PSBreakpoint cmdlet.

```yaml
Type: Int32[]
Parameter Sets: UNNAMED_PARAMETER_SET_1
Aliases: 

Required: True
Position: 1
Default value: None
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: False
```

### -PassThru
Returns an object representing the enabled breakpoint.
By default, this cmdlet does not generate any output.

```yaml
Type: SwitchParameter
Parameter Sets: (All)
Aliases: 

Required: False
Position: Named
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

### System.Management.Automation.Breakpoint
You can pipe a breakpoint object to Enable-PSBreakpoint.

## OUTPUTS

### None or System.Management.Automation.Breakpoint
When you use the PassThru parameter, Enable-PSBreakpoint returns a breakpoint object that represent that breakpoint that was enabled.
Otherwise, this cmdlet does not generate any output.

## NOTES
* The Enable-PSBreakpoint cmdlet does not generate an error if you try to enable a breakpoint that is already enabled. As such, you can enable all breakpoints without error, even when only a few are disabled.

  Breakpoints are enabled when you create them by using the Set-PSBreakpoint cmdlet.
You do not need to enable newly created breakpoints.

*

## RELATED LINKS

[Disable-PSBreakpoint](d4974e9b-0aaa-4e20-b87f-f599a413e4e8)

[Get-PSBreakpoint](0bf48936-00ab-411c-b5e0-9b10a812a3c6)

[Get-PSCallStack](e91a17dc-db39-4c90-ae0e-6eeed3c0efef)

[Remove-PSBreakpoint](4c877a80-0ea0-4790-9281-88c08ef0ddd6)

[Set-PSBreakpoint](6afd5d2c-a285-4796-8607-3cbf49471420)

[about_Debuggers](2b2ce8b3-f881-4528-bd30-f453dea06755)
