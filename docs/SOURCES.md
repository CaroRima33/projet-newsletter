# Sources de veille

Score de qualité de 1 (faible) à 5 (excellente) — revu par la boucle de rétro
hebdomadaire en fonction de ce qui produit réellement des sujets retenus par
Caroline. Toutes les sources ci-dessous sont à valider par Caroline avant le
premier cron (scores de départ = estimation + un premier test de fraîcheur réel,
pas encore de signal d'usage).

Statut : 2026-08-30, mise à jour par la rétro hebdomadaire 2026-W35 après 9
runs quotidiens de collecte. 7 sources actives, 3 retirées (feeds mortes ou
sites à l'arrêt), 1 bloquée (à traiter manuellement).

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
| ~~Cafétech~~ | Newsletter quotidienne | Tech général / IA | https://cafetech.fr/feed/ | — | **Retirée le 2026-08-30** : flux inchangé (dernier item 2 juillet 2026) sur 9 runs quotidiens consécutifs (2026-08-22 → 2026-08-30, ~9 semaines de retard), homepage en HTTP 403 à chaque fetch direct sur la même période. Source considérée à l'arrêt, plus de vérification périodique tant qu'aucun signal externe n'indique une reprise. |
| Growthhacking.fr | Communauté / forum | Growth | https://www.growthhacking.fr/latest.rss | 1 | Flux `/latest.rss` fonctionne et reste frais (items datés du jour même), mais confirmé sur 9 runs quotidiens consécutifs (2026-08-22 → 2026-08-30) : 0 sujet retenu à chaque fois, exclusivement des demandes/offres de practitioners (prestataires, alternances, scraping, autopromotion), jamais d'actualité éditoriale. Score baissé de 2 à 1 : le flux "latest" n'est structurellement pas exploitable tel quel pour cette veille. Conservée car seule source growth fournie par Caroline — à revalider avec elle plutôt qu'à retirer unilatéralement. |

## Sources découvertes (à valider par Caroline)

| Source | Type | Domaine | Flux RSS | Score | Note |
|---|---|---|---|---|---|
| ~~ActuIA~~ | Média éditorial | IA | https://www.actuia.com/feed/ | — | **Retirée le 2026-08-30** : flux inchangé (dernier item 7 juillet 2026) sur 8 runs quotidiens consécutifs (2026-08-23 → 2026-08-30), confirmé également sur la page d'accueil dès le 2026-08-22. Site considéré à l'arrêt de publication, plus de vérification périodique tant qu'aucun signal externe n'indique une reprise. |
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
inaccessible). Le **growth** repose désormais quasi entièrement sur Lenny's
Newsletter (score 4) depuis la baisse de score de Growthhacking.fr (2026-08-30,
9 runs consécutifs à 0 sujet) — un second angle mort à surveiller si Lenny's
venait à se tarir.

## Historique des ajustements

- **2026-08-22** — Passage de "recherche web générale" à "RSS en priorité, fetch
  direct en repli" suite au retour de Caroline ("j'ai besoin que ce soit des
  vraies actualités et nouveautés"). Chaque flux RSS a été testé en direct :
  ActuIA et Cafétech se sont révélés avoir ~7 semaines de retard (scores
  baissés de 3 à 2), nocodechris est retirée (morte depuis 2023), Cairn RFG est
  bloquée par anti-bot (retirée de l'automatisation), Lenny's Newsletter
  confirmée solide (score monté à 4).
- **2026-08-30** (rétro 2026-W35) — Preuve : `logs/2026-08-22-collecte.md` à
  `logs/2026-08-30-collecte.md` montrent Cafétech figée au 2 juillet 2026 et
  ActuIA figée au 7 juillet 2026 sur 9 et 8 runs quotidiens consécutifs
  respectivement, sans la moindre évolution ni sur le flux RSS ni sur la page
  d'accueil (403 persistant pour Cafétech). Changement : les deux sources sont
  retirées de la collecte automatique (au lieu d'une simple baisse de score,
  le seuil "plusieurs semaines de suite" de la skill retro-hebdo étant
  largement dépassé et confirmé jour après jour sans variation). Deuxième
  preuve : Growthhacking.fr à 0 sujet retenu sur les 9 mêmes runs consécutifs,
  toujours pour la même raison structurelle (flux "latest" = forum de
  practitioners, pas d'actualité éditoriale). Changement : score baissé de 2 à
  1, conservée (seule source growth fournie par Caroline) plutôt que retirée.
