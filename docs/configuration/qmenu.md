Manage Qubes via dmenu; qmenu
==============================

[qmenu](https://github.com/sine3o14nnae/qmenu/) is a collection of tools that utilize
[dmenu](https://tools.suckless.org/dmenu/), a dynamic menu for X, to provide you with a drop down menu for Qubes specific tasks.

When configured to execute these tools via hotkeys, you are able to list, start and stop your qubes, attach and detach your connected devices,
adjust your qube preferences, firewall rules, per-qube keyboard layouts, launch applications and more, very quickly with only the keyboard.

List of qmenu tools:

- qmenu-am - Launch domU and dom0 applications.

- qmenu-dm - List and manage your connected devices.

- qmenu-vm - List, manage and configure your qubes.

Warning
-------

qmenu is **not** included in the Qubes dom0 repositories. This page will show you how to install qmenu by cloning its repository
to a disposable qube and [copying its contents to dom0](https://www.qubes-os.org/doc/copy-from-dom0/#copying-to-dom0).
**However, this way of installing software in dom0 is not advised and can compromise the security of your system!**

**Furthermore, qmenu is unreviewed third party software and is in no way endorsed by the Qubes team. You have to trust its maintainers
to not act maliciously and to protect it from malicious and unsafe contributions. Keep in mind that you are
installing these tools in _dom0_, so judge accordingly.**

Installation
------------

- Install dmenu:

       [user@dom0 ~]$ sudo qubes-dom0-update dmenu

- Start a disposable qube that is able to connect to the internet. **Do not use this qube for anything else!**
Then inside this qube; clone the qmenu repository with git:

       [user@dispXXXX ~]$ git clone https://github.com/sine3o14nnae/qmenu/

- Import the authors public key:

       [user@dispXXXX ~]$ gpg2 --keyserver pool.sks-keyservers.net --recv-keys 0xCBE6 0BC2 811F FE14 333F EE05 3435 0BCA 3DDE 9AD9

- Check the validity of the signed commit:

       [user@dispXXXX ~/qmenu/]$ git show --show-signature

- Finally, decide what qmenu tools you want to use, replace 'XX' accordingly, then copy them to dom0, place them
inside your path and change their mode bits.

       [user@dom0 ~]$ qvm-run --pass-io --filter-escape-chars --no-color-output dispXXXX 'cat /home/user/qmenu/qmenu-XX' > /tmp/qmenu-XX

       [user@dom0 ~]$ sudo cp /tmp/qmenu-XX /usr/local/bin/

       [user@dom0 ~]$ sudo chmod 755 /usr/local/bin/qmenu-XX
       
- For `qmenu-vm`, you have to additionally follow the steps below to copy all the files in /home/user/qmenu/lib/qmenu_vm/ to dom0 and place them in /lib/qmenu_vm/.

       [user@dom0 ~]$ mkdir /tmp/qmenu_vm

       [user@dom0 ~]$ for file in fq_keyboard fq_logs fq_pm fqubes_prefs fqvm_appmenus fqvm_clone fqvm_create fqvm_device fqvm_firewall fqvm_pci fqvm_prefs fqvm_remove fqvm_run fqvm_service fqvm_tags fqvm_volume; do qvm-run --pass-io --filter-escape-chars --no-color-output dispXXXX "cat /home/user/qmenu/lib/qmenu_vm/$file" > /tmp/qmenu_vm/$file; done

       [user@dom0 ~]# cp -r /tmp/qmenu_vm/ /lib/


Customization
-------------

### qmenu ###

The colors that correspond to a qube label can be adjusted by appending `--{LABEL}=#{HEX VALUE}`
for any qube label, when executing a qmenu tool.

Try the following example for visually appealing colors:

~~~
 --purple=#a020f0 --blue=#4363d8 --gray=#bebebe --green=#3cb44b --yellow=#ffe119 --orange=#f58231 --red=#e6194b --black=#414141
~~~

### dmenu ###

In order to customize and configure dmenu, instead of downloading it from the repositories,
you have to get it from [suckless](https://tools.suckless.org/dmenu/), compile it yourself
and [copy it to dom0](https://www.qubes-os.org/doc/copy-from-dom0/#copying-to-dom0).
**However, this way of installing software in dom0 is not advised and can compromise the security of your system!**
