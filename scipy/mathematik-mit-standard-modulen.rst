
.. _Mathematik mit Standard-Modulen:

Mathematik mit Standard-Modulen
===============================

In jeder Python-Installation ist das :ref:`Standard-Modul <Standard-Modul>`
:ref:`math <math>` enthalten. Es enthält zahlreiche Funktionen, die aus dem
Python-Interpreter einen umfangreichen und programmierbaren Taschenrechner
machen.

Das ``math``-Modul wird meistens in folgender Form eingebunden:

.. code-block:: python

    import math as m

Anschließend kann so das ``math``-Modul unter der Bezeichnung ``m`` angesprochen
werden; dies spart Tipp-Arbeit. Als einzige (verkraftbare) Einschränkung hat
dies zur Folge, dass keine andere Variable mit ``m`` benannt werden darf.

Das ``math``-Modul enthält unter anderem folgende Funktionen, die jeweils auf
einzelne Zahlenwerte angewendet werden können:

.. list-table::
    :name: tab-math-modul
    :widths: 50 50

    * - ``m.pow(zahl,n)``
      - :math:`n`-te Potenz einer Zahl (oder :math:`n`-te Wurzel wenn :math:`0 <
        n < 1`)
    * - ``m.sqrt(zahl)``
      - Quadrat-Wurzel einer positiven Zahl
    * - ``m.exp(x)``
      - :ref:`Exponentialfunktion <gwm:Exponentialfunktionen>` :math:`e^{x}` mit
        Exponent :math:`x`
    * - ``m.log(x, a)``
      - :ref:`Logarithmusfunktion <gwm:Logarithmusfunktion>`
        :math:`\log_{a}{(x)}` von :math:`x` zur Basis :math:`a`
    * - ``m.radians(winkelwert)``
      - Winkelumrechnung von Grad auf :ref:`Radians <Bogenmaß>`
    * - ``m.degrees(winkelwert)``
      - Winkelumrechnung von Radians auf Grad
    * - ``m.sin(x)``, ``m.cos(x)`` und ``m.tan(x)``
      - Trigonometrische Funktionen für :math:`x`-Werte in Radians
    * - ``m.asin(x)``, ``m.acos(x)`` und ``m.atan(x)``
      - Umkehrfunktionen der trigonometrischen Funktionen (Ergebnisse in Radians)
    * - ``math.floor(zahl)``
      - Nächst kleinerer ``int``-Wert :math:`n \le q` einer Zahl :math:`q`
    * - ``math.ceil(zahl)``
      - Nächst größerer ``int``-Wert :math:`n \ge q` einer Zahl :math:`q`

Zudem sind im ``math``-Modul die Konstanten ``m.pi`` und ``m.e`` für die
Kreiszahl :math:`\pi \approx 3,14` beziehungsweise die Eulersche Zahl :math:`e
\approx 2,71` vordefiniert.

Neben dem ``math``-Modul existieren weiter für mathematische Zwecke nützliche
Module, beispielsweise :ref:`cmath <cmath>` für das Rechnen mit :ref:`komplexen
Zahlen <gwm:Komplexe Zahlen>` oder ``functools`` für das Anwenden von Operatoren
und/oder Funktionen auf eine ganze Liste von Zahlen. Im folgenden werden einige
Anwendungsbeispiele vorgestellt, die sich an den :ref:`Aufgaben zur Arithmetik
<gwm:Aufgaben zur Arithmetik>` orientieren.


.. _Primfaktor-Zerlegung:

.. rubric:: Primfaktor-Zerlegung

Um die Primfaktoren einer ganzen Zahl :math:`n>0` zu bestimmen, kann man
folgende Funktion nutzen (Quelle: `StackOverflow
<https://stackoverflow.com/questions/14550794/python-integer-factorization-into-primes>`__):

.. code-block:: python

    def prim(n):
        '''Calculates all prime factors of the given integer.'''

        from math import sqrt

        pfactors = []
        limit = int(sqrt(n)) + 1
        check = 2
        num   = n

        if n == 1:
            return [1]

        for check in range(2, limit):
            while num % check == 0:
                pfactors.append(check)
                num /= check

        if num > 1:
            pfactors.append(num)

        return pfactors

Die Grund-Idee von diesem Algorithmus liegt darin, dass eine Zahl keinen
Primzahl-Faktor haben kann, der größer ist als die Quadratwurzel dieser Zahl.
Für :math:`n=100` ist beispielsweise :math:`\sqrt{100} = 10` der größtmögliche
Faktor, der eine Primzahl sein könnte.

* Andere Produkte mit größeren Faktoren :math:`100 = 50 \cdot 2` lassen sich
  zwar ebenfalls bilden, enthalten dann allerdings stets einen Faktor, der
  kleiner als die Quadrat-Wurzel der Original-Zahl ist.

* Die einzelnen möglichen Primfaktoren lassen sich finden, indem man
  nacheinander die Faktoren :math:`2,\,3\, \ldots` in aufsteigender Reihenfolge
  durchprobiert. Um eine mögliche Mehrfachheit einzelner Faktoren nicht außer
  Acht zu lassen, muss bei jedem gefundenen Faktor geprüft werden, ob sich die
  Original-Zahl gegebenenfalls auch mehrfach durch diesen dividieren lässt.

* Ist ein Faktor gefunden, so kann man die Original-Zahl durch diesen dividieren
  und den Algorithmus erneut mit dem resultierenden Divisions-Rest fortsetzen.

Die gefundenen Faktoren sind tatsächlich allesamt Primzahlen: Jeder andere
Faktor ließe sich selbst als Produkt jeweils kleinerer Primzahlen darstellen.
Die Teilbarkeit durch diese Zahlen wurde im Lauf des Algorithmus allerdings
bereits geprüft; der jeweilige Divisionsrest, mit dem der Algorithmus fortfährt,
enthält diese Faktoren nicht mehr.

Mittels der obigen Funktion kann nun die Primzahl einer Zahl oder eines
Zahlenbereichs folgendermaßen berechnet werden:

.. code-block:: python

    # Einzelne Zahl in Primfaktoren zerlegen:
    prim(2017)

    # Zahlenbereich in Primfaktoren zerlegen:
    for n in range(1,1000):
            print(prim(n))


.. index:: Euklid-Algorithmus
.. _Euklid-Algorithmus:
.. _Größter gemeinsamer Teiler und kleinstes gemeinsames Vielfaches:

.. rubric:: Größter gemeinsamer Teiler und kleinstes gemeinsames Vielfaches

Beim Rechnen mit rationalen Zahlen, insbesondere beim Kürzen eines Bruchterms
oder beim Multiplizieren zweier Brüche, ist es nützlich, den :ref:`größten
gemeinsamen Teiler <gwm:Multiplikation und Division von Brüchen>`  zweier Zahlen
zu finden. Hierzu wird bis heute ein Algorithmus verwendet, den bereits `Euklid
<https://de.wikipedia.org/wiki/Euklid>`_ in der Antike entdeckt hat:

* Hat man zwei Zahlen :math:`a` und :math:`b` mit :math:`a>b`, so ist der größte
  gemeinsame Teiler von :math:`a` und :math:`b` identisch mit dem größten
  gemeinsamen Teiler von :math:`(a-b)` und :math:`b`. Man kann also wiederholt
  immer wieder die kleinere Zahl von der größeren abziehen und das Prinzip
  erneut anwenden, solange bis man beim größten gemeinsamen Teiler
  angekommen ist.

  Ist beispielsweise :math:`a=72` und :math:`b=45`, so ist der größte gemeinsame
  Teiler dieser beider Zahlen identisch mit dem der Zahlen :math:`45` und
  :math:`(72-45) = 27`. erneut kann man die Differenz beider Zahlen bilden und
  erhält damit das Zahlenpaar :math:`27` und :math:`(45-27)=18`; ein
  wiederholtes Anwenden des gleichen Prinzips liefert das zunächst das
  Zahlenpaar :math:`18` und :math:`(27-18) = 9` und schließlich :math:`9` und
  :math:`9`; der größte gemeinsame Teiler beider Zahlen ist somit :math:`9`.

* Ist die eine Zahl :math:`a` wesentlich größer als die andere Zahl :math:`b`,
  so müsste bei Anwendung des vorherigen Algorithmus sehr oft :math:`b` von
  :math:`a` subtrahiert werden. Ist beispielsweise :math:`a=968` und
  :math:`b=24`, so ergäbe die erste Differenz :math:`(968-24)=944`, die zweite
  Differenz :math:`(944-24)=920`, usw. Bei Verwendung eines Computers ist es
  effektiver, anstelle der wiederholten Subtraktion eine Modulo-Rechnung
  vorzunehmen, also bei einer Division mit Rest nur auf den Divisionsrest zu
  achten. Hier ergibt :math:`968 \text{ mod } 24` den Wert :math:`18`; der
  Algorithmus kann somit unmittelbar mit :math:`24 \text{ mod 18} = 6`
  fortgeführt werden und liefert schon im dritten Schritt als Ergebnis :math:`18
  \text{ mod } 6 = 0`. Der größte gemeinsame Teiler :math:`(6)` wurde so in nur
  drei Rechenschritten bestimmt.

In Python lässt sich diese zweite, schnellere Variante des Euklidschen
Algorithmus dank des Modulo-Operators ``%`` mit nur sehr wenig Code
implementieren. 

.. code-block:: python

    def gcd_simple(a, b):
        '''Quite simple implementation of Euclid's Algorithm.'''
        while b != 0:
            tmp = a % b
            a = b
            b = tmp
        return a

Dieser Code lässt sich noch weiter optimieren. Der Trick der folgenden
Implementierung besteht darin, dass der Zuweisungsoperator ``=`` eine geringere
Priorität besitzt als der Modulo-Operator, und somit erst die rechte Seite
ausgewertet wird, bevor die Ergebnisse in die links angegebenen Variablen
gespeichert werden; dies erspart das Speichern der Zwischenergebnisse in
temporären Variablen:

.. code-block:: python

    def gcd(a, b):
        '''Return the greatest common divisor using Euclid's Algorithm.'''
        while b:
            a, b = b, a % b
        return a

Hat man den größten gemeinsamen Teiler gefunden, so kann auch das kleinste
gemeinsame Vielfache zweier Zahlen einfach berechnet werden: Man multipliziert
beide Zahlen miteinander und dividiert das Ergebnis anschließend durch den
größten gemeinsamen Teiler. In Python könnte die entsprechende Funktion also
folgendermaßen aussehen:

.. code-block:: python

    def lcm(a, b):
        '''Return lowest common multiple.'''
        return a * b / gcd(a, b)

Möchte man das kleinste gemeinsame Vielfache nicht nur zweier, sondern einer
Liste mit beliebig vielen ganzen Zahlen ermitteln, so müsste man die obige
``lcm()``-Funktion iterativ auf die einzelnen Elemente der Liste anwenden. Nutzt
man hierfür die Funktion ``reduce()`` aus dem  Standard-Modul :ref:`functools
<functools>`, so lässt sich der Algorithmus folgendermaßen implementieren
(Quelle: `Stackoverflow
<https://stackoverflow.com/questions/147515/least-common-multiple-for-3-or-more-numbers/11851434#11851434>`__):

.. code-block:: python

    import functools as ft

    def lcmm(*args):
        '''turn lcm of args.'''
        return ft.reduce(lcm, args)


    # Beispiel:

    lcmm(6, 13, 27, 84)
    # Ergebnis: 9828


... to be continued ...




