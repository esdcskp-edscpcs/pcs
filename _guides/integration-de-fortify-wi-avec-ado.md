---
title: Intégration de Microfocus Fortify Web Inspect avec Azure DevOps
layout: default
date: 2020-11-09
---


:warning: WebInspect est installé à l'intérieur du réseau d'EDSC. Par conséquent, votre agent de pipeline doit également être à l'intérieur du réseau d'EDSC.

## Conditions préalables

1. Un compte WebInspect
2. Un projet WebInspect
3. Un modèle de scan WebInspect

:warning: les trois conditions préalables sont nécessaires. S'il vous manque une de celles-ci, vous devez communiquer avec la sécurité informatique d'EDSC.


## Description

Les analyses sont effectuées en dehors de l'agent de pipeline par WebInspect lui-même. L'agent de pipeline est principalement utilisé pour appeler les APIs du serveur.

Le pipeline est composé de commandes de type Shell. La première permet de faire l'authentification avec le serveur WebInspect. Si vous avez besoin d'informations d'identification ou si vous les avez oubliées, vous devez contacter la sécurité informatique. La seconde demande à WebInspect d'effectuer un scan de votre application.


## Variables configurables

Variable | Description
--------- | -----------
WIE_URL | l'url Webinspect
USERNAME | votre nom d'utilisateur Webinspect
PASSWORD | votre mot de passe Webinspect
APP_URL | l'url de votre application
APP_ID | votre identifiant d'application dans Webinspect
SCAN_TEMPLATE_ID | l'id de l'analyse que vous souhaitez effectuer
SENSOR_ID | l'identifiant du capteur utilisé pour le scan

## Exemple de pipeline
````

variables:
  WIE_URL: https://mlapesd4290.hrdc-drhc.net
  USERNAME: monCodeUtilisateur
  PASSWORD: monMotDePasseSécurisé
  APP_URL: http://10.13.227.99:8080/WebGoat
  APP_ID: cdf9406e-1ffa-441c-8729-6735f5ca322b
  SCAN_TEMPLATE_ID: 68350857-af1d-4f8b-afac-3ac535ca84ad
  SENSOR_ID: 255f1e7b-1d09-45d5-b025-10d82e35748a

  
  steps:
  - task: PowerShell@2
    inputs:
      targetType: 'inline'
      script: | 
        try {
            Invoke-RestMethod -Method ''POST'' -Uri ''$(WIEURL)/v1/auth'' -SessionVariable ''Session'' -Body @{username = "$(USERNAME)"; password = "$(PASSWORD)"}
        } catch {
            write-warning "$_"
            exit 1
        }
                
        try {
            Invoke-RestMethod -Uri ''$(WIEURL)/v2/scans'' -Method POST -ContentType ''application/json'' -WebSession $Session -Body ''{"name": "Site: $(APP_URL)", "projectVersion":  { "siteId": "$(APP_ID)"}, "scanTemplateId": "$(SCAN_TEMPLATE_ID)", "overrides": { "startMethod": "url", "startUrls": ["$(APP_URL)"], "priority": 1, "sensorID": "$(SENSOR_ID)"}}''
        } catch {
            write-warning "$_"
            exit 1
        }
````