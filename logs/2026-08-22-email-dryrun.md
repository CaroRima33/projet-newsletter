# Log — dry run email — 2026-08-22

- **Action** : envoi d'un email de test (digest du jour + lien de vote) à
  marie.caroline018@gmail.com via l'API Resend.
- **Expéditeur** : `onboarding@resend.dev` — adresse de test Resend, ne
  nécessite pas de domaine vérifié. À remplacer par un domaine vérifié par
  Caroline avant tout envoi en production (délivrabilité, image de marque).
- **Contenu** : résumé des 8 sujets du digest `inbox/digests/2026-08-22.md` +
  lien vers la page de vote (Artifact) `inbox/digests/2026-08-22-vote.html`.
- **Résultat** : HTTP 200, id Resend `f8f2e0bd-009c-4f9d-9cbb-9033f6b35b17`.
- **Clé API** : stockée dans `.env` (gitignore, jamais commitée). Recommandé à
  Caroline de régénérer cette clé côté Resend puisqu'elle a été collée en
  clair dans la conversation.
- **Suite** : boucle de vote fonctionnelle (page Artifact), mais rien ne relit
  encore ces votes automatiquement — c'est le rôle de la skill `retro-hebdo`,
  pas encore construite (voir `CLAUDE.md`).
