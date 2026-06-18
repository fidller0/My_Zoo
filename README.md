\# Blatt 07: Generics, Sealed Types, Stream-API, Logging



\## Zusammenfassung



In diesem Projekt wurde ein kleines Zoo-System in Java umgesetzt.

Dabei wurden die Themen \*\*sealed interfaces\*\*, \*\*records\*\*, \*\*Generics\*\*, \*\*Stream-API\*\* und \*\*Logging\*\* verwendet.



Das Projekt besteht aus einer Tier-Hierarchie, generischen Gehegen, spezialisierten Gehegen und einer Zoo-Verwaltung.



\---



\## Aufgabe 1.1: Tier-Hierarchie



Die Tierarten wurden mit `sealed interfaces` modelliert.



Die oberste Schnittstelle ist `Animal`. Davon leiten sich folgende Tiergruppen ab:



\* `Mammal`

\* `Fish`

\* `Reptile`

\* `Bird`



Die Säugetiere wurden weiter unterteilt in:



\* `Primate`

\* `Rodent`

\* `Cat`



Für jede Tiergruppe wurden konkrete `record`-Klassen erstellt, zum Beispiel:



\* `Trout` und `Salmon` für Fische

\* `Snake` und `Turtle` für Reptilien

\* `Eagle` und `Parrot` für Vögel

\* `Lion` und `Tiger` für Katzen

\* `Gorilla` und `Chimpanzee` für Primaten

\* `Mouse` und `Squirrel` für Nagetiere



Jedes Tier besitzt mindestens den Namen als Attribut:



```java

String name

```



Durch `sealed interfaces` ist genau festgelegt, welche Klassen und Interfaces zu der Hierarchie gehören dürfen.



\---



\## Aufgabe 1.2: Generische Gehege



Für Gehege wurde die generische Klasse `Enclosure<T extends Animal>` erstellt.



Dadurch kann ein Gehege nur Tiere aufnehmen, die von `Animal` abgeleitet sind.



Die Klasse besitzt:



\* einen eindeutigen Namen,

\* eine Sammlung von Bewohnern,

\* Methoden zum Hinzufügen und Entfernen von Tieren,

\* eine Methode zum Ausgeben aller Bewohner.



Intern wurde ein `LinkedHashSet<T>` verwendet.

Dadurch kann dasselbe Tier nicht mehrfach im gleichen Gehege vorkommen. Gleichzeitig bleibt die Einfügereihenfolge erhalten.



Wichtige Methoden:



```java

boolean add(T animal)

boolean remove(T animal)

List<T> getInhabitants()

```



\---



\## Aufgabe 1.3: Spezialisierte Gehege



Es wurden spezialisierte Gehege erstellt:



\* `Aquarium extends Enclosure<Fish>`

\* `Terrarium extends Enclosure<Reptile>`

\* `MammalHouse extends Enclosure<Mammal>`

\* `CatHouse extends Enclosure<Cat>`



Dadurch verhindert Java bereits zur Compile-Zeit falsche Kombinationen.

Zum Beispiel kann ein `Lion` in ein `CatHouse`, aber ein `Trout` nicht.



\---



\## Aufgabe 1.4: Zoo-Verwaltung



Die Klasse `Zoo` verwaltet mehrere Gehege.



Implementierte Methoden:



\* `addEnclosure`

\* `getEnclosures`

\* `findEnclosureByName`

\* `getAllAnimals`

\* `getAllMammals`

\* `getAnimalsByPredicate`

\* `countAnimalsByType`

\* `getOvercrowdedEnclosures`

\* `summary`



Dabei wurde die Stream-API verwendet, zum Beispiel mit:



\* `filter`

\* `map`

\* `flatMap`

\* `toList`

\* `Collectors.groupingBy`

\* `Collectors.counting`

\* `Collectors.joining`



Die Stream-API ist hier besonders hilfreich, weil viele Methoden Daten aus mehreren Gehegen sammeln, filtern oder gruppieren müssen.



\---



\## Aufgabe 2: Logging



In der Klasse `Zoo` wurde Logging mit `java.util.logging` ergänzt.



Verwendete Log-Level:



\* `INFO` für den Start oder Aufruf einer Methode

\* `FINE` für eine kurze Zusammenfassung nach erfolgreicher Ausführung

\* `WARNING`, wenn ein angefordertes Gehege nicht gefunden wird

\* `SEVERE` bei schweren Fehlern, zum Beispiel ungültigen Parametern



In der Klasse `Main` wird demonstriert, wie das Log-Level gezielt gesetzt werden kann.



\---



\## Reflexion



\### Generics



Generics helfen im Zoo-Szenario, Fehler bereits zur Compile-Zeit zu vermeiden.



Ein Beispiel ist:



```java

Aquarium aquarium = new Aquarium("Amazonas Aquarium");

```



Da `Aquarium` von `Enclosure<Fish>` erbt, können nur Fische eingefügt werden.

Ein Vogel oder Säugetier würde vom Compiler abgelehnt werden. Dadurch entstehen weniger Laufzeitfehler.



Generics machen den Code außerdem klarer, weil schon am Typ sichtbar ist, welche Tiere in welches Gehege gehören.



\---



\### Logging



Systematisches Logging ist sinnvoller als einfache Ausgaben mit `System.out.println`, weil Logging verschiedene Wichtigkeitsstufen besitzt.



Mit Log-Leveln kann gezielt gesteuert werden, welche Informationen sichtbar sind.



Beispiele:



\* `INFO`: Eine öffentliche Zoo-Methode wird aufgerufen.

\* `FINE`: Eine Methode wurde erfolgreich ausgeführt und liefert eine kurze Zusammenfassung.

\* `WARNING`: Ein gesuchtes Gehege wurde nicht gefunden.

\* `SEVERE`: Ein ungültiger Zustand oder ein schwerer Fehler tritt auf.



Dadurch ist das System besser nachvollziehbar und leichter zu debuggen.



\---



\### Streams



Streams helfen besonders beim Durchlaufen, Filtern und Gruppieren von Daten.



Im Zoo-Projekt sind Streams hilfreich, weil die Tiere über mehrere Gehege verteilt sind. Mit `flatMap` können alle Tiere aus allen Gehegen zu einer gemeinsamen Liste zusammengeführt werden.



Beispiele aus der Implementierung:



\* alle Tiere aus allen Gehegen sammeln,

\* nur Säugetiere filtern,

\* Tiere nach Typ zählen,

\* überfüllte Gehege finden,

\* eine Zusammenfassung erzeugen.



Bei sehr komplexer Logik können Streams aber auch unübersichtlich werden. In solchen Fällen wäre eine klassische Schleife manchmal leichter zu lesen.



\---



\## Ausführen



Kompilieren:



```powershell

javac -d out (Get-ChildItem -Recurse src\\main\\java\\\*.java).FullName

```



Programm starten:



```powershell

java -cp out zoo.Main

```



