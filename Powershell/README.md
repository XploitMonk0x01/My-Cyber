# PowerShell Fundamentals Guide

## Overview

PowerShell is a powerful **task automation** and **configuration management framework** from Microsoft. It is built on **.NET Core** and focuses on cross-platform compatibility, making it available on Windows, Linux, and macOS.

---

## Table of Contents

- [Cmdlets](#cmdlets)
- [Variables](#variables)
- [Data Types](#data-types)
- [Comparison Operators](#comparison-operators)
- [Arrays](#arrays)
- [Loops](#loops)
- [Hashtables](#hashtables)
- [Custom Objects](#custom-objects)
- [Pipeline](#pipeline)
- [Conditional Statements](#conditional-statements)
- [Error Handling](#error-handling)

---

## Cmdlets

**Cmdlets** (pronounced "command-lets") are the commands used in PowerShell. They follow a consistent **Verb-Noun** naming convention.

### Common Verbs:

- `Get` - Retrieve data
- `Set` - Modify data
- `Add` - Add data
- `Remove` - Delete data

### Common Nouns:

- `Process` - Running processes
- `Service` - System services
- `Item` - Files and folders
- `ChildItem` - Child items in a location

### Essential Commands:

```powershell
# Check PowerShell version
$psversiontable.psversion

# Get current date and time
Get-Date

# List all services
Get-Service

# List all available commands
Get-Command

# Find commands by noun
Get-Command -Noun Service

# Find commands by verb
Get-Command -Verb Install

# Get detailed help for a command
Get-Help Install-Package -Full
```

### Aliases

PowerShell supports **aliases** - shorter names for common cmdlets:

- `gsv` is an alias for `Get-Service`
- `ls` or `dir` for `Get-ChildItem`
- `cd` for `Set-Location`

### Special Variable: `$_`

`$_` represents the **current object in the pipeline**. It's commonly used in loops and pipeline operations to refer to each item being processed.

---

## Variables

Variables in PowerShell start with a `$` symbol. Common naming conventions include:

- **camelCase** - `$myVariable`
- **PascalCase** - `$MyVariable`
- **snake_case** - `$my_variable`

### String Variables:

```powershell
# String with double quotes
$MyVariable = "Automate with Modi"
$MyVariable

# String with single quotes
$MyVariable = '06'
$MyVariable
```

### Numeric Variables:

```powershell
# Numeric value (no quotes)
$MyVariable = 06

# Working with numbers
$MyVar1 = 06
$MyVar2 = 05
$MyVar1 + $MyVar2  # Output: 11
```

### String Methods:

```powershell
$MyVariable = '06'
$MyVariable.Length    # Get string length
$MyVariable.GetType() # Get data type
```

---

## Data Types

### Boolean Variables:

```powershell
$MyBoolVar = $false
$MyBoolVar.GetType()  # Output: System.Boolean
```

Common boolean values:

- `$true` - Represents true
- `$false` - Represents false

---

## Comparison Operators

PowerShell uses specific operators for comparisons (not symbols like `==`, `>`, `<`):

| Operator | Description           | Example           |
| -------- | --------------------- | ----------------- |
| `-eq`    | Equal to              | `2 -eq 3` → False |
| `-ne`    | Not equal to          | `2 -ne 3` → True  |
| `-gt`    | Greater than          | `2 -gt 3` → False |
| `-ge`    | Greater than or equal | `2 -ge 3` → False |
| `-lt`    | Less than             | `2 -lt 3` → True  |
| `-le`    | Less than or equal    | `2 -le 3` → True  |

```powershell
2 -eq 3  # False
2 -ne 3  # True
2 -gt 3  # False
2 -ge 3  # False
2 -lt 3  # True
2 -le 3  # True
```

---

## Arrays

**Arrays** are collections of items stored in a single variable.

```powershell
# Create an array
$a = 1,2,3,4,5
$a.Count           # Get number of elements
$a.GetType()       # Get type information
$a[0 .. 4]         # Access range of elements

# Create array with range operator
$b = 1 .. 10
$b[-2..-4]         # Access elements from end (reverse order)
```

### Array Indexing:

- `$a[0]` - First element
- `$a[-1]` - Last element
- `$a[0..4]` - Range from index 0 to 4
- `$a[-2..-4]` - Reverse range from second-last to fourth-last

---

## Loops

### ForEach Loop

The `foreach` loop iterates through each item in a collection.

```powershell
$a = 1 .. 10

foreach($i in $a)
{
    $i * 10  # Multiply each element by 10
}
```

### Do-While Loop

The `do-while` loop executes code at least once, then repeats while a condition is true.

```powershell
$count = 0

do {
    Write-Output $count
    $count++
} while ($count -lt 5)
```

---

## Hashtables

**Hashtables** (also called **dictionaries**) store key-value pairs.

```powershell
# Create a hashtable
$settings = @{
    "AppName" = "Chrome"
    "version" = "1.0.0"
    "maxusers" = 100
}

# Access values
$settings["appname","version"]

# Update values
$settings["version"] = "1.4.0"
$settings["version"]

# Iterate through keys
foreach($i in $settings.Keys)
{
    $settings[$i]
}

# Check if key exists
$settings.Contains("version")  # True

# Count entries
$settings.Count
```

---

## Custom Objects

**Custom Objects** allow you to create structured data with properties.

### Single Custom Object:

```powershell
# Create Custom Object
$person = [PSCustomObject]@{
    FirstName = "Modi"
    LastName = "Ji"
    Age = 30
    Occupation = "Software Developer"
}

# Access properties with string interpolation
"Full Name: $($person.FirstName) $($person.LastName)"
```

### List of Custom Objects:

```powershell
# Create array of custom objects
$employees = @(
    [PSCustomObject]@{Name = "Mukesh"; Age = 30; Role = "Manager"}
    [PSCustomObject]@{Name = "Pintu"; Age = 25; Role = "Developer"}
    [PSCustomObject]@{Name = "Rakesh"; Age = 24; Role = "Tester"}
)

# Iterate through list
foreach($i in $employees)
{
    "$($i.Name), $($i.Age), $($i.Role)"
}
```

---

## Pipeline

The **Pipeline** (`|`) passes the output of one command as input to another command.

**Syntax:** `Command1 | Command2 | Command3`

### Pipeline Examples:

```powershell
# Convert string to uppercase using pipeline
"Hello World" | ForEach-Object {$_.ToUpper()}

# Filter processes by name
Get-Process | Where-Object {$_.Name -eq "Notepad"} | Select-Object id,Name

# Get only running services
Get-Service | Where-Object {$_.Status -eq "Running"}

# Find large files
Get-ChildItem -Path "D:\CHFI v11 Book" | Where-Object {$_.Length -gt 550MB}
```

### Key Pipeline Cmdlets:

- `ForEach-Object` - Process each object (uses `$_`)
- `Where-Object` - Filter objects based on condition
- `Select-Object` - Select specific properties

---

## Conditional Statements

### If-ElseIf-Else Statement

```powershell
$age = 25

if ($age -le 18) {
    Write-Output "You are a minor"
} elseif ($age -gt 18 -and $age -le 60) {
    Write-Output "You are an adult"
} else {
    Write-Output "You are a senior"
}
```

### Logical Operators:

- `-and` - Logical AND
- `-or` - Logical OR
- `-not` or `!` - Logical NOT

### Switch Statement

The `switch` statement evaluates an expression against multiple conditions.

```powershell
$input1 = "Yellow"

switch ($input1) {
    "Red" { Write-Output "Stop" }
    "Yellow" { Write-Output "Get Ready" }
    "Green" { Write-Output "Go" }
    Default { Write-Output "Invalid Color" }
}
```

---

## Error Handling

PowerShell uses **try-catch-finally** blocks for error handling.

```powershell
try {
    # Code that might throw an error
    Get-Content -Path "C:\Users\mukesh\Desktop\to-learn.md" -ErrorAction Stop
    Write-Output "Above File Exists"
}
catch {
    # Handle the error
    Write-Output("Error: $($_.Exception.Message)")
    Get-Content -Path "C:\Users\smwlc\Desktop\to-learn.txt"
}
finally {
    # Always executes (cleanup code)
    Write-Output("File operations closed.")
}
```

### Error Handling Components:

- **try** - Contains code that might generate an error
- **catch** - Handles the error if one occurs
- **finally** - Always executes, regardless of errors (optional)
- `$_.Exception.Message` - Accesses the error message

---

## Quick Reference

| Category       | Examples                                     |
| -------------- | -------------------------------------------- |
| **Cmdlets**    | `Get-Service`, `Set-Location`, `Remove-Item` |
| **Variables**  | `$myVar = "value"`                           |
| **Arrays**     | `$arr = 1,2,3,4,5`                           |
| **Hashtables** | `@{key = "value"}`                           |
| **Pipeline**   | `Get-Process \| Where-Object {condition}`    |
| **Comparison** | `-eq`, `-ne`, `-gt`, `-lt`                   |
| **Logical**    | `-and`, `-or`, `-not`                        |

---

## Resources

📺 **Video Tutorial:** [PowerShell Fundamentals](https://youtu.be/Hmkyn4yoLNQ?si=vtd9bupCKuDXlPQK)

---

**Happy Learning PowerShell! 🚀**
