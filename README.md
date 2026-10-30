# Auto-Chose

> Skill OpenCode qui choisit automatiquement le skill pertinent selon le projet détecté — fini les choix manuels au début de session.

**ID** : `auto-chose` · **Portée** : globale (`~/.config/opencode/skills/`) · **Langue** : FR

## Ce que fait le skill

Au début d'une tâche ou d'une session sur un dépôt non identifié :

1. **Détecte le projet** : fichiers marqueurs (`package.json`, `pyproject.toml`, `go.mod`, `Dockerfile`…)
2. **Consulte les skills disponibles** : liste dynamique annoncée au modèle — aucun catalogue codé en dur
3. **Charge le bon skill** via l'outil `skill`, un seul à la fois, priorité au plus spécifique
4. **Annonce son choix** : « Projet Node détecté → skill X chargé »
5. **Ne force rien** : aucun correspondant → ne rien charger, demander si douteux

## Structure

```text
auto-chose/
├── SKILL.md              # Workflow de détection et sélection
└── references/
    └── mapping.md        # Table marqueurs projet → critères de choix
```

## Déclenchement

- **Automatique** : début de session sur un projet non identifié, nouveau projet
- **Manuel** : `@auto-chose`
- Se re-déclenche si le projet change en cours de session

## Vérification

- [ ] IDs exacts utilisés (jamais le nom d'affichage)
- [ ] Jamais de skill forcé sur un doute
- [ ] Choix annoncé à l'utilisateur

## Skills liés

- Peut charger : `frontend-minimaliste`, `anti-dream`, `opti-skill`, `git-discipline`, `debug-methodique`, `pre-ship-review`, `session-handoff`, `git-readme`
