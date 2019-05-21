Ansible Role: autologin
=======================
[![Build Status](https://travis-ci.org/ntd/ansible-role-autologin.svg?branch=master)](https://travis-ci.org/ntd/ansible-role-autologin)

Create a new user and configure the system to log him in automatically,
either in textual or graphical mode. No display manager is required: all
is done at terminal level.

This is useful in many cases, specifically when precise control over the
running software is required. Examples include industrial PC bound to
specific machineries or kiosk systems.

The idea of leveraging `getty` has been borrowed from the
[ArchLinux wiki](https://wiki.archlinux.org/index.php/Getty#Automatic_login_to_virtual_console).

Role Variables
--------------

Available variables are listed below, along with default values (see
`defaults/main.yml` for up to date info):

    autologin_user:
      name: "operator"
      group: "users"
      groups:
      comment:

The user to log automatically in: `name` and `group` are mandatories.
You **must** specify the user.

    autologin_startup: "startx"

The command to execute after login. This line will be appended to the
`.profile` script of the above user. By default, the X server is called.

    autologin_xinitrc: ""

Eventual `.xinitrc` file to upload into the user home folder.

    autologin_override: "/etc/systemd/system/getty@tty1.service.d"

The systemd directory for overriding the tty service. A new file, called
`override.conf` will be created inside that directory that will change
the getty behavior.

Example Playbook
----------------

Automatically start `myprogram` in text mode as user `ntd`:

    - hosts: machinery
      roles:
      - role: ntd.autologin
        autologin_user:
            name:  ntd
            group: users
        autologin_startup: myprogram

Install a custom `.xinitrc` file and start in graphic mode. The
`myxinitrc` script must be already present in your `files/` folder.

    - hosts: kiosk
      roles:
      - role: ntd.autologin
        autologin_user:
            name:    ntd
            group:   users
            groups:  dialout
            comment: Nicola Fontana
        autologin_xinitrc: myxinitrc

License
-------

MIT

Author Information
------------------

This role was created in 2019 by Nicola Fontana (ntd@entidi.it).
