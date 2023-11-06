## Setup

To login to the phone, first enable USB debugging:

- Go to settings > about device
- Find Build number and press it repeatedly, until the message "developer mode activated" shows
- Go to settings > Developer options
- Enable "USB debugging"

Then plug it in to a linux computer with adb installed, open a terminal in this directory and use the scripts as follows.

## Usage

You need to make a list of packages first, and then choose which ones to allow. Crucially, we need to disallow the Google PlayStore to disallow further installation of applications. For this, we need to do: `./list > packages_before.txt`. Then, we need to go through that list and select applications to remove, which we put in packages_removed.txt. Then, we need to do `./remove packages_removed.txt removed_packages/`, and the backup apk files will end up in removed_packages/. To restore an app, we can do `adb install [apk_file]`.

```
list                                             Lists the packages installed, alphabetically sorted
copy [packagename]                               Copy the apk file for a given application
getpath [packagename]                            Gets the path to the apk file of an application on the device
remove [removed_packages.txt] [apk_backup_dir]   Copies the apk packages into the backup dir and then removes them
```

## Debugging

The scripts work based on adb shell commands. You can open an adb shell directly with:

```
adb shell
```

And then:

```
pm list packages
```

Shows the installed packages. This list has been copied into the .txt files here, and then the prefix 'package:' has been removed.

### Extract apk from phone:

```
pm path [packagename]
package:/system/app/Bluetooth/Bluetooth.apk
```

From outside the adb shell, use:

```
adb pull /system/app/Bluetooth/Bluetooth.apk
```

To get the apk. And then:

```
adb shell pm uninstall --user 0 [packagename]
```
