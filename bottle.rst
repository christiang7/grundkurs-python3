.. _Bottle:

``Bottle`` -- Ein Mikro-Framework für interaktive Webseiten
===========================================================

Das `bottle <http://bottlepy.org/docs/dev/index.html>`__-Modul bietet eine
einfache Möglichkeit zum schnellen Erstellen von WSGI-basierten Webseiten ("Web
Server Gateway Interface"). Die eigentliche Anwendung kann dabei aus einer
einzigen Datei bestehen.

Das ``bottle``-Modul lässt sich unter Linux folgendermaßen installieren:

.. code-block:: sh

    sudo aptitude install python3-bottle

Alternativ hierzu kann man ``bottle`` auch, sofern man das Paket
``python3-setuptools`` via ``aptitude`` installiert hat, mittels ``pip3 install
bottle`` installieren.


.. _Hallo-Welt-Beispiel Bottle:

Ein "Hallo Welt"-Beispiel
-------------------------

Um mittels Bottle eine einfache Webanwendung zu programmieren, genügt es, das
gleichnamige ``bottle``-Paket oder einzelne Funktionen daraus in eine
Python-Datei zu importieren. Ein einfaches Code-Beispiel sieht somit etwa
folgendermaßen aus:

.. code-block:: python

    #!/usr/bin/env python3

    from bottle import route, debug, run

    @route('/hallo/<name>')
    def hallo(name):
        return 'Hallo {0}!'.format(name)

    debug(True)
    run()

Speichert man dieses Programm beispielsweise als Datei ``hallo-welt.py`` und
ruft es aus einer Shell heraus mittels ``python3 hallo-welt.py`` auf, so kann
man sich das Ergebnis im Webbrowser unter der Adresse
``http://localhost:8080/hallo/Welt`` anzeigen lassen. Gibt man in diesem 
Pfad einen anderen Namen als "Welt" an, so bekommt man im Webbrowser eine
entsprechend andere Begrüßung angezeigt.

Die Funktionsweise der Bottle-Anwendung liegt darin, einen Browserpfad über
die ``route()``-Funktion mit einer gewöhnlichen Python-Funktion zu verbinden.
Über die ``return``-Anweisung kann wahlweise ein einfacher Text im Browser
ausgegeben oder auch eine andere Funktion aufgerufen werden, die dann
beispielsweise ein HTML-Template rendert und mit Text füllt.

Die ``run()``-Funktion startet den von Bottle ohne weitere Abhängigkeiten
bereitgestellten WSGI-Server mit dem üblichen HTML-Standard-Port ``8080``; man
kann auch mittels beispielsweise ``run(port=8081)`` einen anderen Localhost-Port
vorgeben. Ruft man die Funktion ``run()`` mit der Option ``reloader=True`` auf,
so werden Änderungen unmittelbar, also auch ohne Neustart des WSGI-Servers
übernommen.


.. _HTML-Templates:

HTML-Templates
--------------

Möchte man nicht nur reinen Text im Webbrowser anzeigen, sondern eine Ausgabe in
HTML-Form erreichen, so kann wahlweise die im ``bottle``-Modul bereits
integrierte SimpleTemplate-Engine genutzt werden; als Alternative dazu können
auch `Jinja2 <http://jinja.pocoo.org/>`__, ` oder `Mako
<https://www.makotemplates.org/>`__ eingesetzt werden, welche mittels ``pip3``
und den gleichen Paketnamen nachinstalliert werden können (``pip3 install
Jinja2``).

... to be continued ...

.. Anfangsbuchstabe eines Wortes groß schreiben: 'zeichenkette'.title()

https://www.fullstackpython.com/wsgi-servers.html



.. Webserver: Auch Apache möglich oder `bjoern
.. <https://pypi.python.org/pypi/bjoern>`__

.. http://bottlepy.org/docs/dev/deployment.html

.. Funktionen für das Routing - sowohl statisches als auch dynamisches Routing wird unterstützt
.. Funktionen zum Auslesen von HTML-Formulardaten und dem HTML Header
.. Funktionen zum Upload von Dateien
.. Funktionen zum Generieren und Lesen von Cookies
.. generieren eines HTML Reponse

.. rubric:: Links:

* `Developing with Bottle (RealPython) <https://realpython.com/developing-with-bottle-part-1/>`__


