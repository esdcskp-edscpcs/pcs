---
title: Intégration de Microfocus Fortify SCA avec Azure DevOps
layout: default
date: 2020-11-06
---

## Conditions préalables

1. Un jeton d'authentification de "Software Security Center"
2. Un projet "Software Security Center"

:warning: Les trois conditions préalables sont nécessaires. S'il vous en manque une, vous devez contacter la sécurité informatique d'EDSC.


## Description

Les analyses sont effectuées à l'intérieur de l'agent et téléchargées sur le serveur de "Software Security Center". Une fois votre application créée, SCA sera installé sur votre agent et l'analyse sera effectuée par la suite.

Étapes du pipeline:
1. Créez votre application
2. Installez SCA sur l'agent
3. Effectuez l'analyse et téléchargez le résultat dans le "Software Security Center"

## Installer SCA sur l'agent

Étapes:
1. Contactez la sécurité informatique pour obtenir le fichier d'archive SCA
2. Téléchargez ce fichier dans votre artéfacts de projet
3. Dans le pipeline Azure, téléchargez les artéfacts

Un exemple de l'étape 3 est disponible dans la section exemple

## Effectuer le balayage

Étapes:
1. Nettoyer: suppression des fichiers temporaires SCA
2. Traduire: traduction du code source dans un format traduit intermédiaire
3. Scan: analyse du code traduit et production de rapports de vulnérabilité de sécurité
4. Téléchargement: téléchargement des résultats vers le "Software Security Center"

:warning: La manière dont vous effectuez l'analyse dépend du langage de programmation de l'application. Pour plus d'informations sur ces étapes, vous pouvez consulter le site officiel [Documentation SCA](https://www.microfocus.com/documentation/fortify-static-code-analyzer-and-tools/1810/SCA_Guide_18.10.pdf) 


## Variables configurables

Variable | Description
--------- | -----------
SSC_URL | l'URL du "Software Security Center"
SSC_TOKEN | votre jeton de "Software Security Center"
APP_NAME | votre application dans le "Software Security Center"
APP_VERSION | la version de votre application dans le "Software Security Center"
SCA_BIN | le chemin vers le binaire SCA

## Exemples de pipelines

Installation de SCA sur l'agent à partir d'artefacts

:warning: Cela présuppose que vous disposez d'une archive de type TAR de SCA en tant qu'artefact disponible dans votre projet Azure Devops. 

````
- stage : SCA
    displayName: SCA
    jobs:
      - job:
        steps:
          - download: current
            artifact: build
          
          - task: UniversalPackages@0
            displayName: Download SCA artifact  
            inputs:
              command: 'download'
              downloadDirectory: '$(System.DefaultWorkingDirectory)'
              feedsToUse: 'internal'
              vstsFeed: 'b1be6f55-20c0-4034-9a7c-14009996c54b/fe527817-8cb3-4f8b-8eb1-a5349267dc9b'
              vstsFeedPackage: '14f60f64-658d-4fd6-ad0a-71d6170ef842'
              vstsPackageVersion: '19.2.0'

          - task: UniversalPackages@0
            displayName: Download Fortify license
            inputs:
              command: 'download'
              downloadDirectory: '$(System.DefaultWorkingDirectory)'
              feedsToUse: 'internal'
              vstsFeed: 'b1be6f55-20c0-4034-9a7c-14009996c54b/fe527817-8cb3-4f8b-8eb1-a5349267dc9b'
              vstsFeedPackage: 'bcd5d3ae-4e0a-4e7c-88c7-5a56326a4e65'
              vstsPackageVersion: '1.0.0'

          - task: ExtractFiles@1
            displayName: Extract Tar file
            inputs:
              archiveFilePatterns: '*.tar.gz'
              destinationFolder: '$(System.DefaultWorkingDirectory)'
              cleanDestinationFolder: false
              
          - task: InstallFortifySCA@5
            displayName: Install Fortify SCA on agent  
            inputs:
              InstallerPath: '$(System.DefaultWorkingDirectory)/$(FORTIFY_SCA)'
              VS2013: false
              VS2015: false
              LicenseFile: '$(System.DefaultWorkingDirectory)/$(FORTIFY_LICENSE)'
              RunFortifyRulepackUpdate: true
````

Balayer une application Java
````
- task: Bash@3
displayName: Install Maven Plugin
inputs:
    targetType: 'inline'
    script: |
    cd $HOME/HPE_Fortify/HPE_Fortify_SCA_and_Apps/plugins/maven                
    mkdir maven-plugin-bin
    tar -xsf maven-plugin-bin.tar.gz --directory maven-plugin-bin
    cd maven-plugin-bin
    chmod +x install.sh
    ./install.sh

- task: Bash@3
displayName: Clean
inputs:
    targetType: 'inline'
    script: |
    export set PATH=$(SCA_BIN)
    sourceanalyzer -b $(APP_NAME) -clean

- task: Bash@3
displayName: Translate
inputs:
    targetType: 'inline'
    script: |
    export set PATH=$(SCA_BIN)
    sourceanalyzer -b $(APP_NAME) mvn -f $(Pipeline.Workspace)/build clean package

- task: Bash@3
displayName: Scan
inputs:
    targetType: 'inline'
    script: |
    export set PATH=$(scapath)
    sourceanalyzer -b $(APP_NAME) -scan -f $(APP_NAME).fpr

- task: Bash@3
displayName: Upload
inputs:
    targetType: 'inline'
    script: |
    export set PATH=$(scapath)
    fortifyclient -url $(SSC_URL) -authtoken $(SSC_TOKEN) uploadFPR -file $(APP_NAME).fpr -applicationVersionID $(APP_VERSION)
````