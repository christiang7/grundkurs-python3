.. index:: Pandas
.. _Pandas:

``pandas`` -- eine Bibliothek für tabellarische Daten
=====================================================

Pandas ist eine Python-Bibliothek, die vorrangig zum Auswerten und Bearbeiten
tabellarischer Daten gedacht ist. Dafür sind in Pandas drei Arten von Objekten
definiert:

* Eine ``Series`` entspricht in vielerlei Hinsicht einer "eindimensionalen"
  Liste, beispielsweise einer Zeitreihe, einer Liste, einem Dict, oder einem
  :ref:`Numpy <numpy>` -Array.

* Ein ``Dataframe`` besteht aus einer "zweidimensionalen" Tabelle. Die einzelnen
  Reihen beziehungsweise Spalten dieser Tabelle können wie ``Series``-Objekte
  bearbeitet werden.

* Ein ``Panel`` besteht aus einer "dreidimensionalen" Tabelle. Die einzelnen
  Ebenen dieser Tabelle bestehen wiederum aus ``Dataframe``-Objekten.

In den folgenden Abschnitten sollen in Anlehnung an das berühmte `10 minutes to
pandas <http://pandas.pydata.org/pandas-docs/stable/10min.html>`__-Tutorial die
``Series``- und die ``Dataframe``-Objekte als grundlegende und am häufigsten
verwendeten Pandas-Objekte kurz vorgestellt werden.

.. index:: Series()
.. _Arbeiten mit Series-Objekten:
.. _Series:

Arbeiten mit ``Series``-Objekten
--------------------------------

Ein neues Series-Objekt kann mittels der gleichnamigen Funktion
beispielsweise aus einer gewöhnlichen Liste generiert werden:

.. code-block:: python

    import pandas as pd

    s = pd.Series( [5,10,15,20,25] )

    s
    # Ergebnis:
    # 0     5
    # 1    10
    # 2    15
    # 3    20
    # 4    25
    # dtype: int64

Das Series-Objekt erhält automatisch einen Index, so dass beispielsweise mittels
``s[0]`` auf das erste Element, mit ``s[1]`` auf das zweite Element, usw.
zugegriffen werden kann. Neben diesen numerischen Indizes, die auch bei
gewöhnlichen Listen verwendet werden, können explizit auch andere Indizes
vergeben werden:

.. code-block:: python

    s.index =  ['a','b','c','d','e']

    s
    # Ergebnis:
    # a     5
    # b    10
    # c    15
    # d    20
    # e    25
    # dtype: int64

Nun können die einzelnen Elemente zwar immer noch mit ``s[0]``, ``s[1]``, usw.,
aber zusätzlich auch mittels ``s['a']``, ``s['b']`` usw. ausgewählt werden. [#]_
Wird bei der Generierung eines Series-Objekts ein :ref:`Dict <dict>` angegeben,
so werden automatisch die Schlüssel als Indizes und die Werte als eigentliche
Listenelemente gespeichert.

.. _Slicings:

.. rubric:: Slicings

Sollen mehrere Elemente ausgewählt werden,so können die entsprechenden Indizes
wahlweise als Liste oder als so genannter "Slice" angegeben werden:

.. code-block:: python

    # Zweites und drittes Element auswählen:

    s[ [1,2] ]
    # Ergebnis:
    # b    10
    # c    15

    # Identische Auswahl mittels Slicing:

    s[ 1:3 ]
    # Ergebnis:
    # b    10
    # c    15

Bei Slicings wird, ebenso wie bei :ref:`range() <range()>`-Angaben, die obere
Grenze nicht in den Auswahlbereich mit eingeschlossen. Die Auswahl mittels
Slicing hat bei Series-Objekten also die gleiche Syntax wie die :ref:`Auswahl
von Listenobjekten <Indizierung von Listen und Tupeln>`.

.. index:: Zeitreihe, date_range()
.. _Zeitreihen:

.. rubric:: Zeitreihen

Zeitangaben in Series-Objekten können mittels der Pandas-Funktion
``date_range()`` generiert werden:

.. code-block:: python

    dates = pd.date_range('2000-01-01', '2000-01-07')

    dates
    # <class 'pandas.tseries.index.DatetimeIndex'>
    # [2000-01-01, ..., 2000-01-07]
    # Length: 7, Freq: D, Timezone: None

Als Start- und Endpunkt werden allgemein Datumsangaben mit einer gleichen Syntax
wie im ``datetime``-Modul verwendet. Zusätzlich kann angegeben werden, in
welchen Zeitschritten die Zeitreihe erstellt werden soll:

.. code-block:: python

    weekly = pd.date_range('2000-01-01', '2000-02-01', freq="W")

    weekly
    # Ergebnis:
    # <class 'pandas.tseries.index.DatetimeIndex'>
    # [2000-01-02, ..., 2000-01-30]
    # Length: 5, Freq: W-SUN, Timezone: None


    hourly = pd.date_range('2000-01-01 8:00', '2000-01-01 18:00', freq="H")

    hourly
    # Ergebnis:
    # <class 'pandas.tseries.index.DatetimeIndex'>
    # [2000-01-01 08:00:00, ..., 2000-01-01 18:00:00]
    # Length: 11, Freq: H, Timezone: None

Die Elemente der Zeitreihe können explizit mittels ``list(zeitreihe``,
beispielsweise ``list(dates)``, ausgegeben werden; in Series-Objekten werden
Zeitreihen häufig als Index-Listen verwendet.

.. _Arbeiten mit Dataframe-Objekten:
.. _Dataframe:

Arbeiten mit ``Dataframe``-Objekten
-----------------------------------

.. index:: Dataframe()

Ein neues Dataframe-Objekt kann mittels der Funktion ``DataFrame()``
beispielsweise aus einer gewöhnlichen Liste generiert werden:

.. code-block:: python

    import pandas as pd

    # 1D-Beispiel-Dataframe erstellen:
    df = pd.DataFrame( [5,10,15,20,25] )

    df
    # Ergebnis:
    #     0
    # 0   5
    # 1  10
    # 2  15
    # 3  20
    # 4  25
    #
    # [5 rows x 1 columns]

Als Unterschied zu einem Series-Objekt werden bei einem Dataframe sowohl die
Zeilen als auch die Spalten mit einem Index versehen.

Mehrspaltige Dataframes können auch über ein ``dict``-Objekt definiert werden,
wobei die Schlüsselwerte den Spaltennamen und die damit verbundenen Werte
einzelnen Daten entsprechen, aus denen der Dataframe generiert werden soll:

.. code-block:: python

    # 2D-Beispiel-Dataframe erstellen:
    df2 = pd.DataFrame({
        'A' : 1.,
        'B' : pd.date_range('2000-01-01', '2000-01-07'),
        'C' : pd.Series(range(7), dtype='float32'),
        'D' : np.random.randn(7),
        'E' : pd.Categorical(['on', 'off', 'on', 'off', 'on', 'off', 'on']),
        'F' : 'foo' })

    df2
    # Ergebnis:
    #    A          B  C         D    E    F
    # 0  1 2000-01-01  0 -2.611072   on  foo
    # 1  1 2000-01-02  1  0.630309  off  foo
    # 2  1 2000-01-03  2 -1.645430   on  foo
    # 3  1 2000-01-04  3  1.056535  off  foo
    # 4  1 2000-01-05  4  2.194970   on  foo
    # 5  1 2000-01-06  5  0.537804  off  foo
    # 6  1 2000-01-07  6  1.011678   on  foo

Wie man sieht, wird bei Angabe eines einzelnen Wertes für eine Spalte dieser als
konstant für die ganze Spalte angenommen; listenartige Objekte hingegen müssen
allesamt die gleiche Länge aufweisen.

.. rubric:: Datentypen

Innerhalb einer Spalte eines Dataframe-Objekts müssen alle Werte den gleichen
Datentyp aufweisen. Man kann sich die Datentypen der einzelnen Spalten
folgendermaßen anzeigen lassen:

.. code-block:: python

    # Datentypen anzeigen:

    df2.dtypes
    # Ergebnis:
    # A           float64
    # B    datetime64[ns]
    # C           float32
    # D           float64
    # E          category
    # F            object
    # dtype: object

.. _Daten anzeigen und sortieren:

Daten anzeigen und sortieren
^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Bei längeren Datensätzen kann es bereits hilfreich sein, nur einen kurzen Blick
auf den Anfang oder das Ende der Tabelle werfen zu können. Bei
Dataframe-Objekten ist dies mittels der Funktionen ``head()`` beziehungsweise
``tail()`` möglich:

.. code-block:: python

    # Die ersten fünf Zeilen des Dataframes anzeigen:

    df2.head()
    # Ergebnis:
    #    A          B  C         D    E    F
    # 0  1 2000-01-01  0 -2.611072   on  foo
    # 1  1 2000-01-02  1  0.630309  off  foo
    # 2  1 2000-01-03  2 -1.645430   on  foo
    # 3  1 2000-01-04  3  1.056535  off  foo
    # 4  1 2000-01-05  4  2.194970   on  foo

    # Die letzten drei Zeilen des Dataframes anzeigen:

    df2.tail(3)
    # Ergebnis:
    #   A          B  C         D    E    F
    # 4 1 2000-01-05  4  2.194970   on  foo
    # 5 1 2000-01-06  5  0.537804  off  foo
    # 6 1 2000-01-07  6  1.011678   on  foo

Standardmäßig geben ``head()`` und ``tail()`` je fünf Zeilen aus; ist eine andere
Anzahl gewünscht, so kann diese als Argument angegeben werden.


.. rubric:: Spalten und Index-Werte

Die einzelnen Bestandteile eines Dataframes, d.h. die Spaltennamen, die Index-Werte
sowie die eigentlichen Daten, können über die Attribute ``columns``, ``index``
und ``values`` des Dataframes abgerufen werden:

.. code-block:: python

    # Spaltennamen, Index-Werte und Inhalt des Dataframes ausgeben:

    df2.columns
    # Ergebnis:
    # Index(['A', 'B', 'C', 'D', 'E', 'F'], dtype='object')

    df2.index
    # Ergebnis:
    # Int64Index([0, 1, 2, 3, 4, 5, 6], dtype='int64')

    df2.values
    # Ergebnis:
    # array([[1.0, Timestamp('2000-01-01 00:00:00'), 0.0, -2.611072451193798, 'on', 'foo'],
    #   [1.0, Timestamp('2000-01-02 00:00:00'), 1.0, 0.6303090119623712, 'off', 'foo'],
    #   [1.0, Timestamp('2000-01-03 00:00:00'), 2.0, -1.645429619256174, 'on', 'foo'],
    #   [1.0, Timestamp('2000-01-04 00:00:00'), 3.0, 1.056535156797566, 'off', 'foo'],
    #   [1.0, Timestamp('2000-01-05 00:00:00'), 4.0, 2.1949702833421596, 'on', 'foo'],
    #   [1.0, Timestamp('2000-01-06 00:00:00'), 5.0, 0.5378036597920774, 'off', 'foo'],
    #   [1.0, Timestamp('2000-01-07 00:00:00'), 6.0, 1.01167812002758, 'on', 'foo']],
    #   dtype=object)

.. .

..
    df2 = pd.DataFrame(np.array([[1.0, pd.Timestamp('2000-01-01 00:00:00'), 0.0, -2.611072451193798, 'on', 'foo'],
      [1.0, pd.Timestamp('2000-01-02 00:00:00'), 1.0, 0.6303090119623712, 'off', 'foo'],
      [1.0, pd.Timestamp('2000-01-03 00:00:00'), 2.0, -1.645429619256174, 'on', 'foo'],
      [1.0, pd.Timestamp('2000-01-04 00:00:00'), 3.0, 1.056535156797566, 'off', 'foo'],
      [1.0, pd.Timestamp('2000-01-05 00:00:00'), 4.0, 2.1949702833421596, 'on', 'foo'],
      [1.0, pd.Timestamp('2000-01-06 00:00:00'), 5.0, 0.5378036597920774, 'off', 'foo'],
      [1.0, pd.Timestamp('2000-01-07 00:00:00'), 6.0, 1.01167812002758, 'on', 'foo']])
    )
    df2.columns = ['A', 'B', 'C', 'D', 'E', 'F']

.. rubric:: Statistische Übersicht

Eine Kurz-Analyse der Daten ist über die Methode ``describe()`` des Dataframes
möglich. Man erhält als Ergebnis eine Übersicht über die jeweiligen Mittelwerte
sowie einige statistische Streuungsmaße (Standardabweichung, größter und
kleinster Wert, Quartile). Da sich diese Größen nur für quantitative (genauer:
invervall-skalierte) Merkmalswerte bestimmen lassen, werden die jeweiligen Werte
auch nur für die in Frage kommenden Spalten angezeigt:

.. code-block:: python

    # Statistische Kurz-Info anzeigen:

    df2.describe()
    # Ergebnis:
    #        A         C         D
    # count  7  7.000000  7.000000
    # mean   1  3.000000  0.167828
    # std    0  2.160247  1.681872
    # min    1  0.000000 -2.611072
    # 25%    1  1.500000 -0.553813
    # 50%    1  3.000000  0.630309
    # 75%    1  4.500000  1.034107
    # max    1  6.000000  2.194970


.. rubric:: Sortiermethoden

Die Daten eines Dataframes können zudem wahlweise nach Zeilen oder Spalten oder
auch anhand der jeweiligen Werte sortiert werden:

* Mit der Methode ``sort_index()`` können die Daten nach Zeilen (``axis=0``)
  oder Spalten (``axis=1``) sortiert werden; mittels ``ascending=False`` kann
  zudem die Reihenfolge der Sortierung umgekehrt werden.

  .. code-block:: python

      df2.sort_index(axis=1, ascending=False)
      # Ergebnis:
      #      F    E         D  C          B  A
      # 0  foo   on -2.611072  0 2000-01-01  1
      # 1  foo  off  0.630309  1 2000-01-02  1
      # 2  foo   on -1.645430  2 2000-01-03  1
      # 3  foo  off  1.056535  3 2000-01-04  1
      # 4  foo   on  2.194970  4 2000-01-05  1
      # 5  foo  off  0.537804  5 2000-01-06  1
      # 6  foo   on  1.011678  6 2000-01-07  1

  Wird zusätzlich das optionale Argument ``inline=True`` gesetzt, so wird nicht
  ein verändertes Resultat angezeigt (das beispielsweise in einer neuen
  Variablen gespeichert werden könnte); vielmehr wird in diesem Fall die
  Änderung auch im ursprünlichen Dataframe-Objekt übernommen.

* Mit der Methode ``sort_value()`` können die Daten ihrer Größe nach sortiert
  werden. Standardmäßig werden die Daten dabei zeilenweise (``axis=0``) und in
  aufsteigender Reihenfolge (``ascending=True``) sortiert; bei Bedarf können
  diese Variablen angepasst werden.

  .. code-block:: python

      df2.sort_values(by='D')
      # Ergebnis:
      #    A          B  C         D    E    F
      # 0  1 2000-01-01  0 -2.611072   on  foo
      # 2  1 2000-01-03  2 -1.645430   on  foo
      # 5  1 2000-01-06  5  0.537804  off  foo
      # 1  1 2000-01-02  1  0.630309  off  foo
      # 6  1 2000-01-07  6  1.011678   on  foo
      # 3  1 2000-01-04  3  1.056535  off  foo
      # 4  1 2000-01-05  4  2.194970   on  foo

  Auch bei dieser Sortiermethode können die Änderungen mittels ``inline=True``
  nicht nur angezeigt, sondern direkt in den Original-Dataframe übernommen
  werden.

Daten auswählen
^^^^^^^^^^^^^^^

Dataframe-Objekte ähneln in gewisser Hinsicht ``dict``-Objekten: Die einzelnen
Spalten beziehungsweise Zeilen können mithilfe des Spalten- beziehungsweise
Index-Namens ausgewählt werden.

Ein Zugriff auf einzelne Zeilen oder Spalten ist beispielsweise mit Hilfe des
Index-Operators ``[ ]`` möglich. Gibt man hierbei einen Spaltennamen oder eine
Liste mit Spaltennamen an, so werden die jeweiligen Spalten ausgewählt; gibt man
hingegen eine Zeilennummer oder einen Zeilenbereich an, so erhält man die
jeweilige(n) Zeile(n) als Ergebnis:

.. code-block:: python

    df2['B']
    # Ergebnis:
    # 0   2000-01-01
    # 1   2000-01-02
    # 2   2000-01-03
    # 3   2000-01-04
    # 4   2000-01-05
    # 5   2000-01-06
    # 6   2000-01-07
    # Name: B, dtype: datetime64[ns]

    df2[['B','D']]
    # Ergebnis:
    #            B          D
    # 0 2000-01-01  -2.611072
    # 1 2000-01-02   0.630309
    # 2 2000-01-03  -1.645430
    # 3 2000-01-04   1.056535
    # 4 2000-01-05   2.194970
    # 5 2000-01-06   0.537804
    # 6 2000-01-07   1.011678

    df2[1:3]
    #    A          B  C         D    E    F
    # 1  1 2000-01-02  1  0.630309  off  foo
    # 2  1 2000-01-03  2 -1.645430   on  foo

Bei Bereichsangaben mittels Slicings ist wie gewöhnlich die untere Grenze im
Bereich mit enthalten, die obere hingegen nicht.


.. _Selektion mittels Labeln:

.. rubric:: Selektion mittels Labeln

Um auf einzelne Elemente eines Dataframes zugreifen zu können, muss sowohl eine
Zeilen- wie auch eine Reihenauswahl möglich sein. Für Dataframes ist dafür unter
anderem der ``.loc[]``-Operator definiert, mit dem eine Zeilen- beziehungsweise
Spaltenauswahl anhand der ``index``- beziehungsweise ``columns``-Bezeichnungen
möglich ist. Die Syntax lautet hierbei
``dataframe.loc[zeilenbereich,spaltenbereich]``, wobei für die Bereichsangaben
sowohl einzelne Index-Werte, Werte-Listen oder auch Slicings erlaubt sind; eine
Bereichs-Angabe von ``:`` bewirkt, dass der gesamte Zeilen- beziehungsweise
Spaltenbereich ausgewählt werden soll.

*Beispiel:*

.. code-block:: python

    # df2.loc[1:3, ['B','D']]
    #            B         D
    # 1 2000-01-02  0.630309
    # 2 2000-01-03 -1.645430
    # 3 2000-01-04  1.056535

Anders als beim gewöhnlichen Auswahloperator werden bei Benutzung des
``.loc[]``-Operators bei Slicings *beide* Grenzen zum Bereich dazugerechnet.

Möchte man nur einen *einzelnen* Wert auswählen, als Resultat also einen Skalar
erhalten, so kann mit gleicher Syntax auch der ``.at[]``-Operator verwendet
werden, der für diese Aufgabe eine geringere Rechenzeit benötigt.

.. _Selektion mittels Positionsangaben:

.. rubric:: Selektion mittels Positionsangaben

Ein zweiter Auswahl-Operator für Dataframes ist der ``.iloc[]``-Operator. Das
"i" steht dabei für "integer" und soll darauf hinweisen, dass dieser Auswahl
sowohl für die Angabe des Zeilen- wie auch des Spaltenbereichs eine numerische
Positionsangabe erwartet. Wie bei einer Liste wird die erste Zeile
beziehungsweise Spalte eines Dataframes intern mit ``0``, die zweite mit ``1``,
usw. nummeriert, unabhängig von den ``index``- beziehungsweise
``columns``-Bezeichnungen. Die Syntax für den ``.iloc``-Operator lautet also
``dataframe.iloc[zeilenbereich,spaltenbereich]``, wobei wiederum einzelne Werte,
Werte-Listen oder auch Slicings zur Angabe der Positionen erlaubt sind:

*Beispiel:*

.. code-block:: python

    # df2.iloc[1:3,[1,3]]
    #            B         D
    # 1 2000-01-02  0.630309
    # 2 2000-01-03 -1.645430
    # 3 2000-01-04  1.056535

Auch beim ``.loc[]``-Operator werden bei Slicings *beide* Grenzen zum Bereich
dazugerechnet.

Möchte man nur einen *einzelnen* Wert auswählen, als Resultat also einen Skalar
erhalten, so kann mit gleicher Syntax auch der ``.iat[]``-Operator verwendet
werden, der für diese Aufgabe eine geringere Rechenzeit benötigt.

Eine Mischung zwischen dem ``.loc[]`` und dem ``.iloc[]``-Operator stellt der
``.ix[]``-Operator dar: Dieser versucht anhand der angegebenen Bereiche --
ebenso wie der ``.loc[]``-Operator -- zunächst eine Auswahl anhand der
``index``- beziehungsweise ``columns``-Werte zu erreichen; ist dies allerdings
nicht möglich, so versucht dieser Operator anschließend die fehlgeschlagene
Bereichsauswahl wie der ``.iloc[]``-Operator als Positionsangabe zu deuten.

.. _Selektion mittels Bedingungen:

.. rubric:: Selektion mittels Bedingungen

Oftmals interessiert man sich nur für eine Teilmenge eines Dataframes, deren
Daten bestimmte Bedingungen erfüllen; man weiß jedoch nicht unmittelbar, an
welchen Stellen im Dataframe diese Daten abgelegt sind. Eine schnelle und
elegante Methode für eine derartige Datenauswahl besteht darin, die obigen
Auswahl-Operatoren mit der jeweiligen Bedingung anstelle einer Bereichsangabe zu
verwenden.

Bei der Formulierung der Auswahl-Bedingungen kann genutzt werden, dass man bei
der Anwendung von von Vergleichsoperatoren auf Dataframes boolesche Werte
erhält:

*Beispiel:*

.. code-block:: python

    df2['D'] > 1
    # 0    False
    # 1    False
    # 2    False
    # 3     True
    # 4     True
    # 5    False
    # 6     True
    # Name: D, dtype: bool

Anstelle der obigen Syntax kann auch ``df['D'].gt(0)`` geschrieben werden, wobei
``gt()`` für "greater than" steht. Diese und ähnliche Methoden gibt es sowohl
für (mehrdimensionale) Dataframes als auch für (eindimensionale) Series-Objekte;
ihr Vorteil besteht darin, dass sie sich verketten lassen. Beispielsweise
liefert so ``df2['D'].gt(0).lt(2)`` den booleschen Wert ``True`` für alle Werte,
die größer als ``0`` und kleiner als ``2`` sind.

+-------------------+----------------+
| Boolesche Methode | Bedeutung      |
+-------------------+----------------+
| ``gt()``          | Größer als     |
+-------------------+----------------+
| ``lt()``          | Kleiner als    |
+-------------------+----------------+
| ``ge()``          | Größer gleich  |
+-------------------+----------------+
| ``le()``          | Kleiner gleich |
+-------------------+----------------+
| ``eq()``          | Gleich         |
+-------------------+----------------+

.. http://pandas.pydata.org/pandas-docs/stable/generated/pandas.Series.html

Ein Series-Objekt mit booleschen Werten, wie man sie im obigen Beispiel erhalten
hat, kann wiederum als Bereichsangabe für die oben genannten Auswahl-Operatoren
genutzt werden:

.. code-block:: python

    df2[ df2['D'] > 1 ]
    # Ergebnis:
    #    A                    B  C        D    E    F
    # 3  1  2000-01-04 00:00:00  3  1.05654  off  foo
    # 4  1  2000-01-05 00:00:00  4  2.19497   on  foo
    # 6  1  2000-01-07 00:00:00  6  1.01168   on  foo

    df2.ix[ df2['D'] > 1, 'B' ]
    # Ergebnis:
    # 3   2000-01-04
    # 4   2000-01-05
    # 6   2000-01-07
    # Name: B, dtype: datetime64[ns]

Durch die oben genannten Auswahl-Operatoren werden die ursprünglichen Dataframes
nicht beeinflusst; man kann die Ergebnisse allerdings wiederum in extra
Variablen ablegen und/oder erneut Auswahl-Operatoren auf die Resultate anwenden.

.. ----

.. Auswahl mittels ``df.loc()``: Sinnvoll, wenn die Zeilen nicht automatisch
.. nummeriert sind, sondern als andere Werte festgelegt wurden. In diesem Fall
.. wäre kein Zugriff mehr via df[0:3] möglich; stattdessen
.. df.loc['ilabel1':'ilabel2']
.. dfl = pd.DataFrame(np.random.randn(5,4), columns=list('ABCD'), index=pd.date_range('20130101',periods=5))

.. http://pandas.pydata.org/pandas-docs/stable/indexing.html#indexing-label

.. purely label based indexing:
.. When slicing, the start bound is included, AND the stop bound is included.
.. Integers are valid labels, but they refer to the label and not the position.
.. The .loc attribute is the primary access method. The following are valid inputs:

.. * A single label, e.g. 5 or 'a', (note that 5 is interpreted as a label of the
..   index. This use is not an integer position along the index)
.. * A list or array of labels ['a', 'b', 'c']
.. * A slice object with labels 'a':'f' (note that contrary to usual python slices,
..   both the start and the stop are included!)

.. Setting works as well!

.. ----



.. Fehlende Werte
.. ^^^^^^^^^^^^^^


... to be continued ...


.. raw:: html

    <hr />

.. only:: html

    .. rubric:: Anmerkungen:

.. [#] Die Index-Liste kann auch bereits bei der Erzeugung eines neuen
    Series-Objekts mittels ``Series(datenliste, index=indexliste)`` angegeben
    werden.


.. .. [#] Quellen für tabellarische Daten:

..     Allgemeine Statistiken:
..         Das Statistische Bundesamt gibt jährlich ein umfangreiches `statistisches
..         Jahrbuch
..         <https://www.destatis.de/DE/Publikationen/StatistischesJahrbuch/StatistischesJahrbuch.html>`__
..         heraus; zudem können zu verschiedenen `Fachthemen
..         <https://www.destatis.de/DE/Publikationen/Thematisch/ThematischeVeroeffentlichungen.html>`__
..         PDF- beziehungsweise XLS-Dateien heruntergeladen werden.

..     Klima- und Wetterdaten:
..         Der Deutsche Wetterdienst (DWD) gibt kostenlos `Klimadaten
..         <https://www.dwd.de/DE/leistungen/klimadatendeutschland/klimadatendeutschland.html>`__
..         von verschiedenen Mess-Stationen heraus. Es können sowohl Tages- wie auch
..         Monats-Werte oder sogar langjährige Daten-Tabellen ausgewählt und/oder
..         heruntergeladen werden.

..     Prozess- und Produktdaten:
..         Das Bundesamt für Umwelt gibt unter der Bezeichnung `GEMIS
..         <http://iinas.org/gemis-download-121.html>`__ beziehungsweise `ProBas
..         <http://www.probas.umweltbundesamt.de/php/index.php>`__  eine Datenbank
..         beziehungsweise eine darauf aufbauende Webbrowser-Applikation heraus, in der
..         für eine Vielzahl von Handelsprodukten aufgelistet ist, welcher Energie- und
..         Rohstoffbedarf bei der Herstellung notwendig ist.

..     Energie-Daten
..         Das Bundesamt für Wirtschaft stellt eine regelmäßig aktualisierte Statistik zu
..         `Energie-Daten
..         <http://bmwi.de/DE/Themen/Energie/Energiedaten-und-analysen/energiedaten.html>`__
..         (Energieverwendung, Energiekosten, Ressourcen, usw.) als PDF- beziehungsweise
..         XLS-Datei zum Herunterladen zur Verfügung.
