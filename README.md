# Skills

Eigene Skills für Claude Code, gesammelt als Plugin-Marketplace.

## Aufbau

```text
.claude-plugin/     Katalog (marketplace.json) und Plugin (plugin.json)
skills/             ein Verzeichnis je eigenem Skill, jeweils mit SKILL.md
vendor/             übernommene Skills aus fremden Repositories
```

## Skills

| Skill | Zweck |
| --- | --- |
| [commit-message](skills/commit-message/SKILL.md) | Bereitet eine Commit-Nachricht nach Conventional Commits 1.0.0 vor, ohne zu committen. |

## Beim Entwickeln: Symlink

```bash
mkdir -p ~/.claude/skills
ln -s ~/Git/skills/skills/commit-message ~/.claude/skills/commit-message
```

Danach Claude Code neu starten, auch wenn es bereits läuft. Ab der nächsten Sitzung erscheint der Skill unter `/skills`. Der Aufruf lautet dann `/commit-message`.

Ein Neustart ist nur die Einrichtung notwendig. Spätere Änderungen an einer verlinkten `SKILL.md` übernimmt Claude Code während laufenden Sitzungen.

## Beim Verteilen: Marketplace

```bash
claude plugin marketplace add ~/Git/skills
claude plugin install eigene-skills@benjamin-skills
```

Aufruf: `/eigene-skills:commit-message`.

Nach einer Änderung erst die `version` in `.claude-plugin/plugin.json` erhöhen, sonst bleibt die installierte Kopie auf dem alten Stand:

```bash
git commit -am "…"
claude plugin marketplace update benjamin-skills
claude plugin update eigene-skills@benjamin-skills
```

## Prüfen

```bash
claude plugin validate .
```

## Siehe auch

- Anleitung im Vault: `IT/KI/Eigene Skills in einem Git-Repository verwalten und in Claude einbinden.md`
- Claude Code, Skills: https://code.claude.com/docs/en/skills
- Claude Code, Plugin-Marketplaces: https://code.claude.com/docs/en/plugin-marketplaces
