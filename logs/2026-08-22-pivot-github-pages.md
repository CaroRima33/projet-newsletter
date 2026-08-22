# Log — pivot hébergement + boucle feedback — 2026-08-22

- **Décision** : la page de vote utile/pas utile passe d'un Artifact Claude
  (privé, capability `artifact` avec sauvegarde automatique) à une page
  statique hébergée sur GitHub Pages (`site/digests/<date>/`), à la demande
  de Caroline (garder l'infra sur son propre GitHub).
- **Conséquence technique** : GitHub Pages est du statique pur, pas de
  capability de sauvegarde comme un Artifact. La page compile les votes +
  commentaires (tenus en `localStorage` pendant la session) et ouvre une
  **issue GitHub pré-remplie** (label `feedback-digest`) au clic sur "Envoyer
  mes retours" — Caroline valide elle-même la soumission, aucun token côté
  page.
- **Ajout demandé** : champ commentaire facultatif par sujet, visible après
  le vote.
- **Fichiers créés/modifiés** : `site/digests/2026-08-22/index.html`,
  `site/index.html`, `.github/workflows/pages.yml` (déploiement Pages via
  GitHub Actions, pas via branche/dossier — évite Jekyll), `site/.nojekyll`,
  `CLAUDE.md`, `.claude/skills/veille-collecte/SKILL.md`,
  `prompts/veille-collecte.md`.
- **Bloquant identifié** : le repo `CaroRima/projet-newsletter` est privé —
  GitHub Pages sur un repo privé nécessite un plan payant (Pro/Team/
  Enterprise). Pas encore vérifié quel plan a Caroline. `gh` CLI installé
  localement pour pousser le repo et vérifier ; authentification en attente
  (`gh auth login` à lancer par Caroline).
- **Créneau email confirmé** : 08:30 heure de Paris = 06:30 UTC (heure d'été).
  Cron routine cloud : `30 6 * * *`.
