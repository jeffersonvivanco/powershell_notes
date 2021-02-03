# Powershell Management
The Management module contains cmdlets that help you manage Windows in Powershell.

## `Add-Content`
Adds content to the specified items, such as adding words to a file.

## `Clear-Content`
Deletes the contents of an item, but does not delete the item.

## `Clear-Item`
Clears the contents of an item, but does not delete the item

## `Clear-RecycleBin`
Clears the contents of a recycle bin.

## `Copy-Item`
Copies and item from one location to another.
* copy a file to the specified directory `Copy-Item "C:\Wabash\Logfiles\mar1604.log.txt" -Destination "C:\Presentation"`
* copy directory contents to an existing directory `Copy-Item -Path "C:\Logfiles\*" -Destination "C:\Drawings" -Recurse`
  * copies contents from `LogFiles` into `Drawings`, `LogFiles` directory isn't copied
  * by default, the `Container` parameter is set to `True`, which preserves the directory structure
  * if you need to include the `LogFiles` directory in the copy, remove the `\*` from the `Path`.

## `Get-Clipboard`
Gets the content of the clipboard.

## `Get-ComputerInfo`
Gets a consolidated object of system and OS properties

## `Get-Content`
Gets the content of the item at the specified location.

## `Get-Item`
Gets the item at the specified location. It doesn't get the contents
of the item at the location unless you use a wildcard character `*`
to request all the contents of the item.

* Get the current directory (The dot represents the item at the
  current location, not its contents.) `Get-Item .`
* Get all the items in the current directory `Get-Item *`
* Get items in the specified drive `Get-Item C:\*`

## `Get-Process`
Gets the processes that are running on a local or remote computer.

## `Get-PSDrive`
Gets drives in the current session.

## `Move-Item`
Moves an item from one location to another.

## `New-Item`
Creates a new item.

## `New-PSDrive`
Creates temporary and persistent mapped network drives

## `Remove-Item`
Deletes the specified items.

## `Remove-PSDrive`
Deletes temporary powershell drives and disconnects mapped
network drives.

## `Rename-Item`
Renames an item in a powershell provider namespace.

## `Restart-Computer`
Restarts the OS on local and remote computers.

## `Set-Clipboard`
Sets the contents of the clipboard.

## `Set-Content`
Writes a new content or replaces existing content in a file.

## `Set-Item`
Changes the value of an item to the value specified in the command.

## `Set-Location`
Sets the current working location to a specified location.

## `Start-Process`
Starts one or more processes on the local computer. By default it creates a new
process that inherits all the environment variables that are defined in the current
process. To specify the program that runs in the process, enter an executable file
or script file, or a file that can be opened by using a program on the computer. If
you specify a non-executable file, `Start-Process` starts the program that is associated
with the file, similar to the `Invoke-Item` cmdlet. You can use the parameters of `Start-Process`
to specify options, such as loading a user profile, starting a process in a new window, or
using alternate credentials.

* start a process that uses default values `Start-Process -FilePath "sort.exe"`
* print a text file `Start-Process -FilePath "myfile.txt" -WorkingDirectory "C:\PS-Test" -Verb Print`
* start a process in a maximized window `Start-Process -FilePath "notepad" -Wait -WindowStyle Maximized`
* start powershell as an admin `Start-Process -FilePath "powershell" -Verb RunAs`

## `Stop-Computer`
Stops (shuts down) local and remote computers

## `Stop-Process`
Stops one or more running processes

## `Wait-Process`
Waits for the process to be stopped before accepting more input.
