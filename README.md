# SIGA Pilot — prototype d’application de gestion

Prototype de l’application indépendante de SIGASÉCURITÉ, avec une interface web et un premier backend local pour les sites, les agents, les vacations et les bons de service. Le produit final n’est pas hébergé sur Odoo.

## Mode maquette sans serveur

Ouvrir `index.html` dans un navigateur récent. Ce mode utilise des données d’exemple conservées dans le stockage local du navigateur.

## Mode serveur local de développement

Le runtime du poste doit être Node.js 24.15 ou une version ultérieure de la branche 24. Sous Windows, la base de développement est créée dans `%LOCALAPPDATA%\SIGA-Pilot-dev\siga-dev.sqlite`, sur le disque local et hors de ce dossier partagé. L’emplacement peut être remplacé par la variable `SIGA_DATA_DIR`. Ouvrir un terminal dans ce dossier et lancer :

```powershell
node server.js
```

Dans un deuxième terminal, créer le premier compte administrateur. Le mot de passe est saisi sans être affiché :

```powershell
node server.js create-admin administrateur@example.com "Administrateur SIGA"
```

Arrêter puis redémarrer `node server.js`, puis ouvrir `http://127.0.0.1:4173`. Pour ajouter un utilisateur, arrêter le serveur et lancer, par exemple :

```powershell
node server.js create-user superviseur@example.com "Nom Prénom" supervisor
```

Rôles disponibles : `admin`, `operations_manager`, `training_manager`, `hr_manager`, `supervisor`, `read_only`. Le responsable de formation peut être créé ainsi :

```powershell
node server.js create-user formation@example.com "Responsable Formation" training_manager
```

Le compte du service RH peut être créé ainsi :

```powershell
node server.js create-user rh@example.com "Responsable RH" hr_manager
```

Le serveur écoute uniquement sur `127.0.0.1` par défaut. Ne le rendez pas accessible depuis Internet.

## Fonctionnalités de démonstration

- Vue d’ensemble et navigation entre les rubriques.
- Liste, recherche et filtres des bons de service.
- Création d’un brouillon de bon : mise en place, extra ou arrêt de prestation.
- Validation d’un bon dans la maquette.
- Référentiel de sites de démonstration et ajout d’un site.
- Module Personnel accessible uniquement au rôle `hr_manager` pour les dossiers complets. Les autres rôles ne reçoivent que les champs nécessaires au planning et à la formation.
- Profil agent avec identité, coordonnées, pièce d’identité, CNPS, contact d’urgence et statuts RH. Congés datés avec retour automatique au statut Actif; les statuts Suspendu et Sorti bloquent les nouvelles vacations.
- Gestion des contrats CDD, CDI et stages avec dates, poste, rémunération, primes, période d’essai, renouvellement et avenant. Les échéances à 30 jours sont signalées dans Personnel; à l’échéance, le RH valide la clôture.
- Documents RH PDF/JPEG/PNG, jusqu’à 5 Mo, conservés dans le dossier privé de données du serveur et accessibles uniquement au rôle `hr_manager`. Le RH peut indiquer leur date d’échéance; l’application les affiche dans les alertes à 30 jours.
- Les alertes RH sont visibles dans l’application et peuvent être envoyées par courriel aux comptes actifs `hr_manager`. Configurez `SIGA_SMTP_HOST`, `SIGA_SMTP_PORT=465`, `SIGA_SMTP_USER`, `SIGA_SMTP_PASS` et `SIGA_SMTP_FROM` dans l’environnement du serveur pour activer l’envoi SMTP TLS. Les identifiants ne sont pas stockés dans le code. Sans ces variables, aucun courriel n’est envoyé; l’alerte reste visible dans l’application. Les alertes envoyées sont dédupliquées par élément et date d’échéance, avec nouvelle tentative après échec.
- Menu CNPS séparé de Paie. La rubrique CNPS dispose d’un écran de paramétrage réservé au rôle `hr_manager` : identité employeur et établissement, immatriculation et e-CNPS, taux et assiettes par branche, plafonds, limites d’exonération, périodicité, paiement, DISA et listes de pièces/procédures. Les taux et plafonds sont modifiables et vides par défaut lorsqu’ils doivent être confirmés; aucun calcul ou envoi de déclaration CNPS n’est exécuté.
- La rubrique Paie dispose d’un écran distinct de paramétrage : calendrier et fréquence, bases horaires, rubriques fixes/variables, règles de présence, retenues, ITS et contributions employeur, bulletins, paiements, validations, clôture, archivage et exports comptables. Paramètres persistés et réservés au RH dans cette version; aucun bulletin, retenue ou fichier bancaire n’est calculé ou produit.
- Planning hebdomadaire et création de vacations avec détection de chevauchement pour un agent.
- Pointage des arrivées et départs réels par vacation, avec affichage du retard par rapport à l’horaire prévu.
- Formation : planification d’une promotion avec dates de début et de fin, inscription et présence des agents, évaluation finale individuelle (note avec barème, résultat réussi/à reprendre, commentaire facultatif). Après l’enregistrement de « Réussi », l’attestation PDF est générée et téléchargée automatiquement; elle reste disponible pour un nouveau téléchargement.
- Aucun suivi de validité ni rappel de recyclage n’est prévu pour les formations.
- Facturation : paramètres d’entreprise, taxes et délais configurables; brouillons associés aux bons validés ou terminés; suivi interne de validation, paiements, relances et avoirs; impression d’un document préparatoire. L’application ne certifie pas les documents FNE/RNE et n’intègre pas l’API FNE.
- Comptabilité : paramétrage du référentiel comptable (à confirmer), dates d’exercice, journaux, comptes, soldes d’ouverture, axes analytiques, contrôles de rapprochement et export. L’écran affiche aussi les créances du module Facturation; il ne génère pas encore d’écritures ni d’états financiers.
- Commercial : suivi des opportunités et prospects, contacts, service recherché, valeur estimée, étapes de vente, responsable, prochaines actions et dates de relance; création d’offres chiffrées liées à une opportunité avec date de validité, périmètre, taxe, total TTC et suivi des statuts brouillon/envoyée/acceptée/refusée/expirée. Les contrats et renouvellements restent à intégrer.
- Logistique : inventaire des véhicules, matériels et consommables; suivi des quantités, seuils minimums, entrées/sorties de stock, états, affectations aux sites ou agents et prochaines dates d’entretien.
- Technique : demandes d’interventions correctives, préventives, d’installation ou d’inspection; priorités, équipements de l’inventaire liés au site, planning, technicien, compte rendu et pièces utilisées.
- Dysfonctionnement : signalements interservices (RH, Formation, CNPS, Paie, Planning/Pointage, Facturation, Comptabilité, Commercial, Opérations, Logistique, Technique ou Application), niveau d’impact, affectation, suivi de statut et compte rendu de résolution.
- Mise à disposition anticipée validée par le responsable de formation : elle termine immédiatement la formation de l’agent concerné, le rend disponible au planning et ouvre son évaluation individuelle le lendemain. La promotion continue pour les autres agents.
- Export CSV des bons ou des sites.
- Affichage adaptatif pour ordinateur et téléphone.
- Backend local avec comptes nominatifs, sessions, rôles, sites, agents, vacations, pointages, formations, bons et journal d’audit.
- Données de développement persistées sous `%LOCALAPPDATA%\SIGA-Pilot-dev\siga-dev.sqlite` par défaut.

## Limites

Le backend local utilise l’API SQLite intégrée à Node.js; dans la documentation officielle actuelle, cette API est en version candidate. Il sert uniquement au développement local et aux essais avec données fictives. Ne l’utilisez pas pour des dossiers salariés, pointages réels, informations financières ou pièces clients. Il ne possède pas encore de gestion d’utilisateurs depuis l’interface, de procédure automatisée de sauvegarde/restauration, de chiffrement au repos, de haute disponibilité ni de configuration de production.

Avant un pilote réel, il faudra remplacer cette base locale par un service de base de données de production, mettre en place le déploiement HTTPS, la gestion et la récupération des comptes, les sauvegardes et la restauration, les contrôles de sécurité et les règles de confidentialité. Les agents, vacations et sessions de démarrage sont fictifs. Le seuil de réussite de formation doit être défini par l’entreprise; aucun seuil n’est appliqué automatiquement. Les paramètres CNPS sont une première configuration de suivi et doivent être confirmés par le responsable compétent avant tout calcul; paie et comptabilité devront également être validées. Les déclarations CNPS, les calculs de paie, la comptabilisation automatique, les offres et contrats commerciaux, ainsi que la gestion complète des entretiens et des comptes rendus techniques restent à développer. La facturation est un prototype de cycle interne : les documents imprimés ne sont pas certifiés FNE et ne doivent pas servir de factures légales.
