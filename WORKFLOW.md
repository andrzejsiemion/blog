# Workflow

## Gałęzie

- `main` — publiczna, pushowana do remote (GitHub Pages), bez prywatnych plików
- `local` — lokalna, nigdy nie pushowana, zawiera `CLAUDE.md` i `WORKFLOW.md`

## Codzienna praca

```bash
# Pracuj na local (masz dostęp do CLAUDE.md i WORKFLOW.md)
git checkout local

# Twórz posty, edytuj pliki...
git add content/blog/...
git commit -m "..."
```

## Publikowanie zmian

```bash
# Przełącz na main i cherry-pick commitów z local (bez commita CLAUDE.md/WORKFLOW.md)
git checkout main
git cherry-pick <hash-commita>
git push
```

## Synchronizacja local po zmianach w main

```bash
git checkout local
git rebase main
```

## Zabezpieczenia

- Pre-push hook w `.git/hooks/pre-push` blokuje przypadkowy push gałęzi `local`
- `CLAUDE.md` i `WORKFLOW.md` istnieją tylko w gałęzi `local`
