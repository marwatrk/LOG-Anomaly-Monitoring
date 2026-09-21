# LOG-Anomaly-Monitoring
Rule-based detection of suspicious behavior in server logs (brute-force attempts, sensitive path scanning, and abnormal traffic spikes) in Python.

# Surveillance et détection d'anomalies dans des logs serveur

## Contexte
Ce projet prolonge une compétence déjà acquise sur la centralisation de journaux d'événements (Syslog), en y ajoutant une couche d'analyse et de détection.
L'objectif : identifier automatiquement des comportements suspects dans des logs de type serveur web (Apache/Nginx), sans devoir tout inspecter manuellement.

## Méthode
Détection basée sur des règles métier simples et transparentes, dans la même logique que les règles de corrélation utilisées par un SIEM :
1. Tentatives d'authentification échouées répétées depuis une même IP (indice de brute-force).
2. Accès à des pages sensibles comme /admin ou /phpmyadmin (indice de scan de vulnérabilités).
3. Pic de volume de requêtes sur une courte période depuis une même IP.

Une IP est considérée comme suspecte dès qu'elle déclenche au moins une de ces 3 règles.

## Résultats
Sur un jeu de logs simulant du trafic web normal et deux scénarios d'attaque injectés (brute-force sur /login, scan de pages sensibles), les deux IPs malveillantes ont été correctement identifiées par les règles (voir `rapport.txt` et les graphiques `trafic_temps.png` / `scatter_ips.png`).

## Limites
- Les logs utilisés sont générés synthétiquement, par souci de contrôle expérimental (on sait exactement quelles anomalies chercher).
- Les seuils (20 échecs, 15 requêtes/minute) sont fixés de façon empirique pour cette démo ; en conditions réelles, ils devraient être calibrés sur des données historiques réelles.
- Une approche par règles ne détecte que des patterns connus à l'avance ; elle ne couvrirait pas une attaque totalement inédite (limite classique, comparable à celle d'un IDS basé sur signatures).

## Technologies
Python, pandas, matplotlib.
