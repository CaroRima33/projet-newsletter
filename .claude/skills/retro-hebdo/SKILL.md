---
name: retro-hebdo
description: Boucle de rétro hebdomadaire — relit les votes/commentaires de la semaine (issues GitHub feedback-digest), feedback/ et logs/, propose des modifications concrètes de docs/EDITORIAL.md et docs/SOURCES.md via une Pull Request que Caroline relit et merge elle-même.
---

# Rétro hebdomadaire

C'est la boucle la plus importante du système : c'est elle qui fait que
l'agent s'améliore réellement, pas juste qu'il tourne. Pas de magie — des
preuves (votes, commentaires, logs) qui deviennent des modifications de
fichiers versionnés, que Caroline valide.

## Contrat non négociable

**Cette skill ne pousse jamais sur `main` directement.** Toute modification
proposée va sur une branche `retro/<semaine-ISO>` et devient une **Pull
Request**. Caroline relit le diff sur GitHub et merge (ou rejette) elle-même
— c'est le mécanisme qui remplace "je propose, tu valides" pour cette boucle
spécifiquement, puisque personne n'est présent en direct quand le cron
tourne.

**Ne jamais fabriquer un changement pour avoir l'air actif.** Si les preuves
de la semaine sont insuffisantes (0 ou 1 vote, aucun commentaire exploitable),
la bonne sortie est "pas assez de signal, aucun changement proposé" — pas une
PR spéculative basée sur une seule donnée.

## Fenêtre

Semaine ISO en cours (lundi → dimanche, `date -u +%G-W%V`). Peut être
relancée manuellement sur une semaine précédente si besoin (préciser dans le
prompt).

## Étapes

1. **Collecter les preuves** :
   - `feedback/<semaine>.md` s'il existe (retours libres de Caroline).
   - Issues GitHub avec le label `feedback-digest` créées dans la fenêtre :
     `gh issue list --repo CaroRima33/projet-newsletter --label feedback-digest --state all --json number,title,body,createdAt`.
   - `logs/*-collecte.md` de la semaine (incidents de sources, sources
     signalées anormalement en retard).
   - `logs/*-retro.md` de la rétro précédente, pour ne pas re-proposer un
     changement déjà fait ou déjà rejeté (voir "Historique des ajustements"
     dans `docs/SOURCES.md` / `docs/EDITORIAL.md` — c'est la mémoire de ce qui
     a déjà été tranché).

2. **Enrichir chaque item voté** : une issue `feedback-digest` ne contient que
   titre + domaine + vote + commentaire éventuel. Pour savoir quelle *source*
   et quelle *méthode de collecte* (flux RSS / fetch direct / recherche web)
   sont derrière un item, retrouver le titre dans
   `site/digests/<date>/index.html` du même jour (attributs `data-title`,
   `data-item-id`, et le texte du chip `.method`) ou dans
   `inbox/digests/<date>.md`.

3. **Chercher des patterns**, pas des cas isolés (sauf commentaire très
   explicite) :
   - Une source dont les items sont systématiquement votés "pas utile" →
     candidate à la baisse de score dans `docs/SOURCES.md`.
   - Une méthode de collecte (RSS vs recherche web) corrélée à plus de votes
     "utile" → ajuster la priorité dans `prompts/veille-collecte.md` si le
     signal est net.
   - Des commentaires qui reviennent sur le ton, le niveau de vulgarisation,
     l'angle (ex : "trop technique", "pas assez critique", "bon angle mais
     mal expliqué") → matière directe pour `docs/EDITORIAL.md`.
   - Des sources signalées à l'arrêt plusieurs semaines de suite dans les
     logs (ex : Cafétech, ActuIA au 2026-08-22) → proposer un retrait ou une
     baisse de score si ça continue.

4. **Comparaison brouillons vs publié** : décrite dans l'architecture cible
   mais **pas encore applicable** — la boucle production (brouillons
   d'articles) n'existe pas encore. Sauter cette étape tant que
   `.claude/skills/rediger-brouillon/` n'existe pas, et le noter dans le log
   plutôt que de l'ignorer silencieusement.

5. **Si au moins un changement est justifié par une preuve concrète** :
   - `git checkout -b retro/<semaine-ISO>`
   - Éditer `docs/SOURCES.md` et/ou `docs/EDITORIAL.md` et/ou
     `prompts/veille-collecte.md` — toujours ajouter une entrée dans la
     section "Historique des ajustements" du fichier modifié : date, preuve
     précise (ex : "3/3 items Cafétech votés pas utile, source déjà signalée
     stale depuis 3 semaines dans les logs"), changement fait.
   - Committer, pousser la branche.
   - `gh pr create` avec un corps qui liste, pour chaque changement : la
     preuve → le changement → pourquoi. Pas de jargon, ton direct, écrit pour
     que Caroline puisse décider en 2 minutes.

6. **Toujours** écrire `logs/<date>-retro.md` : preuves collectées (nombre de
   votes, d'issues, de commentaires), patterns trouvés ou absence de pattern,
   PR ouverte (lien) ou décision de ne rien changer et pourquoi.

## Ce que cette skill NE fait PAS

- Ne modifie jamais `main` directement.
- Ne construit pas de nouvelle source ni ne rédige de contenu éditorial —
  seulement des ajustements aux fichiers de configuration existants.
- Ne rejette pas une PR précédente non mergée — si une PR `retro/*` est encore
  ouverte, le signaler dans le log et ne pas en ouvrir une deuxième
  (accumuler les preuves dans la même branche plutôt, en ajoutant des commits).
