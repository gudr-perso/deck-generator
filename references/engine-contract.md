# Contrat du moteur (engine.template.html)

**Version : engine v1** (incrémenter à chaque évolution du moteur ; les decks déjà générés ne changent pas).

## Le moteur est figé
Ne jamais régénérer ni réécrire le JS/CSS du moteur. On le recopie VERBATIM et on ne remplit que les zones de substitution ci-dessous.

## Zones de substitution
| Marqueur | Emplacement | Contenu injecté |
|---|---|---|
| `/*{{THEME_VARS}}*/` | fin du `:root{ … }` de charte (CSS principal) | surcharges CSS de charte (voir theme-format.md) |
| `<!--{{DECK_TITLE}}-->` | `<title>` | titre du deck |
| `<!--{{SLIDES}}-->` | intérieur de `.deck`, après le chrome (progress/logo/counter) | HTML des diapos (voir archetypes.md) |
| `/*{{PREP_NOTES}}*/` | corps de `const PREP_NOTES` | `1:"", … N:""` |
| `/*{{MILESTONES}}*/` | corps de `const MILESTONES` | `1:0, … N:<durée>` |

## THEME_VARS : surcharge, pas remplacement
Le `:root` principal conserve **toute la palette par défaut du moteur** (`--dark`, `--orange`, `--paper`, `--ink`, `--green-*`, `--cyan-*`, `--f-serif`…). Le marqueur `/*{{THEME_VARS}}*/` est injecté **à la fin** de ce bloc : les variables du thème y sont ajoutées et **surchargent** les valeurs par défaut par cascade CSS (la dernière déclaration gagne). Un thème ne fournit qu'un sous-ensemble de rôles ; les nuances dérivées non surchargées gardent la valeur par défaut du moteur. Ainsi un deck reste toujours fonctionnel, même avec un thème minimal.

## Invariants garantis (ne pas casser lors de l'injection)
- Un conteneur `.deck` contenant le chrome (`.progress`, `.logo-badge`, `.counter`) puis les éléments `.slide` (1 par diapo, ordre = ordre d'affichage). La navigation (`.nav`, `#prev`, `#dots`, `#next`) est hors `.deck`.
- Chaque `.slide` peut porter `data-theme="light"` pour une diapo à fond clair.
- La 1re diapo porte la classe `active`.
- Modes via `location.hash` : public (défaut), `#presenter`, `#bare`.
- Numérotation présentateur 1-based = index 0-based + 1 (PREP_NOTES/MILESTONES sont 1-based).
- Aucune ressource réseau : polices système, images base64.
