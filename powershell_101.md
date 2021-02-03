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

