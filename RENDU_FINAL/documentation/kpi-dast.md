# KPI DAST proposés

| KPI | Définition / formule | Source | Fréquence / responsable | Décision associée |
|---|---|---|---|---|
| Alertes ouvertes par priorité | Nombre de constats ouverts P1, P2, P3 à la date de mesure | Registre de qualification et tickets | Hebdomadaire ; RSSI | Escalader si P1 > 0 ou si P2 augmente deux semaines consécutives |
| Vulnérabilités confirmées | Nombre de constats dont preuve et impact ont été validés / nombre total qualifié | Revue sécurité + ticket | Hebdomadaire ; AppSec | Planifier les corrections sur preuve, pas sur le seul bruit de scan |
| Pourcentage de corrections revérifiées | corrections avec re-test DAST concluant / corrections déclarées terminées × 100 | Tickets et rapports ZAP | Mensuel ; responsables applicatifs | Refuser une clôture sous 100 % |
| Délai médian de correction | Médiane(date de clôture vérifiée − date de qualification) | Tickets | Mensuel ; RSSI | Ajuster capacité ou SLA si le délai dépasse l'objectif par priorité |
| Couverture DAST | Routes/flux dans le périmètre réellement explorés / routes/flux inventoriés × 100 | Sitemap ZAP + inventaire applicatif | À chaque release ; QA sécurité | Ajouter authentification, données de test ou parcours manquants |
| Taux d'échec technique des scans | scans sans rapport valide pour incident technique / scans lancés × 100 | Logs CI et artefacts | Mensuel ; DevSecOps | Corriger runner, réseau, disponibilité ou génération d'artefact si > 5 % |
| Taux de faux positifs | constats invalidés / constats qualifiés × 100 | Revue manuelle | Mensuel ; AppSec | Améliorer règles, contexte d'authentification et processus de triage |
