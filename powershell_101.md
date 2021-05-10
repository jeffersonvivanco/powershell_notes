# Powershell 101

## Getting started

### How to launch Powershell

#### Powershell (non-admin)
Some commands run fine, but Powershell cannot participate in User Access Control (UAC).
That means it's unable to prompt for elevation for tasks that require the approval of
an admin. The solution to this problem is to run Powershell as a domain user who is a
local admin. Using the principal of least privilege, this account should NOT be a domain
admin, or have any elevated privileges in the domain. Running pwsh elevated as an admin
to prevent having problems with UAC only impacts commands that are run against the local
computer. It has no effect on commands that target remote computers.

### Version
There are a number of automatic variables in Powershell that store state info. One
of these variables is `$PSVersionTable`, which contains a hashtable that can be used to
display the relevant pwsh version info.

### Execution Policy
Contrary to popular belief, the execution policy in pwsh is not a security boundary.
It's designed to prevent a user from unknowingly running a script. A determined user
can easily bypass the execution policy in Powershell.

| Windows OS version | Default execution policy |
| --- | --- |
| Server 2019 | Remote Signed |
| Server 2016 | Remote Signed |
| Windows 10 | Restricted |

Regardless of the execution policy setting, any pwsh command can be run interactively.
The execution policy only affects commands running in a script. The `Get-ExecutionPolicy`
cmdlet is used to determine what the current execution policy setting is and the
`Set-ExecutionPolicy` cmdlet is used to change the execution policy. It's recommended
to use the **RemoteSigned** policy, which requires downloaded scripts to be signed by
a trusted publisher in order to be run.

## The Help System

### Discoverability
Compiled commands in pwsh are called cmdlets. Cmdlet is pronounced "command-let". Cmdlets
names have the form of singular "Verb-Noun" commands to make them easily discoverable. There
are other types of commands in pwsh such as aliases and functions.

### 3 core Cmdlets in pwsh
* `Get-Command`
* `Get-Help`
* `Get-Member`

How do you figure out what commands are in pwsh? Both `Get-Help` and `Get-Command` can be
used to determine the commands.

#### `Get-Help`
Multipurposed command. It helps you learn how to use commands once you find them. It can
also be used to help locate commands, but in a different and more indirect way when compared
to `Get-Command`. When `Get-Help` is used to locate commands, it first searches for wildcard
matches of command names based on the provided input. If it doesn't find a match, it searches
through the help topics themselves, and if no match is found an error is returned. Contrary
to popular belief, `Get-Help` can be used to find commands that don't have help topics.

* to display the help topic for `Get-Help` `Get-Help -Name Get-Help`

Each of the following parameters are in different parameter sets:
* Full
* Detailed
* Examples
* Online
* Parameter Noun
* ShowWindow

`Help` is a function that pipes `Get-Help` to a function named `more`, which is a wrapper
for the `more.com` executable file in Windows. In the pwsh console, `help` provides 1 page
of help at a time. In the ISE, it works the same way as `Get-Help`. Less typing isn't
always a good thing, however. If you're going to save your commands as a script or share
them with someone else, be sure to use full cmdlet and parameter names. The full names
are self documenting, which makes them easier to understand.

To only return info about the `Name` parameter
`help Get-Help -Parameter Name`

If you want the results in a separate window, use the `Online` parameter or use the
`Full` parameter and pipe the results to `Out-GridView`
`help Get-Command -Full | Out-GridView`

To use `Get-Help` to find commands, use the asterisk `*` wildcard character with
the `Name` parameter. `help *process*`

If what you are attempting to look for are commands that end with `-process`, you
only need to add the `*` wildcard character to the beginning of the value.
`help *-process`

To search vaguely, if pwsh doesn't find any matches for what you search in command
names, it'll search every help topic in pwsh in your system. `Get-Help processes`

The help system in Powershell has to be updated in order for the `About` help topics
to be present. If for some reason the initial update of the help system failed on
your computer, the files will not be available until the `Update-Help` cmdlet has
been run successfully.

#### `Get-Command`
Is designed to help you locate commands. Running `Get-Command` with no parameters
returns a list of all the commands on your system.

Find command to work with process
`Get-Command -Noun Process`

The `Name`, `Noun`, and `Verb` parameters accept wildcards.
`Get-Command -Name *service*`

Not a good idea to use wildcards with the `Name` parameter of `Get-Command` since
it also returns executable files that are not native pwsh commands. It's recommended
to limit results with `CommandType` parameter.

`Get-Command -Name *service* -CommandType Cmdlet, Function, Alias`

A better option is to use either the `Verb` or `Noun` parameter or both of them
since only pwsh commands have both verbs and nouns.

## Discovering objects, properties, and methods

### Get-Member
Helps you discover what objects, properties, and methods are available for commands.
Any command that produces object-based output can be piped to `Get-Member`

#### Properties
* Retrieving windows time service `Get-Service -Name w32time`
* Output
  ```pwsh
  Status   Name               DisplayName
  ------   ----               -----------
  Running  w32time            Windows Time
  ```
* `Status`, `Name` and `DisplayName` are examples of properties. The value for the
  `Status` property is `Running`.
* pipe the same command to `Get-Member` `Get-Service -Name w32time | Get-Member`
* output
  ```pwsh
  TypeName: System.ServiceProcess.ServiceController

  Name                      MemberType    Definition
  ----                      ----------    ----------
  Name                      AliasProperty Name = ServiceName
  RequiredServices          AliasProperty RequiredServices = ServicesDependedOn
  Disposed                  Event         System.EventHandler Disposed(System.Object, Sy...
  Close                     Method        void Close()
  Continue                  Method        void Continue()

  ...

  ```
* The first line of the results in the previous example contains one piece of very
  important information. `TypeName` tells you what type of object was returned.
  In this ex, a `System.ServiceProcess.ServiceController` object was returned. This
  is often abbrv as the portion of the `TypeName` just after the last period; `ServiceController`
  in this ex.
* once u know what type of object a command produces, you can use this info to
  find commands that accept that type of object as input
  `Get-Command -ParameterType ServiceController`
* All of those commands have a paremter that accepts a `ServiceController` object
  type by pipeline, parameter input, or both.

