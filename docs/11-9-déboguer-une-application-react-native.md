# 8. Déboguer une application React Native

## 8.1 Menu Dev de l'émulateur

Pour ouvrir le menu Dev de l'émulateur :

* Dans la fenêtre Metro, appuyez sur d

  ![Fenêtre Metro](assets/11-9-déboguer-une-application-react-native-42.png)

  ou
* Dans la fenêtre de l'émulateur, appuyez sur Ctrl + M sous Windows ou Cmd ⌘ + M sous macOS.

Un menu apparaîtra directement dans l'émulateur et vous offrira différentes options utiles.

![DevMenu](assets/11-9-déboguer-une-application-react-native-43.png)

## 8.2 Les outils de développement (DevTools) { #devtools }

Pour ouvrir les outils de développement de React Native :

* Dans la fenêtre Metro, appuyez sur j

  ![Fenêtre Metro](assets/11-9-déboguer-une-application-react-native-42.png)

  ou
* Dans la fenêtre de l'émulateur, appuyez sur Ctrl + M sous Windows ou Cmd ⌘ + M sous macOS puis sélectionnez Open DevTools.

Une fois ce menu ouvert, vous avez accès à des outils de débogage qui ressemblent à ceux de Google Chrome.

Vous pouvez :

* Cliquer sur l'onglet Console pour accéder à une console qui permet de voir des messages et d'évaluer des expressions
* Cliquer sur l'onglet Sources pour voir le code JavaScript, interroger des variables et mettre des points d'arrêt
* Cliquer sur l'onglet Components pour questionner l'état d'un composant
* etc.

![DevTools](assets/11-9-déboguer-une-application-react-native-45.png)

## Pour plus d'information

« Debugging Basics ». React Native. <https://reactnative.dev/docs/debugging>

« React Native DevTools ». React Native. <https://reactnative.dev/docs/react-native-devtools>

## 8.3 console.log()

Dans une application React Native, il est possible d'utiliser console.log() pour afficher un message et ainsi faciliter le débogage.

Par exemple, pour afficher la valeur de la variable maVariable, vous pouvez utiliser cette commande :

React Native

console.log(`maVariable: ${maVariable}`);

Le résultat apparaîtra dans [la fenêtre DevTools](#devtools), onglet Console.

![console.log](assets/11-9-déboguer-une-application-react-native-46.png)

Notez qu'auparavant, ils apparaissaient dans le Terminal de Metro mais ce comportement a changé depuis l'arrivée de [React Native 0.77](https://reactnative.dev/blog/2025/01/21/version-0.77) en janvier 2025.

## 8.4 Ajouter un point d'arrêt dans React ou React Native

Les points d'arrêt sont un outil indispensable lorsqu'on débogue une application.

Je vous montre ici comment ajouter un point d'arrêt pour déboguer du code React et du code React Native à l'aide de différentes techniques :

* [Dans les outils de développement](#outilsdeveloppement)
* [Par programmation (commande debugger)](#debugger)

## Dans les outils de développement { #outilsdeveloppement }

Pour placer un point d'arrêt à partir des outils de développement :

* dans un projet React, utiliser les outils de développement du navigateur. Par exemple, lorsque le code est exécuté dans Chrome : clic droit / Inspecter;
* dans un projet React Native, appuyer sur j dans la fenêtre Metro.

Dans les deux cas, l'onglet Sources donne accès au code source en cours d'exécution. Un clic sur une ligne ajoutera un point d'arrêt.

Voici un exemple en React.

![Point d'arrêt dans React](assets/11-9-déboguer-une-application-react-native-47.png)

Et voici un exemple en React Native.

![Point d'arrêt dans React Native](assets/11-9-déboguer-une-application-react-native-48.png)

Notez que dans la fenêtre Metro, il est possible d'appuyer sur la touche r pour lancer l'application à nouveau.

Ceci est souvent utile pour exécuter à nouveau du code rencontré seulement lors du lancement de l'application.

## Par programmation (commande debugger) { #debugger }

Il est également possible d'ajouter un point d'arrêt par programmation en ajoutant la commande [debugger](https://dev.to/colocodes/how-to-debug-a-react-app-51l4#using-the-raw-debugger-endraw-statement) dans le code à l'endroit où l'on veut que le débogueur s'arrête.

![debugger;](assets/11-9-déboguer-une-application-react-native-49.png)

En React comme en React Native, le point d'arrêt ne sera pris en compte que s'il est rencontré alors que les outils de développement sont ouverts.