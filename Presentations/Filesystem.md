# Linux filesystem
```
neumann@pi:~ $ ls /
bin   dev  home  lost+found  mnt  proc  run   srv  tmp  var
boot  etc  lib   media       opt  root  sbin  sys  usr
```

notes: https://medium.com/ensias-it/linux-filesystem-thorough-walk-through-3b9c73efca9a

---
# /dev

Devices
* Disks (*sd\**, *hd\**)
* Peripheral (*gpiochip0*)
* Video devices
* etc
---
# /sys

System information exported by the kernel

---
# /proc

System information
* Processes
* System information
* Network informations
* Various odds and ends

---
# /bin and /sbin, /lib

Binaries and system binaries, and libraries

---
# /boot

Kernel and boot files

* *cmdline.txt*
* *config.txt*

---
# /tmp

Public temporary directory

---
# /home

Home directories for all users **except** *root*

---
# /root

Separate directory for the *root* user, for system maintenance.

note: On older UNIX system, */usr* and/or */home* could sometimes be accessed (mounted) from the network. In case the access to the */usr* directory failed, */root* remained accessible

---
# /etc

Configuration files
* *fstab*
* *sudoers*
* systemd
* *apt*
* etc..
---
# /var

* *log*
* *cache*
* *lib*

note: The *cache* directory is used by programs that typically want to store data that is temporary. Some of that data will take a non-trivial amount of space! *lib* is something you should typically not mess with.

---
# /usr

* More of the same?
* */usr/local*... more more of the same?
	* *Installation path for custom-built sources*
---
# /lost+found

Specific to *ext2/3/4* systems: gathers data and content from files that *fsck* could not link to a file