# What is it?

ProperTree is a cross-platform GUI plist editor written using Python *(compatible with both 2.x and 3.x)* and Tkinter.

## Features

- [x] Cross-platform - should work anywhere python and tkinter do
- [x] Document-based to support multiple windows
- [x] Node drag and drop to reorder
- [x] Copy and paste
- [x] Find/Replace - allows searching keys or values
- [x] Ordered - or unordered - dictionary support
- [x] Full undo-redo stack
- [x] Backported support for binary property lists and unicode in python 2
- [x] Expanded integer casting to allow for hex integers (eg. `0xFFFF`) in xml `<integer>` tags
- [x] Context-aware right-click menu that includes template info to OpenCore or Clover config.plist files
- [x] OC (Clean) Snapshot to walk the contents of ACPI, Drivers, Kexts, and Tools for OpenCore config.plist files
- [x] Value converter that supports Base64, Hex, Ascii, and Decimal

***

## Getting ProperTree

### Downloading The Repo As A ZIP File

On any system you can choose the `Clone or download` button, followed by the `Download ZIP` button to download the entire repo as a zip file (note, this does not allow you to update via `git pull` - any updates would require you to download the repo again in the same fashion).

### Cloning The Repo Via Git

#### On *nix systems:

```
git clone https://github.com/corpnewt/ProperTree
python ./ProperTree/ProperTree.command
```

\* On macOS, you can simply double-click the `ProperTree.command` after cloning to launch.

#### On Windows:

```
git clone https://github.com/corpnewt/ProperTree
./ProperTree/ProperTree.bat
```

***

## FAQ

* **How can I have ProperTree open when I double-click a .plist file?**

  On macOS you can run `buildapp-select.command` located in ProperTree's `Scripts` directory to build an application bundle which can be associated with .plist files.
  
  On Windows, you can run `AssociatePlistFiles.bat` located in ProperTree's `Scripts` directory to associate .plist files with `ProperTree.bat`, and also to add an `Open with ProperTree` option to the contextual menu when right-clicking .plist files.  This approach is location-dependent, and moving your copy of ProperTree will require you re-run `AssociatePlistFiles.bat`.

* **When I try to run ProperTree, I get `[ModuleNotFoundError: No module name 'tkinter']`**

  That is because the graphical interface library that ProperTree depends on isn't present or cannot be detected, you need to install `tkinter` from your package manager. 

  To install it on Ubuntu (and Ubuntu-based distros), you can run `sudo apt-get install python3-tk -y`

* **ProperTree doesn't run because it doesn't have permissions, what gives?**

  This shouldn't happen and it is recommended that you download only from the official ProperTree repository, but if you are confident about your source, then running `chmod +x ProperTree.command` should sort it out

* **I use an international keyboard layout on macOS and some keys crash ProperTree with `NSRangeException', reason: '-[__NSCFConstantString characterAtIndex:]: Range or index out of bounds`**

  This is a bug in the Cocoa implementation of Tcl/Tk on macOS (discussed [here](https://bugs.python.org/issue22566)).  The latest python 2 installer from [python.org](https://www.python.org/downloads/release/python-2718/) ships with, and uses Tcl/Tk 8.6.8 which has this issue fixed.  Given that the shebang in `ProperTree.command` leverages `#!/usr/bin/env python` - the first python 2 binary found should be used. `buildapp-select.command` from ProperTree's `Scripts` directory can be used to hardcode a specific python install's path into the .app bundle's executable shebang.
  
* **ProperTree crashes on Big Sur (macOS 11)**

  This is due to the default python installs on macOS leveraging an older `tk` version - which lacks support for macOS 11.  To solve this, you can download and install the latest python 3 version from https://www.python.org/downloads/mac-osx/ (note: Currently the "universal" 3.9.1 installer causes theme issues and should not be used) then leverage the `buildapp-select.command` from ProperTree's `Scripts` directory to build a .app bundle that will leverage that python version.  An example of the output of that script is below (I would select option `3` or `C` to use the non-system python install):

```
 - Currently Available Python Versions -

1. /usr/bin/python 2.7.16 - tk 8.5 (8.6+ recommended)
2. /usr/bin/python3 3.8.2 - tk 8.5 (8.6+ recommended)
3. /Library/Frameworks/Python.framework/Versions/3.9/bin/python3 3.9.1 - tk 8.6
4. /usr/bin/env python
5. /usr/bin/env python3

C: Current (/Library/Frameworks/Python.framework/Versions/3.9/bin/python3)
Q. Quit

Please select the python version to use:  
```
  
  If you already have python 3 installed via `brew` or another package manager - it is likely still linking to the system `tk` version, which will still have issues unless linked against a newer version. 

  As of macOS 11.2 (20D5029f), the system's `tk` install appears to be fixed, and works correctly.  As such, it should not require an external python version to function.
