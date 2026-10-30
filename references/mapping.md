# Mapping : marqueurs projet → critères de choix

Table de détection. Elle guide le choix du skill ; elle ne remplace jamais
la liste réelle des skills disponibles (annoncée au modèle).

## Priorité de détection

Chercher les marqueurs de haut de liste en premier. Le premier groupe
définit le type principal ; les suivants affinent.

| Priorité | Marqueur détecté | Type de projet | Skill(s) pertinents à chercher |
| --- | --- | --- | --- |
| 1 | `opencode.json`, `opencode.jsonc`, `.opencode/` | Config OpenCode du projet | skills liés à la config/agents du projet |
| 2 | `package.json` (+ `yarn.lock`/`pnpm-lock.yaml`/`bun.lockb`) | Node.js / frontend / monorepo JS | skills front-end, React, test JS, build |
| 3 | `pyproject.toml`, `requirements.txt`, `setup.py` | Python | skills Python, data, test Python |
| 4 | `go.mod` | Go | skills Go, backend |
| 5 | `Cargo.toml` | Rust | skills Rust |
| 6 | `composer.json` | PHP | skills PHP, WordPress… |
| 7 | `Gemfile` | Ruby | skills Ruby, Rails |
| 8 | `pom.xml`, `build.gradle` | Java / JVM | skills JVM |
| 9 | `pubspec.yaml` | Dart / Flutter | skills Flutter |
| 10 | `*.csproj`, `*.sln` | .NET | skills .NET |
| 11 | `Dockerfile`, `docker-compose.yml` | Infra / conteneurs | skills déploiement, Docker |
| 12 | `.github/workflows/`, `.gitlab-ci.yml` | CI/CD | skills CI, release |
| 13 | `README.md` seul | Projet non typé | lire le README avant de choisir |

## Affinage (fichiers secondaires)

| Fichier | Indication |
| --- | --- |
| `vite.config.*`, `webpack.config.*`, `next.config.*` | Front-end buildé → skills UI/style |
| `tailwind.config.*` | Tailwind → skill ciblé Tailwind s'il existe |
| `tsconfig.json` | TypeScript → privilégier les skills TS |
| `.eslintrc*`, `biome.json` | Lint/format → skills qualité de code |
| `.prettierrc*` | Formatage → skill format si disponible |
| `Makefile`, `justfile` | Commandes projet → lire les cibles avant d'agir |
| `docker-compose.yml` | Services locaux → skills infra |
| `apps/`, `packages/`, `services/` | Monorepo → identifier le sous-projet cible |
| `.env.example` | Config externe à prévoir |

## Règles de décision

1. **Plusieurs marqueurs de même priorité** (ex. : `package.json` +
   `pyproject.toml`) → projet mixte : choisir le skill correspondant à la
   partie du code sur laquelle porte la tâche, sinon demander.
2. **Monorepo** → détecter dans quel sous-projet se situe la tâche avant
   de choisir, pas seulement la racine.
3. **Aucun marqueur clair** → ne rien charger, poser une question courte
   à l'utilisateur.
4. **Tâche explicite de l'utilisateur** (« ajoute un skill », « fais une
   revue », « lance le build ») → le skill lié à la tâche prime sur le
   skill lié au langage.
5. **Le matching final se fait toujours sur les descriptions réelles des
   skills disponibles**, pas sur cette table — cette table est un aide-mémoire
   de détection, pas un catalogue fermé.
