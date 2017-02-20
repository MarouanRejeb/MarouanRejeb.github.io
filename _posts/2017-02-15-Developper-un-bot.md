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
Ces bots peuvent être utilisés par des organisations ou des entreprisent afin d'offrir un service, ou par des personnes
en tant qu'assistant personnel (comme Hello Jarvis, le bot de facebook).

#### Objectif : Développer un bot en utilisant Bot Builder en REST API  
Pour commencer, vous devez :  

 1. Avoir la dernière version de Visual Studio 2015  
 2. Télécharger le Bot Framework Connector SDK .NET template [ici][connector]  
 3. Placer l'archive sous "_\Visual Studio 2015\Templates\ProjectTemplates\Visual C#_"  
 4. Ouvrir VS et créer un nouveau projet C# avec ce nouveau Bot Application template   

<p align="center">
    <img src="\img\BotApp.png" alt="chart" style="width: 100%; height: 100%"/>
</p>  

Ce template est un exemple fonctionnel, sous forme de **REST API**, de ce qu'on appelle un **Echo bot** qui va retourner l'entrée utilisateur.
La méthode **POST** (Controllers\MessagesController.cs) est la fonctionnalité de base de cet exemple :  

<p align="center">
    <img src="\img\botcode.png" alt="chart" style="width: 100%; height: 100%"/>
</p>  

Quand on build et on lance le bot, un navigateur se lance (Microsoft Edge dans cet exemple) qui va héberger notre programme
avec l'adresse _http://localhost:3979/_ .  
Le message affiché peut-être modifié (le fichier _default.htm_) :  

<p align="center">
    <img src="\img\botlocalhost.png" alt="chart" style="width: 100%; height: 100%"/>
</p>  

#### Comment tester notre Bot ?  

Afin de tester ce bot, localement, on doit installer le **[Bot Framework Emulator][emulator]**.  
Ici, on va spécifier l'adresse locale qu'on a eu au lancement du bot :  
<i class="fa fa-hand-o-right" aria-hidden="true"></i>  localhost:3979/**api/message**

<p align="center">
    <img src="\img\botemulatorconfig.png" alt="chart" style="width: 100%; height: 100%"/>
</p> 

Pour le moment, on va laisser les deux champs _App ID_ et _App Pwd_ vides car on va tester notre bot en local 
(on verra leurs utilisation dans un prochain article). Alors, on teste ?  

<p align="center">
    <img src="\img\botresult.png" alt="chart" style="width: 100%; height: 100%"/>
</p>  

Tatataaa ! Voilà notre echo bot, qui nous répond <i class="fa fa-android" aria-hidden="true"></i>  

#### Conclusion  
Dans cet article, on a vu comment développer et tester localement un simple echo bot en utilisant Microsoft Bot Framework.
Je vous invite à [télécharger][sdk] le **[Bot Builder][builder] SDK** depuis sa page Github qui contient plusieurs autres exemples.  

Dans le prochain article, on va déployer notre bot dans Azure. A bientôt !

[dev]: https://dev.botframework.com/
[builder]: https://github.com/Microsoft/BotBuilder
[sdk]: https://docs.botframework.com/en-us/downloads
[connector]: http://aka.ms/bf-bc-vstemplate
[emulator]: https://aka.ms/bf-bc-emulator