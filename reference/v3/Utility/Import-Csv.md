---
external help file: PSITPro3_Utility.xml
online version: http://go.microsoft.com/fwlink/?LinkID=113341
schema: 2.0.0
---

# Import-Csv
## SYNOPSIS
Creates table-like custom objects from the items in a CSV file.

## SYNTAX

### UNNAMED_PARAMETER_SET_1
```
Import-Csv [[-Path] <String[]>] [[-Delimiter] <Char>] [-Encoding <String>] [-Header <String[]>]
 [-LiteralPath <String[]>]
```

### UNNAMED_PARAMETER_SET_2
```
Import-Csv [[-Path] <String[]>] [-Encoding <String>] [-Header <String[]>] [-LiteralPath <String[]>]
 [-UseCulture]
```

## DESCRIPTION
The Import-Csv cmdlet creates table-like custom objects from the items in CSV files.
Each column in the CSV file becomes a property of the custom object and  the items in rows become the property values.
Import-Csv works on any CSV file, including files that are generated by the Export-Csv cmdlet.

You can use the parameters of the Import-Csv cmdlet to specify the column header row and the item delimiter, or direct Import-Csv to use the list separator for the current culture as the item delimiter.

You can also use the ConvertTo-Csv and ConvertFrom-Csv cmdlets to convert objects to CSV strings (and back).
These cmdlets are the same as the Export-CSV and Import-Csv cmdlets, except that they do not deal with files.

Beginning in Windows PowerShell 3.0, if a header row entry in a CSV file contains an empty or null value, Windows PowerShell inserts a default header row name and displays a warning message.
In previous versions of Windows PowerShell, if a header row entry in a CSV file contains an empty or null value, the Import-Csv command fails.

## EXAMPLES

### -------------------------- EXAMPLE 1 --------------------------
```
This example shows how to export and then import a CSV file of objects.The first command uses the Get-Process cmdlet to get the process on the local computer. It uses a pipeline operator (|) to send the process objects to the Export-CSV cmdlet, which exports the process objects to the Processes.csv file in the current directory.
PS C:\>get-process | export-csv processes.csv

The second command uses the Import-Csv cmdlet to import the processes in the Import-Csv file. Then it saves the resulting process objects in the $p variable.
PS C:\>$p = Import-Csv processes.csv

The third command uses a pipeline operator to pipe the imported objects to the Get-Member cmdlets. The result shows that they are CSV:System.Diagnostic.Process objects, not the System.Diagnostic.Process objects that Get-Process returns.Also, because there is no entry type in the formatting files for the CSV version of the process objects, these objects are not formatted in the same way that standard process objects are formatted.To display the objects, use the formatting cmdlets, such as Format-Table and Format-List, or pipe the objects to Out-GridView.
PS C:\>$p | get-member
TypeName: CSV:System.Diagnostics.Process
Name                       MemberType   Definition
----                       ----------   ----------
Equals                     Method       System.Boolean Equals(Object obj)
GetHashCode                Method       System.Int32 GetHashCode()
GetType                    Method       System.Type GetType()
ToString                   Method       System.String ToString()
BasePriority               NoteProperty System.String BasePriority=8
Company                    NoteProperty System.String Company=Microsoft Corporation
...
PS C:\>$p | out-gridview
```

This example shows how to export and then import a CSV file of objects.

### -------------------------- EXAMPLE 2 --------------------------
```
PS C:\>get-process | export-csv processes.csv -Delimiter :
PS C:\>$p = Import-Csv processes.csv -Delimiter :
```

This example shows how to use the Delimiter parameter of the Import-Csv cmdlet.
In this example, the processes are exported to a file that uses a colon (:) as a delimiter.

When importing, the Import-Csv file uses the Delimiter parameter to indicate the delimiter that is used in the file.

### -------------------------- EXAMPLE 3 --------------------------
```
PS C:\>$p = Import-Csv processes.csv -UseCulture
PS C:\>(get-culture).textinfo.listseparator
,
```

This example shows how to use the UseCulture parameter of the Import-Csv cmdlet.

The first command imports the objects in the Processes.csv file into the $p variable.
It uses the UseCulture parameter to direct Import-Csv to use the list separator defined for the current culture.

The second command displays the list separator for the current culture.
It uses the Get-Culture cmdlet to get the current culture.
It uses the dot (.) method to get the TextInfo property of the current culture and the ListSeparator property of the object in TextInfo.
In this example, the command returns a comma.

### -------------------------- EXAMPLE 4 --------------------------
```
The first command uses the Start-Job cmdlet to start a background job that runs a Get-Process command on the local computer. A pipeline operator (|) sends the resulting job object to the Export-CSV cmdlet, which converts the job object to CSV format. An assignment operator (=) saves the resulting CSV in the Jobs.csv file.
PS C:\>start-job -scriptblock { get-process } | export-csv jobs.csv

The second command saves a header in the $header variable. Unlike the default header, this header uses "MoreData" instead of "HasMoreData" and "State" instead of "JobStateInfo".
PS C:\>$header = "MoreData","StatusMessage","Location","Command","State","Finished","InstanceId","SessionId","Name","ChildJobs","Output","Error","Progress","Verbose","Debug","Warning","StateChanged"

The next three commands delete the original header (the second line) from the Jobs.csv file.
PS C:\># Delete header from file

PS C:\>$a = (get-content jobs.csv)
PS C:\>$a = $a[0], $a[2..($a.count - 1)]
PS C:\>$a > jobs.csv

The sixth command uses the Import-Csv cmdlet to import the Jobs.csv file and convert the CSV strings into a CSV version of the job object. The command uses the Header parameter to submit the alternate header. The results are stored in the $j variable.
PS C:\>$j = Import-Csv jobs.csv -header $header

The seventh command displays the object in the $j variable. The resulting object has "MoreData" and "State" properties, as shown in the command output.
PS C:\>$j

MoreData      : True
StatusMessage :
Location      : localhost
Command       : get-process
State         : Running
Finished      : System.Threading.ManualResetEvent
InstanceId    : 135bdd25-40d6-4a20-bd68-05282a59abd6
SessionId     : 1
Name          : Job1
ChildJobs     : System.Collections.Generic.List`1[System.Management.Automation.Job]
Output        : System.Management.Automation.PSDataCollection`1[System.Management.Automation.PSObject]
Error         : System.Management.Automation.PSDataCollection`1[System.Management.Automation.ErrorRecord]
Progress      : System.Management.Automation.PSDataCollection`1[System.Management.Automation.ProgressRecord]
Verbose       : System.Management.Automation.PSDataCollection`1[System.String]
Debug         : System.Management.Automation.PSDataCollection`1[System.String]
Warning       : System.Management.Automation.PSDataCollection`1[System.String]
StateChanged  :
```

This example shows how to use the Header parameter of Import-Csv to change the names of properties in the resulting imported object.

### -------------------------- EXAMPLE 5 --------------------------
```
The first command uses the Get-Content cmdlet to get the Links.csv file.
PS C:\>Get-Content .\Links.csv
113207,about_Aliases113208,about_Arithmetic_Operators113209,about_Arrays113210,about_Assignment_Operators113212, 
about_Automatic_Variables113213,about_Break113214,about_Command_Precedence113215,about_Command_Syntax144309, 
about_Comment_Based_Help113216,about_CommonParameters113217,about_Comparison_Operators113218,about_Continue113219, 
about_Core_Commands113220,about_Data_Section…

The second command uses the Import-Csv cmdlet to import the Links.csv file. The command uses the Header parameter to specify LinkId and TopicTitle as property names for the new custom objects. The command saves the imported objects in the $a variable.
PS C:\>$a = Import-Csv -Path .\Links.csv -Header LinkID, TopicTitle

The third command uses the Get-Member cmdlet to get the type and members of the custom objects in the $a variable.The output shows that Import-Csv returns a collection of custom objects (PSCustomObject). In addition to some default properties, the custom objects have LinkID and TopicTitle note properties.
PS C:\>$a | Get-Member
   TypeName: System.Management.Automation.PSCustomObject
Name                      MemberType   Definition
----                      ----------   ----------
Equals                    Method       bool 
Equals(System.Object obj) 
GetHashCode Method       int 
GetHashCode()GetType     Method       type 
GetType()ToString    Method       string 
ToString()LinkID      NoteProperty System.String 
LinkID=113207TopicTitle  NoteProperty System.String 
TopicTitle=about_Aliases

This command shows that you can use the custom object like you would any object in Windows PowerShell.The command pipes the custom objects in the $a variable to the Where-Object cmdlet, which gets only objects with a TopicTitle property that includes "alias".The Where-Object command uses the new simplified command format that does not require symbols, script blocks, or curly braces.
PS C:\>$a | Where-Object TopicTitle -like "*alias*"
LinkID            TopicTitle
------            ----------
113207            about_Aliases
113432            Alias Provider
113296            Export-Alias
113306            Get-Alias
113339            Import-Alias
113352            New-Alias
113390            Set-Alias
```

This example shows how to create a custom object in Windows PowerShell by using a CSV file.

### -------------------------- EXAMPLE 6 --------------------------
```
The first command uses the Get-Content cmdlet to get the Projects.csv file on the Server02 remote computer. The output shows that the header row of the file is missing a value between "ProjectName" and "Completed."
PS C:\>Get-Content "\\Server2\c$\Test\Projects.csv"
ProjectID, ProjectName,,Completed13, Inventory, Redmond, True440, , FarEast, True469, Marketing, Europe, False

The second command uses the Import-Csv cmdlet to import the Projects.csv file.The output shows that Import-Csv generates a warning and substitutes a default name, H1, for the missing header row value. H1 is also used for the name of the object property.
PS C:\>Import-Csv "\\Server2\c$\Test\Projects.csv"
PS C:\>WARNING: One or more headers were not specified. Default names starting with "H" have been used in place of any missing headers. 
ProjectID     ProjectName       H1               Completed
---------     -----------       --               ---------
13            Inventory         Redmond          True
440                             FarEast          True
469           Marketing         Europe           False

The third command uses the dot method to get the value of the H1 property of the object that Import-Csv creates.
PS C:\>(Import-Csv "\\Server2\c$\Test\Projects.csv").H1
RedmondFarEastEurope
```

This example shows how the Import-Csv cmdlet in Windows PowerShell 3.0 responds when the header row in a CSV file includes a null or empty value.
Import-Csv substitutes a default name for the header row.
The default name becomes the name of the property of the object that Import-Csv returns.

## PARAMETERS

### -Delimiter
Specifies the delimiter that separates the property values in the CSV file.
The default is a comma (,).
Enter a character, such as a colon (:).
To specify a semicolon (;), enclose it in quotation marks.

If you specify a character other than the actual string delimiter in the file, Import-Csv cannot create objects from the CSV strings.
Instead, it returns the strings.

```yaml
Type: Char
Parameter Sets: UNNAMED_PARAMETER_SET_1
Aliases: 

Required: False
Position: 2
Default value: "," (Comma)
Accept pipeline input: False
Accept wildcard characters: False
```

### -Encoding
Specifies the type of character encoding that was used in the CSV file.
Valid values are Unicode, UTF7, UTF8, ASCII, UTF32, BigEndianUnicode, Default, and OEM.
The default is ASCII.

This parameter is introduced in Windows PowerShell 3.0.

```yaml
Type: String
Parameter Sets: (All)
Aliases: 

Required: False
Position: Named
Default value: ASCII
Accept pipeline input: False
Accept wildcard characters: False
```

### -Header
Specifies an alternate column header row for the imported file.
The column header determines the names of the properties of the object that Import-Csv creates.

Enter a comma-separated list of the column headers.
Enclose each item in quotation marks (single or double).
Do not enclose the header string in quotation marks.
If you enter fewer column headers than there are columns, the remaining columns will have no header.
If you enter more headers than there are columns, the extra headers are ignored.

When using the Header parameter, delete the original header row from the CSV file.
Otherwise, Import-Csv creates an extra object from the items in the header row.

```yaml
Type: String[]
Parameter Sets: (All)
Aliases: 

Required: False
Position: Named
Default value: 
Accept pipeline input: False
Accept wildcard characters: False
```

### -Path
Specifies the path to the CSV file to import.
You can also pipe a path to Import-Csv.

```yaml
Type: String[]
Parameter Sets: (All)
Aliases: PSPath

Required: False
Position: 1
Default value: None
Accept pipeline input: True (ByValue)
Accept wildcard characters: False
```

### -UseCulture
Use the list separator for the current culture as the item delimiter.
The default is a comma (,).

To find the list separator for a culture, use the following command: (Get-Culture).TextInfo.ListSeparator.
If you specify a character other than the delimiter used in the CSV strings, ConvertFrom-CSV cannot create objects from the CSV strings.
Instead, it returns the strings.

```yaml
Type: SwitchParameter
Parameter Sets: UNNAMED_PARAMETER_SET_2
Aliases: 

Required: True
Position: Named
Default value: "," (Comma)
Accept pipeline input: False
Accept wildcard characters: False
```

### -LiteralPath
Specifies the path to the CSV file to import.
Unlike Path, the value of the LiteralPath parameter is used exactly as it is typed.
No characters are interpreted as wildcards.
If the path includes escape characters, enclose it in single quotation marks.
Single quotation marks tell Windows PowerShell not to interpret any characters as escape sequences.

```yaml
Type: String[]
Parameter Sets: (All)
Aliases: 

Required: False
Position: Named
Default value: 
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: False
```

## INPUTS

### System.String
You can pipe a string that contains a path to Import-Csv.

## OUTPUTS

### Object.
Import-Csv returns the objects described by the content in the CSV file.

## NOTES
Because the imported objects are CSV versions of the object type, they are not recognized and formatted by the Windows PowerShell type formatting entries that format the non-CSV versions of the object type.

The result of an Import-Csv command is a collection of strings that form a table-like custom object.
Each row is a separate string, so you can use the Count property of the object to count the table rows.
The columns are the properties of the object and items in the rows are the property values.

The column header row determines the number of columns and the column names.
The column names are also the names of the properties of the objects.
The first row is interpreted to be the column headers, unless you use the Header parameter to specify column headers.
If any row has more values than the header row, the additional values are ignored.

If the column header row is missing a value or contains a null or empty value, Import-Csv uses "H" followed by a number for the missing column header and property name.

In the CSV file, each object is represented by a comma-separated list of the property values of the object.
The property values are converted to strings (by using the ToString() method of the object), so they are generally represented by the name of the property value.
Export-CSV does not export the methods of the object.

## RELATED LINKS

[ConvertFrom-Csv](2a631339-07b3-49c1-8074-6028a220c78b)

[ConvertTo-Csv](02cf7085-f243-45ed-b803-da0466fd6085)

[Export-Csv](99523277-b798-4e42-b2a8-61da33f45a6d)
