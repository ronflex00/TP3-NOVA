# Preuve CI distante — GitHub Actions

Exécution vérifiée le 5 novembre 2026 sur GitHub Actions, branche `feat/tp3-zap-dast`.

| Élément | Preuve |
|---|---|
| Workflow | `DAST - ZAP Baseline (local TIWAP only)` |
| Run | [#42001229252](https://github.com/ronflex00/TP3-NOVA/actions/runs/42001229252) |
| Commit | `356dfe0d95b43d56434c6ac2d474f0465f0f3640` |
| Résultat | `success` : toutes les étapes ont réussi |
| Isolement | TIWAP cloné dans le workspace du runner ; ZAP cible uniquement `http://app:5000` sur le réseau Compose ; aucun port public TIWAP |
| Artefact | `zap-baseline-reports`, ID `13402447743`, disponible jusqu'au 3 février 2027 |

Les étapes réussies sont : checkout, clone TIWAP, application de la configuration loopback, démarrage du laboratoire, attente depuis le réseau Compose, Baseline passif, upload des rapports, arrêt avec `docker compose down -v`.

L'artefact a été téléchargé et contrôlé après le run : archive ZIP valide, contenant `zap-report.html` (100 872 octets) et `zap-report.json` (40 204 octets). Le JSON est parseable. La sortie du Baseline a signalé des avertissements ; le pipeline accepte explicitement le code ZAP `2` comme résultat de détection, tout autre code restant un échec.

Le premier run distant `#42000812594` a échoué uniquement à l'upload : le dossier monté appartenait au runner et l'utilisateur non privilégié de ZAP ne pouvait pas y écrire. Le correctif publié (`chmod 777 tiwap/reports` sur un répertoire éphémère de runner) a permis au second run de créer et conserver les artefacts. Cette permission ne concerne ni un hôte de production ni une cible exposée.
