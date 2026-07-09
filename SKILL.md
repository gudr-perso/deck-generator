---
name: html-deck-presenter
description: Génère un diaporama HTML autonome piloté par un pop-up présentateur (aperçus + notes par diapo + timer avance/retard) à partir d'une source Notion/Word/PDF et d'un thème markdown. À utiliser quand l'utilisateur veut fabriquer/mettre à jour un support de présentation HTML projetable en visio, avec vue présentateur privée.
---

# Générer un diaporama HTML présentateur

## Quand
L'utilisateur fournit un contenu (Notion/Word/PDF) et veut un deck HTML autonome + vue présentateur. Sortie = TOUJOURS un HTML unique (jamais un PPTX).

## Principe
Le moteur (assets/engine.template.html) est FIGÉ : recopié verbatim, on ne remplit que les zones de substitution (voir references/engine-contract.md). Le LLM juge le contenu et l'archétype, et assemble par substitution.

## Workflow (détaillé plus bas)
1. Entrée → contenu   2. plan.md   3. Checkpoint ★   4. Thème   5. Assemblage   6. Vérif navigateur

## 1. Entrée → contenu
Selon la source fournie :
- **PDF** : lire le fichier avec l'outil de lecture de fichiers (lecture PDF native, aucun runtime). Extraire titres, paragraphes, listes, images.
- **Notion** : si les outils MCP Notion sont disponibles, récupérer la page (notion-fetch). SINON : dégradation propre → message « Le connecteur Notion n'est pas disponible : connecte Notion, ou fournis un export PDF/Word. » et s'arrêter proprement.
- **Word (.docx)** : extraction best-effort du texte structuré (titres/paragraphes/listes) et des images. Signaler ce qui n'a pas pu être extrait.

Résultat : une structure de contenu (titres hiérarchiques + blocs + images) qui alimentera le plan. Tout élément non exploité est listé au checkpoint ★.

## 2. Générer le plan.md
À partir du contenu, produire un plan.md (voir references/plan-format.md). Pour chaque section : proposer un archétype via les heuristiques (references/archetypes.md) et remplir les champs.

## 3. Checkpoint de revue ★ (obligatoire)
Présenter le plan.md à l'utilisateur. Il peut : changer un archétype, corriger un texte, réordonner, ajouter/supprimer une diapo, réassigner une image. Ne PAS passer à l'assemblage sans validation explicite. Lister ici tout contenu source non exploité ou toute extraction partielle.

## 4. Thème
Charger le theme.md fourni (ou proposer assets/themes/exemple-sobre.md). Valider (references/theme-format.md : hex, contraste heuristique, piles de polices). Construire la chaîne {{THEME_VARS}} = liste `--var:valeur;` selon le mapping rôle→variable.

## 5. Assemblage (substitution — ne PAS régénérer le moteur)
1. Copier assets/engine.template.html VERBATIM vers le fichier de sortie.
2. Remplacer {{THEME_VARS}} par la chaîne de variables.
3. Remplacer {{DECK_TITLE}} par le titre du plan.
4. Remplacer {{SLIDES}} par la concaténation des diapos : pour chaque `## Diapo` du plan, rendre le patron d'archétype (archetypes.md) avec ses champs. La 1re diapo porte la classe `active`. Fond clair → `data-theme="light"` selon l'archétype.
5. Images : intégrer chaque image en base64 (WebP si possible) directement dans le HTML (aucune ressource externe).
6. Seeds :
   - {{PREP_NOTES}} = `1:"", 2:"", … N:""` (N = nombre de diapos).
   - {{MILESTONES}} = si une diapo `agenda` horodatée existe, mapper chaque horaire au numéro de diapo de la section correspondante ; sinon `1:0, N:<durationTotalMin>`.
7. Vérifier qu'il ne reste AUCUN marqueur `{{…}}` non substitué.
