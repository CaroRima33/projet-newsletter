---
name: veille-collecte
description: Collecte quotidienne des actualités no-code, growth, gestion de projet et IA depuis docs/SOURCES.md, filtrées selon docs/EDITORIAL.md, écrites en digest daté dans inbox/digests/.
---

# Veille — collecte quotidienne

## Objectif

Produire un digest daté des actualités pertinentes des dernières 24h (ou depuis
le dernier run) sur les 4 domaines de veille : no-code, growth, gestion de
projet, IA. Ce digest est un tri, pas une rédaction — les brouillons d'article
sont produits séparément par la boucle production, à partir des sujets marqués
"à traiter" dans un digest.

## Étapes

1. Lire `docs/SOURCES.md` pour la liste des sources et leurs scores actuels.
   Ignorer ou déprioriser les sources à score 1 si le volume est déjà suffisant.
2. Lire `docs/EDITORIAL.md` pour les critères de filtrage (angle, ce qui est bon
   / mauvais, priorité à l'actualité chaude).
3. Pour chaque source, récupérer les publications des dernières 24h (ou depuis
   le dernier digest s'il existe dans `inbox/digests/`), **dans cet ordre de
   priorité** (voir aussi `docs/SOURCES.md`) :
   1. **Flux RSS** si `docs/SOURCES.md` en liste un pour cette source (`curl`
      + parsing XML, ou tout moyen équivalent). C'est la méthode par défaut :
      dates réelles, pas d'interprétation.
   2. **Fetch direct ciblé** (WebFetch sur l'URL de la source) si pas de RSS
      mais que la page est lisible.
   3. **Recherche web générale** seulement en dernier recours — elle remonte
      surtout du contenu SEO intemporel plutôt que de l'actu datée, à éviter
      comme méthode principale.
   Ne jamais inventer un article, une date ou une URL.
   Si le dernier item d'un flux RSS est significativement plus vieux que ce à
   quoi on s'attend pour cette source (ex : "quotidien" mais rien depuis
   plusieurs semaines), le signaler dans le log — c'est un signal pour la
   boucle de rétro (source à requalifier) et un point à noter dans le digest,
   pas à ignorer silencieusement.
4. Filtrer selon la ligne éditoriale : écarter le putaclic, les communiqués de
   presse sans recul, le hors-sujet, et — spécifiquement pour les sources de
   type forum (ex : growthhacking.fr) — les demandes/questions de practitioners
   qui ne sont pas de l'actualité. Garder l'actualité chaude en priorité, sans
   exclure une analyse de fond si elle est notable.
5. Pour chaque sujet retenu, écrire dans le digest :
   - Titre du sujet (pas juste le titre de l'article source)
   - Domaine(s) concerné(s)
   - Source(s), avec URL réelle
   - Résumé en 2-3 phrases, ton académique-neutre, acronymes définis
   - Pourquoi c'est intéressant (angle possible pour un futur article)
   - Statut : `[ ] à traiter` — case que Caroline coche pour la boucle production
6. Écrire le digest dans `inbox/digests/YYYY-MM-DD.md`.
7. Générer la page jumelle statique `site/digests/YYYY-MM-DD/index.html` —
   copier `site/digests/2026-08-22/index.html` comme gabarit (structure HTML,
   CSS, script de vote/commentaire/envoi vers issue GitHub) et remplacer le
   contenu des `<li class="item">` par les sujets du jour. Ajouter le lien
   vers cette page dans `site/index.html`.
8. Committer et pousser `inbox/digests/`, `site/digests/<date>/` et
   `site/index.html` sur `main` (le déploiement Pages se déclenche sur push).
9. Envoyer l'email quotidien (voir `prompts/veille-collecte.md` pour le
   template) avec un résumé des sujets et un lien vers la page de vote publiée.
10. Écrire un log dans `logs/YYYY-MM-DD-collecte.md` : sources consultées,
    nombre de sujets retenus/écartés, incidents éventuels (source injoignable,
    etc.), et confirmation d'envoi de l'email.

## Ce que cette skill NE fait PAS

- Ne rédige pas d'article — seulement un digest de tri.
- Ne publie pas d'article sur le site vitrine (`site/` ici ne contient que les
  pages de vote du digest, pas le site public de Caroline).
- Ne modifie pas `docs/SOURCES.md` ou `docs/EDITORIAL.md` — c'est le rôle de la
  boucle de rétro hebdomadaire, sur la base des retours accumulés dans
  `feedback/` et des issues GitHub labellisées `feedback-digest`.

## Format du digest — voir un exemple

Le run manuel du 2026-08-22 sert de référence de format tant que Caroline n'a
pas donné de retour dessus. Voir `inbox/digests/2026-08-22.md` — la première
moitié du fichier documente aussi pourquoi la recherche web générale a été
abandonnée comme méthode principale au profit du RSS/fetch direct.
