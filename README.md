# html-deck-presenter

Skill de génération de diaporamas HTML autonomes avec vue présentateur privée (aperçus + notes par diapo + timer avance/retard), à partir d'une source Notion/Word/PDF et d'un thème markdown.

## Démarrage rapide
1. Fournir la source de contenu (Notion/Word/PDF).
2. Fournir un thème (voir assets/themes/exemple-sobre.md) ou en utiliser un d'exemple.
3. Le skill produit un plan.md → tu le valides → il génère le deck HTML.
4. Ouvrir le HTML par double-clic ; appuyer sur P pendant la projection pour la vue présentateur.

## Autonomie
Fichier unique, aucune dépendance réseau, polices système. Vue présentateur non partagée dans la visio.

## Guide détaillé
Voir [docs/guide-utilisateur.md](docs/guide-utilisateur.md) : piloter un deck (navigation, vue présentateur, notes, timer), générer un deck avec Claude, créer un thème, dépannage.

## Personnalisation
- Thème : voir references/theme-format.md.
- Gabarits : voir references/archetypes.md.
- Moteur : figé et versionné (references/engine-contract.md) — ne pas régénérer.
