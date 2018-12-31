# Installing & Using Visual Studio Code for C++ learning - Part 2 - using WSL

Some time ago (see previous gist) I began to explore Visual Studio Code with the intent to re-learn C++. I was looking for some simple IDE and I was not very pleased with CodeBlocks or NetBeans. VSC seemed to satisfy my requirements, but implied the installation of a C++ compiler & other tools needed to compile and run the programs.

I tried MinGW / MSYS / Cygwin, and I thought that, since Microsoft was offering Linux natively in the latest editions of Windows 10, to use WLS (Windows Subsystem for Linux). (Another alternative was to use the native Microsoft tools - but these seems to be too big and/or complicated - although the debugging is the best...)

> Please note that we are still discussing about (re)learning programming, C++ in this case.

> Also, don't forget that VSC is still an editor.

> Keep in mind that, when using wsl and Linux tools (such as gcc, g++) **the default executables are Linux-native** - so they will run on Linux, not on Windows. There are cross-compilers available.

Because my objective was learning (i.e. mainly writing many small and/or independent programs that work using console input/output - not big projects, nor graphical ones), wsl seemed a logical approach.

So, please follow the next steps:

## Prerequisites:
- You should be connected to the Internet, preferably with a good speed :-)
- Minimal knowledge about Linux would be nice.
 
Step 1 - install WSL (just query the net…). You should check that your Windows is compatible with it. That means Windows 10 at least 1803. It will work in Windows Server versions too, but the installation procedure is different (and not for our discussion).

* You can see it by checking "Turn Windows features on or off" in the Control Panel. 
* Find and check "Windows Subsystem for Linux"
* Aternatively, use Powershell command (as Administrator):
```powershell
PS> Enable-WindowsOptionalFeature -Online -FeatureName Microsoft-Windows-Subsystem-Linux
```
A restart will be needed.
* Go to Store
* Search for "Linux" and choose "Run Linux on Windows".
* Install your preferred version. (I used Ubuntu 18.04)

Step 2.
After installation, launch it:
* From the store
* From the start menu - Ubuntu or wsl or bash (please be careful if you already have some other tools installed such as MinGW or Cygwin or MSYS or something else - Mobaxterm or mintty comes to mind).

* A console will launch - now the "real" installation takes place - in will take some time depending on the speed of your computer.
* At the end of installation you will be asked for a username and a password.
> Keep in mind the username and password are specific to WSL and not related to Windows user (although you can choose the same name and password it you want...)

Once Ubuntu is installed, you have to run the latest upgrades. If you use other Linux distros, the commands will be specific to those.
> Note the `sudo` command that will elevate your rights to *superuser*. Your *Linux* password will be needed.

```bash
user@station:~$ sudo apt install update && sudo apt upgrade
```
After that install the compiler and toold (mainly `make` and the debugger `gdb`)
```bash
user@station:~$ sudo apt install g++ make gdb
```
Even safer instead of the above, is to install the `build-essential` (note the missing 's') package. The gdb still has to be installed separately.
```bash
user@station:~$ sudo apt-get install build-essential gdb
```
## And now to the work - using Visual Studio Code:
### Other prerequisites
* **VSC is installed** (if not, install it - go with the "user install" option).
* Change the **default shell to WSL shell**: 
  * Via F1 command and `Terminal: select default shell`: (select the wsl bash option)
  * Via menu: File -> Preferences -> Settings -> search for `terminal.integrated.shell.windows`
  
* The **C++ extension by Microsoft** is installed in VSC. (See in Marketplace at https://marketplace.visualstudio.com/items?itemName=ms-vscode.cpptools. The github page at https://github.com/Microsoft/vscode-cpptools)

If the default shell int VSC terminal is bash, the following `tasks.json` should work.
## Sample `tasks.json` for VSC, C++, WSL
```json
{
    // See https://go.microsoft.com/fwlink/?LinkId=733558
    // for the documentation about the tasks.json format
    "version": "2.0.0",
    "tasks": [
        {
            "label": "compile C++",
            "type": "shell",
            "command": "g++ -std=c++11 -g ${fileBasename} -o ${fileBasenameNoExtension}",
            "presentation": {
                "reveal": "always",
                "panel": "shared"
            },
            "group": {
                "kind": "build",
                "isDefault": true
            },
            // Use the standard less compilation problem matcher.
            "problemMatcher": {
                "owner": "cpp",
                "fileLocation": [
                    "relative",
                    "${workspaceRoot}"
                ],
                "pattern": {
                    "regexp": "^(.*):(\\d+):(\\d+):\\s+(warning|error):\\s+(.*)$",
                    "file": 1,
                    "line": 2,
                    "column": 3,
                    "severity": 4,
                    "message": 5
                }
            }
            }
        ]
    }

```
As an alternative, install Easy C++ Projects (https://marketplace.visualstudio.com/items?itemName=ACharLuk.easy-cpp-projects#overview) from the marketplace. It already provides prebuild files for wsl. But beware, in this case you should keep the Windows terminals (cmd.exe or powershell).

## `launch.json` and comments
For debugging purposes, a `launch.json` file is needed.
### Remarks
1. VSC is an editor, do not forget.
2. The debugging in C, C++ in not natively implemented, so it depends on the Extension (C/C++ extension in this case).
3. `lldb` (from `llvm + clang`) does not work under wsl as it seems to be based on some linux kernel semaphores and whatnot. (and WSL does not have a Linux kernel... please read more at [Jessie Frazelle Blog]( https://blog.jessfraz.com/post/windows-for-linux-nerds/) or at Microsoft https://docs.microsoft.com/en-us/windows/wsl/about).

It follows in my experience that *debugging does not really work* (especially in this WSL setup). 
I managed to get `gdb` working with the following configuration, but w/o input from console.
(I know it looks ugly, just paste in VSC or other json editor, it will look better.)

```json
{
    // Use IntelliSense to learn about possible attributes.
    // Hover to view descriptions of existing attributes.
    // For more information, visit: https://go.microsoft.com/fwlink/?linkid=830387
    "version": "0.2.0",
    "configurations": [
        
        {
            "name": "(gdb) Bash on Windows Launch",
            "type": "cppdbg",
            "request": "launch",
            // I had to put the full path manually;
	    // it should had worked with variables (see tasks.json), but it didn't
	    "program": "/mnt/c/Users/Adrian/Documents/hello",
            "args": [
                "-fThreading"
            ],
            "stopAtEntry": true,
            // see comment about path above.
	    // I tried some workaround
	    //"cwd": "$(wslpath '{workspaceFolder}')" - this did not work...
            // "." works though
	    "cwd": "/mnt/c/Users/Adrian/Documents",
            "environment": [],
            // also tried the following with true or false, but no avail...
	    "externalConsole": false,
            "pipeTransport": {
                "debuggerPath": "/usr/bin/gdb",
                "pipeProgram": "${env:windir}\\system32\\bash.exe",
                "pipeArgs": [
                    "-c"
                ],
                "pipeCwd": ""
            },
            "MIMode": "gdb",
	     // tried to give input from a file instead of console
             // did not worked, but I left it here for documentation purposes
            "miDebuggerArgs": "-ex 'start < arguments.in'",
            "setupCommands": [
                {
                    "description": "Enable pretty-printing for gdb",
                    "text": "-enable-pretty-printing",
                    "ignoreFailures": true,
                }
            ],
            // this seems to be IMPORTANT
		"sourceFileMap": {
                "/mnt/c": "c:\\"
            }
        }
        
    ]
}
```
