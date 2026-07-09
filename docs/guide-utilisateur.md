# Guide utilisateur — html-deck-presenter

Ce guide explique **ce que fait le skill**, **comment piloter un deck déjà généré**, et **comment demander à Claude d'en générer un**. Aucun prérequis technique : le résultat est un simple fichier `.html`.

---

## 1. Ce que produit le skill

Le skill `html-deck-presenter` fabrique un **diaporama HTML** :

- **Un seul fichier `.html`**, autonome : il s'ouvre par double-clic, fonctionne **hors-ligne**, ne charge **aucune ressource réseau** (polices système, images intégrées en base64).
- Doté d'une **vue présentateur privée** en fenêtre séparée : aperçus de la diapo en cours et de la suivante, **notes par diapo**, et un **timer avance/retard**. Cette fenêtre **n'est pas partagée** quand vous ne partagez que la fenêtre principale en visio.

C'est un support de **présentation projetable**, pas un document Word/PDF et **jamais un PowerPoint**. Notion / Word / PDF servent uniquement de **source de contenu**.

---

## 2. Piloter un deck déjà généré

### Ouvrir

Double-cliquez sur le fichier `.html` (ou glissez-le dans un navigateur). Il s'ouvre en **mode public** — c'est la vue à projeter.

### Naviguer

| Action | Touche / geste |
|---|---|
| Diapo suivante | `→`, `↓`, ou flèche de navigation à l'écran |
| Diapo précédente | `←`, `↑`, ou flèche à l'écran |
| Aller à une diapo | clic sur un point (dots) en bas |
| **Ouvrir la vue présentateur** | **`P`** |

### La vue présentateur (touche `P`)

Depuis la fenêtre publique, appuyez sur **`P`** : une **seconde fenêtre** s'ouvre (popup). Elle affiche :

- **Aperçu en cours + aperçu de la diapo suivante** (synchronisés avec la fenêtre publique).
- **Notes de la diapo** — deux couches :
  - **Notes préparées** (lecture seule) : le texte saisi à l'avance dans le deck.
  - **Notes du jour** (zone éditable) : ce que vous tapez en direct. Elles sont **sauvegardées automatiquement** dans le navigateur (localStorage). Boutons `📂 Charger` / `🆕 Créer` pour lier un **fichier de notes** (`notes-jour.json`) si votre navigateur le permet.
- **Timer** : `▶ Démarrer` / `⏸ Pause`, `↺` remise à zéro, et l'horloge de session. À côté, l'**écart avance/retard** : la différence entre votre temps écoulé et le **jalon horaire** prévu pour la diapo courante (voir §6, les jalons sont déduits d'une diapo « agenda » si elle existe).
- Navigation `◀ ▶` : elle **pilote aussi la fenêtre publique** (les deux restent synchronisées).

> **Popup bloquée ?** Si rien ne s'ouvre, autorisez les popups pour ce fichier puis réappuyez sur `P`. Un message le rappelle en bas de l'écran.

### Utilisation en visioconférence

1. Ouvrez le deck, appuyez sur `P` pour sortir la vue présentateur sur votre écran.
2. Dans l'outil de visio, **partagez uniquement la fenêtre publique** (pas tout l'écran).
3. Vous voyez notes + timer en privé ; l'audience ne voit que les diapos.

### Modes d'URL (avancé)

Le comportement dépend du `#` à la fin de l'URL :

- *(rien)* → **public** (à projeter).
- `#presenter` → la **vue présentateur** seule (c'est ce que la touche `P` ouvre).
- `#bare` → **projection nue** : une diapo, sans habillage ni transition (utilisé en interne pour les aperçus).

---

## 3. Générer un deck (le workflow avec Claude)

La génération est **assistée par Claude** via le skill. Vous fournissez le fond, Claude assemble la forme, et **vous validez** avant le rendu final.

### Ce qu'il vous faut

1. **Une source de contenu**, au choix :
   - **PDF** — lu nativement.
   - **Notion** — via le connecteur Notion (s'il est branché ; sinon Claude vous demandera un export PDF/Word).
   - **Word (`.docx`)** — extraction « best-effort » du texte et des images.
2. **Un thème** (fichier `.md`) — le vôtre, ou l'un des exemples fournis (`assets/themes/exemple-sobre.md`, `exemple-clair.md`). Voir §5.

### Comment le lancer

Demandez-le simplement à Claude, en pointant votre source et votre thème. Par exemple :

> « Génère un deck à partir de ce PDF avec le thème `exemple-sobre`. »
> « Fais-moi une présentation à partir de cette page Notion, thème maison ci-joint. »

### Les 6 étapes

```
1. Entrée      → Claude extrait titres, textes, listes, images de la source
2. plan.md     → Claude propose un plan : 1 section = 1 diapo + un archétype suggéré
3. Checkpoint ★→ VOUS validez / ajustez (voir ci-dessous) — étape obligatoire
4. Thème       → Claude charge et valide le thème (couleurs, polices, contraste)
5. Assemblage  → Claude injecte contenu + thème dans le moteur figé, intègre les images
6. Vérification→ Claude sert le deck et teste public + présentateur au navigateur
```

### Le checkpoint ★ (votre moment de contrôle)

Avant tout rendu, Claude vous montre le **`plan.md`** (voir §7). Vous pouvez :

- **changer l'archétype** d'une diapo (ex. passer de `points` à `stats`) ;
- **corriger un texte**, un titre, une accroche ;
- **réordonner, ajouter ou supprimer** des diapos ;
- **réassigner ou retirer une image**.

Claude ne passe à l'assemblage **qu'après votre feu vert**. Tout contenu source non exploité est signalé à cette étape.

### Le résultat

Un fichier `.html` unique, vérifié au navigateur. Vous le récupérez, double-clic, et c'est prêt (§2).

---

## 4. Comment ça marche (en bref)

Le cœur du skill est un **moteur figé** : `assets/engine.template.html`. Il contient **tout le comportement** (les 3 modes, la synchro public/présentateur, les notes, le timer) et n'est **jamais réécrit** — on le recopie tel quel et on remplit seulement 5 emplacements :

| Emplacement | Ce qui y est injecté |
|---|---|
| `{{DECK_TITLE}}` | le titre du deck |
| `{{THEME_VARS}}` | les couleurs / polices du thème |
| `{{SLIDES}}` | le HTML des diapos (un archétype rempli par diapo) |
| `{{PREP_NOTES}}` | les notes préparées, une par diapo |
| `{{MILESTONES}}` | les jalons horaires du timer |

Conséquence : le comportement est **identique et stable** d'un deck à l'autre — seuls le contenu et le thème changent.

---

## 5. Les thèmes

Un thème est un fichier markdown lisible. Exemple minimal :

```markdown
# Thème — Mon thème

## Couleurs
- Fond sombre : #14181d
- Fond clair : #f7f8fa
- Accent primaire : #4c8bf5
- Accent secondaire : #35c28a
- Encre (texte) : #14181d

## Polices
- Affichage : Georgia, serif
- Texte : system-ui, sans-serif
- Mono : ui-monospace, monospace

## Options
- Thème par défaut : sombre
- Durée totale (min) : 60
```

**Les 8 rôles** (5 couleurs + 3 polices) sont appliqués par-dessus la palette par défaut du moteur. Un rôle omis garde simplement la valeur par défaut.

**Règles à respecter :**
- **Couleurs** : hex valide (`#rgb` ou `#rrggbb`).
- **Polices** : toujours une pile finissant par une famille générique (`serif`, `sans-serif` ou `monospace`). Aucune police n'est téléchargée — une police nommée n'est qu'une **préférence** en tête de pile ; sur un poste sans cette police, la suivante de la pile s'applique.
- **Contraste** : Claude alerte si le texte risque d'être illisible sur un fond (visée ~4.5:1), sans bloquer.
- **Options** : `Thème par défaut` = `sombre` ou `clair` ; `Durée totale (min)` = borne du timer si aucun agenda horodaté n'est détecté.

Détail complet : `references/theme-format.md`.

---

## 6. Les archétypes (gabarits de diapo)

Chaque diapo suit l'un des **10 gabarits**. Claude en propose un par section (heuristiques), vous tranchez au checkpoint.

| Archétype | Rôle | Détecté quand… |
|---|---|---|
| `cover` | couverture d'ouverture | 1re section (titre + accroche) |
| `agenda` | déroulé horodaté | liste d'items en `hh:mm` |
| `divider` | intercalaire de partie | titre court seul / « Partie N » |
| `big-title` | affirmation forte | une phrase unique mise en avant |
| `points` | points clés en cartes | énumération à puces (3–6) |
| `stats` | chiffres clés | plusieurs nombres/pourcentages |
| `image-full` | visuel plein cadre | image dominante |
| `list` | liste numérotée / ressources | liste d'étapes ou de liens |
| `cta` | appel à l'action / conclusion | invitation à agir |
| `qa` | « questions & échanges » | dernière section d'échanges |

> Un **agenda horodaté** a un rôle spécial : ses horaires servent à **dériver automatiquement les jalons du timer** (l'écart avance/retard de la vue présentateur). Sans agenda, le timer utilise deux bornes (début → `Durée totale`).

Détail + patrons HTML : `references/archetypes.md`.

---

## 7. Le plan intermédiaire (`plan.md`)

C'est le **pivot révisable** présenté au checkpoint. Format :

```markdown
# Titre du deck

## Diapo 1 — cover
Titre : Titre de la présentation
Sous-titre : Accroche en une ligne

## Diapo 2 — agenda
- 00:00 Introduction & objectifs
- 00:10 Partie 1 · Le principe
- 00:30 Partie 2 · Démonstration
```

- `# ` (une seule fois) = titre du deck.
- Chaque `## Diapo <n> — <archétype>` = une diapo ; le mot après `—` est l'archétype (§6), **éditable à la main**.
- Le corps sous chaque section alimente les champs du gabarit.
- **Réordonner / ajouter / supprimer** une diapo = éditer ce fichier.
- Une image se note `Image : <chemin ou description>` (intégrée en base64 à l'assemblage).

Détail : `references/plan-format.md`.

---

## 8. Dépannage (FAQ)

- **La vue présentateur ne s'ouvre pas.** Popups bloquées : autorisez-les pour ce fichier, réappuyez sur `P`.
- **L'audience a vu mes notes.** Vous avez partagé tout l'écran ou la mauvaise fenêtre. Partagez **uniquement la fenêtre publique**.
- **Mes notes du jour ont disparu.** Elles sont stockées dans le navigateur (localStorage) **du poste utilisé**. Sur un autre poste/navigateur, utilisez `📂 Charger` sur un fichier `notes-jour.json` exporté via `🆕 Créer`.
- **Le texte est peu lisible.** Contraste couleur trop faible : ajustez `Encre` / `Fond` du thème (§5).
- **Une image manque.** Elle n'a pas pu être extraite de la source : signalez-la au checkpoint, fournissez-la à part.
- **Je veux modifier le comportement (timer, notes…).** Le moteur est **figé et versionné** (`references/engine-contract.md`) : on ne le régénère pas. Une évolution du moteur incrémente sa version ; les decks déjà produits restent inchangés.

---

## 9. Où trouver quoi

| Fichier | Contenu |
|---|---|
| `SKILL.md` | le workflow orchestré (ce que Claude suit) |
| `assets/engine.template.html` | le moteur figé + zones d'injection |
| `assets/themes/` | thèmes d'exemple (`exemple-sobre`, `exemple-clair`) |
| `assets/demo-deck.html` | un deck de démonstration généré |
| `references/theme-format.md` | format et validation des thèmes |
| `references/archetypes.md` | catalogue des 10 gabarits + heuristiques |
| `references/plan-format.md` | grammaire du `plan.md` |
| `references/engine-contract.md` | contrat du moteur (zones, invariants, version) |
