# Log — mise en prod GitHub Pages — 2026-08-22

- Repo `CaroRima33/projet-newsletter` créé (n'existait pas encore malgré les
  commandes données par Caroline) via `gh repo create --source=. --push`.
- Premier push refusé : token OAuth sans scope `workflow` (fichier
  `.github/workflows/pages.yml`). Caroline a relancé `gh auth refresh -s
  workflow`, push réussi ensuite. Branche renommée `master` → `main`.
- GitHub Pages sur repo privé : bloqué, plan Free ne le supporte pas
  (confirmé par erreur API 422 "Your current plan does not support GitHub
  Pages for this repository"). Caroline a choisi de rendre le **repo public**
  plutôt que de revenir à l'Artifact ou de passer sur GitHub Pro.
- Repo basculé en public (`gh repo edit --visibility public`), Pages activée
  (`build_type: workflow`), workflow redéclenché manuellement (le premier run,
  avant activation de Pages, avait échoué). Déploiement réussi.
- URL live confirmée : https://carorima33.github.io/projet-newsletter/digests/2026-08-22/
  (HTTP 200, testé).
- Corrigé les références `CaroRima/projet-newsletter` → `CaroRima33/...`
  (erreur de propriétaire dans plusieurs fichiers, dont la constante `REPO` du
  script de la page de vote — sans ça le bouton "Envoyer mes retours" aurait
  pointé vers un repo inexistant).
- Deuxième email de test envoyé à marie.caroline018@gmail.com (id Resend
  `43582165-3caa-4f22-86d3-93405e64009b`) avec le vrai lien GitHub Pages, pour
  valider le parcours complet (vote → commentaire → issue GitHub).

## Point non résolu

Le repo (et donc le digest + les commentaires de feedback dans les issues)
est maintenant **public**. Caroline en a été informée avant de choisir cette
option. Si elle change d'avis plus tard, il faudra soit repasser en privé (et
perdre Pages, sauf upgrade de plan), soit héberger la page de vote ailleurs.
