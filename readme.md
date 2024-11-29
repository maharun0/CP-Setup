# VSCode CP Setup

Requirements:

1. Make sure compiler path is set up properly.<br>
   or in `settings.json` add these lines: (optional)
   ```
   // [C++]
    "cmake.configureOnOpen": false,
    "C_Cpp.autocompleteAddParentheses": true,
    "C_Cpp.default.intelliSenseMode": "windows-gcc-arm64",
    "C_Cpp.default.includePath": [
        "C:/ProgramData/mingw64/mingw64/bin"
    ],
   ```

3. Install 'Code Runner' extension.

4. In settins.json add this shell script in code-runner.executorMap

    ```
    // [Remote Setup]

    "cpp": "if (-not (Test-Path './io')) { New-Item -Type Directory './io' } ; if (-not (Test-Path './io/input.txt')) { New-Item -Type File './io/input.txt' } ; if (-not (Test-Path './io/output.txt')) { New-Item -Type File './io/output.txt' } ; cd $dir && g++ -std=c++17 \"-Wl,--stack=268435456\" '$fileName' -o '$fileNameWithoutExt' && Get-Content './io/input.txt' | & $dir$fileNameWithoutExt.exe | Set-Content './io/output.txt' && del '$dir$fileNameWithoutExt.exe' && exit",
    ```
<br>
Now the main question, what deoes this shell do?

If you run a '.cpp' file it will create (if not already created)
 - io
   - input.txt
   - output.txt
 - random.cpp

**Input and Output will be redirected from and to these files.**



Credit: [Nadman Khan](https://github.com/NadmanKhan) wrote the shell script to redirect input and output.
I just added shell for creating input.txt and output.txt file for convinience.
Stack size part is added by [Mahir Shahriar](https://github.com/mahirshahriar1)

---
Final look:
![VS Code Setup - Final look](./VS%20Code%20Setup%20-%20Final%20look.png)

# Sublime Setup

### Set Script
- Tools -> Build System -> New Build System (write script & save)
- Tools -> Build System (select recently created script)
   `cp_linux.sublime-build`
   ```
   {
   "cmd" : ["g++ -std=c++20 $file_name -o $file_base_name && timeout 10s ./$file_base_name < input.txt > output.txt 2> debug.txt && rm $file_base_name"], 
   "selector" : "source.cpp",
   "shell": true,
   "working_dir" : "$file_path"
   }
   ```
### Set Editor
- Press `Alt + Shift + 4` to split window in 4 parts.
- save 'input.txt', 'output.txt', 'debug.txt'
- Press `Ctrl + B` to run code.

### Precompile HeaderFile
- just go to file explorer and serach 'stdc++.h'
- go to that folder and open folder in terminal
- sudo g++ -std=c++20 stdc++.h
- stdc++.h.gch is created precompile done

- preferences -> settings add "save_on_window_deactivation": true

// -----------------------------------------------------------------------------

// my windows sublime setup
`cp_windows.sublime-build`
```
{
  "cmd": [
    "g++.exe", "-std=c++14", "${file}", "-o", 
    "${file_base_name}.exe", "&&", "${file_base_name}.exe", 
    "<", "C:/Users/MARU/Documents/Dev/CP/io/input.txt", 
    ">", "C:/Users/MARU/Documents/Dev/CP/io/output.txt", 
    "2>", "C:/Users/MARU/Documents/Dev/CP/io/debug.txt", 
    "&&", "del", "${file_base_name}.exe"
  ],
  "selector": "source.cpp",
  "shell": true,
  "working_dir": "$file_path"
}
```

---
Final look:
![sublime_setup](https://github.com/user-attachments/assets/a164dd85-f727-4a32-8e2c-17391f6c3cd9)
