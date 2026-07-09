# Format du plan intermédiaire (plan.md)

Pivot révisable par l'humain (checkpoint ★). Markdown :

```markdown
# <Titre du deck>

## Diapo 1 — cover
Titre : Formation RFE
Sous-titre : Démat des factures

## Diapo 3 — agenda
- 00:00 Introduction & objectifs
- 00:10 Partie 1 · Comprendre la réforme
```

## Règles
- `# ` (une fois) = titre du deck → {{DECK_TITLE}}.
- Chaque `## Diapo <n> — <archétype>` = une diapo. `<archétype>` ∈ catalogue (archetypes.md).
- Le corps sous chaque `##` alimente les champs du patron (clé « Libellé : valeur » et/ou listes à puces selon l'archétype).
- L'ordre des `## Diapo` = l'ordre d'affichage ; l'humain peut réordonner, ajouter, supprimer, changer l'archétype à la main.
- Une image référencée est notée `Image : <chemin ou description>` et sera intégrée en base64 à l'assemblage.
