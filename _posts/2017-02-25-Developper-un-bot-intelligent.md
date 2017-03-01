---
layout: post
title: Développer un Bot intélligent
subtitle: Avec MS Bot Framework & LUIS
tags: [C#, .net, Bot Framework, LUIS]
bigimg: /img/banner.png
comments: true
excerpt_separator: <!--more-->
author: Marouan REJEB
---

Hello ! Comme promis, dans cet article, on va continuer avec le Microsoft Bot Framework.  
Objectif ? **Développer un bot intélligent**.  
<!--more-->  
Comme je l'ai déjà expliqué dans l'article précédent, le bot peut-être utilisé comme assistant personnel (qui va nous rappeler nos RDV ou nos tâches à faire, nous commander une pizza ou réserver un billet d'avion...etc).
Vous connaissez _Cortana_ l'assistant personnel intélligent de Windows 10, vous lui posez une question, et il vous répond.  

Comment ces bots arrivent-ils à comprendre ce qu'on a demandé, nous les humains ? C'est grâce à un **service intélligent de compréhension de langage**.  
Il existe plusieurs services de ce types (la plupart d'entre eux sont en version bêta) tels que [wit.ai][wit] et [api.ai][api]. Pour notre bot (un bot de météo), on va utiliser [**LUIS**][luis], un service qui fait partie de [Microsoft Cognitive Services][mcs].  

---

#### LUIS  

[LUIS][luis] (Language Understanding Intelligent Service) est un service qui fournit des modèles (de compréhension) étendus de bing et cortana pour les développeurs à utiliser dans leurs applications. LUIS nous permet également de créer nos propres modèles, et crée des points de terminaison HTTP qui peuvent être utilisés pour renvoyer des réponses JSON simples.  

Pour simplifier, LUIS est le service qui va traduire à notre bot l'**intention** de notre utilisateur. On va faire la démonstration pour notre bot de météo :  

On commence par créer un compte sur [https://luis.ai][luis] et créer notre application. Le mot application ici désigne le modèle LUIS. Ici, on va renseigner le nom de notre modèle, le scénario d'utilisation (IoT, Bot, Mobile Application...etc), la catégorie (_weather_ dans notre cas) et la langue.

![alt text][luisCreate]  

Une fois créé, on arrive sur la page de configuration de notre modèle. Pour cet article, je vais expliquer les 4 fonctionalités qui nous sont primordiales au début : **Intents**, **Entities**, **Utterances** et **Train**. 

 * Intents : C'est les intentions que notre bot doit reconnaitre ou plutôt comprendre. None est une intention créée par défaut pour tous les modèles.
 * Entities : L'entité dans une phrase peut représenter un nom, une date, une ville...etc.
 * Utterances : Ou énoncés. C'est là où on va rentrer des exemples d'entrées utilisateur.
 * Train : C'est le bouton qui va déclencer l'entraînement de notre modèle.

![alt text][luisHome]  

Pour notre bot de météo, on va commencer par créer l'intention **Météo**, l'entité **Ville** (on a la possibilité ici d'utiliser les entités prédéfinies par LUIS) :

![alt text][luisIntent]  

Ici, je ne vais pas créer une action, car comme vous voyez, cette fonctionnalité sera bientôt supprimée (MS Bot Framework est encore en version _Preview_ au moment de l'écriture de cet article). Quand on enregistre cette nouvelle _intention_, LUIS nous demande de saisir un exemple de phrase qu'il devra mapper à notre _intention_ : "Il fait comment à Paris ?". Ensuite, on choisit l'Intent souhaité et on valide (une manière de dire à LUIS que l'intention derrière cette phrase c'est la météo).

![alt text][luisIntentSubmit]  

Ensuite, on rajoute une entité. Cette variable est importante pour notre exemple, car elle va désigner la ville pour laquelle on souhaite avoir l'information. 

![alt text][luisEntity]  

Enfin, la dernière étape de configuration de LUIS, c'est l'entrainement. Sous l'onglet "new utterances", on saisi quelques exemples de phrases, on choisit l'intention qui va avec, on selectionne le mot qui représente notre entité (Paris comme ville), on valide, et on finit par appuyer sur le bouton "**Train**" (en bas à gauche de la page de notre modèle) pour lancer l'entrainement.

![alt text][luisUtterance]  

Maintenant que notre modèle est prêt et entrainé, on va le publier en appuyant sur le bouton "Publish" :

![alt text][luisPublish]  

 Une fois le modèle est publié, la page suivante s'affiche. Comme vous pouvez le remarquer, on champ de saisie est disponible. Quand on tape "Il fait comment à Paris ?", un lien de test se met à jour :

![alt text][luisQuery] 

On clique sur le lien, un nouvel onglet s'ouvre avec une page web affichant ce résultat en Json : 

```json
{
  "query": "Il fait comment à Paris ?",
  "topScoringIntent": {
    "intent": "Meteo",
    "score": 0.999999046
  },
  "intents": [
    {
      "intent": "Meteo",
      "score": 0.999999046
    },
    {
      "intent": "None",
      "score": 0.152071819
    }
  ],
  "entities": [
    {
      "entity": "paris",
      "type": "Ville",
      "startIndex": 18,
      "endIndex": 22,
      "score": 0.9335881
    }
  ]
}
```

Cette réponse Json contient notre question, la liste des intentions (météo et none) et la liste des entités (Paris comme ville). Ce qui est plus important dans tout ça, c'est la notion des scores associés à chaque élèment. Le score retournée a une valeur entre 0 et 1 (0 étant le score le plu bas et 1 le plus élevé).  

Avec ce résultat, LUIS nous dit que pour notre phrase "Il fait comment à Paris ?", il est confiant à 99% (top Scoring Intent) que l'intention c'est la météo, et que l'entité ville, c'est Paris à 93%.

---

#### Comment intégrer LUIS à notre bot ?  

Dans le projet que j'ai créé ([disponible sur Github][source]), j'ai intégré LUIS en utilisant deux méthodes (C'est pour ça que les réponses du bot seront en double). Ici, je vais expliquer la méthode utilisant `LuisDialog` :  

On crée une classe qui va hériter de `LuisDialog` et qu'on va décorer avec l'attribut `LuisModel` dans lequel on va spécifier l'ID de notre modèle (model id) et la clé de l'abonement (subscription key) afin de mapper directement notre application au modèle LUIS. Ensuite, on crée autant de méthodes que d'intentions, comme le montre l'exemple suivant :

![alt text][luisCode]  

Pour appeler cette classe, il suffit de rajouter cette ligne dans la méthode POST qu'on a vu dans l'article précédent :  

```cs
await Conversation.SendAsync(activity, () => new BotDialog());
```

Et voici le résultat final :  

![alt text][WeatherBotResult]  

---  

#### Conclusion
J'espère que cet article était clair et pourra vous aider pour développer votre propre bot intélligent. N'hésitez pas à [télécharger le code source du projet sur github][source] afin de le tester (et l'améliorer), et surtout à lire la documentation officielle. Dans le prochain article, on va montrer comment publier not bot sur Azure. A bientôt !


[wit]: https://wit.ai/
[api]: https://api.ai/
[luis]: https://www.luis.ai/
[mcs]: https://www.microsoft.com/cognitive-services/en-us/
[source]: https://github.com/MarouanRejeb/MsBotFramework

[luisCreate]: https://cdn.rawgit.com/MarouanRejeb/marouanrejeb.github.io/205cf835/img/luisCreate.png
[luisHome]: https://cdn.rawgit.com/MarouanRejeb/marouanrejeb.github.io/205cf835/img/luisHome.png
[luisIntent]: https://cdn.rawgit.com/MarouanRejeb/marouanrejeb.github.io/205cf835/img/luisIntent.png
[luisIntentSubmit]: https://cdn.rawgit.com/MarouanRejeb/marouanrejeb.github.io/205cf835/img/luisIntentSubmit.png
[luisEntity]: https://cdn.rawgit.com/MarouanRejeb/marouanrejeb.github.io/205cf835/img/luisEntity.png
[luisUtterance]: https://cdn.rawgit.com/MarouanRejeb/marouanrejeb.github.io/205cf835/img/luisUtterance.png
[luisPublish]: https://cdn.rawgit.com/MarouanRejeb/marouanrejeb.github.io/205cf835/img/luisPublish.png
[luisQuery]: https://cdn.rawgit.com/MarouanRejeb/marouanrejeb.github.io/205cf835/img/luisQuery.png
[WeatherBotResult]: https://cdn.rawgit.com/MarouanRejeb/marouanrejeb.github.io/205cf835/img/WeatherBotResult.png
[luisCode]: https://cdn.rawgit.com/MarouanRejeb/marouanrejeb.github.io/205cf835/img/luisCode.png