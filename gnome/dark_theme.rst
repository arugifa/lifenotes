How to enable Gnome Dark Theme?
===============================

Create a file ``~/.config/gtk-3.0/settings.ini`` and put the following lines inside::

    [Settings]
    gtk-application-prefer-dark-theme = true

Eventually, open a new session if necessary.


Ressources
----------

- https://wiki.archlinux.org/index.php/GTK%2B#Dark_theme_variant
- https://developer.gnome.org/gtk3/unstable/GtkSettings.html#GtkSettings--gtk-application-prefer-dark-theme
