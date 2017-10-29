===============================
How to fill and sign a PDF form
===============================

Requirements
============

You first need to install:

- Flpsed,
- Imagemagick,
- Inkscape,
- Poppler.


Workflow
========

Fill the Form
-------------

To fill the PDF form, open it with **Flpsed**::

    $ flpsed <PDF_FORM>.pdf

Then, just click somewhere on the page and type text.
*Tip: you can use arrow keys to place exactly the text zone where you want.*

After you finished to complete the form, export it in PDF in **Flpsed**.
**Be aware that exporting and saving are different actions.**
The latter create files wich are not compatible with **Inkscape**.


Sign it
-------

- Open the completed PDF form into **Inkscape**,
- Then select the page where you have to put your signature,
- In the import settings, check the box *"import via Poppler"* and validate,
- Once the page is displayed, choose to *"Draw calligraphic or brush strokes"* (Ctrl+F6),
- You can now sign the form and save a copy in PDF with default settings.

You have now your complete form unsigned, and the last page of the form which is signed. To join them, you have first to split your PDF form: repeat the import and save process with **Inkscape** for all pages of the form (exept the one already signed). Finally, use **pdfunite** to join the pages::

    $ pdfunite [<form_page_1>.pdf <form_page_2>.pdf ...] <completed_form>.pdf


Sign scanned form
-----------------

If you want to sign a scanned form, the process is different.
Start by converting all pages of your form in PNG::

    $ pdftocairo -png <PDF_FORM>.pdf

Then, open the images one by one with **Inkscape**.
Sign or initial pages by *"Drawing calligraphic or brush strokes"* (Ctrl+F6).
And do not forget to save your changes!


Ressources
----------

- http://www.cyberciti.biz/tips/open-source-linux-pdf-writer.html
- https://wiki.archlinux.org/index.php/PDF_forms
- http://stackoverflow.com/questions/2507766/merge-convert-multiple-pdf-files-into-one-pdf
