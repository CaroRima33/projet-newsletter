# Prompt cron — veille-collecte

Prompt exact envoyé par le cron quotidien. Versionné ici séparément de la skill
(`.claude/skills/veille-collecte/SKILL.md`) pour que la boucle de rétro puisse
l'ajuster indépendamment (ex : changer la fenêtre temporelle, le nombre de sujets
max) sans toucher à la logique générale de la skill.

## Déclenchement

Routine cloud `claude.ai/code/routines`, cron `30 6 * * *` (UTC) = 08:30
heure de Paris en heure d'été. À décaler de ±1h aux changements d'heure
(dernier dimanche de mars et d'octobre) — le cron est fixe en UTC, pas la
correspondance avec l'heure de Paris.

## Prompt

```
Lance la collecte de veille du jour avec la skill veille-collecte, dans le
repo CaroRima33/projet-newsletter (branche main).

Fenêtre : dernières 24h (ou depuis le dernier digest dans inbox/digests/ s'il
est plus récent que 24h).

Méthode : flux RSS listés dans docs/SOURCES.md en priorité, fetch direct
ciblé pour les sources sans RSS, recherche web générale seulement en dernier
recours.

Sorties :
- inbox/digests/<date-du-jour>.md (digest markdown)
- site/digests/<date-du-jour>/index.html (page de vote, gabarit :
  site/digests/2026-08-22/index.html)
- site/index.html mis à jour avec le lien du jour
- logs/<date-du-jour>-collecte.md

Commit et push ces fichiers sur main (déclenche le déploiement Pages).

Puis envoie l'email quotidien via l'API Resend (clé dans la config du
routine) :
- from: Veille Caroline <onboarding@resend.dev> (ou le domaine vérifié de
  Caroline si configuré depuis)
- to: marie.caroline018@gmail.com
- sujet : "Digest veille — <date du jour>"
- corps : résumé des sujets retenus (titre, domaine, 1-2 phrases) + lien
  bouton vers https://REPO.github.io/projet-newsletter/digests/<date>/ pour
  voter utile/pas utile et laisser un commentaire
- pour l'appel à l'API Resend, utiliser un script `python3` (`urllib.request`)
  avec un en-tête `User-Agent` explicite (ex : `curl/8.5.0`), en lisant la clé
  directement dans `.env` plutôt que de l'exposer en argument de commande.
  Sans ce User-Agent explicite, l'appel se fait bloquer par Cloudflare (HTTP
  403, code 1010) côté Resend, reproduit 3 fois (2026-08-25, 2026-08-28,
  2026-08-30). **Ne pas utiliser `curl` directement** : dans cet
  environnement, l'outil Bash bloque le motif `curl` + variable
  d'environnement + pipe comme « commande multiple » (constaté le
  2026-09-14) — `curl` avait été proposé comme correctif le 2026-08-30 mais
  ne s'est jamais avéré exécutable en pratique une fois testé ; le script
  python3 avec User-Agent explicite, lui, a tourné sans incident sur 6 runs
  consécutifs (2026-09-15 → 2026-09-20).
- dédupliquer les items par URL/guid d'article contre l'ensemble des digests
  déjà publiés, pas seulement par date de publication ou en supposant que le
  flux RSS est renvoyé dans un ordre chronologique stable : le 2026-09-20,
  10 items ActuIA sont apparus d'un coup avec une `pubDate` du 2026-09-18,
  absents du fetch de la veille (2026-09-19) qui n'en avait vu que 5 pour
  cette même date — republication en lot ou troncature de flux, cause exacte
  non tranchée, mais un contrôle par guid plutôt que par date aurait
  détecté le manque dès le 2026-09-19 (voir `logs/2026-09-20-collecte.md`).

Ne modifie ni docs/EDITORIAL.md ni docs/SOURCES.md — signale juste dans le log
si une source semble anormalement à jour ou pas.
```

## Historique des ajustements

- **2026-08-22** — Ajout de la consigne de priorité RSS > fetch direct >
  recherche web générale, suite au retour de Caroline sur la qualité de la
  première collecte (trop de contenu SEO/intemporel remonté par la recherche
  web seule).
- **2026-08-22** — Passage de la page de vote en Artifact Claude à une page
  statique `site/digests/<date>/` déployée sur GitHub Pages, avec envoi email
  et commit/push ajoutés au prompt. Créneau fixé à 08:30 Paris (06:30 UTC).
- **2026-08-30** (rétro 2026-W35) — Preuve : `urllib` bloqué par Cloudflare
  (HTTP 403, code 1010) sur l'appel Resend dans `logs/2026-08-25-collecte.md`,
  `logs/2026-08-28-collecte.md` et `logs/2026-08-30-collecte.md`, `curl`
  fonctionnant à chaque fois en contournement. Changement : consigne explicite
  d'utiliser `curl` plutôt que `urllib` pour cet appel, pour éviter l'essai
  raté systématique.
- **2026-09-20** (rétro 2026-W38, commit ajouté sur la même branche
  `retro/2026-W35` — la PR #2 n'est toujours pas mergée, 4e cycle de rétro
  consécutif) — Preuve : la consigne `curl` du 2026-08-30 n'a jamais tourné
  en prod (branche non mergée), et le 2026-09-14
  (`logs/2026-09-14-collecte.md`) l'agent de collecte a constaté que l'outil
  Bash bloque `curl` utilisé avec variable d'environnement + pipe comme
  « commande multiple » — le correctif proposé ne fonctionne donc pas tel
  quel dans cet environnement. Contournement adopté depuis par l'agent de
  collecte de lui-même (faute de consigne officielle sur `main`) : script
  `python3` (`urllib.request`) avec en-tête `User-Agent` explicite, clé lue
  dans `.env`. Vérifié sans incident sur 6 runs consécutifs
  (`logs/2026-09-15-collecte.md` à `logs/2026-09-20-collecte.md`), contre un
  double envoi le premier jour (2026-09-14, avant que le User-Agent soit
  ajouté dès le premier essai). Changement : remplacement de la consigne
  `curl` par la consigne python3/User-Agent, effectivement éprouvée.
  Deuxième preuve : `logs/2026-09-20-collecte.md` documente un flux ActuIA
  ayant révélé 10 items supplémentaires datés du 2026-09-18 un jour après le
  fetch qui aurait dû les voir. Changement : ajout d'une consigne de
  déduplication par URL/guid plutôt que par date seule.
