===============================
How to fill and sign a PDF form
===============================

Start by installing **flpsed** and **Inkscape**.

Then, you can open your PDF form::

    $ flpsed <pdf_form>.pdf

To add text, just click somewhere on the page and type text.
*Tip: you can use arrow keys to place exactly the text zone where you want.*

After you finished to complete the form, export it in PDF. Be aware that exporting and saving in PDF are different actions in **flpsed**. The latter create files wich are not compatible with **Inkscape**.

To add a signature:

- open the completed PDF form into **Inkscape**,
- then select the page where you have to put your signature,
- in the import settings, check the box *"import via Poppler"* and validate,
- once the page is displayed, choose to *"Draw calligraphic or brush strokes"* (Ctrl+F6),
- you can now sign the form and save a copy in PDF with default settings.

You have now your complete form unsigned, and the last page of the form which is signed. To join them, you have first to split your PDF form: repeat the import and save process with **Inkscape** for all pages of the form (exept the one already signed). Finally, use **pdfunite** (provided by **poppler**, which is used by **Inkscape**) to join the pages::

    $ pdfunite [<form_page_1>.pdf <form_page_2>.pdf ...] <completed_form>.pdf


Ressources
----------

- http://www.cyberciti.biz/tips/open-source-linux-pdf-writer.html
- https://wiki.archlinux.org/index.php/PDF_forms
- http://stackoverflow.com/questions/2507766/merge-convert-multiple-pdf-files-into-one-pdf
