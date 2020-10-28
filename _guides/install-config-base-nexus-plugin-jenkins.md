---
title: Installation et configuration de base du plugin de la plateforme Nexus pour Jenkins
layout: default
date: 2020-10-28
---

Nexus IQ Server est un moteur de politiques alimenté par [intelligence précise](https://guides.sonatype.com/iqserver/technical-guides/sonatype-vuln-data/) sur des composants de type "open source". Il fournit un certain nombre d'outils pour améliorer l'utilisation des composants dans votre chaîne d'approvisionnement logicielle, vous permettant d'automatiser vos processus et d'accélérer la livraison tout en augmentant la qualité des produits.

Le moteur de politiques Nexus IQ Server alimente les produits Nexus Firewall, Lifecycle, et Auditor. Avec IQ Server, vous pouvez:

   - Partager l'intelligence des composants avec les équipes de développement, les aider à prendre de meilleures décisions et à créer de meilleurs logiciels.
   - Mettre en oeuvre un moteur de politiques entièrement personnalisable qui vous permet de définir quels composants sont acceptables et lesquels ne le sont pas.
   - Intégrer des outils de développement populaires tels que Maven, Eclipse, IntelliJ, Visual Studio, GitHub, Bamboo et Jenkins.
   - Fournir une suite complète d'API de type REST qui permettent d'accéder aux fonctionnalités de base pour les mises en oeuvre personnalisées.

## Installation

   1. Recherchez la plate-forme Nexus dans "Manage Plugins" dans Jenkins.
   2. Download now and install after restart.

## Ajouter un certificat à Jenkins (Ignorer si déjà fait)

    1. Téléchargez le certificat du serveur Nexus IQ (demandez l'URL à SADE)
    2. Sur le serveur Jenkins, ouvrez PowerShell en tant qu'administrateur
    3. `cd "<dossier_installation_jenkins>\jre\bin"`
    4. Entrez la commande

       `.\keytool.exe -importcert -alias Sonatype -keystore ..\lib\security\cacerts -file "<chemin_du_certificat>" -trustcacerts`

    5. Entrez le mot de passe du "keystore" - la valeur par défaut est "changeit"
    6. "Trust the certificate"
    7. Redémarrez Jenkins

## Connexion de Jenkins à IQ Server

    1. Accédez à "Manage Jenkins"
    2. "Configure System"
    3. Faites défiler jusqu'à Sonatype Nexus et ajoutez Nexus IQ Server
    4. Ajoutez l'URL du serveur:

       > Demander l'URL à SADE

    5. Ajoutez des informations d'identification à IQ Server
    6. Tester la connexion

## Ajout d'une tâche Sonatype aux projets Jenkins Freestyle

    1. Dans la section "Build" de votre écran de configuration de projet, cliquez sur le menu déroulant "Add Build Step" et sélectionnez "Invoke Nexus Policy Evaluation".
    2. Remplissez les valeurs des champs de l'étape

       - "Stage": Build
	   - Sélectionnez une application IQ (utilisez l'application Sandbox à des fins de test)

    3. Enregistrer
    4. Exécutez le "Build".

## Ajout de Sonatype au pipeline Jenkins

    1. Dans la section Pipeline d'un écran de configuration de projet Pipeline, ouvrez le "Snippet Generator" en cliquant sur le lien "Pipeline Syntax".
    2. Dans la section "Steps" de la fenêtre "Snippet Generator", sélectionnez "nexusPolicyEvaluation: Invoke Nexus Policy Evaluation".
    3. Remplissez les valeurs des champs de l'étape

       - "Stage": Build
	   - Sélectionnez une application IQ (utilisez l'application Sandbox à des fins de test)

    4. Après avoir fourni les valeurs des champs de l'étape, copiez l'extrait de code généré dans votre script de pipeline.

    Remarque: une compilation du projet doit être complétée dans l'espace de travail avant l'évaluation Nexus. Nexus recherchera toutes les extensions de fichier autorisées et les analysera.