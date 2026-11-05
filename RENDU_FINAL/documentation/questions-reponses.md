# Réponses techniques (10 questions)

Le document de sujet/Word n'était pas présent dans le dossier initial. Les réponses suivantes couvrent les dix questions usuelles du TP DAST et restent cohérentes avec les preuves produites.

1. **Qu'est-ce que le DAST ?** C'est une analyse de l'application en fonctionnement, depuis l'extérieur, à partir des requêtes et réponses HTTP.
2. **Pourquoi OWASP ZAP Baseline ?** Il réalise une exploration puis des contrôles passifs. Il convient à une première porte CI sans envoyer de charges d'attaque actives.
3. **Pourquoi la cible est-elle `http://app:5000` ?** C'est le nom DNS interne Docker. ZAP atteint l'application sans l'exposer sur Internet ; côté hôte, seul `127.0.0.1:5000` est publié.
4. **Que signifie le code de sortie 2 de ZAP ?** Dans ce Baseline Scan, il signifie que des avertissements ont été trouvés. Ce n'est pas un échec technique ; 0 signifie aucune alerte et les autres codes doivent être analysés comme incidents potentiels.
5. **Le rapport prouve-t-il une SQL injection ?** Non. Il montre un fragment SQL dans une réponse d'erreur, pas une injection réussie. Le scan passif ne teste pas l'exploitation SQLi.
6. **Que montre le SAST sur `sqli_medium()` ?** La chaîne `userid` est interpolée dans une requête SQLite. C'est un risque source à corriger par des requêtes paramétrées, mais il faut un test contrôlé pour confirmer le comportement à l'exécution.
7. **Pourquoi SAST et DAST sont-ils complémentaires ?** Le SAST voit le code, y compris des chemins non explorés ; le DAST voit la configuration, les en-têtes et les erreurs réelles. L'un ne remplace pas l'autre.
8. **Une alerte ZAP est-elle automatiquement une vulnérabilité ?** Non. Elle doit être qualifiée : preuve, contexte, impact, reproductibilité et risque de faux positif.
9. **Pourquoi conserver HTML et JSON ?** Le HTML sert à la lecture humaine ; le JSON permet les comptes, l'automatisation, le suivi et la comparaison entre exécutions.
10. **Comment ferme-t-on un constat ?** Après correction, test fonctionnel et re-scan ciblé ou Baseline Scan montrant que la preuve a disparu ; sans re-vérification, le statut reste ouvert.
