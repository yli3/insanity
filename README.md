# insanity
Trying to maintain the same user experience across multiple VMs.

## Cleaning the home directory
`xdg-user-dirs-update`

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

## vim arrow keys
Avoid typing lots of ABCDs and use the arrow keys to navigate instead. Create the .vimrc file:
```
  vim ~/.vimrc
```
Add the following to the file:
```
  set nocompatible
  set tabstop=8 softtabstop=0 expandtab shiftwidth=4 smarttab
```

## Visual Studio Code 
My preferred user settings:
```
{
    "editor.minimap.enabled": false,
    "editor.wordWrap": "bounded",
    "editor.renderWhitespace": "all",
    "workbench.sideBar.location": "right",
    "workbench.editor.enablePreview": false 
}
```
Optionally, when the convention is to use 2 spaces for tab, add:
```
  "editor.tabSize": 2
```
VS code doesn't (always?) update its file list automatically for an open folder. You can define a custom key code:
```
{
  "key": "ctrl+f5",
  "command": "workbench.files.action.refreshFilesExplorer"
}
```
Or View->Command Palette (Shift+Cmd+P on Mac OS X), then "refresh explorer", or "Preferences->Keyboard Shortcuts", search for "refresh files" and define your own custom keyboard shortcut (e.g, Cmd+R).

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

I think a slight modification to the above would make it recursive, e.g apply to all executables in all directories:
```
**
!/**/
!**.*
```
These rules first exclude all files, then whitelist parent folders recursively, and then re-include files that have an extension. See here: https://stackoverflow.com/questions/19023550/how-do-i-add-files-without-dots-in-them-all-extension-less-files-to-the-gitign

## git Collaboration
To make a development branch into master on the remote server `origin`, provided it is locally up-to-date:
```
git push -u origin devbranch:master
```

To grab a file from another branch with commit hash XXXXXX:
```
git checkout XXXXXX filename.c
```

## git Reset
See: https://stackoverflow.com/questions/1628088/reset-local-repository-branch-to-be-just-like-remote-repository-head

```
git fetch origin
git reset --hard origin/master
```

This restores your local branch to match the remote branch exactly.

## git shallow clone
For large repositories and where you only need a small subset, combine **sparse checkout** with **shallow clone**:

```
git init <repo>
cd <repo>
git remote add origin <url>
git config core.sparsecheckout true
echo '/.gitignore' >> .git/info/sparse-checkout 
echo 'desiredFolder/*' >> .git/info/sparse-checkout
echo '!desiredFolder/exclude/' >> .git/info/sparse-checkout
git pull --depth=1 origin master
```
Depth is the number of latest commits to pull from each branch. You may want some of the project history locally. Note that single quotes is necessary or you will get an "event not found" error for strings beginning with **!**. This is because **!** is a special character to bash. 

When `.git/info/sparse-checkout` is edited, run `git read-tree -mu HEAD`.

## git clean and git reset
`git clean -dfxn` to remove new files (-n is for dry run). 

To rollback uncommitted changes, `git reset` followed by `git checkout`.

## git multiplatform development
On Windows: `git config --global core.autocrlf true` (probably the default option used at git install)
On Linux: `git config --global core.autocrlf input`

## superuser on Ubuntu
Ubuntu doesn't have a root password by default. To run root commands use sudo. To run the shell in super mode use sudo -i or sudo -s.

## VMware Tools
https://www.vmware.com/support/ws55/doc/ws_newguest_tools_linux.html

open-vm-tools https://kb.vmware.com/s/article/2073803

## VirtualBox Guest Additions
Can't symlink from Windows host, so: 
```
VBoxManage guestproperty set "Ubuntu 16" /VirtualBox/GuestAdd/SharedFolders/MountPrefix /
VBoxManage guestproperty set "Ubuntu 16" /VirtualBox/GuestAdd/SharedFolders/MountDir /home/<yourfolderhere>/
```

To mount in home without the sf_ prefix. Check with `VboxManage guestproperty enumerate "Ubuntu 16"`.

## SSH credentials
To avoid the password prompt with every ssh or scp command for known machines, 

```
ssh-copy-id remote-user@remote-host
```
Additionally: create an alias in the SSH config file so that you can simply `ssh alias` (also applies to scp). See: https://askubuntu.com/questions/257472/host-alias-for-ssh

## Chrome update broken on Ubuntu
Do this:
```
wget -q -O - https://dl.google.com/linux/linux_signing_key.pub | sudo apt-key add -
```
## Chrome keyring
By default Chrome likes to ask for a keyring password every time on first start.

To do it once, and unlock every time at login, install `libpam-gnome-keyring` and then log out, and back in again.
