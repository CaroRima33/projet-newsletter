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
- utiliser `curl` pour l'appel à l'API Resend, pas `urllib` (Python) : sans
  User-Agent explicite, `urllib` se fait bloquer par Cloudflare (HTTP 403,
  code 1010) côté Resend, reproduit 3 fois (2026-08-25, 2026-08-28,
  2026-08-30). `curl` fonctionne du premier coup à chaque fois.

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
