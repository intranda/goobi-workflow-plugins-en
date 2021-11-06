---
description: >-
  Mit diesem Plugin lassen sich Regelsatz-Dateien direkt im Webbrowser bearbeiten
---

# Bearbeiten von Regelsatz-Dateien

## Einführung

Dieses Plugin dient zur direkten Bearbeitung von Regelsatz-Dateien in der Benutzeroberfläche im Webbrowser.

| Details |  |
| :--- | :--- |
| Identifier | intranda\_administration\_ruleset\_edition |
| Source code | [https://github.com/intranda/goobi-plugin-administration-ruleset-edition](https://github.com/intranda/goobi-plugin-administration-ruleset-edition) |
| Lizenz | GPL 2.0 oder neuer |
| Kompatibilität | Goobi workflow 2020.03 |
| Dokumentationsdatum | 13.09.2021 |

## Installation

Zur Nutzung des Plugins müssen diese beiden Dateien an folgende Orte kopiert werden:

```text
/opt/digiverso/goobi/plugins/administration/plugin_intranda_administration_ruleset_edition.jar
/opt/digiverso/goobi/plugins/GUI/plugin_intranda_administration_ruleset_edition-GUI.jar
```

Die Konfiguration des Plugins findet innerhalb dessen Konfigurationsdatei `plugin_intranda_administration_ruleset_edition.xml` statt. Diese wird unter folgendem Pfad erwartet:

```text
/opt/digiverso/goobi/config/plugin_intranda_administration_ruleset_edition.xml
```

## Konfiguration des Plugins

Die Konfiguration des Plugins ist folgendermaßen aufgebaut:

```markup
<config_plugin>
	<rulesetDirectory>/opt/digiverso/goobi/rulesets/</rulesetDirectory>
	<!-- By editing a ruleset file in the browser GUI, a backup file will be stored in the backup directory -->
	<rulesetBackupDirectory>/opt/digiverso/goobi/rulesets/backup/</rulesetBackupDirectory>
	<!-- backup files will be stored as ruleset.xml.1, ruleset.xml.2, ..., ruleset.xml.n -->
	<numberOfBackupFiles>8</numberOfBackupFiles>
</config_plugin>
```

Die Parameter innerhalb dieser Konfigurationsdatei haben folgende Bedeutungen:

| Wert | Beschreibung |
| :--- | :--- |
| `rulesetDirectory` | Dies ist der Pfad, an dem die Regelsatz-Dateien gespeichert werden. |
| `rulesetBackupDirectory` | Dies ist der Pfad, an dem die Backup-Dateien nach dem Bearbeiten der Regelsatz-Dateien gespeichert werden. |
| `numberOfBackupFiles` | Dieser ganzzahlige Wert gibt an, wie viele Backup-Dateien pro Regelsatz-Datei gespeichert bleiben, bevor sie durch neue Backups überschrieben werden. |

## Integration des Plugins in den Workflow

Nach der Installation ist das Plugin in einem eigenen Eintrag im Administration-Menü zu finden.

Auf der linken Seite sind alle Regelsatz-Dateien aufgelistet. Diese kann man durch Anklicken des jeweiligen Bearbeiten-Symbols öffnen.

![Geschlossener Texteditor](plugin_administration_ruleset_edition_closed_texteditor.png)

Öffnet man eine Datei, erscheint auf der rechten Seite ein Texteditor, in dem die Datei bearbeitet werden kann. Speichert man dieser, wird automatisch ein Backup angelegt. Entsprechend dem eingestellten Wert in der Konfigurationsdatei bleiben eine gewisse Anzahl an älteren Backups erhalten, bevor diese durch neuere ersetzt werden.

![Geöffneter Texteditor](plugin_administration_ruleset_edition_open_texteditor.png)

Hat man eine Datei verändert und wechselt zu einer anderen, bekommt man die Möglichkeit, die Änderungen zu speichern oder zu verwerfen.

![Nachfrage der Speicherung](plugin_administration_ruleset_edition_request.png)
