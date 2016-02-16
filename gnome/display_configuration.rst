===========================
Gnome Display Configuration
===========================

How to keep display configuration at startup?
=============================================

Configure as you like display configuration in Gnome settings, and then::

    # cp /home/<username>/.config/monitors.xml /var/lib/gdm/.config/monitors.xml

Do not forget to change your username in the command.

Ressources
----------

    - https://wiki.archlinux.org/index.php/GDM#Rotate_login_screen
    - https://unix.stackexchange.com/a/35160/148748
