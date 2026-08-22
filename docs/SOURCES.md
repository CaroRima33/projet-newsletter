# Sources de veille

Score de qualité de 1 (faible) à 5 (excellente) — revu par la boucle de rétro
hebdomadaire en fonction de ce qui produit réellement des sujets retenus par
Caroline. Toutes les sources ci-dessous sont à valider par Caroline avant le
premier cron (scores de départ = estimation + un premier test de fraîcheur réel,
pas encore de signal d'usage).

Statut : 2026-08-22, mise à jour après un test réel de chaque source (flux RSS
testés en direct, voir `logs/2026-08-22-collecte.md`). 9 sources actives, 1
retirée (feed morte), 1 bloquée (à traiter manuellement).

**Méthode de collecte, par ordre de priorité** (décidé le 2026-08-22, voir
`CLAUDE.md`) :
1. **Flux RSS** quand il existe — le plus fiable, dates réelles, pas de bruit
   d'interprétation.
2. **Fetch direct ciblé** (WebFetch sur l'URL de la source) quand il n'y a pas
   de RSS mais que la page est lisible sans navigateur.
3. **Recherche web générale** — dernier recours seulement, tend à remonter du
   contenu SEO intemporel plutôt que de l'actu datée (voir le run manuel du
   2026-08-22, avant correction).

## Sources fournies par Caroline

| Source | Type | Domaine | Flux RSS | Score | Note |
|---|---|---|---|---|---|
| Cafétech | Newsletter quotidienne | Tech général / IA | https://cafetech.fr/feed/ | 2 | ⚠️ Flux RSS testé le 2026-08-22 : dernier item daté du 2 juillet 2026, soit ~7 semaines — anormal pour une newsletter dite quotidienne. Homepage bloque le fetch direct (403). À revérifier avant de faire confiance à cette source pour du chaud. |
| Growthhacking.fr | Communauté / forum | Growth | https://www.growthhacking.fr/latest.rss | 2 | Flux `/latest.rss` fonctionne et est frais (items datés du jour même), mais c'est un forum : le flux "latest" est surtout des demandes de practitioners (recherche de prestataire, questions techniques), pas de l'actualité éditoriale. Nécessite un filtrage éditorial fort, peu de signal brut exploitable tel quel. |

## Sources découvertes (à valider par Caroline)

| Source | Type | Domaine | Flux RSS | Score | Note |
|---|---|---|---|---|---|
| ActuIA | Média éditorial | IA | https://www.actuia.com/feed/ | 2 | ⚠️ Testé le 2026-08-22 : dernier article publié le 3 juillet 2026, confirmé aussi sur la page d'accueil (pas juste un flux en retard — le site semble avoir arrêté de publier depuis ~7 semaines). À revérifier périodiquement ; si ça ne reprend pas, à retirer. |
| The Batch (DeepLearning.AI) | Newsletter hebdo | IA | _aucun flux RSS trouvé_ | 3 | Pas de RSS (app Next.js). Fonctionne bien en fetch direct ciblé (WebFetch) : contenu daté et réel confirmé le 2026-08-22 (dernier numéro : 21 août 2026). EN, par Andrew Ng, sérieux. |
| arXiv — cs.AI | Preprints académiques | IA | https://export.arxiv.org/rss/cs.AI | 2 | Flux officiel arXiv, fonctionne bien mais **vide le week-end** (arXiv ne publie pas samedi/dimanche — confirmé le 2026-08-22, samedi, flux vide). Brut et non vulgarisé : utile pour repérer un papier qui devient un sujet, pas pour la veille quotidienne telle quelle. |
| International Journal of Project Management (Elsevier) | Revue académique | Gestion de projet | https://rss.sciencedirect.com/publication/science/02637863 | 2 | Flux ScienceDirect fonctionne, remonte de vrais titres d'articles récents, mais sans date fiable dans le flux (rythme de parution par numéro, pas quotidien) et texte complet payant. |
| Revue française de gestion (Cairn.info) | Revue académique FR | Gestion de projet | _bloqué_ | — | Le site (et son flux RSS) renvoie une page de vérification anti-bot (Cloudflare, HTTP 403) aussi bien en fetch direct qu'en RSS. Pas automatisable sans contourner une protection anti-bot — **retirée de la collecte automatique**, à consulter manuellement de temps en temps si besoin (revue trimestrielle, donc peu d'impact sur une veille quotidienne). |
| Lenny's Newsletter | Newsletter | Growth / produit | https://www.lennysnewsletter.com/feed | 4 | Flux fiable et frais (dernier item : 18 août 2026), contenu substantiel et à angle affirmé (ex : test critique d'outils IA). EN, référence practitioner reconnue. |
| Bubble Blog | Blog officiel plateforme | No-code | https://bubble.io/blog/rss/ | 2 | Flux frais (items du 19-21 août 2026) mais mélange annonces produit réelles et articles SEO intemporels ("8 meilleurs outils..."). Source primaire donc biaisée — utile pour suivre les annonces Bubble spécifiquement, pas comme seule source no-code. |
| ~~nocodechris (Substack)~~ | Newsletter practitioner | No-code | https://nocodechris.substack.com/feed | — | **Retirée** : flux testé le 2026-08-22, dernier article publié en juin 2023. Newsletter à l'arrêt. |

## Angle mort identifié

Le **no-code** manque toujours de source d'actualité éditoriale solide et
active — après retrait de nocodechris, il ne reste que le blog Bubble (biaisé,
mono-plateforme). Si Caroline suit des comptes ou newsletters no-code
spécifiques en pratique, ce serait la meilleure source à ajouter ici. La
**gestion de projet** reste également faible en actu datée (les deux sources
académiques publient par numéro, pas au fil de l'eau, et l'une est
inaccessible).

## Historique des ajustements

- **2026-08-22** — Passage de "recherche web générale" à "RSS en priorité, fetch
  direct en repli" suite au retour de Caroline ("j'ai besoin que ce soit des
  vraies actualités et nouveautés"). Chaque flux RSS a été testé en direct :
  ActuIA et Cafétech se sont révélés avoir ~7 semaines de retard (scores
  baissés de 3 à 2), nocodechris est retirée (morte depuis 2023), Cairn RFG est
  bloquée par anti-bot (retirée de l'automatisation), Lenny's Newsletter
  confirmée solide (score monté à 4).
