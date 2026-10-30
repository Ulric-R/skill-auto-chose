---
name: Auto-Chose
description: Choisir automatiquement le skill approprié selon le projet détecté — à utiliser au début d'une tâche, sur un projet nouvellement créé, ou quand une nouvelle session démarre sur un dépôt non identifié. Détecte le type de projet, charge le skill pertinent et annonce son choix.
---

# Auto-Chose : sélection automatique des skills

Rôle : éviter de devoir demander manuellement quel skill appliquer. Au début
d'une tâche sur un projet, détecter la nature du dépôt et charger le skill
correspondant s'il existe.

## Workflow

1. **Détecter le projet** : lister les fichiers marqueurs à la racine et
   dans les répertoires principaux (voir `references/mapping.md`).
   Priorité aux marqueurs les plus spécifiques :
   - `opencode.json(c)` / `.opencode/` → config du projet, vérifier `skills`
   - Fichiers de dépendances (`package.json`, `pyproject.toml`, `go.mod`…)
   - Fichiers d'infrastructure (`Dockerfile`, `docker-compose.yml`…)
   - Fichiers de CI (`.github/workflows/`…)
   - README et structure des dossiers (`src/`, `apps/`, `packages/`…)

2. **Consulter les skills disponibles** : la liste des skills autorisés
   est annoncée à chaque étape (ID + name + description). Ne jamais coder
   une liste en dur : elle deviendrait obsolète.

3. **Matcher projet ↔ descriptions** : pour chaque skill disponible,
   vérifier si sa description correspond au type de projet détecté.
   - Un projet peut correspondre à **plusieurs** skills → charger celui
     qui est le plus spécifique à la tâche en cours, pas tous.
   - Si la tâche de l'utilisateur est explicite (ex. : « ajoute un
     test »), prioriser le skill lié à la tâche sur le skill lié au
     langage.

4. **Charger** : appeler l'outil `skill` avec l'ID exact correspondant.
   Un seul skill à la fois, chargement paresseux — en charger un second
   uniquement si la tâche l'exige réellement.

5. **Annoncer le choix** en une phrase, avec la raison :
   > « Projet Node détecté (`package.json`) → skill `xxx` chargé. »
   Si aucun skill ne correspond, ne rien charger et ne rien inventer :
   continuer la tâche normalement.

6. **Re-valider** si le projet change au cours de la session (nouveau
   package installé, migration de stack…) → re-exécuter la détection.

## Règles de sélection

- **Jamais de skill forcé** : si le doute est réel (pas de marqueur
  clair), demander à l'utilisateur plutôt que charger un skill au hasard.
- **Un seul skill chargé par défaut** ; les suivants au besoin.
- **IDs exacts et sensibles à la casse** : utiliser l'ID annoncé dans la
  liste, jamais le `name` d'affichage.
- Si deux skills semblent équivalents, préférer celui dont la description
  est la plus spécifique au projet, puis annoncer le choix pour que
  l'utilisateur puisse corriger.

## Déclenchement

Ce skill s'applique en début de tâche ou de session, dès que le projet
n'a pas encore été identifié. Il ne se re-déclenche pas inutilement si un
choix a déjà été fait et que le projet n'a pas changé.
