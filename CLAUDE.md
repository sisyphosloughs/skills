# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

Dieses Repository enthält keinen Anwendungscode. Es ist ein Plugin-Marketplace, der eigene Claude-Code-Skills sammelt. Es gibt daher keinen Build und keine Tests, nur Validierung.

## Befehle

```bash
claude plugin validate .                          # Katalog: .claude-plugin/marketplace.json + plugin.json
claude plugin validate ./skills                   # alle SKILL.md-Dateien (Frontmatter)
claude plugin validate ./skills/commit-message    # ein einzelner Skill

claude plugin marketplace add ~/Git/skills        # Katalog einmalig registrieren
claude plugin install eigene-skills@benjamin-skills
```

`plugin.json` trägt eine feste `version` (derzeit `0.1`). Claude Code leitet die Version damit nicht mehr aus dem Commit ab: **eine Aktualisierung erreicht installierte Kopien erst, wenn die Nummer erhöht wird.** Nach einer inhaltlichen Änderung an einem Skill also `version` anheben.

## Aufbau

- `.claude-plugin/plugin.json` — das Plugin `eigene-skills`. Es lädt seine Skills implizit aus `skills/`, deshalb genügt eine einzige Plugin-Definition ohne `skills`-Feld.
- `.claude-plugin/marketplace.json` — der Katalog `benjamin-skills`. Sein einziger Eintrag zeigt mit `"source": "./"` auf dieses Repository selbst.
- `skills/<name>/SKILL.md` — ein Verzeichnis je Skill. **Der Verzeichnisname wird zum Befehl** (`skills/commit-message` → `/commit-message`), also Kleinbuchstaben und Bindestriche. Das Feld `name` im Frontmatter muss dem Verzeichnisnamen entsprechen.
- `vendor/` — Übernommenes aus fremden Repositories, sichtbar getrennt von Eigenem. Noch leer. Die Aufnahme geschieht als Submodul, damit die Herkunft erhalten bleibt und nichts davon hier bearbeitet wird:

  ```bash
  git submodule add https://github.com/<owner>/<repo>.git vendor/<name>
  ```

  Was in `vendor/` liegt, verteilt dieses Plugin nicht mit: `plugin.json` lädt ausschließlich `skills/`.

Lange Referenztexte gehören in eigene Dateien neben die `SKILL.md`, nicht in sie hinein: der Rumpf eines geladenen Skills bleibt für die restliche Sitzung im Kontext.

## Einen Skill hinzufügen

1. `skills/<name>/SKILL.md` anlegen; `name` im Frontmatter gleich dem Verzeichnisnamen.
2. Eine Zeile in der Skill-Tabelle in `README.md` ergänzen. Die Tabelle wird von Hand gepflegt und ist sonst sofort unvollständig.
3. `claude plugin validate ./skills`.
4. Für die lokale Arbeit den Symlink setzen: `ln -s ~/Git/skills/skills/<name> ~/.claude/skills/<name>`. Ein neuer Symlink wird erst nach einem Neustart von Claude Code erkannt; spätere Änderungen an der verlinkten `SKILL.md` wirken dann sofort.

## Zwei Verteilwege, mit unterschiedlichem Verhalten

Beide Wege können gleichzeitig aktiv sein, mit demselben Skill unter zwei Namen.

1. **Symlink** (`~/.claude/skills/<name>` → `skills/<name>`): Änderungen an der `SKILL.md` wirken sofort in der laufenden Sitzung. Aufruf `/<name>`. Nur lokal, nicht in Cowork- oder Cloud-Sitzungen.
2. **Plugin** (`claude plugin install eigene-skills@benjamin-skills`): Claude Code **kopiert** nach `~/.claude/plugins/cache`. Eine Bearbeitung im Repository wirkt erst nach `version`-Erhöhung in `plugin.json`, `git commit`, `claude plugin marketplace update benjamin-skills` und `claude plugin update eigene-skills@benjamin-skills`. Aufruf `/eigene-skills:<name>`. Verweise innerhalb eines Skills dürfen nicht über `../` aus dem Plugin-Verzeichnis herausführen.

Beim Testen einer Änderung also zuerst klären, welcher der beiden Wege gerade geladen ist — ein scheinbar wirkungsloser Edit ist meist der Plugin-Cache.

## Konventionen für eine SKILL.md

- **`description`** ist das einzige Feld, das dauerhaft im Kontext liegt; es entscheidet allein, ob Claude den Skill lädt. Es benennt deshalb sowohl den Zweck als auch die Auslöser in der Formulierung, die der Nutzer tatsächlich tippt.
- **Dynamischer Kontext** wird als ``!`befehl` `` eingebettet, inline in einer Zeile — nicht als ```` ```! ````-Block, der nur in Slash-Commands gilt. Was ein Skill so ausführt, muss in `allowed-tools` gedeckt sein; diese Freigabe gilt nur für die Runde, in der der Skill geladen wird.
- **`allowed-tools`** ist eine kommagetrennte Liste, deren Einträge das Muster mit `*` eingrenzen: `Bash(git status *), Bash(git diff *)`.
- **Cowork und Cloud**: Dort sind ausschließlich die Frontmatter-Felder `name`, `description`, `license`, `compatibility`, `metadata` und `allowed-tools` erlaubt, und die Shell-Einbettung wird nicht ausgeführt. Ein Skill, der dort laufen soll, darf sich nicht auf ``!`befehl` `` verlassen.
- Skills und Dokumentation sind auf Deutsch verfasst und duzen den Leser. Ausnahme: Was ein Skill als Ergebnis produziert, kann eine andere Sprache vorschreiben — `commit-message` etwa verlangt englische Commit-Nachrichten.

## Commits

Für Commits in diesem Repository gilt der eigene Skill `commit-message` und nicht eine abweichende Standardvorgabe der Sitzung: Conventional Commits 1.0.0, englisch, und **keine Footer, die auf eine Beteiligung einer KI hinweisen** — weder `Co-Authored-By: Claude` noch ein `Generated with`-Hinweis. Das Repository ist die Referenz für seine eigene Konvention; `git log` soll zeigen, was der Skill erzeugt.

## Herkunft

Der Aufbau folgt der Anleitung `IT/KI/Eigene Skills in einem Git-Repository verwalten und in Claude einbinden.md` im Obsidian-Vault des Nutzers (`~/Notizen`). Strukturelle Änderungen an diesem Repository gehören dort ebenfalls nachgezogen.
