# Preuves de commandes et résultats réels

Exécution locale isolée le 2026-11-04 (Europe/Paris).

```text
Docker version 29.8.2, build 7fc2dff9bc
Docker Compose version 5.6.0
git version 2.56.0
TIWAP commit: 1f9d2ac01b19c86ed20242fce5556ea77417d2a9
ZAP report version: 2.17.0
```

`docker compose config` a validé : `app` sur `127.0.0.1:5000:5000`, aucun port hôte pour `db` (seulement `27017/tcp` interne) et le volume ZAP `./reports:/zap/wrk/:rw`.

Après `docker compose up -d --build`, `app` était `Up` sur `127.0.0.1:5000->5000/tcp`, `db` était `Up` sur `27017/tcp` interne, et `zap` était `Up` sans port publié. Le contrôle hôte a retourné `HTTP=200 SIZE=2709 URL=http://localhost:5000/`. Depuis ZAP, `wget -qO- http://app:5000/` a retourné le HTML TIWAP. La capture associée est `screenshots/tiwap-localhost.png`.

## Baseline Scan

La commande prescrite a d'abord été lancée textuellement :

```text
docker compose exec zap zap-baseline.py -t http://app:5000 -m 2 -r /zap/wrk/zap-report.html -J /zap/wrk/zap-report.json
```

Elle a analysé 19 URL et listé 13 avertissements, puis retourné `ZAP_EXIT=2` avec `Job report failed to generate report: NoSuchFileException /zap/wrk/zap/wrk/zap-report.html`.

Avec ZAP 2.17.0, l'Automation Framework préfixe ce chemin absolu par le répertoire de travail. Cet échec de rapport est technique ; il ne faut pas confondre ce code avec le code de présence d'alertes. Pour produire les artefacts demandés, le même Baseline passif, même cible et mêmes paramètres a été relancé avec uniquement les noms de fichiers relatifs :

```text
docker compose exec -T zap zap-baseline.py -t http://app:5000 -m 2 -r zap-report.html -J zap-report.json
ZAP_CORRECTED_EXIT=2
FAIL-NEW: 0  WARN-NEW: 13  INFO: 0  PASS: 55
```

Ici, `2` est le code de sortie ZAP pour des avertissements trouvés, pas une panne technique. Les deux rapports valides sont présents : HTML 97 223 octets et JSON 38 453 octets. **Limite de traçabilité : deux exécutions passives ont eu lieu, la première sans artefact à cause du défaut de chemin de ZAP stable.** Aucun scan actif ni cible externe n'a été utilisé.
