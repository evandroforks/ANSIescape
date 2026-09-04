## ANSI escape codes color highlighting for ST3

From time to time, you end up with a file in your editor that has ANSI escape codes in plain text which makes it really hard to read the content. Something that has been added to make your life easier, stands in your way and it's really annoying.

This plugin solves this annoying problem. Not by just removing the ANSI escape codes but by bringing back the color highlighting to those files.

![Sublime ANSI Screenshot](https://s3.amazonaws.com/f.cl.ly/items/0e3a0V1A3y392W0R3z20/sublime_ansi.gif)


## Installation

### By Package Control

1. Download & Install **`Sublime Text 3`** (https://www.sublimetext.com/3)
1. Go to the menu **`Tools -> Install Package Control`**, then,
    wait few seconds until the installation finishes up
1. Now,
    Go to the menu **`Preferences -> Package Control`**
1. Type **`Add Channel`** on the opened quick panel and press <kbd>Enter</kbd>
1. Then,
    input the following address and press <kbd>Enter</kbd>
    ```
    https://raw.githubusercontent.com/evandrocoan/StudioChannel/master/channel.json
    ```
1. Go to the menu **`Tools -> Command Palette...
    (Ctrl+Shift+P)`**
1. Type **`Preferences:
    Package Control Settings – User`** on the opened quick panel and press <kbd>Enter</kbd>
1. Then,
    find the following setting on your **`Package Control.sublime-settings`** file:
    ```js
    "channels":
    [
        "https://packagecontrol.io/channel_v3.json",
        "https://raw.githubusercontent.com/evandrocoan/StudioChannel/master/channel.json",
    ],
    ```
1. And,
    change it to the following, i.e.,
    put the **`https://raw.githubusercontent...`** line as first:
    ```js
    "channels":
    [
        "https://raw.githubusercontent.com/evandrocoan/StudioChannel/master/channel.json",
        "https://packagecontrol.io/channel_v3.json",
    ],
    ```
    * The **`https://raw.githubusercontent...`** line must to be added before the **`https://packagecontrol.io...`** one, otherwise,
      you will not install this forked version of the package,
      but the original available on the Package Control default channel **`https://packagecontrol.io...`**
    > [!WARNING]
    > Placing this custom channel before the default channel changes Package Control's resolution globally. Packages from this channel with the same name will override versions from the default channel.
    >
    > You can review the channel contents here:
    > https://raw.githubusercontent.com/evandrocoan/StudioChannel/master/channel.json
1. Now,
    go to the menu **`Preferences -> Package Control`**
1. Type **`Install Package`** on the opened quick panel and press <kbd>Enter</kbd>
1. Then,
    search for **`ANSIescape`** and press <kbd>Enter</kbd>

See also:

1. [ITE - Integrated Toolset Environment](https://github.com/evandrocoan/ITE)
1. [Package control docs](https://packagecontrol.io/docs/usage) for details.


## Usage

When you see garbage in your editor change the syntax to `ANSI` and you're good!

The plugin works by detecting the syntax change event and marking ANSI color chars regions with the appropriate scopes matching the style defined in a tmTheme file.

### Using this plugin as a dependency for your plugin/build output panel
If you're writing a plugin that builds something using a shell command and shows the results in an output panel, use this plugin! Do not remove ANSI codes, just set the syntax file of your output to `Packages/ANSIescape/ANSI.tmLanguage` and ANSI will take care of color highlighting your terminal output.

Likewise, if you would like to display ANSI colors in your existing [build-command](http://sublime-text-unofficial-documentation.readthedocs.org/en/latest/reference/build_systems/basics.html) output, you would only need to set `ansi_color_build` as the target and `Packages/ANSIescape/ANSI.tmLanguage` as the syntax; for example:

```javascript
// someproject.sublime-project
{
    "build_systems":
    [
        {
            /* your existing build command arguments */
            "name": "Run",
            "working_dir": "${project_path}/src",
            "env": {"PYTHONPATH": ".../venv/python2.7/site-packages"},
            "cmd": ["nosetests", "--rednose"],

            /*  add target and syntax */
            "target": "ansi_color_build",
            "syntax": "Packages/ANSIescape/ANSI.tmLanguage"
        }
    ]
}
```

If you use a custom build script and sub-programms don't output color, it could be that they assume the output has no colors. On Linux some applications can be forced to use colors by setting the environment variable `CLICOLOR_FORCE=1`. It is not recommended to set it permanently since it could cause issues if color is not supported and applications still output color. But in a SublimeANSI build you can use it for the usage in a Makefile build script, e.g.:

```javascript
// someproject.sublime-project
{
    "build_systems":
    [
        {
            /* your existing build command arguments */
            "name": "Build",
            "working_dir": "${project_path}",
            "env": {"CLICOLOR_FORCE": "1"},
            "cmd": ["sh", "build.sh"],

            /*  add target and syntax */
            "target": "ansi_color_build",
            "syntax": "Packages/ANSIescape/ANSI.tmLanguage"

             "variants":
            [
                {
                    "name": "Clean",
                    "cmd": ["sh", "build.sh", "clean"]
                }
            ]
        }
    ]
}
```


#### Killing the build process

If you want to kill build process during execution, use this command in sublime console (``ctrl+` ``):

```shell
window.run_command("ansi_color_build", args={"kill": True})
```

You can also add key binding eg.:

```javascript
// Preferences > Key Binding - User
[
    { "keys": ["ctrl+alt+c"], "command": "ansi_color_build", "args": {"kill": true} },
    // other key-bindings
]
```

#### Formatting ANSI codes during build process

In order to format ANSI codes during building process change 'ANSI_process_trigger' in [`ansi.sublime-settings`](ansi.sublime-settings).

### Customizing ANSI colors
All the colors used to highlight ANSI escape code can be customized through
[`ansi.sublime-settings`](ansi.sublime-settings).
Create a file named `ansi.sublime-settings` in your user directory, copy the content of default settings and change them to your heart's content.

### Caveats:
- ANSI views are read-only. But you can switch back to plain text to edit them if you want.
- Does not render ANSI bold as bold, although we support it. You can assign a unique foreground color to bold items to distinguish them from the rest of the content.
- Does not support dim, underscore, blink, reverse and hidden text attributes, which is fine since they are not supported by many terminals as well and their usage are pretty rare.

### License
Copyright 2014-2016 [Allen Bargi](https://twitter.com/aziz). Licensed under the MIT License

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.

