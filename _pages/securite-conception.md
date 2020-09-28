---
title: Sécurité dès la conception
layout: default
permalink: /securite-conception/
---

<ul class="list-unstyled">
  <li>
  <details>
    <summary>
		<h1 class="h3">Table des matières</h1>
	</summary>
	<p>
		<ul>
		<li><a href="#la-sécurité-dès-la-conception-à-edsc"><strong>La sécurité dès la conception à EDSC</strong></a></li>
		<li><a href="#la-sécurité-dès-la-conception-selon-owasp">La sécurité dès la conception selon OWASP</a></li>
		<li><a href="#classification-des-actifs">Classification des actifs</a></li>
		<li><a href="#à-propos-des-attaquants">À propos des attaquants</a></li>
		<li><a href="#principaux-piliers-de-la-sécurité-de-linformation">Principaux piliers de la sécurité de l’information</a></li>
		<li><a href="#architecture-de-sécurité">Architecture de sécurité</a></li>
		<li><a href="#principes-de-sécurité">Principes de sécurité</a><ul>
		<li><a href="#limitez-au-maximum-la-surface-dattaque">Limitez au maximum la surface d’attaque</a></li>
		<li><a href="#configurez-des-paramètres-sécurisés-par-défaut">Configurez des paramètres sécurisés par défaut</a></li>
		<li><a href="#principe-de-moindre-privilège">Principe de moindre privilège</a></li>
		<li><a href="#principe-de-défense-en-profondeur">Principe de défense en profondeur</a></li>
		<li><a href="#échec-sécurisé">Échec sécurisé</a></li>
		<li><a href="#méfiez-vous-des-services">Méfiez vous des services</a></li>
		<li><a href="#séparation-des-tâches">Séparation des tâches</a></li>
		<li><a href="#évitez-la-sécurité-par-lobscurité">Évitez la sécurité par l’obscurité</a></li>
		<li><a href="#recherchez-la-simplicité-en-matière-de-sécurité">Recherchez la simplicité en matière de sécurité</a></li>
		<li><a href="#réglez-correctement-les-problèmes-de-sécurité">Réglez correctement les problèmes de sécurité</a></li>
		</ul>
		</li>
		</ul>
	</p>
  </details>
  </li>
</ul>	

# La sécurité dès la conception à EDSC 
*La sécurité dès la conception est une méthode de développement des logiciels et du matériel informatique qui vise à mettre au point des systèmes qui sont dépourvus de toute vulnérabilité et qui résistent aux attaques informatiques dans la mesure du possible. Ce processus intègre la sécurité dans le cycle de vie de développement des systèmes.*

* En ce moment, lors du développement des solutions, l’assurance de la sécurité est exécutée à la fin du projet, ce qui entraîne des mesures d’atténuation inefficaces.
* Compte tenu de la dynamique des menaces qui se posent à l’heure actuelle, la sécurité de la technologie de l’information (TI) ne peut plus constituer un enjeu accessoire.
* L’objectif est de favoriser une culture axée sur la sécurité dès la conception qui permet d’intégrer la sécurité des TI dès le début du projet ou de la solution, que la solution soit infonuagique ou non.
* Cette méthode permettra au Ministère de se conformer aux conseils en matière de sécurité des TI (ITSG-33), ainsi qu’à la version révisée de la Directive sur la gestion de la sécurité et de la Politique sur la sécurité du gouvernement du Conseil du Trésor, qui sont entrées en vigueur le 1er juillet 2019.

**La Direction générale de l’innovation, de l’information et de la technologie (DGIIT) est en train d’intégrer la sécurité dès la conception dans le cycle de vie de développement des logiciels d’EDSC. Cela signifie que:**
Les applications reliées aux projets:
* intègrent la sécurité, selon un modèle de conception axé sur le risque
* sont dotés des contrôles de sécurité propres au système
* produisent des données probantes concernant l’assurance de la sécurité
La sécurité de la TI:
* évalue les données probantes concernant l’assurance de la sécurité
* autorise le fonctionnement des applications

La sécurité dès la conception constitue un partenariat entre le personnel opérationnel, la Direction générale des services d’intégrité, l’équipe chargée des solutions de la DGIIT et les agents de la sécurité de la TI.
De plus, l’équipe d’ingénierie des Services de technologie de l’information prendra les dispositions nécessaires pour qu’EDSC se joigne au mouvement DevOps (développement et opérations) de sorte que la sécurité soit intégrée de manière permanente dans le cycle de vie du développement (DevSecOps ou développement et opérations de sécurité).
L’Open Web Application Security Project (OWASP ou projet ouvert de la sécurité des applications Web) a établi des principes relatifs à la sécurité dès la conception, décrits ci-dessous, afin d’aider les développeurs à concevoir des applications Web hautement sécurisées.

# La sécurité dès la conception selon OWASP
![Open Web Application Security Project (OWASP)](../assets/OWASP-180x100.png)

Les architectes et les fournisseurs de solutions ont besoin de directives pour développer des applications sécurisées dès la conception. Ils peuvent remplir ce mandat non seulement en mettant en place les contrôles de base documentés dans le texte principal, mais aussi en faisant référence au motif sous-jacent de ces principes. Les principes de sécurité tels que la confidentialité, l’intégrité et la disponibilité – même s’ils sont importants, de portée générale et vagues – ne changent pas. Plus on applique ces principes, plus l’application est résistante aux attaques.

Par exemple, au moment de la validation des données, c’est bien d’inclure une routine de validation centralisée pour toutes les données saisies dans les formulaires. Toutefois, c’est encore mieux d’intégrer un processus de validation à chaque niveau pour toutes les données saisies par l’utilisateur, jumelé à un mécanisme permettant de traiter les erreurs de manière appropriée et de mieux contrôler l’accès.

Depuis un an environ, on constate un véritable engouement pour la normalisation de la terminologie et de la taxonomie. Dans la présente version du Guide de développement, les principes sont normalisés conformément à ceux des principaux textes de l’industrie; toutefois, un ou deux principes figurant dans la première édition du Guide ont été délaissés. Ce changement a été apporté afin de ne pas confondre le lecteur et de mieux se conformer à un ensemble de principes de base. Les principes qui ont été supprimés sont adéquatement abordés par les contrôles dans le texte.

# Classification des actifs
Il est possible de choisir les contrôles uniquement après avoir classifié les données à protéger. Par exemple, le niveau et le nombre des contrôles qui s’appliquent aux systèmes de faible valeur, comme les blogues et les forums, sont différents de ceux qui s’appliquent aux systèmes entourant la comptabilité, les services bancaires de grande valeur et la négociation électronique.

# À propos des attaquants
Au moment de concevoir des contrôles visant à empêcher l’utilisation abusive de l’application, il faut envisager les attaquants les plus probables (en tenant compte de la probabilité et de la perte réelle, de la plus importante à la moins importante):
* Le mécontentement des membres du personnel ou des développeurs
* Les attaques « à la dérobée », comme les effets secondaires ou les conséquences directes d’un virus, d’un ver ou d’une attaque de cheval de Troie
* Les attaquants criminels ayant un motif valable, comme le crime organisé
* Les attaquants criminels n’ayant aucun motif valable d’attaquer votre organisation, comme les défaceurs
* Les pirates adolescents
Le terme « pirate informatique » (hacker, en anglais) ne figure pas dans la liste, car les médias utilisent ce terme au sens affectif de manière incorrecte. Toutefois, il est beaucoup trop tard pour récupérer l’utilisation incorrecte du mot « pirate informatique » pour l’employer dans son sens d’origine. Le Guide de développement emploie systématiquement le mot « attaquant » pour désigner une personne ou un objet qui tente activement d’exploiter une fonctionnalité particulière.

# Principaux piliers de la sécurité de l’information
La sécurité de l’information repose sur les piliers suivants:
* **La confidentialité** – ne permettre l’accès qu’aux données pour lesquelles l’utilisateur a une autorisation
* **L’intégrité** – veiller à ce que les données ne soient pas altérées par des utilisateurs non autorisés
* **La disponibilité** – veiller à ce que les systèmes et les données soient accessibles aux utilisateurs autorisés au moment où ils en ont besoin.

Les principes suivants sont tous reliés à ces trois piliers. Ainsi, au moment de mettre au point un contrôle, il est important de tenir compte de chaque pilier afin de concevoir un contrôle de sécurité efficace.

# Architecture de sécurité
Les applications dépourvues d’une architecture de sécurité ressemblent à des ponts construits sans avoir procédé à une analyse par éléments finis ni à un essai en soufflerie. Ils ressemblent bel et bien à des ponts, mais ils s’effondreront au premier battement des ailes d’un papillon. À l’instar du domaine de la construction de bâtiments ou de ponts, il est essentiel de veiller à la sécurité des applications au moyen de l’architecture de sécurité.

Les architectes d’applications sont chargés de concevoir des modèles de conception qui assurent une protection efficace contre les risques posés par une utilisation normale et les attaques extrêmes. Pour leur part, les concepteurs de ponts doivent prendre en compte non seulement la circulation automobile et piétonnière, mais aussi les vents cycloniques, les tremblements de terre, les incendies, les incidents de circulation et les inondations. Quant aux concepteurs d’applications, ils doivent tenir compte des événements extrêmes, dont les attaques par force brute ou par injection, et la fraude. Les risques qui pèsent sur les concepteurs d’applications sont bien connus. L’époque de l’excuse « on ne le savait pas » est révolue depuis longtemps. La sécurité est désormais considérée comme une composante essentielle, et non comme une nouvelle dépense excessive ou un élément à délaisser.

L’architecture de sécurité renvoie aux piliers fondamentaux : l’application doit être dotée des contrôles nécessaires pour protéger la confidentialité de l’information, préserver l’intégrité des données et donner accès aux données lorsqu’elles sont requises (disponibilité) – et seulement aux utilisateurs autorisés. L’architecture de sécurité n’est pas un mélange de marketing et d’architecture, c’est à dire une multitude de produits de sécurité regroupés ensemble et appelés « solution ». Il s’agit plutôt d’un ensemble soigneusement étudié de fonctionnalités, de contrôles, de processus plus sécuritaires et de posture de sécurité par défaut.

Au moment de développer une nouvelle application ou de réusiner une application existante, il faut tenir compte de chaque fonctionnalité et se poser les questions suivantes: 
* Le processus entourant cette fonctionnalité est-il le plus sécuritaire possible? En d’autres termes, présente t il des lacunes?
* Si j’étais mal intentionné, par quels moyens est-il possible d’utiliser cette fonctionnalité de façon abusive?
* Est ce que la fonctionnalité doit être activée par défaut? Dans l’affirmative, y a-t-il des limites ou des options qui pourraient réduire le risque associé à cette fonctionnalité?

Andrew van der Stock désigne comme suit le processus décrit ci-dessus : « Pensez comme une personne mal intentionnée ». Il recommande de se mettre à la place de l’attaquant et de réfléchir à toutes les façons possibles de faire un usage abusif de chaque fonctionnalité, en tenant compte des trois piliers fondamentaux et en utilisant le modèle STRIDE.

En respectant ce guide, et en utilisant la modélisation des menaces des risques STRIDE/DREAD dont il est question dans le présent document et dans le livre rédigé par Howard et LeBlanc, vous serez en mesure d’adopter officiellement une architecture de sécurité pour vos applications.

Les meilleures conceptions d’architecture de système et les meilleurs documents de conception détaillés prennent en considération la sécurité de chaque fonctionnalité, prévoient des mesures d’atténuation des risques et exposent les dispositions ayant été prises pendant le codage.

L’architecture de sécurité commence à partir de la modélisation des exigences opérationnelles et se termine au moment de la mise hors service de la dernière copie de l’application. La sécurité est un processus continu, et non un accident ponctuel.

# Principes de sécurité
Les principes de sécurité décrits ci après sont extraits de l’édition antérieure du Guide de développement de l’OWASP. Ils ont été normalisés selon les principes de sécurité énoncés dans Écrire du code sécurisé, un livre excellent rédigé par Howard et LeBlanc.

## Limitez au maximum la surface d’attaque
Chaque nouvelle fonctionnalité qui est ajoutée à une application expose l’application à un certain niveau de risque. Le développement sécurisé a pour but d’atténuer le risque global en réduisant la surface d’attaque.

Par exemple, une application Web met en place une aide en ligne avec une fonction de recherche. La fonction de recherche peut être vulnérable aux attaques par injection SQL. Si l’option d’aide est limitée aux utilisateurs autorisés, la probabilité de subir une attaque est réduite. Si la fonction de recherche de l’option d’aide est contrôlée au moyen de routines centralisées de validation des données, la capacité d’effectuer une injection SQL est considérablement réduite. Toutefois, si l’option d’aide est réécrite afin d’éliminer la fonction de recherche (grâce à une meilleure interface utilisateur, par exemple), ce changement élimine quasiment la surface d’attaque, même si l’option d’aide est accessible à l’ensemble des utilisateurs du Web.

## Configurez des paramètres sécurisés par défaut
Il existe de nombreuses façons d’offrir une expérience « hors des schémas habituels » aux utilisateurs. Toutefois, par défaut, l’expérience devrait être sécurisée, et c’est l’utilisateur qui devrait décider de réduire ou non le niveau de sécurité – s’il est autorisé à le faire.

Par exemple, par défaut, l’expiration et la complexité des mots de passe devraient être activées. Les utilisateurs peuvent être autorisés à désactiver ces deux fonctions pour simplifier l’utilisation de l’application et accroître leur risque.

## Principe de moindre privilège
Le principe de moindre privilège recommande d’octroyer aux comptes le minimum de privilèges nécessaires à l’exécution de leurs processus opérationnels. Cela comprend les droits d’utilisateur, les autorisations relatives aux ressources comme les limites de l’unité centrale, la mémoire, le réseau, et les autorisations relatives au système de fichiers.

Par exemple, si un serveur intergiciel n’a besoin que d’un accès au réseau, d’un accès en lecture à un tableau dans une base de données et de la capacité d’écrire dans un registre, il faut uniquement lui accorder ces autorisations. Les intergiciels ne doivent en aucun cas bénéficier de privilèges administratifs.

## Principe de défense en profondeur
Le principe de défense en profondeur implique qu’il est raisonnable d’instaurer un seul contrôle, mais qu’il est préférable de mettre en place plusieurs contrôles qui abordent les risques de différentes façons. Lorsque les contrôles sont utilisés en profondeur, il est très difficile d’exploiter les vulnérabilités graves, réduisant ainsi la probabilité de subir une attaque.

Avec le codage sécurisé, la défense en profondeur peut se manifester par la validation par niveaux, les contrôles centralisés de vérification et le fait de demander aux utilisateurs d’ouvrir une session sur toutes les pages.

Par exemple, il est peu probable qu’une interface administrative défectueuse soit vulnérable à une attaque anonyme si elle bloque correctement l’accès aux réseaux de gestion de la production, vérifie l’autorisation de l’administrateur et journalise tous les accès.

## Échec sécurisé
Il est fréquent que les applications ne soient pas en mesure de traiter les transactions, et ce, pour de nombreux motifs. Cette situation peut être causée par l’échec de la connexion à une base de données ou par l’inexactitude des données saisies par l’utilisateur. La façon dont une application échoue peut déterminer si elle est sécurisée ou non. L’échec ne devrait pas donner de privilèges supplémentaires à l’utilisateur. De plus, il ne devrait pas afficher de données de nature délicate le concernant, comme les requêtes dans les bases de données ou les journaux. Selon ce principe, les applications devraient échouer de façon sécurisée.

## Méfiez vous des services
De nombreuses organisations utilisent les capacités de traitement de partenaires tiers, qui ont probablement mis en place des politiques et une posture de sécurité différentes des leurs. Il est peu probable que vous puissiez influencer ou contrôler un tiers externe, qu’il s’agisse d’un utilisateur à domicile, ou bien d’un fournisseur ou d’un partenaire important.

Par conséquent, il n’est pas justifié de se fier implicitement aux systèmes gérés à l’externe. Tous les systèmes externes devraient être traités de la même façon.

Par exemple, un fournisseur de programme de fidélisation fournit des données, utilisées par les services bancaires par Internet, qui indiquent le nombre de points de récompense accumulés et la courte liste d’articles pouvant être obtenus en échange des points. Toutefois, les données devraient être vérifiées pour s’assurer qu’il est sécuritaire de les divulguer aux utilisateurs finaux et que les points de récompense sont un nombre positif, et pas très élevé.

## Séparation des tâches
La séparation des tâches est l’une des principales mesures de lutte contre la fraude. Par exemple, un utilisateur qui demande un ordinateur ne doit pas être autorisé à signer cette tâche ni ne devrait recevoir directement l’ordinateur. Cette procédure l’empêche de demander plusieurs ordinateurs et de prétendre qu’ils ne sont jamais arrivés.

Certains rôles sont associés à des niveaux de confiance différents de ceux des utilisateurs normaux. Les administrateurs, par exemple, sont différents des utilisateurs no
# Paste Your Document In Here

## And a table of contents

will be generated

## On the right

side of this page.

rmaux. En règle générale, les administrateurs ne devraient pas utiliser l’application.

Par exemple, un administrateur devrait être en mesure d’activer ou de désactiver le système, ou bien d’établir la politique relative aux mots de passe, mais il ne devrait pas être autorisé à ouvrir une session dans la vitrine virtuelle en tant qu’utilisateur jouissant de « super privilèges », notamment pour « acheter » des articles au nom d’autres utilisateurs.

## Évitez la sécurité par l’obscurité

La sécurité par l’obscurité est un contrôle de sécurité faible; ce contrôle a presque toujours du mal à assurer la sécurité lorsqu’il est utilisé seul. Le fait de garder secrète la conception n’est pas une mauvaise idée en soi. Toutefois, la sécurité des systèmes clés ne devrait pas reposer sur la non divulgation d’informations.

Par exemple, la sécurité d’une application ne devrait pas reposer sur la non divulgation du code source. La sécurité devrait plutôt reposer sur de nombreux autres facteurs tels que des politiques raisonnables en matière de mots de passe, la défense en profondeur, des restrictions concernant les transactions commerciales, une architecture de réseau solide, ainsi que des contrôles de vérification et de lutte contre la fraude.

Prenons Linux comme exemple. Le code source de Linux est largement disponible. Or, lorsqu’il est bien sécurisé, Linux est un système d’exploitation robuste, sécuritaire et fiable.

## Recherchez la simplicité en matière de sécurité
La surface d’attaque et la simplicité vont de pair. Certaines tendances éphémères entourant le génie logiciel privilégient des approches trop complexes au lieu d’opter pour un code relativement simple.

Les développeurs devraient éviter d’utiliser des doubles négatifs et des architectures complexes lorsqu’une approche plus simple serait plus rapide et plus facile.  La mise en place de mécanismes très complexes peut augmenter le risque d’erreurs.

Par exemple, bien qu’il puisse être à la mode d’avoir un mélange de composants Bean de session singleton sur un serveur intergiciel distinct, il est plus sécuritaire et plus rapide d’utiliser des variables globales avec un mécanisme approprié d’exclusion mutuelle pour se protéger contre une situation de compétition.

## Réglez correctement les problèmes de sécurité
Une fois qu’un problème de sécurité a été identifié, il est important d’en faire l’essai pour comprendre la cause fondamentale. Lorsque des modèles de conception sont utilisés, il est probable que le problème de sécurité affecte toutes les bases de codes. C’est pourquoi il est essentiel de trouver la bonne solution sans introduire de régressions.

Par exemple, un utilisateur constate qu’il a accès au solde d’un autre utilisateur en ajustant son témoin. La solution semble relativement simple. Or, puisque le code relatif à la gestion des témoins s’applique à toutes les applications, le fait de modifier une seule application aura une incidence sur toutes les autres applications. La solution doit donc être testée sur toutes les applications touchées.