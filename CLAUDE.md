# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Le projet

Système de veille et de publication semi-autonome pour Caroline sur 4 domaines
traités comme un seul groupement thématique : no-code, growth, gestion de
projet, IA. Le site web est la vitrine ; le cœur du projet est ici — des
boucles qui tournent en cron, produisent, et s'améliorent par les retours de
Caroline. Humain dans la boucle à chaque étape : rien ne se publie sans
validation explicite.

Statut actuel (2026-08-22) : structure posée, 1 collecte manuelle faite,
méthode RSS validée, page de vote + boucle de feedback en place. Repo poussé
sur GitHub (`CaroRima/projet-newsletter`, privé) — en cours de câblage du cron
cloud quotidien (envoi 08:30 Paris).

## Où est quoi

- `docs/EDITORIAL.md` — ligne éditoriale (ton, angle, ce qui est bon/mauvais).
  Le fichier le plus important : c'est lui que la boucle de rétro fait évoluer.
- `docs/SOURCES.md` — sources de veille avec score de qualité par source.
- `.claude/skills/` — une skill par tâche récurrente. `veille-collecte` existe ;
  `veille-digest`, `rediger-brouillon`, `retro-hebdo` viennent après.
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
3. **Rétro hebdomadaire** (pas encore construite, la plus importante) — relit
   les logs et `feedback/` de la semaine, compare brouillons produits vs
   versions publiées par Caroline, propose des modifications concrètes de
   `docs/EDITORIAL.md`, `docs/SOURCES.md` et `prompts/` en commit git séparé.

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

La skill `retro-hebdo` (pas encore construite) devra lire les issues
`feedback-digest` de la semaine (`gh issue list --label feedback-digest`) en
plus de `feedback/` et `logs/` pour ajuster `docs/SOURCES.md` et
`docs/EDITORIAL.md`.

## Hébergement et automatisation

- Repo : `https://github.com/CaroRima/projet-newsletter` (privé).
- Pages : déployée via GitHub Actions (`.github/workflows/pages.yml`), pas via
  branche/dossier — évite le traitement Jekyll par défaut et les limites de
  dossier de Pages classique. **Point à vérifier** : GitHub Pages sur un repo
  privé nécessite un plan payant (Pro/Team/Enterprise) — sinon Pages refuse de
  se déployer tant que le repo reste privé ou le plan ne couvre pas Pages.
- Cron quotidien : géré par un routine cloud (`claude.ai/code/routines`), pas
  par `CronCreate` (limité à la session, 7 jours max) — le routine clone le
  repo GitHub à chaque run, donc tout ce qui doit être lu par le cron
  (`docs/`, `.claude/skills/`, `prompts/`) doit être poussé sur `main`.
- Créneau : 08:30 heure de Paris = 06:30 UTC en heure d'été (cron
  `30 6 * * *`). À ajuster manuellement de ±1h aux changements d'heure
  (dernier dimanche de mars et d'octobre) puisque le cron du routine est fixe
  en UTC.
- Clé Resend : stockée dans la config du routine cloud (accepté par Caroline
  le 2026-08-22), pas dans le repo.
