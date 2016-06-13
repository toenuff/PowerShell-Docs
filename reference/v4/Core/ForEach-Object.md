---
external help file: PSITPro4_Core.xml
online version: http://go.microsoft.com/fwlink/p/?linkid=289582
schema: 2.0.0
---

# ForEach-Object
## SYNOPSIS
Performs an operation against each item in a collection of input objects.

## SYNTAX

### UNNAMED_PARAMETER_SET_1
```
ForEach-Object [-Process] <ScriptBlock[]> [-Begin <ScriptBlock>] [-End <ScriptBlock>] [-InputObject <PSObject>]
 [-RemainingScripts <ScriptBlock[]>] [-Confirm] [-WhatIf]
```

### UNNAMED_PARAMETER_SET_2
```
ForEach-Object [-MemberName] <String> [-ArgumentList <Object[]>] [-InputObject <PSObject>] [-Confirm] [-WhatIf]
```

## DESCRIPTION
The ForEach-Object cmdlet performs an operation on each item in a collection of input objects.
The input objects can be piped to the cmdlet or specified by using the InputObject parameter.

Beginning in Windows PowerShell 3.0, there are two different ways to construct a ForEach-Object command.

Script block.
You can use a script block to specify the operation.
Within the script block, use the $_ variable to represent the current object.
The script block is the value of the Process parameter.
The script block can contain any Windows PowerShell script.

For example, the following command gets the value of the ProcessName property of each process on the computer.

Get-Process | ForEach-Object {$_.ProcessName}

Operation statement.
You can also write a operation statement, which is much more like natural language.
You can use the operation statement to specify a property value or call a method.
Operation statements were introduced in Windows PowerShell 3.0.

For example, the following command also gets the value of the ProcessName property of each process on the computer.

Get-Process | ForEach-Object ProcessName

When using the script block format, in addition to using the script block that describes the operations that are performed on each input object, you can provide two additional script blocks.
The Begin script block, which is the value of the Begin parameter, runs before the first input object is processed.
The End script block, which is the value of the End parameter, runs after the last input object is processed.

## EXAMPLES

### -------------------------- EXAMPLE 1 --------------------------
```
PS C:\>30000, 56798, 12432 | ForEach-Object -Process {$_/1024}
29.296875
55.466796875
12.140625
```

This command takes an array of three integers and divides each one of them by 1024.

### -------------------------- EXAMPLE 2 --------------------------
```
PS C:\>Get-ChildItem $pshome | ForEach-Object -Process {if (!$_.PSIsContainer) {$_.Name; $_.Length / 1024; "" }}
```

This command gets the files and directories in the Windows PowerShell installation directory ($pshome) and passes them to the ForEach-Object cmdlet.
If the object is not a directory (the value of the PSISContainer property is false), the script block gets the name of the file, divides the value of its Length property by 1024, and adds a space ("") to separate it from the next entry.

### -------------------------- EXAMPLE 3 --------------------------
```
PS C:\>$Events = Get-EventLog -LogName System -Newest 1000
PS C:\>$events | ForEach-Object -Begin {Get-Date} -Process {Out-File -Filepath Events.txt -Append -InputObject $_.Message} -End {Get-Date}
```

This command gets the 1000 most recent events from the System event log and stores them in the $Events variable.
It then pipes the events to the ForEach-Object cmdlet.

The Begin parameter displays the current date and time.
Next, the Process parameter uses the Out-File cmdlet to create a text file named events.txt and stores the message property of each of the events in that file.
Last, the End parameter is used to display the date and time after all of the processing has completed.

### -------------------------- EXAMPLE 4 --------------------------
```
PS C:\>Get-ItemProperty -Path HKCU:\Network\* | ForEach-Object {Set-ItemProperty -Path $_.PSPath -Name RemotePath -Value $_.RemotePath.ToUpper();}
```

This command changes the value of the RemotePath registry entry in all of the subkeys under the HKCU:\Network key to uppercase text.
You can use this format to change the form or content of a registry entry value.

Each subkey in the Network key represents a mapped network drive that will reconnect at logon.
The RemotePath entry contains the UNC path of the connected drive.
For example, if you map the E: drive to \\\\Server\Share, there will be an E subkey of HKCU:\Network and the value of the RemotePath registry entry in the E subkey will be \\\\Server\Share.

The command uses the Get-ItemProperty cmdlet to get all of the subkeys of the Network key and the Set-ItemProperty cmdlet to change the value of the RemotePath registry entry in each key.
In the Set-ItemProperty command, the path is the value of the PSPath property of the registry key.
(This is a property of the Microsoft .NET Framework object that represents the registry key; it is not a registry entry.) The command uses the ToUpper() method of the RemotePath value, which is a string (REG_SZ).

Because Set-ItemProperty is changing the property of each key, the ForEach-Object cmdlet is required to access the property.

### -------------------------- EXAMPLE 5 --------------------------
```
PS C:\>1, 2, $null, 4 | ForEach-Object {"Hello"}
Hello
Hello
Hello
Hello
```

This example shows the effect of piping the $null automatic variable to the ForEach-Object cmdlet.

Because Windows PowerShell treats null as an explicit placeholder, the ForEach-Object cmdlet generates a value for $null, just as it does for other objects that you pipe to it.

For more information about the $null automatic variable, see about_Automatic_Variables.

### -------------------------- EXAMPLE 6 --------------------------
```
PS C:\>Get-Module -List | ForEach-Object -MemberName Path
PS C:\>Get-Module -List | Foreach Path
```

These commands gets the value of the Path property of all installed Windows PowerShell modules.
They use the MemberName parameter to specify the Path property of modules.

The second command is equivalent to the first.
It uses the Foreach alias of the Foreach-Object cmdlet and omits the name of the MemberName parameter, which is optional.

The ForEach-Object cmdlet is very useful for getting property values, because it gets the value without changing the type, unlike the Format cmdlets or the Select-Object cmdlet, which change the property value type.

### -------------------------- EXAMPLE 7 --------------------------
```
PS C:\>"Microsoft.PowerShell.Core", "Microsoft.PowerShell.Host" | ForEach-Object {$_.Split(".")}
PS C:\>"Microsoft.PowerShell.Core", "Microsoft.PowerShell.Host" | ForEach-Object -MemberName Split -ArgumentList "."
PS C:\>"Microsoft.PowerShell.Core", "Microsoft.PowerShell.Host" | Foreach Split "."
Microsoft
PowerShell
Core
Microsoft
PowerShell
Host
```

These commands split two dot-separated module names into their component names.
The commands call the Split method of strings.
The three commands use different syntax, but they are equivalent and interchangeable.

The first command uses the traditional syntax, which includes a script block and the current object operator ($_).
It uses the dot syntax to specify the method and parentheses to enclose the delimiter argument.

The second command uses the MemberName parameter to specify the Split method and the ArgumentName parameter to identify the dot (".") as the split delimiter.

The third command  uses the Foreach alias of the Foreach-Object cmdlet and omits the names of the MemberName and ArgumentList parameters, which are optional.

The output of these three commands, shown below, is identical.

Split is just one of many useful methods of strings.
To see all of the properties and methods of strings, pipe a string to the Get-Member cmdlet.

## PARAMETERS

### -Begin
Specifies a script block that runs before processing any input objects.

```yaml
Type: ScriptBlock
Parameter Sets: UNNAMED_PARAMETER_SET_1
Aliases: 

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -End
Specifies a script block that runs after processing all input objects.

```yaml
Type: ScriptBlock
Parameter Sets: UNNAMED_PARAMETER_SET_1
Aliases: 

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -InputObject
Specifies the input objects.
ForEach-Object runs the script block or operation statement on each input object.
Enter a variable that contains the objects, or type a command or expression that gets the objects.
When you use the InputObject parameter with ForEach-Object, instead of piping command results to ForEach-Object, the InputObject value-even if the value is a collection that is the result of a command, such as -InputObject (Get-Process)-is treated as a single object.
Because InputObject cannot return individual properties from an array or collection of objects, it is recommended that if you use ForEach-Object to perform operations on a collection of objects for those objects that have specific values in defined properties, you use ForEach-Object in the pipeline, as shown in the examples in this topic.

```yaml
Type: PSObject
Parameter Sets: (All)
Aliases: 

Required: False
Position: Named
Default value: None
Accept pipeline input: True (ByValue)
Accept wildcard characters: False
```

### -Process
Specifies the operation that is performed on each input object.
Enter a script block that describes the operation.

```yaml
Type: ScriptBlock[]
Parameter Sets: UNNAMED_PARAMETER_SET_1
Aliases: 

Required: True
Position: 1
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -RemainingScripts
Takes all script blocks that are not taken by the Process parameter.

This parameter is introduced in Windows PowerShell 3.0.

```yaml
Type: ScriptBlock[]
Parameter Sets: UNNAMED_PARAMETER_SET_1
Aliases: 

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -ArgumentList
Specifies the arguments to a method call.

This parameter is introduced in Windows PowerShell 3.0.

```yaml
Type: Object[]
Parameter Sets: UNNAMED_PARAMETER_SET_2
Aliases: Args

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -MemberName
Specifies the property to get or the method to call.
Wildcard characters are permitted, but work only if the resulting string resolves to a unique value.
If, for example, you run Get-Process | ForEach -MemberName *Name, and more than one member exists with a name that contains the string Name--such as the ProcessName and Name properties--the command fails.

This parameter is introduced in Windows PowerShell 3.0.

```yaml
Type: String
Parameter Sets: UNNAMED_PARAMETER_SET_2
Aliases: 

Required: True
Position: 1
Default value: None
Accept pipeline input: False
Accept wildcard characters: True
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

### System.Management.Automation.PSObject
You can pipe any object to ForEach-Object.

## OUTPUTS

### System.Management.Automation.PSObject
The objects that ForEach-Object returns are determined by the input.

## NOTES
The ForEach-Object cmdlet works much like the Foreach statement, except that you cannot pipe input to a Foreach statement.
For more information about the Foreach statement, see about_Foreach (http://go.microsoft.com/fwlink/?LinkID=113229).

## RELATED LINKS
