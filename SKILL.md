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
