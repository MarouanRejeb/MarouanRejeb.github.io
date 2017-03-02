---
layout: post
title: Déployer un Bot dans Azure
subtitle: MS Bot Framework & Microsoft Azure
tags: [Bot Framework, Azure]
bigimg: /img/banner.png
comments: true
excerpt_separator: <!--more-->
author: Marouan REJEB
---

Hello ! Dans ce dernier article de la série "Microsoft Bot Framework", on va déployer le bot météo qu'on a développé, dans Azure.  
<!--more-->  

#### Prérequis  
Afin de pouvoir déployer dans Azure, vous devez avoir un abonnement Azure. Vous pouvez avez la possibilité de profiter de l'une de ces deux offres (ou les deux ensemble) : 
 *  [Developer benefit program][essentials] : Si vous êtes inscrit en tant que développeur Windows, vous faites automatiquement partie du programme Dev Center Benefits. Vous aurez 20€ par mois à dépenser dans Azure.
 *  [Microsoft Azure trial][azure] : Créez un compte et profitez de 170€ pendant un mois.  

---

#### Enregistrer le bot avec Microsoft Bot Framework 

La première chose qu'on fera, c'est d'enregistrer notre bot.  
On va sur le site [https://dev.botframework.com][dev] et on se connecte avec notre compte Microsoft. Le menu suivant s'affiche sous l'onglet "_Register a bot_". On saisi le nom du bot, le titre et la description. ensuite, on demande la création de l'**ID** et le **Password** du bot.

![alt text][botRegister]  

Quand on appuie sur "_Generate Microsoft App ID and Password_", un ID (guid) de bot sera généré. 
Ensuite, on génére le mot de passe :

![alt text][pwdGenerate]  

On note ces informations quelque part (dans fichier texte par exemple).

![alt text][pwdGenerate2]  

Maintenant que le Bot est enregistré, on doit mettre à jour les clés du fichier web.config dans notre [projet Visual Studio][source]. On modifie les valeurs dans le fichier de configuration pour qu'elles correspondent à celles générées à l'étape précedente.

![alt text][botWebConfig]  

Tout est prêt pour l'étape suivante.  

#### Deployer dans Azure

Dans Visual Studio, clic droit sur la solution, ensuite "Publish"...

![alt text][botPublish1]  

On choisit comme cible : "Microsoft Azure App Service", suivant...

![alt text][botPublish2]  

On choisit l'abonnement à utiliser et le groupe des ressources si on en a un déjà, sinon, créer un nouveau (bouton _new_), et on valide...

![alt text][botPublish3]  

On arrive sur un écran pré-rempli avec l'URL de déploiement (qu'on doit garder pour nos tests), le mot de passe...etc. Suivant...

![alt text][botPublish4]  

On choisit la configuration du déploiement (debug ou release). Suivant...

![alt text][botPublish5]  

Tout est prêt, tout est bon. On publie.

![alt text][botPublish6]  

C'était simple et rapide. Le bot est maintenant déployé dans Azure.  


#### Tester le bot  

On lance le _Bot emulator_ et on renseigne :  

 * L'URL du bot dans Azure (qui a été générée lors de la publication du bot).
 * Microsoft App ID (généré lors de l'enregistrement du bot).
 * Microsoft App Password (généré lors de l'enregistrement du bot).

![alt text][botAzureTest1]  

Une manière plus simple de tester, c'est directement sous l'onglet "_My bots_" sur le site [https://dev.botframework.com][dev] : Le web chat.  

![alt text][botAzureTest2]  

On peut également tester le bot sur skype (et oui, on n'a rien à faire). Il suffit de le chercher par le nom qu'on la donné au bot au moment de son enregistrement.  

![alt text][botSkype] 

#### Conclusion  

Tout au long de ces trois derniers articles, on a vu comment développer un bot avec _Microsoft Bot Framework_, le rendre intélligent 
avec _LUIS_, et enfin le déployer dans _Azure_. J'espère que ça pourra vous aider à bien démarrer avec les bots. Et comme d'habitude, n'hésitez pas à me faire part de vos remarques.  

A bientôt !



[essentials]: https://www.visualstudio.com/fr/dev-essentials/
[azure]: https://azure.microsoft.com/fr-fr/
[dev]: https://dev.botframework.com
[source]: https://github.com/MarouanRejeb/MsBotFramework


[botRegister]: /img/botRegister.png
[pwdGenerate]: /img/pwdGenerate.png
[pwdGenerate2]: /img/pwdGenerate2.png
[botWebConfig]: /img/botWebConfig.png
[botPublish1]: /img/botPublish1.png
[botPublish2]: /img/botPublish2.png
[botPublish3]: /img/botPublish3.png
[botPublish4]: /img/botPublish4.png
[botPublish5]: /img/botPublish5.png
[botPublish6]: /img/botPublish6.png
[botAzureTest1]: /img/botAzureTest1.png
[botAzureTest2]: /img/botAzureTest2.png
[botSkype]: /img/botSkype.png