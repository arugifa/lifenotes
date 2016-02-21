==================================
Cups error: cannot print documents
==================================

One day, without any reason, I could no longer print documents. The error message returned by Gnome was very weird.

I decided to print a test page directly on the web interface of Cups. The following error message appeared::

    The PPD version (5.2.10) is not compatible with Gutenprint 5.2.11.

The solution was to modify the printer's driver, ``CUPS+Guttenprint v5.2.10`` by ``CPUS+Guttenprint v5.2.11``.
