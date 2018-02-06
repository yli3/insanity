# insanity
Trying to maintain the same user experience across multiple VMs.

## Keyboards
Edit the X keyboard extension to remap the SUPER key to control. Clear cache afterwards, then reboot:
```
  sudo vi /usr/share/X11/xkb/symbols/pc
  sudo rm -rf /var/lib/xkb/*
```

If you mess up this file and aren't able to enter keys via the GUI anymore, boot into recovery mode (root)  and edit the `pc` file from console. You may need to remount to be able to write to files:

```
  mount -w -o remount /
```

## vim arrow key insanity
Avoid typing lots of ABCDs and use the arrow keys to navigate instead. Create the .vimrc file:
```
  vim ~/.vimrc
```
Add the following to the file:
```
  set nocompatible
```

## Visual Studio Code 
My preferred user settings:
```
{
    "editor.minimap.enabled": false,
    "editor.wordWrap": "bounded",
    "editor.renderWhitespace": "all",
}
```
Optionally, when the convention is to use 2 spaces for tab, add:
```
  "editor.tabSize": 2
```
## .gitignore
To ignore extensionless files automatically:
1. add important extensionless files first (already versioned files will not be ignored)
2. at the beginning of .gitignore
```
*
!/**
!*.*
```
3. For future extensionless files, `git add -f -- myFile`

These rules first exclude all files, then whitelist parent folders recursively, and then re-include files that have an extension. See here: https://stackoverflow.com/questions/19023550/how-do-i-add-files-without-dots-in-them-all-extension-less-files-to-the-gitign
