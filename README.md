# Remote C/C++ Debugging with VSCode and GDB

This guide will help you set up Visual Studio Code (VSCode) for developing and debugging C/C++ code on a remote machine using SSH and GDB.

## What You Need

1. **Visual Studio Code**: [Download it here](https://code.visualstudio.com/).
2. **SSH Access**: You need to be able to connect to your remote machine.
3. **GCC & GDB**: Make sure these are installed on your remote machine.

## Setup Steps

### 1. Install VSCode Extensions

1. Open VSCode.
2. Click on the Extensions icon on the sidebar or press `Ctrl+Shift+X`.
3. Search for and install the **Remote - SSH** extension.

### 2. Connect to Your Remote Machine

1. Press `F1` or `Ctrl+Shift+P` to open the Command Palette.
2. Type `Remote-SSH: Connect to Host...` and select it.
3. Enter your SSH connection string (e.g., `user@hostname` or `user@IP_Address`).
4. Follow the prompts to complete the connection.

### 3. Install C/C++ Extension on the Remote Machine

1. Once connected, VSCode will open a new window for remote work.
2. Go to the Extensions view in this window.
3. Search for and install the **C/C++** extension.

### 4. Open Your Project

1. Navigate to your project folder on the remote machine.
2. In VSCode, go to `File > Open Folder` and select your project folder.

### 5. Configure C/C++ Settings

1. Create a file named `.vscode/c_cpp_properties.json` in your project directory:

   ```bash
   mkdir .vscode
   touch .vscode/c_cpp_properties.json
   ```

2. Add this configuration to `c_cpp_properties.json`:

   ```json
   {
       "configurations": [
           {
               "name": "Remote GCC",
               "intelliSenseMode": "linux-gcc-x64",
               "compilerPath": "/usr/bin/gcc",
               "includePath": [
                   "${workspaceFolder}"
               ],
               "cStandard": "c11",
               "cppStandard": "c++14"
           }
       ],
       "version": 4
   }
   ```

### 6. Set Up Debugging with GDB

1. Create a file named `.vscode/launch.json` in your project directory:

   ```bash
   touch .vscode/launch.json
   ```

2. Add this configuration to `launch.json`:

   ```json
   {
       "version": "0.2.0",
       "configurations": [
           {
               "name": "GDB Launch",
               "type": "cppdbg",
               "request": "launch",
               "program": "${workspaceFolder}/path/to/your/program",
               "args": [
                   "/path/to/your/argv[1]",
                   "/path/to/your/argv[2]"
               ],
               "stopAtEntry": false,
               "cwd": "${workspaceFolder}",
               "externalConsole": false,
               "internalConsoleOptions": "neverOpen",
               "MIMode": "gdb",
               "miDebuggerPath": "/usr/bin/gdb",
               "setupCommands": [
                   {
                       "description": "Enable pretty-printing for GDB",
                       "text": "-enable-pretty-printing",
                       "ignoreFailures": true
                   }
               ],
               "preLaunchTask": "Compile"
           }
       ]
   }
   ```

   **Replace** `"path/to/your/program"` with the actual path to your executable.

### 7. Set Up Build Task

1. Create or update `.vscode/tasks.json`:

   ```bash
   touch .vscode/tasks.json
   ```

2. Add this configuration to `tasks.json`:

   ```json
   {
       "version": "2.0.0",
       "tasks": [
           {
               "label": "Compile",
               "command": "make",
               "type": "shell",
               "group": {
                   "kind": "build",
                   "isDefault": true
               },
               "options": {
                   "cwd": "${workspaceFolder}"
               },
               "problemMatcher": ["$gcc"],
               "detail": "Compile the project."
           }
       ]
   }
   ```

### 8. Adjust VSCode Settings (if you see a GDB warning)

1. Create or update `.vscode/settings.json`:

   ```bash
   touch .vscode/settings.json
   ```

2. Add this configuration to `settings.json`:

   ```json
   {
       "terminal.integrated.env.linux": {
           "SHELL": null
       }
   }
   ```

### 9. Build and Debug

1. Set breakpoints by clicking in the gutter next to line numbers.
2. Press `F5` to start debugging. VSCode will compile your code (if set up) and start a GDB debugging session.

## Troubleshooting

- **SSH Problems**: Check your SSH keys and ensure you can access the remote server.
- **GDB Issues**: Verify paths in `launch.json` and ensure GDB is installed on the remote machine.

## Additional Resources

- [VSCode Remote Development Documentation](https://code.visualstudio.com/docs/remote/remote-overview)
- [GCC, the GNU Compiler Collection](https://www.gnu.org/software/gcc/)
- [GDB Documentation](https://www.gnu.org/software/gdb/documentation/)

Feel free to ask if you have any questions. Happy coding!
