
Ziel: Beispieldaten in SQL Server bringen, später mit Power BI verbinden.

## Ziel des Projekts

Ein kleines Verkaufsdashboard mit drei Tabellen:
- **Kunden**
- **Produkte**
- **Bestellungen** (verbindet Kunden und Produkte)

## Schritt 1: Beispieldaten mit Python erstellen

Python-Skript mit Claude erstellt, das drei CSV-Dateien erzeugt:
- `kunden.csv` (200 Kunden)
- `produkte.csv` (50 Produkte)
- `bestellungen.csv` (1000 Bestellungen)

Wichtige Punkte aus dem Skript:
- Zufällige Namen, Städte, Kategorien mit `random.choice`
- Zufällige Preise mit `random.uniform(5, 300)`
- Zufällige Bestelldaten zwischen zwei festen Daten

## Schritt 2: Neue Datenbank erstellen

```sql
CREATE DATABASE VerkaufsDashboard;
```

**Problem:** Die Datenbank war im Objekt-Explorer nicht sichtbar, obwohl die Erstellung erfolgreich war.

**Lösung:** **Databases** → **Refresh**. Die Ansicht aktualisiert sich nicht automatisch.

## Schritt 3: CSV-Dateien importieren

Import über **Import Flat File** (Rechtsklick auf Datenbank → Tasks → Import Flat File).

Reihenfolge wichtig wegen der späteren Foreign Keys:
1. `kunden.csv`
2. `produkte.csv`
3. `bestellungen.csv`

**Wichtig:** Der Import-Assistent erstellt die Tabelle automatisch selbst und rät die Datentypen anhand der CSV-Werte. Das führt zu Problemen (siehe Schritt 4).

## Schritt 4: Datentypen korrigieren

### Problem 1: `kunden_id` wurde als `tinyint` importiert

Der Import-Assistent hat bei kleinen Zahlen automatisch `tinyint` statt `int` gewählt.

Korrektur (Primary Key musste dafür kurz entfernt werden):

```sql
-- Primary Key entfernen
ALTER TABLE Kunden DROP CONSTRAINT PK_kunden;

-- Datentyp ändern (NOT NULL nötig für Primary Key!)
ALTER TABLE Kunden
ALTER COLUMN kunden_id INT NOT NULL;

-- Primary Key wieder hinzufügen
ALTER TABLE Kunden ADD PRIMARY KEY (kunden_id);
```

**Merksatz:** Ein Primary Key kann nur auf einer `NOT NULL`-Spalte definiert werden.

### Problem 2: Preise falsch importiert (Komma vs. Punkt)

Preise wie `29.44` wurden als `2944` importiert.

**Ursache:** Der Import-Assistent hat mit deutschen Zahlenformat-Einstellungen gearbeitet (Punkt = Tausendertrennzeichen, Komma = Dezimaltrennzeichen), aber die CSV nutzt den Punkt als Dezimaltrennzeichen (Python-Standard).

**Für zukünftige Importe:** Im Import-Assistenten das Dezimaltrennzeichen explizit auf Punkt stellen, statt sich auf die Automatik zu verlassen.

![Falsch importierte Preise](preise_fehler.png)

### Datentypen einer Tabelle prüfen

Per SQL:

```sql
SELECT column_name, data_type, is_nullable
FROM information_schema.columns
WHERE table_name = 'Kunden';
```

**Per Klick: Objekt-Explorer → Tabelle → Columns aufklappen, oder Rechtsklick auf Tabelle → Design**.

## Schritt 5: Primary Keys und Foreign Keys setzen

Der Import-Assistent kann zwar einen Primary Key setzen, aber **keine Foreign Keys**. Die mussten manuell ergänzt werden:

```sql
ALTER TABLE Bestellungen
ADD CONSTRAINT FK_Bestellungen_Kunden FOREIGN KEY (kunden_id) REFERENCES Kunden(kunden_id);

ALTER TABLE Bestellungen
ADD CONSTRAINT FK_Bestellungen_Produkte FOREIGN KEY (produkt_id) REFERENCES Produkte(produkt_id);
```

Ergebnis geprüft:

![Bestellungen mit Foreign Keys](bestellungen_spalten.png)

Alle Foreign Keys einer Tabelle anzeigen:

```sql
SELECT name AS constraint_name
FROM sys.foreign_keys
WHERE parent_object_id = OBJECT_ID('Bestellungen');
```

## Schritt 6: Beziehungen visualisieren

Über **Database Diagrams** (Rechtsklick → New Database Diagram) lässt sich das Datenmodell grafisch anzeigen:

![Datenbankdiagramm](diagramm.png)

Das Ergebnis ist ein **Sternschema**: `Bestellungen` als Faktentabelle in der Mitte, `Kunden` und `Produkte` als Dimensionstabellen.

## Wichtigste Lektionen

- Neue Objekte im Objekt-Explorer erscheinen manchmal erst nach **Refresh**.
- Der Import-Assistent rät Datentypen oft nicht sinnvoll (`tinyint` statt `int`, `float` statt `decimal` bei Preisen) — nach jedem Import kurz gegenprüfen.
- Ein Primary Key braucht immer `NOT NULL`.
- Foreign Keys müssen nach dem Import manuell per SQL ergänzt werden.
- In SQL Server heißt der Befehl zum Ändern einer Spalte `ALTER COLUMN`, nicht `MODIFY` (wie in MySQL).

## Nächster Schritt

Verbindung von Power BI zur Datenbank `VerkaufsDashboard` herstellen und mit dem Datenmodell + ersten DAX-Kennzahlen starten.
