---
title: Comment créer des certificats auto-signés sur Windows 10
layout: default
date: 2020-10-16
---

McAfee analyse le contenu du code et se déclenche lorsqu'il devient nerveux à propos de certains comportements (comme jouer dans le registre ou appeler d'autres programmes, etc.). Il peut bloquer les programmes internes ainsi que les logiciels standard. La solution la plus simple consiste à reconnaître la source comme étant fiable. Par exemple, si Adobe émet un logiciel, McAfee lui fera confiance car nous faisons confiance à sa réputation (même si une partie du code est suspecte).

La solution recommandée consiste à émettre des certificats de "signature de code" aux programmeurs. Dans McAfee Threat Intelligence Exchange (TIE), une réputation de confiance pourrait être donnée à la provenance (émetteur de certificat) et nous ferions automatiquement confiance à tous les programmeurs. C'est la solution propre et soutenable. Si un programmeur vendait son certificat EDSC sur le "Dark Web" et que nous le découvrions, le certificat pourrait être révoqué par l'autorité de certification ou se voir attribuer une non-confiance à ce certificat particulier dans TIE.

## Instructions pour créer des certificats auto-signés

1. Ouvrez une fenêtre PowerShell **en tant qu'Administrateur** et entrez la commande suivante: 

   `New-SelfSignedCertificate -CertStoreLocation Cert:\LocalMachine\My -DnsName "mysite.local" -FriendlyName "MySiteCert" -NotAfter (Get-Date).AddYears(10)`

   Cela créera un certificat auto-signé spécifique pour mysite.local qui est valable 10 ans. Vous pouvez modifier le nombre d'années en modifiant la valeur dans la fonction AddYears.
   
   ![Créer un nouveau certificat à l'aide de PowerShell](../assets/creer-cert-auto-signes/instruction-1.PNG)
   
2. Une fois le certificat créé, vous devez le copier dans le magasin des autorités de certification racines de confiance.

3. À l'aide de la recherche Cortana dans Windows 10, tapez «certlm.msc» et **exécutez-le en tant qu'administrateur**.

4. Dans le panneau de gauche, accédez à Certificats - Ordinateur local → Personnel → Certificats
   
   ![Afficher le magasin de certificats](../assets/creer-cert-auto-signes/instruction-2.PNG)
	   
5. Localisez le certificat créé (dans cet exemple, regardez sous la colonne Émis à "mysite.local" ou sous la colonne Nom convivial "MySiteCert").
   
6. Dans le panneau de gauche, ouvrez (mais ne naviguez pas vers) Certificats - Ordinateur local → Autorités de certification racines de confiance → Certificats
   
7. Avec le bouton droit de la souris, faites glisser et déposez le certificat de **Personnel → Certificats** vers **Autorités de certification racines de confiance → Certificats** ouverts à l'étape précédente.
   
8. Sélectionnez "Copier ici" dans le menu contextuel.

## Exporter un certificat

Pour exporter le certificat du magasin local vers un fichier PFX (Personal Information Exchange), utilisez la commande Export-PfxCertificate.

Lorsque vous utilisez **Export-PfxCertificate**, vous devez soit créer et utiliser un mot de passe, soit utiliser le paramètre "-ProtectTo" pour spécifier les utilisateurs ou groupes qui peuvent accéder au fichier sans mot de passe. Notez qu'une erreur s'affichera si vous n'utilisez ni le paramètre "-Password" ni "-ProtectTo".

### Utilisation du mot de passe

   `$password = ConvertTo-SecureString -String <Your Password> -Force -AsPlainText`

   `Export-PfxCertificate -cert "Cert:\CurrentUser\My\<Certificate Thumbprint>" -FilePath <FilePath>.pfx -Password $password`

### Utilisation avec ProtectTo
   
   `Export-PfxCertificate -cert Cert:\CurrentUser\My\<Certificate Thumbprint> -FilePath <FilePath>.pfx -ProtectTo <Nom d'utilisateur ou nom de groupe>`

   Après avoir créé et exporté votre certificat, vous êtes prêt à signer votre package d'application avec **SignTool**. Pour l'étape suivante du processus d'empaquetage manuel, voir [Signer un package d'application à l'aide de SignTool](https://docs.microsoft.com/fr-ca/windows/msix/package/sign-app-package-using-signtool). 
   
> Référence: [https://docs.microsoft.com/fr-ca/windows/msix/package/create-certificate-package-signing](https://docs.microsoft.com/fr-ca/windows/msix/package/create-certificate-package-signing)
   