# Fiches de priorisation — NOVA SERVICES

La priorité métier ci-dessous n'est pas une recopie du risque ZAP. Elle combine exposition, type de donnée, effet sur un parcours client et facilité de correction. Les actifs NOVA sont des analogies de gouvernance, pas des systèmes scannés.

| Référence | TIWAP / actif NOVA analogue | Impact | Priorité métier et justification | Responsable / échéance | Action / vérification | Statut |
|---|---|---|---|---|---|---|
| ZAP-10099 | Fragment SQL dans l'erreur de connexion / portail client NOVA analogue | Divulgation du modèle de données et aide au ciblage d'attaques sur l'authentification | P1 — parcours exposé aux clients et fuite de détail serveur, même si ZAP est « Moyen » | Lead applicatif ; 5 jours ouvrés | Désactiver debug, erreurs génériques, journalisation interne. Rejouer la requête de connexion contrôlée : aucun SQL ni trace dans la réponse ; Baseline Scan. | Ouvert — preuve de fuite, impact complet à confirmer |
| ZAP-10020 | Absence anti-clickjacking / espace client NOVA analogue | Un utilisateur pourrait interagir avec une interface encadrée de façon trompeuse | P2 — exposition web, mais correction d'en-tête simple et absence de preuve d'exploitation | Équipe plateforme web ; 15 jours | Définir `frame-ancestors` ou `X-Frame-Options`, tests d'intégration d'en-têtes et `curl -I`, puis Baseline Scan. | Ouvert — configuration confirmée |
| ZAP-90022 | Erreur 500 au login / service d'identité NOVA analogue | Révélation possible de chemins, composants ou logique interne ; instabilité du parcours | P2 — authentification concernée, mais rapport ne prouve pas de données sensibles divulguées | Responsable identité ; 10 jours | Corriger la cause, page d'erreur générique, identifiant de corrélation côté logs. Vérifier code attendu et absence de détail dans la réponse. | À investiguer — 500 confirmé |

La clôture exige une revérification DAST et un test fonctionnel ; « corrigé en code » n'est pas un statut de fermeture.
