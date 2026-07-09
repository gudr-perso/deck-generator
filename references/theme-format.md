# Format du thème (theme.md)

Fichier markdown. Parsing : lignes `- Libellé : valeur` sous des titres `##`.

## Exemple
```markdown
# Thème — <nom>

## Couleurs
- Fond sombre : #04271b
- Fond clair : #fffdf7
- Accent primaire : #ff8200
- Accent secondaire : #3ad29f
- Encre (texte) : #12130f

## Polices
- Affichage : Fraunces, Georgia, serif
- Texte : Manrope, system-ui, sans-serif
- Mono : "JetBrains Mono", monospace

## Options
- Thème par défaut : sombre
- Durée totale (min) : 120
```

## Mapping rôle → variable CSS (injecté dans {{THEME_VARS}})
Les valeurs sont injectées **à la fin** du `:root` de charte et **surchargent** les
défauts du moteur par cascade (voir engine-contract.md). Un rôle absent du theme.md
garde donc simplement la valeur par défaut du moteur.

| Rôle (theme.md) | Variable CSS (réelle, du moteur) | Défaut moteur |
|---|---|---|
| Fond sombre | --dark | #0a3d2b |
| Fond clair | --paper | #f2f9fb |
| Accent primaire | --orange | #ff8200 |
| Accent secondaire | --green | #31b700 |
| Encre (texte) | --ink | #232020 |
| Affichage | --f-serif | 'Fraunces',Georgia,serif |
| Texte | --f-sans | 'Manrope',system-ui,sans-serif |
| Mono | --f-mono | 'JetBrains Mono',monospace |

Les noms de variables ci-dessus existent tels quels dans engine.template.html.
Les nuances dérivées (`--green-light`, `--cyan-*`, `--peach`, `--ice-*`…) ne sont
pas exposées comme rôles en v1 : elles conservent la valeur par défaut du moteur.

## Règles
- Polices : toujours une pile finissant par une famille générique (serif/sans-serif/monospace). Aucune police n'est chargée par le réseau ; une police nommée n'est qu'une préférence en tête de pile.
- Options.durationTotalMin : borne haute par défaut des jalons du timer si aucun agenda détecté.
- Options.defaultTheme : sombre|clair.
