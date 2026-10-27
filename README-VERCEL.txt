SIGA Pilot - version démo statique pour Vercel

Cette version est faite pour une démonstration. Elle ne se connecte pas au serveur SIGA : les données sont des exemples et les saisies restent dans le navigateur qui les a créées. Ne saisissez aucune donnée réelle RH, paie, CNPS, client ou agent.

Déploiement Vercel :
1. Importez ce dossier comme projet statique (Framework Preset : Other).
2. Laissez Build Command vide.
3. Définissez Output Directory à . (racine du projet).
4. Déployez.

Pour l’application réelle multi-utilisateurs, il faudra héberger aussi une API et une base de données avec stockage persistant et authentification sécurisée.
