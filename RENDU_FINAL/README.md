# TP3 — DAST OWASP ZAP — NOVA SERVICES / TIWAP

Ce rendu documente un laboratoire local isolé, réalisé le 4 novembre 2026. TIWAP est une application volontairement vulnérable utilisée exclusivement à des fins pédagogiques.

## Résultat synthétique

| Élément | Valeur réelle |
|---|---|
| Commit TIWAP | `1f9d2ac01b19c86ed20242fce5556ea77417d2a9` |
| Versions | Docker 29.8.2 ; Compose 5.6.0 ; Git 2.56.0 ; ZAP report 2.17.0 |
| Cible | `http://app:5000` sur le réseau Compose ; hôte `127.0.0.1:5000` seulement |
| Résultat HTTP hôte | 200 |
| Groupes d'alertes JSON | 0 élevé, 4 moyen, 7 faible, 5 informationnel |
| Occurrences JSON | 0 élevé, 8 moyen, 21 faible, 10 informationnel |
| Sortie Baseline générant les artefacts | 2 = avertissements présents |

Consulter [la qualification](documentation/qualification-alertes.md), [la priorisation NOVA](documentation/priorisation-nova.md), [les KPI](documentation/kpi-dast.md), [les réponses](documentation/questions-reponses.md) et [les preuves de commandes](preuves/commandes-et-resultats.md).

La CI GitHub Actions a aussi été vérifiée à distance avec succès : voir [la preuve CI et l'artefact](preuves/ci-distante-github-actions.md).

## Inventaire

- `lab/docker-compose.yml` — Compose durci : app uniquement sur loopback, MongoDB non publié, service ZAP officiel.
- `reports/zap-report.html` et `reports/zap-report.json` — rapports ZAP authentiques.
- `screenshots/tiwap-localhost.png` — capture authentique de TIWAP en local.
- `ci/gitlab-ci.yml` — pipeline GitLab CI conçu pour un runner Docker-in-Docker autorisé.
- `../.github/workflows/dast-zap.yml` — alternative GitHub Actions ne ciblant que le laboratoire du runner.
- `documentation/` — analyse, fiches NOVA, KPI et réponses techniques.
- `preuves/ci-distante-github-actions.md` — exécution CI distante réussie et vérification de l'artefact.

## Reproduction locale

1. Cloner TIWAP dans un dossier de travail isolé.
2. Copier `lab/docker-compose.yml` à la racine de ce clone sous le nom `docker-compose.yml`.
3. Adapter `app.py` pour l'écoute HTTP locale (la version amont force HTTPS, incompatible avec l'URL prescrite `http://app:5000`). Les deux pipelines livrés appliquent cette adaptation explicitement avant le build.
4. Créer `reports/`, puis exécuter `docker compose config` et `docker compose up -d --build`.
5. Lancer le Baseline Scan depuis le service `zap` avec des noms de fichiers de rapport relatifs, comme dans le CI.
6. Terminer par `docker compose down`.

Le runner GitLab doit autoriser Docker-in-Docker privilégié. L'alternative GitHub utilise seulement Docker local au runner. Aucun workflow ne scanne une URL externe.

## Limites

- Le sujet Word initial et ses questions exactes n'étaient pas présents : un nouveau document modifiable est livré avec dix réponses techniques usuelles.
- Le Baseline passif ne confirme pas une SQL injection ; voir la distinction SAST/DAST dans la qualification.
- L'image ZAP `stable` 2.17.0 a un comportement de chemin qui empêche la génération de rapport avec les chemins absolus exactement fournis dans l'énoncé. La preuve de cette exécution et le contournement strictement local sont conservés.
- Les rapports proviennent d'un lab TIWAP uniquement, pas d'un actif NOVA.
