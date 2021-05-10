# Powershell Core

## About Environment Variables
Environment variables store info about the OS environment. This info
includes details such as OS path, the number of processors used by
the OS, and the location of temp folders. The environment variables
store data that is used by the OS and other programs. For example,
the `WINDIR` environment variable contains the location of the Windows
installation directory. Programs can query the value of this variable
to determine where Windows operating system files are located.

Powershell can access and manage environment variables in any of the
supported OS platforms. The powershell environment provider simplifies
the process by making it easy to view and change environment variables.

Environment variables, unlike other types of variables in Powershell,
are inherited by child processes, such as local background jobs and
sessions in which module members run. This makes environment variables
well suited for storing values that are needed by both parent and child
processes.