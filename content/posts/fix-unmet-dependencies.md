---
title: "Fix: E: Unmet dependencies. and E: Sub-process /usr/bin/dpkg returned an error code (1)"
date: 2021-06-13T15:14:44+05:30
draft: false
tags: ["ubuntu", "dpkg", "troubleshooting"]
showToc: false
TocOpen: true
---

Hi there  :wave:

If you're trying to install/update/remove/fix a debian package and getting the error mentioned in title, and none of the following commands are working, 
```bash
sudo apt-get remove
sudo apt-get install
sudo apt-get purge
sudo apt-get autoremove
sudo apt-get autoclean
sudo apt-get install -f
```
you're at the right place.

You'll face this error if you've terminated the installation process of a software package. You may've forgotten, when and why did you terminate the installation, but that's not an issue, you'll still be able to fix this issue.


Alongwith this error message, you'll find information about the name of the package, and its dependencies and their version.

```
The following packages have unmet dependencies:
 libpython3.9 : Depends: libpython3.9-stdlib (= 3.9.5-3~20.04.1) but 3.9.4-1+bionic1 is to be installed
 libpython3.9-dev : Depends: libpython3.9-stdlib (= 3.9.5-3~20.04.1) but 3.9.4-1+bionic1 is to be installed
 python3.9 : Depends: libpython3.9-stdlib (= 3.9.5-3~20.04.1) but 3.9.4-1+bionic1 is to be installed
 python3.9-minimal : Depends: libpython3.9-minimal (= 3.9.5-3~20.04.1) but 3.9.4-1+bionic1 is to be installed
 software-properties-common : Depends: python3-software-properties (= 0.98.9.5) but 0.98.9.4 is to be installed
E: Unmet dependencies. Try 'apt --fix-broken install' with no packages (or specify a solution).
```

But, if you try `apt --fix-broken install`, you'll get another error

```
dpkg: error processing archive /var/cache/apt/archives/libpython3.9-minimal_3.9.5-3~20.04.1_amd64.deb (--unpack):
 trying to overwrite '/usr/lib/python3.9/typing.py', which is also in package libpython3.9-stdlib:amd64 3.9.4-1+bionic1
dpkg-deb: error: paste subprocess was killed by signal (Broken pipe)
Errors were encountered while processing:
 /var/cache/apt/archives/libpython3.9-stdlib_3.9.5-3~20.04.1_amd64.deb
 /var/cache/apt/archives/libpython3.9-minimal_3.9.5-3~20.04.1_amd64.deb
E: Sub-process /usr/bin/dpkg returned an error code (1)
```

Solution of this issue is pretty simple, you've to manually remove the files of broken package. 

To get the list of the files you can run

```
sudo ls –l /var/lib/dpkg/info | grep -i libpython3.9-dev
```
Replace `libpython3.9-dev` with your own package name.

Output
```
libpython3.9-dev:amd64.list
libpython3.9-dev:amd64.md5sums
```

Now, you can to discard these files.

```
sudo rm /var/lib/dpkg/info/python3.9-dev*
```

Perform the same operations for the rest of packages.

Once it's done, you can run

```
sudo apt --fix-broken install
```

This time the above command and other commands that were failing earlier should work fine.

Have fun, Keep hacking!