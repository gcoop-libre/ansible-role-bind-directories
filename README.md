Bind Directories
================

An Ansible Role that bind directories using mount. The main objective of the role is to mount directories located on a data volume on the system directories.

For example if you want to locate MySQL data dir in another volume, and don't want to change the MySQL configuration, you can bind the directories with this role.

Requirements
------------

If you want to synchronize the directories content before mounting them, you need to install rsync on the host.

Role Variables
--------------

Available variables are listed below, along with default values (see `defaults/main.yml`):

    bind_directories_basedir: ''

If all the directories to mount would be inside the same directory, you can set it with this property. For example, all your directories may be subdirectories of `/data`, so you can set `bind_directories_basedir: /data` (without trailing slash).

Then, the role will concantenate the value to the `dir` property of every element of the `bind_directories` list.

    bind_directories:
      - dir: /data/home
        mount_point: /home
        owner: root
        group: root
        mode: '0755'
        create: True
        create_mount_point: False
        sync: False
        mount: True
        services:
          - mysqld
          - apache2

The list of directories to bind. Every directory has at least the `dir` property and the `mount_point`. You can specify the owner user, the group and the filemode that would be set to the directory on creation.

The next four properties allow you to define if you want to `create` the directory (`dir` property), `create` the `mount_point`, `sync`hronize the content and, finally, `mount` the directory and persist the mount on /etc/fstab.

After the directories are synchronized, an empty file `.synchronized` will be created inside the directory. If this flag exists, the directory will not be synchronized, even if you set `sync: True` for it.

The last property, `services`, allow the definition of a list a services which will be stopped before binding the directory and restarted after it.

Dependencies
------------

None.

Example Playbook
----------------

    - hosts: servers
      vars_files:
        - vars/main.yml
      roles:
        - gcoop-libre.bind-directories

*Inside `vars/main.yml`*:

    bind_directories:
      - dir: /data/home
        mount_point: /home
        owner: root
        group: root
        mode: '0755'
        create: True
        sync: True
        mount: True

License
-------

GPLv2

Author Information
------------------

This role was created in 2016 by [gcoop Cooperativa de Software Libre](http://gcoop.coop).
