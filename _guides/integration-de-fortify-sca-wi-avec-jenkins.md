---
title: Intégration de Microfocus Fortify SCA & WI avec Jenkins
layout: default
date: 2020-03-09
---

> Ce guide d'installation suppose que vos outils de développement sont déjà installés sur l'agent

# Configuration de l'agent

## Installation de Fortify SCA

1. Les lignes directrices concernant l'installation sont disponibles dans le document ["ESDC Fortify SCAN 18.20 Install Guide"](SCA_Install_18.20.pdf) (en anglais)
2. L'agent doit être réinitialisé

## Installation du module Fortify Maven (uniquement pour les applications Java)

1. À partir de C:\Program Files\Fortify\Fortify_SCA_and_Apps_*version*\plugins\maven\, extraire le fichier maven-plugin-src.zip vers une autre destination
2. Extraire le dossier
3. Dans le dossier extrait, utilisez la commande suivante : `mvn clean install`
4. Copiez maintenant C:\Users\\[username]\\.m2 dans C:\Windows\System32\config\systemprofile\\.m2 

## Ajouter un certificat auto-signé WIE

1. Avec Internet Explorer, accédez à https://mlapesd4290.hrdc-drhc.net/WIE/WebConsole/Applications.aspx
2. Sous **Plus d'informations**, cliquez sur **Aller sur la page Web (non recommandé)**
3. Utilisez les informations de connexion fournies par la Sécurité de la TI
4. Dans la **barre d'adresse**, cliquez sur **l'icône de cadenas** et cliquez sur **Afficher le certificat**. De là, cliquez sur **Installer le certificat…**. Choisissez **Machine locale** et cliquez sur **Suivant**. Sélectionnez **Placer tous les certificats dans le magasin suivant**, cliquez sur **Parcourir** et sélectionnez **Autorités de certification racines de confiance** et cliquez sur **Suivant**. Cliquez sur **Terminer**

# Configuration principale

## Installez le plugin Fortify

1. Dans Jenkins, sélectionnez **Gérer Jenkins** -> **Gérer les Plugins** et cliquez sur l'onglet **Disponible** et saisissez **Fortify**. Cliquez ensuite sur **Télécharger et installer après le redémarrage**
2. Dans Jenkins, sélectionnez **Gérer Jenkins** -> **Configurer le système**. De là, faites défiler jusqu'à **Évaluation Fortify**. Entrez ensuite les informations suivantes:
    1. URL SPC : https://mlapesd4289.hrdc-drhc.net:8443/ssc
    2. Jeton d'authentification: le jeton fourni par la Sécurité de la TI
    3. Cliquez sur **Paramètres avancés**, puis sur **Tester la connexion**

## Variables d'environnement de l'agent

1. Dans Jenkins, sélectionnez **Gérer Jenkins** -> **Gérer les nœuds et les nuages​​** -> **[Votre agent]** -> **Configurer**. De là, faites défiler jusqu'à **Variables d'environnement** et créez la suivante:
    1. Nom : FORTIFY_HOME
    2. Valeur: [Emplacement de SCA dans l'agent]

## Configurer les informations d'identification

1. Depuis votre travail, cliquez sur Configurer
2. Ensuite, faites défiler jusqu'à Définition du modèle de pipeline. Vous verrez les informations d'identification du registre, cliquez sur Ajouter et cliquez sur le nom de votre travail
3. Saisissez les informations suivantes:
    1. Type: nom d'utilisateur avec mot de passe
    2. Nom d'utilisateur: le nom d'utilisateur fourni par la Sécurité de la TI
    3. Mot de passe: le mot de passe fourni par la Sécurité de la TI
    4. ID: un identifiant (cet identifiant sera utilisé dans le script de pipeline)
4. Cliquez sur ** Ajouter **

# Exemples de pipelines

## Exemple de pipeline SCA - Maven

```
node('ITSEC_SCA_01') {
    stage ('clean') {
        fortifyClean addJVMOptions: '', buildID: 'WebGoat-Lecacy', logFile: '', maxHeap: ''
    }
    
    stage ('Update') {
        fortifyUpdate updateServerURL: 'https://update.fortify.com'
    }
    
    stage ('Translate') {
        fortifyTranslate addJVMOptions: '', buildID: 'WebGoat-Legacy', excludeList: '', logFile: '', maxHeap: '', projectScanType: fortifyMaven3(mavenOptions: '"-f" "C:\\WebGoat-Legacy\\pom.xml"')
        
    }

    stage ('Scan') {
        fortifyScan addJVMOptions: '', addOptions: '', buildID: 'WebGoat-Legacy', customRulepacks: '', logFile: '', maxHeap: '14000', resultsFile: 'wg.fpr'
    }
    
    stage ('Upload') {
        fortifyUpload appName: 'TestingSCA', appVersion: '1', failureCriteria: '', filterSet: '', pollingInterval: '', resultsFile: 'wg.fpr'
    }
}
```

## Exemple de pipeline SCA - dotNet

```
node('ITSEC_SCA_01') {
    stage ('clean') {
        fortifyClean addJVMOptions: '', buildID: 'SimpleWebApp', logFile: '', maxHeap: ''
    }
    
    stage ('Update') {
        fortifyUpdate updateServerURL: 'https://update.fortify.com'
    }
    
    stage ('Translate') {
        fortifyTranslate addJVMOptions: '', buildID: 'SimpleWebApp', excludeList: '', logFile: '', maxHeap: '', projectScanType: fortifyDotnetSrc(dotnetAddOptions: '', dotnetFrameworkVersion: '3.1', dotnetLibdirs: '', dotnetSrcFiles: 'C:\\SimpleWebApp')
        
    }

    stage ('Scan') {
        fortifyScan addJVMOptions: '', addOptions: '', buildID: 'SimpleWebApp', customRulepacks: '', logFile: '', maxHeap: '14000', resultsFile: 'FortifySimpleWebApp.fpr'
    }
    
    stage ('Upload') {
        fortifyUpload appName: 'TestingSCA', appVersion: '1', failureCriteria: '', filterSet: '', pollingInterval: '', resultsFile: 'FortifySimpleWebApp.fpr'
    }
}
```

## Exemple de pipeline WIE

```
node ('ITSEC_SCA_01') {
    withCredentials([usernamePassword(credentialsId: 'wieapi', passwordVariable: 'PASS', usernameVariable: 'USER')]) {
        stage ('WIE')  {
            powershell '''
                try {
                    Invoke-RestMethod -Method 'POST' -Uri 'https://mlapesd4290.hrdc-drhc.net/WIE/REST/api/v1/auth' -SessionVariable 'Session' -Body @{username = $ENV:USER; password = $ENV:PASS}
                } catch {
                    write-warning "$_"
                    exit 1
                }
                
                try {
                    Invoke-RestMethod -Uri 'https://mlapesd4290.hrdc-drhc.net/WIE/REST/api/v2/scans' -Method POST -ContentType 'application/json' -WebSession $Session -Body '{"name": "Site: http://10.13.227.99:8080/WebGoat", "projectVersion":  { "siteId": "cdf9406e-1ffa-441c-8729-6735f5ca322b"}, "scanTemplateId": "68350857-af1d-4f8b-afac-3ac535ca84ad", "overrides": { "startMethod": "url", "startUrls": ["http://10.13.227.99:8080/WebGoat"], "priority": 1, "sensorID": "255f1e7b-1d09-45d5-b025-10d82e35748a"}}'
                } catch {
                    write-warning "$_"
                    exit 1
                }
                '''        
        }
    }
}
```

