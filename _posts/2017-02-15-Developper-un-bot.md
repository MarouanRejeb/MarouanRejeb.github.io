---
layout: post
title: Développer un Bot
subtitle: Avec Microsoft Bot Framework
tags: [C#, .net, Bot Framework]
bigimg: /img/banner.png
comments: true
excerpt_separator: <!--more-->
author: Marouan REJEB
---

Hello ! Dans cet article, on va expérimenter le nouveau [bot framework (preview)][dev] de Microsoft.
<!--more-->

Ce framework nous permet de développer (en C# ou en Node.js) et de déployer des bots intélligents avec lesquels on pourra échanger
sur différentes plateformes (Slack, Skype, Facebook messenger...).  
Ces bots peuvent être utilisés par des organisations ou des entreprisent afin d'être plus proches de leurs clients, ou par des personnes
en tant qu'assistant personnel (comme Hello Jarvis, le bot de facebook).  

---

#### Microsoft Bot Framework  
Microsoft Bot Framework est une plateforme [open-source][builder] et gratuite pour développer et créer des bots.
Elle est composée de : 

* Bot Builder SDK : Cet SDK est open source (disponible en C# et NodeJs) et nous fournit des fonctionnalités pour modéliser notre conversation, la gestion d'état, etc.  
* Bot Connector : C'est un adaptateur entre notre bot et de nombreux canaux qu'il supporte. Il gère aussi le service de stockage, le routage des messages, etc.  
* Bot Directory : C'est un répertoire public pour les bots publiés. Les bots sont validés avant d'être listés et accessibles au public sur [Bot Directory][directory].  

---  

#### Objectif : Développer un bot en utilisant Bot Builder en REST API  
Pour commencer, vous devez :  

 1. Avoir la dernière version de Visual Studio 2015  
 2. Télécharger le Bot Framework Connector SDK .NET template [ici][connector]  
 3. Placer l'archive sous "_\Visual Studio 2015\Templates\ProjectTemplates\Visual C#_"  
 4. Ouvrir VS et créer un nouveau projet C# avec ce nouveau Bot Application template   

![alt text][BotApp] 

Lorsque nous allons créer un nouveau projet à l'aide du modèle de bot (le template téléchargé), nous verrons que le bot n'est rien d'autre qu'un simple projet Web Api.  
En fait, ce bot est un service Web stupide. Cette **Web API** sera hébergée et enregistrée avec _Microsoft Bot Connector_. 
Ce connecteur sert d'adaptateur entre notre service Web et différents canaux. 

![alt text][archi]  

Ce template est un exemple fonctionnel, sous forme de **REST API**, de ce qu'on appelle un **Echo bot** qui va retourner l'entrée utilisateur.
La méthode **POST** (Controllers\MessagesController.cs) est la fonctionnalité de base de cet exemple :  

![alt text][botcode]  

Cet méthode POST accepte un objet de type `Activity` qui représente les données JSON que nous échangeons avec le bot connector.
La propriété `Text` va contenir le message tapé par l'utilisateur.  
Le type d'activié `ActivityTypes.Message` représente la communication entre le bot et l'utilisateur.
Les autres types d'activités sont présents dans la méthode `HandleSystemMessage(Activity message)`.

Quand on build et on lance le bot, un navigateur se lance (Microsoft Edge dans cet exemple) qui va héberger notre programme
avec l'adresse _http://localhost:3979/_ .  
Le message affiché peut-être modifié (le fichier _default.htm_) :  

![alt text][botlocalhost] 

---

#### Comment tester notre Bot ?  

Afin de tester ce bot, localement, on doit installer le **[Bot Framework Emulator][emulator]**.  
Ici, on va spécifier l'adresse locale qu'on a eu au lancement du bot :  
<i class="fa fa-hand-o-right" aria-hidden="true"></i>  localhost:3979/**api/message**

![alt text][botemulatorconfig] 

Pour le moment, on va laisser les deux champs _App ID_ et _App Pwd_ vides car on va tester notre bot en local 
(on verra leurs utilisation dans un prochain article). Alors, on teste ?  

![alt text][botresult] 

Tatataaa ! Voilà notre echo bot, qui nous répond <i class="fa fa-android" aria-hidden="true"></i>  

---

#### Conclusion  
Dans cet article, on a vu comment développer et tester localement un simple echo bot en utilisant Microsoft Bot Framework.
Je vous invite à [télécharger][sdk] le **[Bot Builder][builder] SDK** depuis sa page Github qui contient plusieurs autres exemples.  

Dans le prochain article, on va rendre notre bot plus intélligent. Si vous avez des questions, posez-les en commentaires. A bientôt !

[directory]: https://bots.botframework.com/
[dev]: https://dev.botframework.com/
[builder]: https://github.com/Microsoft/BotBuilder
[sdk]: https://docs.botframework.com/en-us/downloads
[connector]: http://aka.ms/bf-bc-vstemplate
[emulator]: https://aka.ms/bf-bc-emulator

[BotApp]: https://cdn.rawgit.com/MarouanRejeb/marouanrejeb.github.io/f7eb51fd/img/BotApp.png
[archi]:  https://cdn.rawgit.com/MarouanRejeb/marouanrejeb.github.io/5a5b3e52/img/archi.png
[botcode]: https://cdn.rawgit.com/MarouanRejeb/marouanrejeb.github.io/f7eb51fd/img/botcode.png
[botlocalhost]: https://cdn.rawgit.com/MarouanRejeb/marouanrejeb.github.io/f7eb51fd/img/botlocalhost.png
[botemulatorconfig]: https://cdn.rawgit.com/MarouanRejeb/marouanrejeb.github.io/f7eb51fd/img/botemulatorconfig.png
[botresult]: https://cdn.rawgit.com/MarouanRejeb/marouanrejeb.github.io/f7eb51fd/img/botresult.png