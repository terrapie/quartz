# Was ich schon installiert habe

- Microsoft SQL Server 2022 (Express) und SQL Server 2025 — beide laufen parallel
- SQL Server Management Studio 22 (SSMS)
- Power BI Desktop
- ODBC Driver 17 und 18 für SQL Server

## Wichtige Begriffe

**localhost** = "dieser Computer". Der SQL Server läuft als Dienst im Hintergrund auf meinem eigenen Laptop, deshalb verbinde ich mich mit `localhost` statt mit einem entfernten Server. In der Firma wird das später der Name des Firmenservers sein.

**NVARCHAR vs. VARCHAR**: NVARCHAR speichert Unicode-Zeichen (ä, ö, ü, ą, ę, ß usw.), VARCHAR nur einfache Zeichen. Bei deutschen/polnischen Namen und Wörtern immer NVARCHAR benutzen.

## Dienste prüfen (PowerShell)

Prüfen, ob SQL Server läuft:

```powershell
Get-Service *SQL*
```

Status sollte bei `MSSQLSERVER` und/oder `MSSQL$SQLEXPRESS` auf "Running" stehen.

## Schritt 1: Mit SSMS verbinden

1. SQL Server Management Studio öffnen
2. Servername: `localhost`
3. Authentifizierung: Windows-Authentifizierung
4. Verschlüsseln: Optional (sonst gibt es manchmal einen Zertifikatsfehler)
5. Verbinden klicken

## Schritt 2: Testdatenbank erstellen

Rechtsklick auf "Databases" → "Neue Datenbank..." → Name: `UebungsDB`

## Schritt 3: Tabelle mit Testdaten erstellen

```sql
CREATE TABLE Kunden (
    KundenID INT PRIMARY KEY IDENTITY(1,1),
    Vorname NVARCHAR(50),
    Nachname NVARCHAR(50),
    Stadt NVARCHAR(50),
    Umsatz DECIMAL(10,2)
);

INSERT INTO Kunden (Vorname, Nachname, Stadt, Umsatz)
VALUES
    ('Anna', 'Kowalski', 'Marl', 1200.50),
    ('Peter', 'Schmidt', 'Recklinghausen', 890.00),
    ('Lena', 'Müller', 'Essen', 2100.75),
    ('Tom', 'Weber', 'Marl', 450.20);

SELECT * FROM Kunden;
```

`UebungsDB` ist die Datenbank (wie ein Ordner), `Kunden` ist die Tabelle darin.

## Schritt 4: Power BI mit SQL Server verbinden

1. Power BI Desktop öffnen
2. "Daten abrufen" → "SQL Server-Datenbank"
3. Server: `localhost`
4. Datenbank: `UebungsDB`
5. Verbindungsmodus: Import
6. Anmeldung: Windows
7. Tabelle "Kunden" auswählen → "Load"

## Schritt 5: Visualisierung erstellen

- Im Datenbereich rechts die Felder "Stadt" und "Umsatz" anhaken
- Power BI erkennt "Stadt" oft automatisch als Ort und erstellt eine Karte
- Falls die Kartenvisuals deaktiviert sind: Datei → Optionen und Einstellungen → Optionen → Global → Sicherheit → "Verwenden von Kartenvisuals und Flächenkartogrammen" aktivieren, dann Power BI neu starten

## Ergebnis

Kompletter Weg funktioniert:

```
SQL Server (Daten) → Power BI (Import) → Visualisierung
```

![[Pasted image 20260831202003.png]]