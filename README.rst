
Grundkurs Python3
=================

In dieser Dokumentation aus der `Grund-Wissen <https://www.grund-wissen.de>`_
-Reihe geht es um eine Einführung in die Programmiersprache Python.

Das Projekt verwendet `Sphinx <https://sphinx-doc.org/>`_ als Programm zum
Erstellen der HTML- bzw. PDF-Dokumente. Die Dokumentation wird kontinuierlich
ausgebaut, Unterstützung bei der Weiterentwicklung ist gerne willkommen.

Eine HTML-Version dieser Seite ist unter folgender Adresse abrufbar:

https://www.grund-wissen.de/informatik/python/


Lokale Kopien und eigene Änderungen
-----------------------------------

Um lokale Kopien der Dokumentation zu erstellen, müssen einige Pakete
installiert sein. Unter Debian, Ubuntu oder Linux Mint können diese
folgendermaßen installiert werden:

.. code-block:: bash

    aptitude install python3-setuptools

	easy_install3 -U Sphinx

Anschließend kann das Repository heruntergeladen werden:

.. code-block:: bash

    git clone https://gitlab.com//grund-wissen/grundkurs-python3.git

Im Projektverzeichnis können dann aus den Quelldateien wahlweise HTML-Seiten
oder ein PDF-Dokument erstellt werden:

.. code-block:: bash

    # HTML-Seiten erzeugen:
    make html

    # PDF-Dokument erzeugen:
    make latexpdf

Die fertigen Dokumente befinden sich anschließend im Verzeichnis ``_build/html``
beziehungsweise ``_build/latex``.

Sollen eigene, lokale Änderungen an der Dokumentation in dieses Repository
übernommen werden, so wird um eine `Email
<https://www.grund-wissen.de/impressum.html#Email-Adresse>`__ oder einen
entsprechenden Merge-Request gebeten.


Herzlichen Dank an alle Mitwirkenden!


