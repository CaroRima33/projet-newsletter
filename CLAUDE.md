# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Le projet

Système de veille et de publication semi-autonome pour Caroline sur 4 domaines
traités comme un seul groupement thématique : no-code, growth, gestion de
projet, IA. Le site web est la vitrine ; le cœur du projet est ici — des
boucles qui tournent en cron, produisent, et s'améliorent par les retours de
Caroline. Humain dans la boucle à chaque étape : rien ne se publie sans
validation explicite.

Statut actuel (2026-08-22) : collecte quotidienne en cron réel (VPS,
`crontab -l`), testée de bout en bout avec succès. Page de vote + commentaire
sur GitHub Pages, feedback capturé via issues GitHub. Boucle de rétro
hebdomadaire en cours de mise en place (cron posé, pas encore de premier run
avec assez de données pour juger). Boucle production (brouillons d'articles)
pas encore construite.

## Où est quoi

- `docs/EDITORIAL.md` — ligne éditoriale (ton, angle, ce qui est bon/mauvais).
  Le fichier le plus important : c'est lui que la boucle de rétro fait évoluer.
- `docs/SOURCES.md` — sources de veille avec score de qualité par source.
- `.claude/skills/` — une skill par tâche récurrente. `veille-collecte` et
  `retro-hebdo` existent ; `rediger-brouillon` (boucle production) vient
  après.
- `.claude/settings.json` — permissions du cron non-interactif (allowlist
  scopée : curl, git, gh, python3, lecture/écriture fichiers — pas de bypass
  général). Versionné, à ajuster si un run cron bloque sur un outil manquant.
- `prompts/` — prompts exacts lancés par les crons, versionnés séparément des
  skills pour pouvoir être ajustés sans toucher la logique générale.
- `inbox/` — sorties des boucles pour Caroline : digests markdown
  (`inbox/digests/`), brouillons à valider (à venir).
- `site/` — pages publiques déployées sur GitHub Pages (workflow
  `.github/workflows/pages.yml`). Contient la page de vote utile/pas utile de
  chaque digest (`site/digests/<date>/`), envoyée par email.
- `feedback/` — retours de Caroline en texte libre (un fichier par semaine) +
  les issues GitHub labellisées `feedback-digest` (votes + commentaires
  capturés depuis `site/`, voir section Feedback plus bas).
- `logs/` — trace de chaque run (manuel ou cron) : ce qui a été fait, coût.
- `.env` — secrets locaux (clé Resend). Gitignoré, jamais commité.

## Les 3 boucles (cible)

1. **Collecte** (quotidienne, en place) — lit `docs/SOURCES.md`, filtre selon
   `docs/EDITORIAL.md`, écrit un digest dans `inbox/digests/`.
2. **Production** (2-3×/semaine, pas encore construite) — prend les sujets
   marqués "à traiter" dans un digest, rédige un brouillon vulgarisé dans
   `inbox/`. Rien ne se publie automatiquement.
3. **Rétro hebdomadaire** (en place le 2026-08-22, la plus importante) — relit
   les issues `feedback-digest`, `feedback/` et `logs/` de la semaine, propose
   des modifications concrètes de `docs/EDITORIAL.md`, `docs/SOURCES.md` et
   `prompts/` via une **Pull Request** (jamais un push direct sur `main`) que
   Caroline relit et merge elle-même. Détails : `.claude/skills/retro-hebdo/SKILL.md`.
   La comparaison brouillons vs publié n'est pas encore possible — dépend de
   la boucle production, pas construite.

## Le contrat

- Proposer, jamais publier automatiquement.
- Chaque run laisse une trace dans `logs/`.
- Tout changement de comportement passe par un fichier versionné dans git,
  jamais par de la mémoire implicite du modèle.

## Méthode de collecte (tranchée le 2026-08-22)

Priorité RSS > fetch direct ciblé (WebFetch) > recherche web générale (dernier
recours). Décidé après un premier run manuel où la recherche web seule
remontait surtout du contenu SEO intemporel plutôt que de l'actu datée. Chaque
source de `docs/SOURCES.md` a un flux RSS testé quand il existe ; sinon une
note explique la méthode de repli. Playwright/Chrome pas installés — Caroline a
donné le feu vert (2026-08-22) pour la prochaine source qui en aurait
vraiment besoin, mais RSS + WebFetch couvrent 9 des 10 sources actuelles. La
seule source bloquée (Cairn.info, protection anti-bot) est une revue
trimestrielle payante, retirée de l'automatisation plutôt que contournée.

## Feedback utile / pas utile (mis en place le 2026-08-22)

Chaque digest a une page jumelle statique dans `site/digests/<date>/` : mêmes
sujets, avec un bouton 👍 utile / 👎 pas utile par sujet et un champ commentaire
facultatif qui apparaît après le vote. L'email quotidien pointe vers cette
page (hébergée sur GitHub Pages, pas un Artifact Claude — Caroline a demandé
ce choix pour rester sur de l'infra qu'elle possède).

Comme GitHub Pages est du statique pur (pas de backend), la page ne
sauvegarde rien elle-même : un bouton "Envoyer mes retours" en bas compile
tous les votes + commentaires (tenus en `localStorage` le temps de la
session) et ouvre une **issue GitHub pré-remplie** (`.../issues/new?...`,
label `feedback-digest`) que Caroline valide d'un clic. Aucun token ni secret
côté page — c'est elle qui soumet, avec son propre compte GitHub.

La skill `retro-hebdo` lit ces issues (`gh issue list --label feedback-digest`)
en plus de `feedback/` et `logs/` pour ajuster `docs/SOURCES.md` et
`docs/EDITORIAL.md` — voir `.claude/skills/retro-hebdo/SKILL.md`.

**Important** : voter sur la page ne suffit pas — il faut cliquer "Envoyer mes
retours" en bas de page pour que le vote devienne une vraie issue GitHub
(sinon il reste seulement dans le `localStorage` du navigateur, invisible à
la rétro).

## Hébergement et automatisation

- Repo : `https://github.com/CaroRima33/projet-newsletter` — **public**
  (bascule décidée le 2026-08-22 : le plan GitHub de Caroline ne permet pas
  Pages sur repo privé). Digest et commentaires de feedback (issues) sont
  donc visibles par quiconque a le lien.
- Pages : déployée via GitHub Actions (`.github/workflows/pages.yml`), pas via
  branche/dossier — évite le traitement Jekyll par défaut.
- **Cron réel sur VPS** (`crontab -l` en tant que root), pas un mécanisme lié
  à une session Claude — persistant, `cron.service` actif au démarrage du
  système. `claude` CLI est authentifié localement (`~/.claude/.credentials.json`),
  donc pas de facturation API séparée. Le workspace `/root/projects/veille`
  doit rester marqué "trusted" (`hasTrustDialogAccepted: true` dans
  `/root/.claude.json`) pour que `.claude/settings.json` s'applique en mode
  non-interactif — sinon le cron tourne sans l'allowlist et peut bloquer.
- **Collecte** : `30 6 * * *` UTC = 08:30 Paris en heure d'été. À décaler
  manuellement de ±1h aux changements d'heure (dernier dimanche de mars et
  d'octobre) puisque le cron est fixe en UTC.
- **Rétro** : dimanche 07:00 UTC (~09:00 Paris).
- Clé Resend : dans `.env` à la racine du repo sur le VPS (gitignoré). Le cron
  la lit avec l'outil Read, ne la commite jamais.
- On a considéré un routine cloud (`claude.ai/code/routines`) et GitHub
  Actions avec clé API Anthropic pour le cron — écartés au profit du cron VPS
  local, plus simple ici puisque l'infra persistante existait déjà.
