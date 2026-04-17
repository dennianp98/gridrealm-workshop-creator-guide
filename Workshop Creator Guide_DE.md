# Workshop Gebäude-Erstellungsleitfaden

Willkommen, Creator! Vielen Dank für Ihr Interesse an der Erstellung von benutzerdefinierten Inhalten! Ihr Beitrag wird der Community zum Gedeihen verhelfen! Dieser Leitfaden hilft Ihnen, Inhalte zu erstellen, die den Spezifikationen entsprechen, und sie in die Steam Workshop hochzuladen.

## Technische Anforderungen
Das Hochladen von Inhalten erfordert zwei Skripte: BuildingData und WorkshopUploadTool. BuildingData wird zum Ausfüllen der Gebäudedaten verwendet, und WorkshopUploadTool wird zum Hochladen verwendet. Sie benötigen auch Steamworks.NET. Wenn Sie Steamworks.NET aktualisieren müssen, können Sie es von der GitHub-Seite herunterladen: https://github.com/rlabrecque/Steamworks.NET/releases, es in Unity importieren und das Steam Manager-Skript an ein leeres GameObject anhängen.
Ich habe die erforderlichen Skripte und Steamworks.NET in eine Unity-Paketdatei gepackt. Sie können diese Paketdatei verwenden oder sie separat herunterladen: Link XX

### Gebäudemodell-Spezifikationen

> ⚠️ **Erforderliche Komponenten Erinnerung**: Gebäude-Prefabs müssen die folgenden drei Kernkomponenten enthalten. Das Fehlen einer davon führt zu Funktionsstörungen:
> - **Mesh Filter**: Zeigt das Modell-Mesh an
> - **Mesh Renderer**: Rendert das Modell (erfordert Materialzuweisung)
> - **Collider** (Box Collider): Wird für Maus-Hover-Hervorhebung, Klick-Erkennung und Platzierungs-Kollisionserkennung verwendet

### Unterstützte Ressourcenformate

- **Modelle**: FBX
- **Texturen**: PNG
- **Materialien**: Unity URP Lit Shader

### Dateigrößenbeschränkungen

- Einzelnes Gebäude-AssetBundle: ≤ 50 MB
- Vorschaubild: ≤ 1 MB
- Gesamte Download-Größe: ≤ 100 MB

## Gebäude-Design-Richtlinien

### 1. Größenspezifikationen

Entwerfen Sie basierend auf der vom Gebäude belegten Gittergröße. Die Gebäudebasis (Fundament) sollte mindestens 1 Meter Länge und Breite haben:
Damit die Gitterbelegung des Gebäudes korrekt berechnet wird, wird empfohlen, ungerade Quadrate für die Gebäudebasisgröße zu verwenden, wie: 1×1, 3×3, 5×5. Gerade Quadrate und nicht-quadratische Formen werden nicht empfohlen, wie: 1×2, 2×2, 2×3, 4×4.
Höhe: Keine strikte Begrenzung, aber es wird empfohlen, nicht unter 0,1 Meter zu gehen

### 2. Visueller Stil

Um die Konsistenz des Spiels zu wahren, wird empfohlen:
- LowPoly-Stil verwenden
- Die Gesamtproportionen und Skalierungsverhältnisse des Gebäudes sicherstellen (Spiel-Standardskalierungsreferenz: Charakterhöhe 0,1 Meter)
- Isometriefreundliche Designs verwenden
- Übermäßig kleine Details vermeiden (aus der Ferne schwer zu sehen)
- Klare Umrisse und Farbkontraste verwenden
- Details aus verschiedenen Winkeln berücksichtigen

### 3. Leistungsoptimierungsempfehlungen

- Polygonanzahl angemessen kontrollieren
- Transparente Materialien (Alpha Test statt Alpha Blend verwenden)

## Erstellungsprozess

### Schritt 1: Modellierung in 3D-Software

1. Modelle mit Blender, Maya, 3ds Max usw. erstellen
2. Sicherstellen, dass der Modellursprung im unteren Zentrum liegt
Am Beispiel von Blender:
✅ Richtig: Gebäude-Mittelpunkt liegt am Weltursprung
((Bild 1))
❌ Falsch: Gebäude versetzt oder Rotation nicht ausgerichtet
((Bild 2))
3. Alle Transformationen anwenden (Scale, Rotation)
4. Als FBX-Format exportieren

### Schritt 2: In Unity importieren

1. Das FBX in das Unity-Projekt ziehen
2. Die Transform-Position des Gebäude-Prefabs auf (0, 0, 0) setzen
3. Sicherstellen, dass die Gebäudebasis auf der Y=0-Ebene liegt
4. Das Gebäude sollte in positive Z-Achsen-Richtung zeigen

### Schritt 3: Materialien und Texturen einrichten

1. Materialien erstellen (URP Lit Shader verwenden)
2. Materialparameter anpassen

### Schritt 4: Erforderliche Komponenten hinzufügen

Sicherstellen, dass das Gebäude-Prefab die folgenden drei **erforderlichen Komponenten** enthält:

#### 1. Mesh Filter und Mesh Renderer

1. Das Gebäude-Stammobjekt auswählen
2. Bestätigen, dass die **Mesh Filter**-Komponente hinzugefügt wurde (falls nicht, Add Component → Mesh Filter klicken)
3. Bestätigen, dass die **Mesh Renderer**-Komponente hinzugefügt wurde
4. Materialien im Mesh Renderer zuweisen (Standard oder URP Lit Shader verwenden)

> ⚠️ **Wichtig**: Gebäude ohne Mesh Renderer werden nicht angezeigt und lösen keinen Maus-Hover-Hervorhebungseffekt aus!

#### 2. Collider

1. Das Gebäude-Stammobjekt auswählen
2. Eine **Box Collider**-Komponente hinzufügen (empfohlen) oder **Mesh Collider**
3. Die Collider-Größe anpassen, um das gesamte Gebäude abzudecken
4. Sicherstellen, dass der Collider nicht über den Gebäudebereich hinausgeht, da dies zu Kollisionen mit anderen Gebäuden führt und das Platzieren verhindert
((Bild 3))

> ⚠️ **Wichtig**: Collider ist eine notwendige Voraussetzung für Maus-Hover-Hervorhebung und Klick-Erkennung! Gebäude ohne Collider können nicht vom Spieler ausgewählt oder interagiert werden.

### Schritt 5: Prefab erstellen

1. Das Gebäude in das Project-Fenster ziehen, um ein Prefab zu erstellen
2. Namenskonvention: `{BuildingName}_Prefab`
3. Testen, dass das Prefab normal instanziiert werden kann

### Schritt 6: BuildingData erstellen

1. Rechtsklick → **Create → City Builder → Building Data**
2. Die folgenden Felder ausfüllen:

| **Building Name** | Gebäudeanzeigename, kann Name oder Nummer verwenden | Z.B.: Ferriswheel, SSmbl2 |
| **Building ID** | Eindeutiger Bezeichner, empfohlen wird Stilname plus Nummer oder reine Nummer | Z.B.: SunsetCity_01, SSmbl2 |
| **Style** ⭐ | **Bestimmt, unter welchem Stil-Tab das Gebäude im Spiel-Gebäude-Menü erscheint**, empfohlen wird Ihr Entwicklername, Paketname oder Themenname. Gebäude derselben Serie sollten denselben Wert haben | Z.B.: Lyon'sPack, SunsetCity |
| **Category** ⭐ | **Bestimmt, in welcher Kategorie-Spalte es unter diesem Stil erscheint**, kann integrierte Spielkategorien für Kompatibilität verwenden (siehe Tabelle unten) oder nach eigener Gebäudeklassifikation ausfüllen | Z.B.: Large, Residential |
| **Grid Size** | Gebäude-Gitterbelegungsgröße | Z.B.: 3x3 |
| **Building Prefab** | Ihr Gebäude-Prefab hineinziehen | — |

**Integrierte Spielkategorien-Referenz**:

| `Large` | Groß |
| `Medium` | Mittel |
| `Small` | Klein |

> ⚠️ **Die Felder Style und Category sind sehr wichtig**: Sie werden in `metadata.json` verpackt (automatisch vom Upload-Tool geschrieben). Wenn das Spiel Ihren Mod lädt, verwendet es diese beiden Felder, um die Position des Gebäudes im Menü zu bestimmen. Bitte füllen Sie sie unbedingt aus, sonst wird das Gebäude möglicherweise nicht korrekt kategorisiert und angezeigt.

3. Ein Gebäude-Symbol erstellen (128×128 PNG empfohlen), in das **Icon**-Feld ziehen

### Schritt 7: Gebäude testen

1. Das Gebäude im Spiel testen:
   - Kann es korrekt platziert werden
   - Funktioniert die Rotation normal
   - Ist die Kollisionserkennung korrekt
   - Sind Sie mit dem visuellen Effekt zufrieden
2. Anpassen bis zufrieden

### Schritt 8: Upload vorbereiten

- Ein Vorschaubild erstellen (600×600 empfohlen)
- Bestätigen, dass **Style** und **Category** in BuildingData korrekt ausgefüllt sind (das Upload-Tool liest sie automatisch)
Checkliste vor dem Upload:
   - [ ] Modell-Dreiecksanzahl innerhalb der Grenzen
   - [ ] Texturauflösung ist angemessen
   - [ ] Enthält **Mesh Filter**-Komponente
   - [ ] Enthält **Mesh Renderer**-Komponente (Material zugewiesen)
   - [ ] Enthält **Collider**-Komponente
   - [ ] Prefab kann normal instanziiert werden
   - [ ] Gebäude am Gitter ausgerichtet
   - [ ] Keine fehlenden Materialien oder Texturen
   - [ ] Alle BuildingData-Felder ausgefüllt
   - [ ] Vorschaubild ist scharf
   - [ ] Im Spiel getestet

## Schritt 9: In den Workshop hochladen

> ⚠️ **Der Upload wird in zwei Schritten abgeschlossen**, da AssetBundle-Verpackung und Steam-Upload unterschiedliche Anforderungen an den Editor-Zustand haben und separat durchgeführt werden müssen.

### Erster Schritt: Ressourcen verpacken (Nicht-Play-Modus)

1. Sicherstellen, dass Sie sich im **Nicht-Play-Modus** befinden (Editor läuft nicht)
2. **City Builder → Workshop Upload Tool** öffnen
3. Mod-Typ auswählen: **Building**
4. **BuildingData** hineinziehen (das Tool liest automatisch Style und Category, kann manuell geändert werden)
5. **Gebäude-Prefab** hineinziehen
6. Workshop-Informationen ausfüllen (hauptsächlich den Titel, Beschreibung kann im Workshop bearbeitet werden)
7. Auf **"Erster Schritt: Ressourcen als AssetBundle verpacken"** klicken
8. Wenn Sie die Meldung **"Verpackung abgeschlossen! Bitte treten Sie in den Play-Modus ein und klicken Sie auf Schritt zwei zum Hochladen"** sehen, war es erfolgreich

> 💡 Der Verpackungspfad wird automatisch gespeichert und geht nicht verloren, wenn Sie in den Play-Modus eintreten. Sie müssen die Informationen nicht erneut eingeben.

### Zweiter Schritt: Auf Steam hochladen (Play-Modus)

1. Auf die **Play**-Schaltfläche des Unity-Editors klicken, auf Initialisierung von SteamManager warten
2. Im Tool-Fenster bestätigen, dass Steam initialisiert ist (keine Warnmeldungen)
3. Auf **"Zweiter Schritt: In den Steam Workshop hochladen"** klicken
4. Auf Abschluss des Uploads warten, die angezeigte **Workshop-Element-ID** notieren (für zukünftige Updates)

> 💡 Das Upload-Tool generiert automatisch `metadata.json` und verpackt es in das AssetBundle-Inhaltsverzeichnis, das Style- und Category-Informationen enthält. Nachdem Spieler abonniert haben, zeigt das Spiel automatisch in der richtigen Kategorie an.

### Schritt 10: Veröffentlichen und Warten

1. Informationen auf der Steam Workshop-Seite vervollständigen
2. Weitere Screenshots hinzufügen

## Hinweise zu Himmel- und Bodenmaterialien
- Himmel- und Bodenshader müssen die URP-Pipeline verwenden
- Bodenshader sollten vorzugsweise prozedurale Normalen verwenden, um dynamische Gitterbereich-Größenanpassungen zu bewältigen

## Häufige Fehler und Lösungen

### Fehler 0: Verpackungsfehler "Building AssetBundles while in play mode is not allowed"

**Ursache**: Die Verpackungsschaltfläche wurde im Play-Modus geklickt, aber AssetBundle-Verpackung kann nur im Nicht-Play-Modus ausgeführt werden

**Lösung**:
1. Play-Modus beenden (Stop-Schaltfläche klicken)
2. Im Tool erneut auf "Erster Schritt: Ressourcen als AssetBundle verpacken" klicken
3. Nach Abschluss der Verpackung in den Play-Modus eintreten, um hochzuladen

### Fehler 1: Gebäude wird rosa angezeigt

**Ursache**: Fehlendes Material oder Shader

**Lösung**:
1. Sicherstellen, dass alle Texturen korrekt zugewiesen sind
2. Überprüfen, ob Materialien im AssetBundle enthalten sind

### Fehler 2: Gebäude kann nicht platziert werden

**Ursache**: Fehlende erforderliche Komponenten (Mesh Filter/Mesh Renderer/Collider) oder falsche Gittergrößeneinstellungen

**Lösung**:
1. Bestätigen, dass das Gebäude-Prefab die Komponenten **Mesh Filter**, **Mesh Renderer** und **Collider** enthält
2. Überprüfen, ob dem Mesh Renderer Materialien zugewiesen sind
3. Box Collider zum Gebäude-Stammobjekt hinzufügen
4. Die Grid Size-Einstellung des BuildingData überprüfen
5. Die Gebäudeabmessungen bestätigen

### Fehler 3: Gebäude-Position versetzt

**Ursache**: Prefab-Transform-Einstellungen sind falsch

**Lösung**:
1. Prefab-Position auf (0, 0, 0) setzen
2. Sicherstellen, dass die Gebäudebasis auf der Y=0-Ebene liegt
3. Die Position des Pivot Point des Modells überprüfen

### Fehler 4: Gebäude nach Rotation versetzt

**Ursache**: Gebäude-Mittelpunkt ist nicht im Gitterzentrum

**Lösung**:
1. Modellursprung in 3D-Software anpassen
2. Oder Position der Kind-Objekte in Unity anpassen
3. Sicherstellen, dass sich das Gebäude um den Mittelpunkt dreht

### Fehler 5: Datei zu groß zum Hochladen

**Ursache**: Texturauflösung zu hoch oder Modell zu komplex

**Lösung**:
1. Texturauflösung senken (z.B. von 4K auf 2K)
2. Modell optimieren, um Polygonanzahl zu reduzieren
3. Texturen komprimieren (DXT5 oder BC7-Format verwenden)
4. Unnötige Details entfernen

## Fortgeschrittene Techniken

### Animationen hinzufügen

Wenn Ihr Gebäude Animationen enthält (wie eine sich drehende Windmühle):
1. Einen Animator Controller erstellen, die Animation in den Controller ziehen
2. Eine Animator-Komponente zum Prefab hinzufügen
3. Den Animator Controller in die Animator-Komponente ziehen

### LOD-System verwenden

Detail-Culling-Skripte für große Gebäude anhängen:
1. BuildingDetailCulling-Skript hinzufügen
2. Dem Modell verschiedene LOD-Stufen zuweisen
3. Culling-Entfernungen für verschiedene Stufen festlegen

### Benutzerdefinierte Materialeffekte

Shader Graph verwenden, um Spezialeffekte zu erstellen:
1. Ein Shader Graph-Asset erstellen
2. Benutzerdefinierte Effekte entwerfen (wie leuchtende Fenster)
3. Ein Material erstellen, das diesen Shader verwendet
4. Sicherstellen, dass der Shader mit der Zielplattform kompatibel ist

## Abschluss

Vielen Dank für Ihre Beiträge zur Erstellung von Community-Inhalten. Ich hoffe, dass das Spiel durch Ihre Inhalte an dauerhafter Vitalität gewinnt!

Ich freue mich darauf, Ihre Kreationen dem Spiel mehr farbenfrohe Möglichkeiten hinzuzufügen!

---

**Version**: 1.0  
**Letzte Aktualisierung**: 2026  
**Anwendbar auf**: Unity 2022.3 LTS
