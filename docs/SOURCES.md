# Sources de veille

Score de qualité de 1 (faible) à 5 (excellente) — revu par la boucle de rétro
hebdomadaire en fonction de ce qui produit réellement des sujets retenus par
Caroline. Toutes les sources ci-dessous sont à valider par Caroline avant le
premier cron (scores de départ = estimation + un premier test de fraîcheur réel,
pas encore de signal d'usage).

Statut : 2026-09-06, mise à jour par la rétro hebdomadaire 2026-W36. 8 sources
actives, 2 retirées (feeds mortes ou sites à l'arrêt), 1 bloquée (à traiter
manuellement).

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
| ActuIA | Média éditorial | IA | https://www.actuia.com/feed/ | 1 | **Retrait annulé le 2026-09-06** (rétro 2026-W36) : la retirer le 2026-08-30 supposait qu'aucun signal externe n'indiquerait de reprise — or le 2026-09-02 le flux a publié 3 nouveaux articles d'un coup après ~7 semaines de silence, dont un retenu dans le digest du 2026-09-03. Silence à nouveau du 2026-09-03 au 2026-09-06 (4 jours). Réintégrée avec un score bas (1) : source vivante mais très irrégulière (rafales ponctuelles), à revérifier à chaque run plutôt qu'à considérer comme morte. Précision du 2026-09-13 (rétro 2026-W37) : entre le 2026-09-07 et le 2026-09-10, la source a au contraire publié 4 jours de suite avec plusieurs articles de bonne qualité analytique chaque jour — le silence court qui a suivi (vendredi 11 → dimanche 13 septembre, donc incluant tout un week-end) n'est pas comparable au schéma Cafétech et ne doit pas être traité comme un signal d'alerte. Le vrai signal à surveiller reste une absence totale sur plusieurs semaines, pas quelques jours incluant un week-end. |
| The Batch (DeepLearning.AI) | Newsletter hebdo | IA | _aucun flux RSS trouvé_ | 3 | Pas de RSS (app Next.js). Fonctionne bien en fetch direct ciblé (WebFetch) : contenu daté et réel confirmé le 2026-08-22 (dernier numéro : 21 août 2026). EN, par Andrew Ng, sérieux. |
| arXiv — cs.AI | Preprints académiques | IA | https://export.arxiv.org/rss/cs.AI | 2 | Flux officiel arXiv, fonctionne bien mais **vide le week-end** (arXiv ne publie pas samedi/dimanche — confirmé le 2026-08-22, samedi, flux vide). Brut et non vulgarisé : utile pour repérer un papier qui devient un sujet, pas pour la veille quotidienne telle quelle. |
| International Journal of Project Management (Elsevier) | Revue académique | Gestion de projet | https://rss.sciencedirect.com/publication/science/02637863 | 2 | Flux ScienceDirect fonctionne, remonte de vrais titres d'articles récents, mais sans date fiable dans le flux (rythme de parution par numéro, pas quotidien) et texte complet payant. |
| Revue française de gestion (Cairn.info) | Revue académique FR | Gestion de projet | _bloqué_ | — | Le site (et son flux RSS) renvoie une page de vérification anti-bot (Cloudflare, HTTP 403) aussi bien en fetch direct qu'en RSS. Pas automatisable sans contourner une protection anti-bot — **retirée de la collecte automatique**, à consulter manuellement de temps en temps si besoin (revue trimestrielle, donc peu d'impact sur une veille quotidienne). |
| Lenny's Newsletter | Newsletter | Growth / produit | https://www.lennysnewsletter.com/feed | 4 | Flux fiable et frais (dernier item : 18 août 2026), contenu substantiel et à angle affirmé (ex : test critique d'outils IA). EN, référence practitioner reconnue. ⚠️ Deux articles de la semaine du 2026-09-01 (podcast GPT-6 Astra le 2026-09-04, "Community Wisdom" le 2026-09-06) intégralement réservés aux abonnés payants, écartés faute de contenu accessible malgré un angle éditorial pertinent — score inchangé pour l'instant, mais si le motif revient souvent un abonnement payant vaudrait la peine d'être discuté avec Caroline. ⚠️ Format « Community Wisdom » (compilation hebdomadaire de conseils communautaires) écarté deux fois pour deux raisons différentes (payant le 2026-09-06, puis simplement dépourvu d'avis tranché le 2026-09-13 alors qu'il était accessible) — à écarter par défaut dès identification du format, sans ré-analyser à chaque occurrence : il ne correspond pas au critère d'angle affirmé qui justifie le score 4 de cette source. |
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
venait à se tarir, ce qui rend d'autant plus sensible le motif "paywall"
apparu deux fois cette semaine sur des articles Lenny's par ailleurs
pertinents (voir note dans le tableau ci-dessus).

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
- **2026-09-06** (rétro 2026-W36, commit ajouté sur la même branche
  `retro/2026-W35` car la PR #2 de la semaine précédente n'était pas encore
  mergée — voir contrat de la skill retro-hebdo) — Preuve :
  `logs/2026-09-03-collecte.md` montre ActuIA publiant 3 nouveaux articles le
  2026-09-02 (un retenu dans le digest du jour), après ~7 semaines de silence
  et alors que le retrait proposé le 2026-08-30 était justifié par "rien ...
  ne suggère qu'elles redeviendront actives". `logs/2026-09-04-collecte.md` à
  `2026-09-06-collecte.md` confirment un nouveau silence de 4 jours après
  cette rafale. Changement : annulation du retrait d'ActuIA décidé le
  2026-08-30 (la prémisse "site mort" est falsifiée), réintégrée en source
  active à score bas (1) plutôt qu'à score normal, pour refléter un pattern
  de rafales imprévisibles plutôt qu'une reprise franche. Le retrait de
  Cafétech n'est en revanche pas remis en cause : 16 runs consécutifs
  (2026-08-22 → 2026-09-06, ~10 semaines) sans la moindre évolution, contraste
  net avec ActuIA qui a bougé. Deuxième preuve : `logs/2026-09-04` et
  `2026-09-06-collecte.md` signalent chacun un article Lenny's Newsletter
  autrement pertinent écarté pour mur payant intégral — pas encore un pattern
  assez net pour changer le score, mais noté dans le tableau pour suivi et
  comme point de discussion possible avec Caroline (abonnement payant ?).
- **2026-09-13** (rétro 2026-W37, commit ajouté sur la même branche
  `retro/2026-W35` — la PR #2 n'est toujours pas mergée, voir alerte dans le
  corps de la PR) — Preuve : `logs/2026-09-07-collecte.md` à
  `2026-09-13-collecte.md` (7 runs). Cafétech reste figée au 2 juillet 2026
  sur 23 runs consécutifs (2026-08-22 → 2026-09-13, ~10 semaines et demie) —
  aucune remise en cause du retrait déjà proposé, la tendance ne fait que se
  renforcer. ActuIA a publié 4 jours de suite (2026-09-07 → 2026-09-10) avec
  plusieurs articles de bonne qualité chaque jour, puis un silence du
  2026-09-11 au 2026-09-13 incluant tout un week-end : pas un signal
  d'alerte, précision ajoutée dans le tableau pour éviter une fausse alerte
  les prochaines fois qu'un silence court chevauche un week-end. Anomalie
  arXiv cs.AI signalée les 2026-09-08 et 09 (flux vide deux jours ouvrés de
  suite, inhabituel) : résolue dès le 2026-09-10, retour à un volume normal
  — incident ponctuel clos, pas de changement de score, à ne rouvrir qu'en
  cas de récidive. Lenny's Newsletter : le format "Community Wisdom" a été
  écarté une deuxième fois (2026-09-13, cette fois accessible mais sans avis
  tranché) après un premier cas payant le 2026-09-06 — pattern net sur ce
  format précis, note ajoutée pour l'écarter par défaut. Aucune nouvelle
  issue `feedback-digest` ni entrée `feedback/2026-W37.md` cette semaine
  (toujours uniquement l'issue #1 du 2026-08-22) — aucun changement basé sur
  un retour explicite de Caroline cette semaine.
- **2026-09-20** (rétro 2026-W38, commit ajouté sur la même branche
  `retro/2026-W35` — la PR #2 n'est toujours pas mergée, 4e cycle de rétro
  consécutif, voir alerte mise à jour dans le corps de la PR) — Preuve :
  `logs/2026-09-14-collecte.md` à `logs/2026-09-20-collecte.md` (7 runs).
  Cafétech reste figée au 2 juillet 2026 sur 30 runs consécutifs
  (2026-08-22 → 2026-09-20, ~11 semaines) — retrait déjà proposé
  reconfirmé, aucun changement supplémentaire. Growthhacking.fr : 0 sujet
  retenu sur les 7 runs, toujours exclusivement des demandes/offres de
  practitioners — score 1 déjà proposé reconfirmé. ActuIA : nouveau silence
  de 8 jours (2026-09-11 → 2026-09-18, au-delà d'un simple week-end cette
  fois), puis reprise en rafale le 2026-09-18 (5 articles) suivie d'une
  anomalie le 2026-09-20 où 10 items supplémentaires datés du 2026-09-18
  sont apparus dans le flux un jour après coup (voir
  `logs/2026-09-20-collecte.md` et la nouvelle consigne de déduplication
  par guid dans `prompts/veille-collecte.md`). Le pattern « rafales
  imprévisibles » déjà noté le 2026-09-06 se confirme et s'accentue plutôt
  qu'il ne se dément — pas de changement de score (1 déjà bas), pas de
  nouveau retrait à proposer non plus. Lenny's Newsletter : le format
  « Community Wisdom » a été écarté une troisième fois (2026-09-20), même
  motif que les deux précédentes occurrences — confirme la note déjà
  ajoutée, aucun changement. Une nouvelle issue `feedback-digest` (#3,
  digest du 2026-09-11, créée le 2026-09-13 après le run de la rétro
  2026-W37 donc non traitée jusqu'ici) contient un commentaire explicite
  suggérant un tag « légal »/« gouvernance » pour ce type de contenu — un
  seul vote et un seul commentaire, insuffisant comme preuve de pattern, et
  il s'agit d'une fonctionnalité d'affichage (tags sur `site/digests/`) plutôt
  que d'un critère de score de source ou de ton éditorial : hors du
  périmètre des fichiers que la rétro ajuste. Noté ici pour ne pas le
  perdre, à réévaluer si le même type de demande revient.
