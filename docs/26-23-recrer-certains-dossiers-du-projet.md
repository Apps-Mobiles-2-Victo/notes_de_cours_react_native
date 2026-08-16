# 23. Recréer certains dossiers du projet

# 23. Recréer certains dossiers du projet

## 23.1 Recréer le dossier node\_modules

Le dossier node\_modules d'une application React Native est généralement volumineux et il contient un nombre impressionnant de petits fichiers. Par exemple, dans une petite application toute simple que j'ai codée, il pesait 381 Mo et contenait 29 715 fichiers.

Ce dossier peut facilement être recréé à partir des informations du fichier package.json qui liste les dépendances du projet. C'est pourquoi il est d'usage de ne pas l'inclure lorsqu'on crée un dossier compressé du projet. Il ne faut surtout pas l'inclure lorsqu'on soumet le projet à Git.

La procédure qui suit permet donc d'installer un projet React Native sur un autre ordinateur à partir d'un fichier .zip ou à partir de Git.

* Assurez-vous d'avoir pris en note les configurations effecutées dans les fichiers du dossier node\_modules, par exemple [apical\_lien\_interne][interagir\_avec\_sqlite\_dans\_une\_application\_react\_native,celles qui permettent d'utiliser la bibliothèque react-native-sqlite-storage][/apical\_lien\_interne].
* Si ce n'est pas déjà fait, supprimez le dossier node\_modules du projet.
* Supprimez le fichier package-lock.json situé à la racine du projet.
* Dans une fenêtre Terminal, lancez ces commandes.

  Terminal

  cd /chemin/projet  
  npm install
* Au besoin, remettez en place les configurations spécifiques qui avaient été réalisées dans les fichiers du dossier node\_modules.
* Si vous travaillez sous macOS, assurez-vous que le fichier projet/android/gradlew est exécutable.

  Terminal

  cd /chemin/projet/android  
  chmod 777 gradlew
* Vous pouvez maintenant lancer l'application à partir du Terminal.

  Terminal

  cd /chemin/projet  
  npm run android
* Si l'application ne démarre pas, essayez plutôt cette commande. Ceci ouvrira deux fenêtres et la première pourrait vous donner plus d'informations sur ce qui ne va pas.

  Terminal

  cd /chemin/projet  
  npm run android --verbose

## 23.2 Recréer le dossier android

Dans un projet React Native, le dossier Android situé à la base du projet peut être passablement volumineux.

Puisque ce dossier peut être regénéré, il n'est pas utile de l'inclure dans votre outil de suivi de versions.

Pour recréer le dossier Android dans votre application, que j'appellerai ici MonProjet :

* Créez un nouveau projet React Native, que j'appellerai ici ProjetDeBase.
* Ajoutez les paquets dont votre application a besoin.
* Copiez le dossier Android de cette application vide vers l'application MonProjet.
* Il faut maintenant corriger les endroits où le nom du projet est codé en dur :
  + Renommer le dossier MonProjet/android/app/src/main/java/com/projetdebase  en  MonProjet/android/app/src/main/java/com/monprojet.
  + Dans le fichier MonProjet/android/app/src/main/java/com/monprojet/MainActivity.kt, corriger le nom du projet à la ligne getMainComponentName().

    Fichier MainActivity.kt

    override fun getMainComponentName(): String = "MonProjet"

Il reste encore des traces du nom ProjetDeBase dans l'application mais elles n'empêchent pas l'application de fonctionner.

Vous pouvez donc lancer votre projet normalement.

Si vous désirez retirer toutes les traces, ouvrez le projet dans Visual Studio Code et appuyez sur Ctrl + Maj + F sous Windows ou ⌘ Cmd + ⇧ Maj + F sous Mac pour effectuer une recherche du mot ProjetDeBase dans tout le projet puis remplacez chaque occurence par le nom de votre projet.