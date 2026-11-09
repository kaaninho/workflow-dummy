# Workflow: Merge vs Rebase und weiteres für Entwicklerteams

## Vorarbeiten

Lest folgende zwei Artikel, um mit dem Thema vertraut zu werden:

### Merge vs Rebase Argument

https://shiftasia.com/community/git-merge-vs-rebase-the-great-debate-and-which-one-you-should-use-2/

### Anleitung "How to rebase"

https://www.atlassian.com/git/tutorials/merging-vs-rebasing

✅ done von allen

## Mein Vorschlag: Rebase-Vorgehen

(so wie es in der Anleitung eigentlich steht)

Hier ein paar Points dazu:

### Grundregel: nur private Branches rebasen

- Rebase nur auf Branches, auf denen außer dir niemand arbeitet.
- Sonst: doppelte Commits möglich

### Genereller Workflow

#### Während der Arbeit an einem Feature

- Person X macht Feature XYZ. Person X legt lokal einen Branch an mit
  Namen `[abc]-[XYZ]`, wobei `[abc]` für das Oberthema ("UI",
  "Backend", ...) steht.

- Person X macht Commits auf den lokalen Branch. Zur Sicherheit kann
  sie den Branch auf Remote pushen.

- Preson X macht regelmäßig Rebases auf `main`, damit sie auf
  aktuellem Stand arbeitet.
  
- Person X muss, wenn sie den lokalen Stand pushen möchte, ggf. einen
  *Force-Push* machen (siehe unten)

- Person X entfernt "unnötige" Commits via *Interactive Rebase* und
  *Squash* (s. u.)

#### Am Ende der Arbeit an einem Feature

- Person X ist fertig mit Feature XYZ und möchte in `main` integrieren.
  - Zuerst Rebase auf `main`
  - Check: Laufen alle Tests und Pipelines durch? (s. u.)
  - Push auf remote Branch
  - Auf Github-Seite: PR (Pull Request) erstellen
    - Reviewer auswählen, die sinnvoll erscheinen
  - In Zenhub: Ticket auf "Review" verschieben
  - Reviewer hat Einwände: beheben, so lange bis keine Einwände (kein
    Rebase in der Zwischenzeit!)
  - Reviewer hat keine Einwände: Noch einmal Rebase auf `main` und
    force-pushen
  - In Github: Pull Request durchführen.
  - Remote Branch löschen
  
### Befehle

Generelle Empfehlung: Benutz eine UI (Source Tree, VS Code, ...), um
die Befehle durchzuführen.


#### (Interactive) Rebase auf `main`

```bash
git rebase -i main
```

Auch ohne `-i` möglich: Dann werden einfach alle neuen Commits von
`main` unter deinen aktuellen Stand gesetzt.

Bei `-i` öffnet sich "interaktives" Fenster mit Commits und möglichen
Befehlen:

| Befehl   | Aktion                                                   |
|----------|----------------------------------------------------------|
| `pick`   | Commit unverändert übernehmen                            |
| `reword` | übernehmen, Commit-Message ändern                        |
| `edit`   | anhalten, um den Commit zu ändern                        |
| `squash` | mit vorherigem Commit vereinen, Messages zusammenführen  |
| `fixup`  | mit vorherigem Commit vereinen, eigene Message verwerfen |
| `drop`   | Commit entfernen                                         |


#### Rebase der letzten X Commits

```bash
git rebase -i HEAD~[X]
```

(also bei 3: `git rebase -i HEAD~3`

Sinnvoll für: *Squashen* von Commits!

#### Force-Push: 

- Lage: 
  - bereits gepushter privater Branch
  - habe lokal Rebase gemacht
  - möchte pushen (-> geht aber nicht "diverging ...")
- Lösung:
  - Benutze `git push --force-with-lease`. Nicht nur `--force`.

## Tests und Pipelines

Es wird verschiedene Pipelines geben:

- Tests: z. B. Pythontests oder auch Typescript-Tests, typisch:
  Unit-Tests, manchmal Integrationstests
- Codequalitäts-Tests:
  - Einrückung (Formatter, `ruff`)
  - Linter (`ruff check` ?)
- Integrationstests, Klicktests

Nur wenn Pipelines grün sind, darf PR gestellt werden
Yoyo, drink holy
another reminder to drink holy
Zelda OoT came out recently
