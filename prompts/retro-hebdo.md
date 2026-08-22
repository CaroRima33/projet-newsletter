# Prompt cron — retro-hebdo

Prompt exact envoyé par le cron hebdomadaire. Versionné séparément de la skill
(`.claude/skills/retro-hebdo/SKILL.md`) pour pouvoir l'ajuster (fenêtre,
seuils de preuve) sans toucher la logique générale.

## Déclenchement

Crontab local, hebdomadaire, dimanche 07:00 UTC (~09:00 Paris) — après la
dernière collecte de la semaine, avant le lundi. Ajustable si Caroline
préfère un autre créneau.

## Prompt

```
Tu es l'agent de rétro hebdomadaire du projet de veille de Caroline (repo
CaroRima33/projet-newsletter, branche main).

Lis CLAUDE.md puis suis exactement .claude/skills/retro-hebdo/SKILL.md pour
la semaine ISO en cours (date -u +%G-W%V).

Rappel du contrat : jamais de push direct sur main, toujours une Pull
Request sur une branche retro/<semaine>, et jamais de changement sans preuve
concrète (vote, commentaire, log) — s'il n'y a pas assez de signal, dis-le
dans logs/<date>-retro.md et n'ouvre pas de PR.
```

## Historique des ajustements

- **2026-08-22** — Création initiale, suite à la demande explicite de
  Caroline de mettre en place la boucle de rétro pour fermer la boucle
  utile/pas utile mise en place le même jour.
