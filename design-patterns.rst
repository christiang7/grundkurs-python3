.. _Design Patterns:

Design Patterns
===============

An dieser Stelle sollen einige hilfreiche Entwurfsmuster ("Design Patterns") und
ihre Implementierungen in Python3 vorgestellt werden. [#]_ Derartige
Entwicklungsmuster kann man als "Entwicklungshilfen" ansehen, da sie bewährte
Code-Strukturen für bestimmte Problem-Typen bieten.bestimmte Problem-Typen
bieten.


.. beschreibt ein erprobtes Lösungskonzept

.. _Erzeugungsmuster:

Erzeugungsmuster
----------------

Als Erzeugungsmuster ("Creational Patterns") werden Entwurfsprinzipien
bezeichnet, die für das Erstellen neuer Objekte hilfreich sein können.

.. Abstract Factory, Builder, Singleton, ...

Factory
^^^^^^^

.. {{{

Als "Factories" werden Hilfsobjekte bezeichnet, deren Aufgabe es ist, die
Erzeugung eines Objekts von seiner Verwendung zu trennen. Man unterscheidet im
Allgemeinen zwischen der "Factory Method" und der "Factory Class" als zwei
verschiedenen Möglichkeiten, dieses Prinzip zu implementieren.


.. _Factory Method:

.. rubric:: Factory Method

Bei einer "Factory Method" wird ein neues Objekt durch den Aufruf einer
Funktion erzeugt und von dieser als Ergebnis zurückgegeben. Die erzeugende
Funktion erstellt das neue Objekt dabei in Abhängigkeit vom Kontext.
Beispielsweise kann nach diesem Prinzip eine Funktion ``read_file()`` ein
Datei-Reader-Objekt in Abhängigkeit vom Dateityp bzw. der Datei-Endung des
angegebenen Pfads generieren:

.. code-block:: python

    # Factory-Method-Beispiel

    class CSV_Reader():

        def __init__(self, path):
            pass

    def file_reader(path):

        # Todo: Check if file exists

        if path.endswith(".csv"):
            return CSV_READER(path)
        else:
            return None

    csv_reader = file_reader('test.csv')

Soll durch eine Factory Method eine direkte Instanziierung einer Klasse
verhindert werden, so kann die Definition dieser Klasse auch innerhalb der
Funktionsdefinition erfolgen.

.. Die generierende Funktion kann selbstverständlich auch als eine Methode einer
.. "Factory Class" implementiert werden. Eine solche Klasse kann wiederum mehrere
.. verschiedene Factory Methods beinhalten und somit die Generierung von mehreren
.. Objekten bündeln. (Eine echte Firma erzeugt auch meist mehr als ein Produkt.)

.. _Factory Class:

.. rubric:: Factory Class

Bei einer "Factory Class" wird eine Factory-Methode (oder mehrere davon) zu
einer :ref:`Klasse <Klasse>` zusammengefasst. Bei Verwendung dieses Patterns
kann man beispielsweise eine Klasse names ConnectionCreator mit einer Methode
``build_connection(connection_type)`` erstellen, die je nach angegebenem
Protokoll-Typ eine SSH- oder FTP-Verbindung zu einem Webserver aufbaut.
Strukturell könnte der Code dann folgendermaßen aussehen:

.. code-block:: python

    # Factory-Class-Beispiel

    # Connection-Klassen:

    class SSH_Connection():

        def __init__(self, path):
            pass

    class FTP_Connection():

        def __init__(self, path):
            pass


    # Factory-Klasse:

    class ConnectionCreator():

        def __init__(self, path):
            self.path == path

        def build_connection(self, connection_type, path):

            if connection_type == "HTML":
                return HTML_Connection(path)

            elif connection_type == "SSH":
                return SSH_Connection(path)

            else:
                  return None

Nach dem gleichen Prinzip denkbar wäre beispielsweise auch eine
DatabaseConnection-Klasse, die eine Verbindung zu einer bestehenden Datenbank
herstellen kann, oder gegebenenfalls auch eine neue Datenbank anlegen kann.


.. _Abstract Factory:

.. rubric:: Abstract Factory

In der Programmierung werden bisweilen "abstrakte" Klassen definiert. Diese
geben zwar bereits strukturelle Prinzipien vor, es können allerdings noch keine
Instanzen einer solchen Klasse erzeugt werden, da konkrete Ausprägungen noch
nicht festgelegt sind. Beispielsweise könnte eine abstrakte Klasse ein
"Kraftfahrzeug" sein, das ganz allgemein Methoden wie "Motor starten" oder
"Bremse betätigen" bereit stellt. Jeder reelle Kraftfahrzeug-Typ, der auf dieser
Klasse via :ref:`Vererbung <Vererbung>` aufbaut, implementiert diese Funktionen,
allerdings konkretisiert auf die konkrete Ausprägung.

Bei Verwendung einer "Abstract Factory" wird entsprechend ein Factory-Typ mit
strukturellen Prinzipien vorgegeben, aus dem wiederum konkrete Factory-Klassen
(mittels Vererbung) hervorgehen können. [#]_ Dieses Pattern kann beispielsweise
für ein Computer-Strategiespiel wie `0.A.D
<https://wiki.ubuntuusers.de/Spiele/0_A.D./>`__ genutzt werden, so dass
"Gebäude-Typ" je nach Kultur und Entwicklungsstufe zwar ähnliche, aber nicht
komplett identische Objekte generieren kann.

.. Factories erzeugen nach ihrem Grundprinzip fertige Objekte oftmals "aus einem
.. Guss". Soll ein ein Objekt aus einzelnen Teilen erzeugt werden, so kann
.. auch die Verwendung des folgenden "Builder"-Patterns nützlich sein.

.. todo abc-Modul mit Vorlagen für abstrakte Klassen...

.. _Builder:

.. }}}

Builder
^^^^^^^

.. {{{

Das "Builder"-Pattern kann verwendet werden, wenn ein Objekt schrittweise aus
einzelnen Komponenten zusammengestellt werden muss. Die einzelnen Komponenten
werden dabei durch Factory-Methoden einer (oder mehrerer) "Builder"-Klassen
erzeugt. Die Builder-Methoden werden wiederum von einem "Director"-Objekt in der
gewünschten Reihenfolge aufgerufen.

Das gewünschte Objekt als Ganzes wird also über den Direktor in Auftrag gegeben,
der die Anfrage an den passenden Builder weiter reicht. Ist das Objekt erstellt,
kann der Director es wiederum beim Builder abholen und als Ergebnis zurückgeben.
Während die einzelnen Builder wiederum "Factories" darstellen, ist der Director
ein steuerndes Element, das kontext-bezogen den relevanten Builder auswählt und
gewissermaßen "nach Rezept" nacheinander dessen Methoden aktiviert.

.. TODO: Beispiel

.. _Prototyp:
.. _Prototype:

.. }}}

Prototype
^^^^^^^^^

.. {{{

.. Das Entwurfsmuster Prototyp verwendet ein Objekt als Vorlage (Prototyp), um
.. daraus weitere Objekte zu erzeugen, die anschließend modifiziert werden können.

Mittels eines "Prototyps" kann ein neues Objekt erstellt werden, indem ein
bestehendes Objekt als Startpunkt verwendet wird. Um einen Prototypen zu
erzeugen, muss also zunächst eine exakte Kopie eines bestehenden Objekts erzeugt
werden.

In Python ist dies einfach mittels der Funktion ``deepcopy()`` aus dem Paket
``copy`` der Standard-Library möglich.

.. _Singleton:

.. }}}

Singleton
^^^^^^^^^

.. {{{

Als Singleton bezeichnet man ein Objekt, das innerhalb eines laufenden Programms
nur in einer Ausprägung ("Instanz") existieren darf; beispielsweise ist bei
jedem Betriebsystem mit grafischer Oberfläche genau ein Window-Manager in
Betrieb. Zugleich muss das Singleton-Objekt unter Umständen für viele
Stellen zugriffsbereit sein.

Singletons stellen also eine Art von klar definierten "Access Points" dar, auf
die von mehreren Clienten aus zugegriffen werden kann. Ein solches Objekt könnte
zwar prinzipiell auch mittels einer globalen Variable initiiert werden, jedoch
könnten dabei immer noch mehrere Instanzen des Objekts existieren -- man hätte
dann zwar das gleiche, aber nicht das selbe Objekt. Zudem soll die Klasse des
Grundobjekts durch die Erstellung von Unterklassen erweiterbar sein.

.. rubric:: Singleton-Klasse

In Python kann eine Singleton-Klasse folgendermaßen als Klasse implementiert
werden:

.. code-block:: python

    class Singleton(object):
        def __new__(cls):
            if not hasattr(cls, 'instance'):
                cls.instance = super().__new__(cls)
            return cls.instance

Wird ein solches Objekt initiiert, so wird es nur dann eine neue Instanz des
Objekts erzeugt, falls noch keine solche existiert; andernfalls gibt die
Initiierung die bereits existierende Instanz als Ergebnis zurück. Auf diese
Weise kann man von beliebiger Stelle aus auf das Singleton zugreifen, indem
man eine neue Instanz des Singletons erzeugt:

.. code-block:: python

    # Ein neues Singleton erzeugen:
    singleton_1 = Singleton()

    # Das existierende Singleton an anderer Stelle nutzen:
    singleton_2 = Singleton()


Jedes Objekt, das ein Singleton darstellen soll, kann damit der obigen
Implementierung als Unterklasse eines Singletons definiert werden:

.. code-block:: python

    class Any_Singleton_Object(Singleton):
        """
        A Class for a Singleton Object.
        """

        # Class methods and attributes..

Bei der Initiierung eines solchen Objekts wird aufgrund der geerbten
``__new__()``-Funktion nur dann ein neues Objekt (mit allen
"Standardeinstellungen") erstellt, falls ein solches noch nicht existiert.
Ansonsten wird dieses mit all seinen Methoden und Attributen genutzt.

.. _Singleton-Module:

.. rubric:: Singleton-Module

Die Initiierung eines Objekts ist stets mit etwas Rechenaufwand verbunden. Soll
auf ein Singleton häufig und möglichst schnell zugegriffen werden und ist
keine Aufgliederung des Singletons in mehrere mögliche Unterklassen nötig, so
kann anstatt der oben beschriebenen Klasse auch ein Singleton-Modul erzeugt
werden. Dieses Modul, das den Namen des Singletons (in Kleinbuchstaben) als
Dateinamen (mit Endung ``.py``) trägt, bekommt "Methoden" als Funktionen und
"Attribute" als Variablen auf Modulebene zugewiesen -- d.h. in diesem Modul
werden keine Klassen angelegt.

Da Module nach erstmaligem Importieren durch ``import modulname`` stets nur in
Form einer Referenz genutzt werden, kann auf die gewünschten Singleton-Methoden
unmittelbar mittels ``modul.funktionsname()`` und die gewünschten Attribute
mittels ``modul.variable`` zugegriffen werden.

.. .. rubric:: Multiton

.. https://de.wikipedia.org/wiki/Multiton

.. Variante des Singleton-Musters, das die Anzahl erzeugter Objekte kontrolliert,
.. beispielsweise um die Anzahl gleichzeitig geöffneter Datenbankverbindungen auf
.. einen für eine gute Gesamtleistung erforderlichen Wert zu reduzieren.

.. Um auf das richtige Objekt zuzugreifen, wird ein eindeutiger Schlüssel
.. verwendet. Die Objekte und deren Schlüssel werden meist als assoziatives
.. Datenfeld in Form von Schlüssel und Werten umgesetzt, die über eine statische
.. Methode auf Wunsch geliefert werden. So gibt es immer für jeden Schlüssel
.. höchstens ein Objekt. Wird ein Schlüssel angegeben, für das das Objekt nicht
.. existiert, wird das benötigte Objekt erzeugt und zur Verfügung gestellt.
.. Dadurch ist ein Multiton eigentlich nichts anderes als eine Gruppe von
.. Einzelstücken.

.. }}}


.. _Strukturmuster:

Strukturmuster
--------------

Als Strukturmuster ("Structural Patterns") werden Entwurfsprinzipien bezeichnet,
die für das Zusammenwirken mehrerer Objekte im Programm nützlich sein können.

.. Composite, Adapter, Decorator, Flyweight, Proxy, . . .

.. _Adapter:

Adapter
^^^^^^^


.. _Komposition:
.. _Composite:

Composite
^^^^^^^^^

.. definiert Teil-Ganzes-Hierarchien von primitiven und zusammengesetzten Objekten
.. Hierarchie leicht durch neue Objekte erweiterbar, ohne dass Benutzer ihren Code ändern müssen

.. * Interface für die Objekte in der Komposition (z.B. Auswerten bei
..   arithmetischen Ausdrücken)
.. * Interface für gemeinsames Verhalten (z.B. Verschieben bei geometrischen
..   Figuren)
.. * Interface für den Zugriff auf die Teilkomponenten.

.. _Fasade:
.. _Facade:

Facade
^^^^^^

Ein Facade-Pattern kann genutzt werden, um einem Benutzer ein einfaches,
intuitiv nutzbares Interface zu bieten, so dass sich dieser nicht mit den
Schnittstellen der einzelnen Klassen eines Programms auseinander setzen muss. In
einer solchen "Fassade" eines Programms sollen also keine neuen Funktionen hinzu
kommen, es soll vielmehr der Zugriff auf die eigentlichen Programm-Funktionen
erleichtert werden.

.. Wrapper!

.. Beispielsweise stellt ein Betriebsystem ein Interface für die eigentlichen
.. Funktionen eines Mainboards bereit, oder eine graphische Bedienoberfläche
.. ein Interface auf die darunter liegende Shell.

.. In einem Auto stellen beispielsweise die Bremspedale eine solche
.. Schnittstelle zum eigentlichen Bremssystem dar; der Benutzer kann sie
.. bedienen, ohne sich mit den internen Details beschäftigen zu müssen; das
.. Pedal verschließt den Zugriff zum inneren System gleichzeitig nicht.

.. Die Fassade *nutzt* die einzelnen Klassen und/oder Module eines Programms, so
.. bündelt also ihre Funktionen
.. Beim Dekompositions-Prinzip zerlegt man ein Programm in kleinere Teile, um
.. eine Aufgabe leichter lösbar zu machen; die einzelnen Sub-Programme sollten
.. möglichst unabhängig voneinander funktionieren, so dass eine Änderung im Modul A
.. möglichst keine negative Auswirkung auf das Modul B hat (das vielleicht sogar von einer
.. anderen Person entwickelt wird). Anstelle einen direkten Informationsaustausch
.. zwischen Modulen zuzulassen, kann man diesen auch über die Fassade als
.. Schnittstelle ablaufen lassen; so bleiben die einzelnen Module unabhängig
.. voneinander.

.. Kabelsalat oder Verfilzung als anschauliches Beispiel.

.. Auch externer Code kann so leichter auf die Funktionen eines Programms als
.. "Library" zugreifen.

.. Vorteile: 
.. Lose Bindung zwischen Client und Subsystem (keine Verfilzung)
.. *Eine* Schnittstelle zum Subsystem anstelle mehrerer einzelner
.. Das Subsystem wird aus Sicht des Entwicklers flexibler, aus Sicht des
.. Anwenders einfacher nutzbar


.. _Model-View-Controller:

Model-View-Controller
^^^^^^^^^^^^^^^^^^^^^

Das Prinzip "Model-View-Controller" soll dabei helfen, die eigentliche Logik des
Programms (das "Modell") von der Datenausgabe (dem "View") zu trennen.

Der "Controller" vermittelt als Schnittstelle zwischen diesen Ebenen:

* Er soll die Eingabe des Benutzers, die ebenfalls in der View-Ebene
  erfolgt, entgegennehmen und an das Modell weiterleiten.

* Er soll, nachdem die Eingabe in der Modell-Ebene verarbeitet wurde, die
  resultierende Ausgabe wieder an die View-Ebene weiterleiten.

| Der Controller wirkt wie ein "Kleber" zwischen der Modell- und der
  View-Schicht; man sagt entsprechend, der Kontroller solle "dünn" sein, also nur
  die für die Vermittlung absolut nötigen Funktionen beinhalten. 
| Die Modell-Ebene hingegen soll "schlau" sein, an dieser Stelle sollte also die
  eigentliche Programm-Logik liegen. 
| Die View-Ebene wiederum soll "dumm" sein, also nur die zur Entgegennahme des
  Inputs und zur Darstellung der Ausgabe nötigen Funktionen (und keine weitere
  Logik) beinhalten; beispielsweise sollte aus der View-Ebene heraus kein
  Zugriff auf eine Datenbank erfolgen.

Der Vorteil dieses Entwicklungs-Musters liegt darin, dass das eigentliche
Programm von der Benutzeroberfläche abstrahiert wird. Etliche Linux-Programme,
aber beispielsweise auch den Python-Interpreter :ref:`Ipython <Ipython>`, gibt
es dadurch sowohl als text-basierte Anwendungen für die Shell wie auch als
Variante mit einer eigenen graphischen Bedienoberfläche.

.. Kivy?
.. Bottle

.. _Verhaltensmuster:

Verhaltensmuster
----------------

.. Als Strukturmuster ("Behavioral Patterns") werden Entwurfsprinzipien bezeichnet

.. Iterator, Observer, Visitor, Interpreter, ...

Memento
^^^^^^^

* `Memento (Wikipedia, de.)
  <https://de.wikipedia.org/wiki/Memento_(Entwurfsmuster)>`__


.. Das Memento-Muster hat die Akteure Originator und Memento. Der Originator ist
.. ein Objekt mit einem internen Zustand, der verändert werden kann. Im Memento
.. kann dieser Zustand abgespeichert werden, um zu einem späteren Zeitpunkt
.. wiederhergestellt zu werden.


.. _Observer:

Observer
^^^^^^^^

Das Observer-Muster besteht darin, dass ein bestimmtes Objekt als "Subjekt"
deklariert wird, dass von anderen Objekten "beobachtet" wird. Das Subjekt
verwaltet dabei eine Liste aller Objekte, die es beobachten. Ändert sich eine
bestimmte Eigenschaft des Subjekts, dann benachrichtigt es darüber alle
Beobachter-Objekte, indem es eine deren Methoden aufruft.

.. wichtig für Event-gesteuerte Software und GUIs mit einer Model-View-Controller-Struktur

Visitor
^^^^^^^

... to be continued ...

.. Probleme der Datenrepräsentation eines Composite-Patterns:

.. * Benutzer kann keine neue Operation selbst definieren und zur Hierarchie hinzufügen.
.. * Quelltext der operation() über mehrere Klassen verstreut.

.. Das Visitor-Pattern löst diese Probleme:

.. * Es erlaubt, neue Operationen zu definieren, ohne die Klassen der Struktur zu
..   ändern.
.. * Es erlaubt, Operationen, die auf einer Objektstruktur ausgeführt werden
..   sollen, kompakt zu repräsentieren.


Links
-----


.. raw:: html

    <hr />

.. only:: html

    .. rubric:: Anmerkungen:

.. [#] Eine allgemeine, nicht Python-spezifische Übersicht über Design Patterns
    gibt es unter anderem auf `Wikipedia
    <https://de.wikipedia.org/wiki/Design_Patterns>`__.

.. [#] Konkrete Factories können konkrete Objekte generieren, abstrakte
    Faktories hingegen nicht. Zu den konkreten objekten können zwar separat
    abstrakte Klassen definiert werden, eine abstrakte Factory kann allerdings
    nicht einmal diese generieren, da sie selbst eine abstrakte Klasse
    darstellt: Eine Abstract Factory kann nicht instanziiert werden, sie gibt
    also nur den strukturellen Aufbau einer konkreten Factory vor.


