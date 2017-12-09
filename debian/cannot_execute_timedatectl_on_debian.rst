====================================
Cannot execute timedatectl on Debian
====================================

Error
=====

When trying to execute ``timedatectl``, for example::

    # timedatectl set-ntp true
    Failed to create bus connection: No such file or directory


Solution
========

Check if the Systemd NTP service is started::

    # systemctl status systemd-timesyncd.service
    ● systemd-timesyncd.service - Network Time Synchronization
       Loaded: loaded (/lib/systemd/system/systemd-timesyncd.service; disabled)
       Active: inactive (dead)
         Docs: man:systemd-timesyncd.service(8)

Otherwise, just do it::

    # systemctl enable systemd-timesyncd.service
    Created symlink from /etc/systemd/system/sysinit.target.wants/systemd-timesyncd.service to /lib/systemd/system/systemd-timesyncd.service.
    # systemctl start systemd-timesyncd.service


Ressources
==========

- https://github.com/helmuthdu/aui/commit/79bba7728eb37051f2fee83ec20aace007c59429
