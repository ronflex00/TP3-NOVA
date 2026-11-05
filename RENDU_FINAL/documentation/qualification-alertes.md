# Qualification des alertes ZAP

Source : `reports/zap-report.json`, rapport créé le 2026-11-04T08:52:21Z, cible exclusivement interne `http://app:5000`.

Le JSON contient 16 groupes de règles : 4 moyens, 7 faibles et 5 informationnels. En comptant chaque URL affectée, cela représente 8 occurrences moyennes, 21 faibles et 10 informationnelles. Aucun risque élevé n'est présent. Le résumé de `zap-baseline.py` affiche 13 avertissements : il s'agit du nombre de règles d'avertissement signalées par le script, pas du nombre de groupes JSON.

| Référence / statut | Nom, règle, risque et confiance ZAP | URL et preuve observée | Qualification, recommandation et vérification manuelle |
|---|---|---|---|
| 10020 — vulnérabilité de configuration confirmée | Missing Anti-clickjacking Header ; plugin 10020 ; Moyen ; confiance moyenne (2) | `http://app:5000` ; aucun en-tête `X-Frame-Options` ni directive CSP `frame-ancestors` n'est présent. | L'absence est confirmée par la réponse. Risque de placement de l'interface dans une iframe hostile. Ajouter `Content-Security-Policy: frame-ancestors 'none'` ou `X-Frame-Options: DENY` selon le besoin. Vérifier après correction avec `curl -I` et un nouveau Baseline Scan. |
| 10099 — observation technique confirmée, impact à confirmer | Source Code Disclosure - SQL ; plugin 10099 ; Moyen ; confiance moyenne (2) | `http://app:5000/login` ; réponse 500 contenant `SELECT username, password FROM users WHERE username`. | La fuite du fragment SQL est confirmée ; elle ne démontre ni injection SQL exploitable ni exposition d'une base de données. Désactiver le mode debug, retourner une page d'erreur générique et journaliser côté serveur. Vérifier manuellement que la réponse 5xx n'affiche plus de requête ni de trace. |
| 90022 — observation technique confirmée, cause à confirmer | Application Error Disclosure ; plugin 90022 ; Faible ; confiance moyenne (2) | `http://app:5000/login` ; preuve `HTTP/1.0 500 INTERNAL SERVER ERROR`. | Le code 500 est confirmé. La cause exacte et les informations visibles doivent être revues : dans un laboratoire Flask en debug, cette alerte est plausible, mais le rapport seul ne prouve pas l'impact métier. Mettre une gestion d'erreurs sans détail client et inspecter les journaux serveur. |

Autres constats ZAP : CSP absent (10038), politique de permissions absente (10063), en-têtes COEP/COOP/CORP absents (90004), version `Werkzeug/2.0.3 Python/3.6.15` divulguée (10036), `X-Content-Type-Options` absent (10021), commentaire suspect (10027), requête d'authentification identifiée (10111), application moderne détectée (10109), contenu cacheable (10049) et absence de jeton anti-CSRF (10202).

## Limites essentielles

Le Baseline Scan est passif : il explore puis analyse les réponses, sans tenter une injection. Il ne détecte donc **aucune injection SQL** dans ce rapport. Le code de `sqli_medium()` concatène pourtant `userid` dans une requête SQLite : c'est un constat SAST à traiter séparément, qui demanderait des tests autorisés, authentifiés et contrôlés pour être confirmé en DAST. Ne pas conclure que l'absence d'alerte SQLi prouve l'absence de SQLi.

La cible est le laboratoire TIWAP volontairement vulnérable ; ces observations ne constituent pas une évaluation d'un système NOVA réel.
