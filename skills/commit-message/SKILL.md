---
name: commit-message
description: Bereitet eine Commit-Nachricht nach Conventional Commits 1.0.0 vor, ohne zu committen. Verwende diesen Skill immer dann, wenn es um einen Commit, eine Commit-Nachricht oder das Festschreiben von Änderungen geht, auch wenn ich den Skill nicht ausdrücklich nenne.
allowed-tools: Bash(git status *), Bash(git diff *)
---

## Aktuelle Änderungen

- Status: !`git status --short`
- Diff: !`git diff HEAD || true`

## Ablauf

Arbeite diese Schritte vollständig ab:

1. Lies die Änderungen oben. Verlasse Dich nicht auf die Dateinamen allein.
2. Gruppiere die Änderungen thematisch. Gehören sie zu mehreren unabhängigen Themen, erstelle mehrere getrennte Vorschläge und gib zu jedem an, welche Dateien dazugehören.
3. Bestimme für jeden Vorschlag den Typ.
4. Bestimme, ob ein Scope sinnvoll ist. Wenn ja, wähle ein Substantiv, das den betroffenen Teil der Codebasis benennt.
5. Formuliere die Beschreibung: kurz, im Imperativ, kleingeschrieben, ohne Punkt am Ende, höchstens 72 Zeichen einschließlich Präfix.
6. Ergänze bei Bedarf einen Body. Beschreibe dort das Warum, nicht das Was. Das Was steht im Diff.
7. Prüfe, ob die Änderung ein Breaking Change ist, und kennzeichne sie entsprechend.
8. Gib die fertige Nachricht als Codeblock aus, als Klartext und nicht als `git commit`-Kommando.
9. Führe den Commit nicht aus. Warte auf meine Freigabe.

## Format

```
<type>[optionaler scope]: <beschreibung>

[optionaler body]

[optionale footer]
```

- Der Typ steht am Anfang, gefolgt von optionalem Scope, optionalem `!`, einem Doppelpunkt und einem Leerzeichen.
- Der Scope steht in runden Klammern, zum Beispiel `fix(parser):`.
- Der Body beginnt nach genau einer Leerzeile nach der Beschreibung.
- Footer beginnen nach genau einer Leerzeile nach dem Body.
- Typen und Scopes schreibst Du klein. Einzige Ausnahme: `BREAKING CHANGE` steht in Großbuchstaben.
- Commit-Nachrichten schreibst Du auf Englisch.

## Erlaubte Typen

- `feat`: neue Funktion, entspricht MINOR nach SemVer
- `fix`: Fehlerbehebung, entspricht PATCH nach SemVer
- `docs`: ausschließlich Dokumentation
- `style`: Formatierung ohne Auswirkung auf das Verhalten
- `refactor`: Umbau ohne neue Funktion und ohne Fehlerbehebung
- `perf`: Verbesserung der Performance
- `test`: Tests ergänzt oder korrigiert
- `build`: Build-System oder externe Abhängigkeiten
- `ci`: Konfiguration und Skripte der CI
- `chore`: sonstige Wartung ohne Auswirkung auf Quellcode oder Tests
- `revert`: Rücknahme eines früheren Commits

Verwende keine anderen Typen. Passt eine Änderung auf mehrere Typen, teile sie in mehrere Commits auf.

## Breaking Changes

Kennzeichne einen Breaking Change auf eine der beiden Arten:

- ein `!` direkt vor dem Doppelpunkt, zum Beispiel `feat(api)!: ...`
- ein Footer `BREAKING CHANGE: <beschreibung>`

Bei `!` darf der Footer entfallen. Dann beschreibt die Beschreibung den Bruch.

## Footer

- Ein Footer besteht aus einem Token, gefolgt von `: ` oder ` #`, und einem Wert.
- Im Token ersetzt Du Leerzeichen durch Bindestriche, zum Beispiel `Reviewed-by`. Ausnahme: `BREAKING CHANGE`.
- Verweise auf Tickets gehören in einen Footer, zum Beispiel `Refs: #123`.
- Füge keine Footer hinzu, die auf eine Beteiligung einer KI hinweisen, also weder `Co-Authored-By: Claude` noch einen `Generated with`-Hinweis.

## Beispiele

```
feat(lang): add Polish language
```

```
feat!: drop support for Node 6

BREAKING CHANGE: use JavaScript features not available in Node 6.
```
