# Sammlung nützlicher freier Kinco DTools Komponenten von spstiger

Hier folgt die Erklärung

- Screenshots Projekt und Komponenten

- Variablen erklärt

- Aufbau Balkendiagramm erklärt - RAM und Flash

- Import der Bibliothek erklärt

## Komponente – Speicherauslastung (RAM / Flash)

Diese Komponente visualisiert die Speicherauslastung (RAM und Flash) eines beliebigen Kinco-HMI-Geräts.  
Das Beispiel wurde auf einem **MK070E** erstellt, ist aber auf andere Modelle übertragbar.

### Aufbau

Im **Fenster 0 (Hauptfenster)** werden fünf Elemente angezeigt, die die Speicherbelegung unterschiedlicher HMI-Modelle darstellen:

- **RAM:** 64 MB, 128 MB, 256 MB

- **Flash:** 128 MB, 256 MB

Jedes Element zeigt grafisch die Belegung des Speichers durch den Benutzer, das System und den noch freien Bereich.

![](./assets/2025-10-08-092621-inage.png)

---

### Funktionsprinzip

#### 1. RAM-Anzeige

Für die Anzeige des benutzten und freien RAM-Speichers werden **Balkendiagramme** und **Zahlenkomponenten** mit **Systemvariablen** kombiniert.

- **Benutzter RAM (User):**

- **Systemregister:** LW9136

![](./assets/2025-10-08-092622-inage.png)

- **Datentyp:** 2 Words (32 Bit ohne Vorzeichen)

- **Ausrichtung:** *Rechts* (Balken wächst nach links)

- **Wertebereich (Beispiel 64 MB):**
  
  - **Minimum:** 0
  
  - **Maximum:** 64 MB × 1024² = 67 108 864 Byte

![](./assets/2025-10-08-092623-inage.png)

  Für andere Modelle:

| RAM-Größe | Maximalwert (Bytes) |
| --------- | ------------------- |
| 64 MB     | 67 108 864          |
| 128 MB    | 134 217 728         |
| 256 MB    | 268 435 456         |

- **Freier RAM:**
  
  - **Systemregister:** LW9134
  
  - **Datentyp:** 2 Words (32 Bit ohne Vorzeichen)
  
  - **Ausrichtung:** *Links* (Balken wächst nach rechts)

![](./assets/2025-10-08-092624-inage.png)

Beide Balkendiagramme werden **übereinander auf einer Bitmap-Komponente** platziert, die den vom System genutzten Speicherbereich darstellt.  
So entsteht eine dreifarbige Gesamtanzeige:

- **Orange:** vom Benutzer belegter RAM

- **Blau:** vom System belegter RAM

- **Grün:** freier RAM

Zur besseren Lesbarkeit sind unterhalb des Diagramms **Zahlenkomponenten** angeordnet:

- LW9136 → belegter RAM (User)

- LW9134 → freier RAM

#### Skalierung der Zahlenkomponenten

Damit die Werte leicht ablesbar sind, werden die **Zahlenkomponenten skaliert**, um nicht die Byte-Zahl, sondern den **Speicher in Megabyte (MB)** darzustellen.

- **Skalierung für 64 MB-Gerät:**
  
  - **Unterer Wert:** 0
  
  - **Oberer Grenzwert:** 64

- **Skalierung für 128 MB-Gerät:**
  
  - **Unterer Wert:** 0
  
  - **Oberer Grenzwert:** 128

- **Skalierung für 256 MB-Gerät:**
  
  - **Unterer Wert:** 0
  
  - **Oberer Grenzwert:** 256

![](./assets/2025-10-08-092625-inage.png)

Damit zeigen die Zahlenkomponenten den aktuellen RAM-Stand in **Megabyte** an – z. B. „27.4 MB frei“ oder „12.8 MB belegt“.

---

#### 2. Flash-Anzeige

Der **freie Flashspeicher** wird über das **Systemregister LW9036** dargestellt.  
Da Kinco-HMIs in der Regel nur **128 MB oder 256 MB Flash** besitzen, gibt es hier nur zwei Varianten.

- **Zahlenkomponente:** zeigt verbleibenden Flashspeicher (in MB)

- **Balkendiagramm:** visualisiert belegten Flashbereich
  
  - Liegt auf einer Bitmap-Komponente, die den gesamten Flash-Speicher symbolisiert

---

### Hinweise zur Anpassung

- Alle Adressen (LW9134, LW9136, LW9036) sind **systemweit gültig**.

- Die Farben können nach Belieben im Balkendiagramm-Editor geändert werden





## Eigene Komponentenbibliothek erstellen und verwenden

Kinco DTools enthält bereits eine **Standardbibliothek** mit vordefinierten Komponenten.  
Diese sollte **nicht verändert oder überschrieben** werden.  
Um eigene, benutzerdefinierte Elemente zu speichern (z. B. selbst erstellte Speicheranzeigen oder Visualisierungskomponenten),  
empfiehlt es sich, eine **eigene Bibliothek** anzulegen.

---

### 1. Voraussetzungen

Bevor eine Komponente gespeichert werden kann,  
müssen alle zugehörigen Elemente im HMI-Projekt **fertig aufgebaut und korrekt positioniert** sein  
(z. B. Balkendiagramme, Zahlenkomponenten, Texte).

---

### 2. Elemente gruppieren

1. Alle zugehörigen Elemente markieren

2. Mit **Strg + G** oder per Rechtsklick → **„Gruppieren“** auswählen

3. Die markierten Objekte werden zu **einem einzigen Element** zusammengefasst

> Gruppieren ist notwendig, damit DTools die Objekte als eine Einheit behandelt und sie später als komplette Komponente speichern kann.

---

### 3. Eigene Bibliothek anlegen

1. Rechtsklick auf das **gruppierte Element**

2. Menü wählen: **„Eigene Komponenten → Eigene Komponenten speichern“**

3. Im Dialogfenster unten auf **„Neue Bibliothek“** klicken

4. Einen **eindeutigen Namen** für die Bibliothek eingeben  
   (z. B. `spstiger_components`)

5. Mit **OK** bestätigen

> Dadurch wird eine neue, eigene Bibliothek erstellt.  
> Die vorinstallierte Standardbibliothek von Kinco DTools bleibt unverändert.

---

### 4. Komponente in die eigene Bibliothek speichern

Bevor die Komponente gespeichert wird,  
sollte überprüft werden, ob in der **Liste der Komponentenbibliotheken**  
der **Name der eigenen Bibliothek** (z. B. `spstiger_components`) angezeigt wird.

Wenn der Name dort erscheint, befindet man sich **in der richtigen Bibliothek**  
und kann die Komponente sicher speichern.

**Vorgehensweise:**

1. Menü wählen: **„Eigene Komponenten speichern“**

2. Einen **Namen für die neue Komponente** eingeben  
   (z. B. `RAM_Flash_Monitor`)

3. Mit **OK** bestätigen

> Pro Speichervorgang kann **immer nur eine Komponente** gespeichert werden. Wird versehentlich die Standardbibliothek ausgewählt,  
> landet die Komponente dort – das sollte vermieden werden.

---

### 5. Eigene Bibliothek verwenden

Nach erfolgreicher Erstellung kann die Bibliothek in jedem Projekt genutzt werden:

1. Auf eine freie Fläche im Projekt **rechtsklicken**

2. Menü wählen: **„Eigene Komponenten → Eigene Komponenten nutzen“**

3. In der **Liste der Komponentenbibliotheken**  
   die gewünschte Bibliothek (z. B. `spstiger_components`) auswählen

4. Die gespeicherten Komponenten erscheinen in der Liste  
   und können per Klick in das Projekt eingefügt werden

---

### 6. Export und Import

Eigene Bibliotheken können exportiert und in andere Projekte oder Computer importiert werden:

- **Exportieren:**  
  Menü → *Eigene Komponenten → Bibliothek exportieren*

- **Importieren:**  
  Menü → *Eigene Komponenten → Bibliothek importieren*

So lassen sich eigene Komponenten wie Bausteine zwischen unterschidliche Versionene des DTools austauschen.

---

### Ergebnis

Nach diesen Schritten steht deine **eigene Komponentenbibliothek** zur Verfügung:

- unabhängig von der Kinco-Standardbibliothek

- wiederverwendbar in allen Projekten

- einfach zu erweitern, exportieren und importieren
