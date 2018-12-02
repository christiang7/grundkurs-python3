
Django
======

Django ist ein Framework zum Erstellen dynamischer Webseiten. Es bietet viele
vordefinierte Funktionen (inklusive eines Test-Webservers), so dass einfache
Seiten verhältnismäßig schnell erstellt werden können. Durch die einfach zu
handhabende Verbindung zu einem Datenbank-System lassen sich auch komplexe
Webseiten mit Django verhältnismäßig übersichtlich erstellen.


Installation
------------

Um auch ohne SuperUser-Rechte beziehungsweise innerhalb des normalen
Home-Verzeichnisses Webseiten entwickeln und testen zu können, ist für Django
die Verwendung von virtuellen Umgebungen sinnvoll. Eine solche kann
für Django folgendermaßen eingerichtet werden:

.. code-block:: sh

    # Django Code-Verzeichnis erstellen:
    mkdir ~/code/django && cd ~/code/django

    # In diesem Verzeichnis eine virtuelle Python-Umgebung
    # mit der Bezeichnung "env" installieren:
    virtualenv -p python3 --no-site-packages env

    # Virtuelle Umgebung aktivieren:
    source env/bin/activate

Arbeitet man mit der :ref:`Z-Shell <Z-Shell>`, so wird (je nach Theme)
beispielsweise durch eine Anzeige von ``(env)`` vor dem Eingabe-Prompt
angezeigt, dass diese virtuelle Umgebung aktiv ist. Durch eine Eingabe von
``deactivate`` oder eines Schließen des Shell-Fensters wird die virtuelle
Umgebung wieder deaktiviert, durch eine erneute Eingabe von ``source
env/bin/activate`` kann sie wieder geladen werden.

In der virtuellen Umgebung kann man mittels des Python-Paketmanagers :ref:`pip
<pip>` innerhalb dieser Umgebung auch ohne SuperUser-Rechte und ohne
Auswirkungen auf andere Projekte Pakete installieren:

.. code-block:: sh

    pip3 install django

Damit wird das Django-Basis-Paket installiert; für dieses gibt es je nach
Anwendungszweck zahlreiche Erweiterungen (in Django "Apps" genannt), die
zusätzlich in gleicher Weise installiert werden können.


Links
-----

* `Effective Django Tutorial (en.) <http://www.effectivedjango.com/tutorial/index.html>`__

.. https://modwsgi.readthedocs.io/en/develop/user-guides/quick-configuration-guide.html

