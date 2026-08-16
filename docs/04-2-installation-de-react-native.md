# 2. Installation de React Native

# 2. Installation de React Native

## 2.1 React Native

React Native utilise le langage de programmation JavaScript.

Si vous avez déjà développé une application Web avec React, vous réussirez à coder en React Native rapidement.

Avec React Native, votre code vous permettra de créer une application pour Android et pour iOS mais à certaines conditions.

D'abord, pas la plupart des situations, vous aurez besoin d'un ordinateur Apple pour tester et pour publier une application pour iOS.

Ensuite, il faudra probablement apporter quelques modifications au code afin de le rendre optimal pour Android ou pour iOS. Le but est d'obtenir une application qui est le plus près de l'apparence et du comportement par défaut de la plateforme.

> React Native’s tagline, “Learn once, write anywhere” differs from the usual cross-platform mantra, “Write once, run anywhere.”[1](https://www.toptal.com/mobile/comparing-react-native-to-cordova#:~:text=React%20Native%E2%80%99s%20tagline)

![Welcome to React Native](assets/04-2-installation-de-react-native-31.png)

## Source

1. « The Duel: React Native vs. Cordova ». Toptal. [https://www.toptal.com/mobile/comparing-react-native-to-cordova](https://www.toptal.com/mobile/comparing-react-native-to-cordova#:~:text=React%20Native%E2%80%99s%20tagline)

## Pour plus d'information

« React Native Beginners Tutorial—A Cold Dive ». Toptal. <https://www.toptal.com/react-native/cold-dive-into-react-native-a-beginners-tutorial>

« The Ultimate React Native Tutorial ». Simpli Learn. <https://www.simplilearn.com/react-native-tutorial-article>

« Créer une application React Native ». Grafikart. <https://grafikart.fr/tutoriels/react-native-pokedex-2245>

« Formation React ». Dyma. <https://dyma.fr/formations/react>

## 2.2 Installation de React Native

Pour développer une application React Native, vous aurez besoin de quelques morceaux. Les détails d'installation sont fournis plus bas, je vous donne ici simplement une vue d'ensemble pour vous aider à comprendre.

Vous pouvez utiliser l'éditeur de votre choix pour développer votre application mais vous aurez besoin d'Android Studio pour gérer les émulateurs.

Notez qu'ici, j'ai choisi de ne pas utliser [Expo](https://expo.dev/), un écosystème d'outils pour React Native.

Pour comprendre ce que l'utilisation d'Expo implique, lisez ceci : <https://upstackhq.com/blog/software-development/should-i-use-expo-for-react-native>.

Dans cette fiche :

* [Programmation pour Android](https://apical.xyz/formations/pageunique/applications_mobiles_avec_react_native20251210101311#android)
* [Programmation pour iOS](https://apical.xyz/formations/pageunique/applications_mobiles_avec_react_native20251210101311#ios)
* [Installation](https://apical.xyz/formations/pageunique/applications_mobiles_avec_react_native20251210101311#installation)
* [Vérification](https://apical.xyz/formations/pageunique/applications_mobiles_avec_react_native20251210101311#verification)
* [Création d'un projet React Native](https://apical.xyz/formations/pageunique/applications_mobiles_avec_react_native20251210101311#projet)

## Programmation pour Android

Les morceaux requis sont :

* Un ordinateur Windows ou macOS
* Android Studio ou IntellilJ (requis pour construire l'application et pour fournir un émulateur)
* Android
* Node LTS
* Java SE Development Kit (JDK)
* L'interface en ligne de commande de React Native (React Native CLI - Command Line Interface)
* Un IDE de votre choix

## Programmation pour iOS

Les morceaux requis sont :

* Un ordinateur macOS
* Xcode (requis pour construire l'application et pour fournir un émulateur)
* Xcode command Line Tools
* CocoaPods
* Node
* Watchman
* Java SE Development Kit (JDK)
* L'interface en ligne de commande de React Native (React Native CLI - Command Line Interface)
* Un IDE de votre choix

## Installation

Voici quelques précisions sur l'installation. Je n'ai traité que l'installation sous Windows. Je vous conseille d'en faire une lecture mais de ne rien faire à l'ordinateur pour l'instant puisque les étapes d'installation officielles pourraient différer.

Après en avoir pris connaissance, suivez les instructions d'installation pour votre système d'exploitation : <https://reactnative.dev/docs/set-up-your-environment>.

Notez qu'il est également possible d'[apical\_lien\_interne][react\_native\_et\_docker,installer React Native dans un conteneur Docker][/apical\_lien\_interne].

* Ouvrez la fenêtre Terminal en mode administrateur.
* Effectuez cette configuration qui vous permettra de lancer des commandes dans une fenêtre Terminal :

  Terminal

  Set-ExecutionPolicy Unrestricted -Scope CurrentUser -Force
* Installez chocolatey (<https://chocolatey.org/install>) :

  Terminal

  Set-ExecutionPolicy Bypass -Scope Process -Force; [System.Net.ServicePointManager]::SecurityProtocol = [System.Net.ServicePointManager]::SecurityProtocol -bor 3072; iex ((New-Object System.Net.WebClient).DownloadString('https://community.chocolatey.org/install.ps1'))
* Installez JDK :

  Terminal

  choco install -y nodejs-lts microsoft-openjdk17
* Installez Git (sinon, quand on crée un projet, on obtient le message « Git is not installed on your system. This might cause some features to work incorrectly ») :

  Terminal

  choco install git.install
* Les instructions officielles demandent d'installer Android Studio en cochant Android SDK, Android SDK Platform et Android Virtual Device. Puisque vous avez déjà installé Android Studio, pas de problème. Les étapes suivantes assureront que tout est conforrme en plus de compléter l'installation requise.
* Démarrez Android Studio.
* Cliquez sur les trois points verticaux sur l'écran d'accueil puis choisissez SDK Manager (ou, si un projet est déjà ouvert, File / Settings / Languages & Frameworks / Android SDK).
* Dans l'onglet SDK Platforms, cochez Show Package Details.
* Plusieurs versions du SDK peuvent coexister. Au moment d'écrire ces lignes, React Native nécessitait Android 15.   
  Cliquez sur la flèche pour développer Android 15 (VanillaIceCream). Vous devez cocher Android SDK Platform 35 puis l'un des suivants : Intel x86\_64 Atom System Image ou, si vous désirez coder des applications qui nécessitent les API Google (Google Play Services), Google APIs Intel x86\_64 Atom System Image.
* Cliquez sur l'onglet SDK Tools.
* Cochez Show Package Details.
* Cliquez sur la flèche pour développer Android SDK Build-Tools puis cochez 35.0.0.
* Cliquez sur la flèche pour développer Android SDK Command-line Tools (latest) puis cochez Android SDK Command-line Tools (latest).
* Plus bas, cochez Android Emulator et Android SDK Platform-Tools.
* Cliquez sur Apply pour lancer l'installation.
* Notez le chemin du SDK Android qui est affiché dans le haut de l'écran. Ex : C:\Users\MonNom\AppData\Local\Android\Sdk. Ce chemin peut être retrouvé dans Android Studio à partir du menu Tools / SDK Manager / Languages & Frameworks / Android SDK, dans la zone Android SDK Location.
* Faites pointer la variable d'environnement ANDROID\_HOME vers le dossier du SDK Android (vous pouvez utiliser %USERPROFILE% au lieu de C:\Users\MonNom).
* Modifiez la variable d'environnement PATH pour lui ajouter le dossier %LOCALAPPDATA%\Android\Sdk\platform-tools.
* Pour configurer l'émulateur qui sera utilisé pour tester vos applications, ouvrez un projet bidon dans Android Studio.
* [apical\_lien\_interne][configurer\_un\_peripherique\_virtuel,Créer un nouveau périphérique virtuel][/apical\_lien\_interne] qui utilise l'image système VanillaIceCream (API 35).
* Pour éditer vos applications, si vous désirez utiliser VSCode, il est conseillé d'installer l'extension React Native Tools.
* Maintenant que vous avez pris connaissance de ces précisions, suivez les instructions d'installation pour votre système d'exploitation : <https://reactnative.dev/docs/set-up-your-environment>.

## Vérification

Avant de poursuivre, effectuez les vérifications suivantes pour vous assurer que tout est correctement en place.

* Vérifiez que les variables d'environnement sont bien mises à jour en entrant ces commandes dans une nouvelle fenêtre Terminal (les changements ne sont pas effectifs dans les fenêtres qui étaient ouvertes avant les modifications) :

  Terminal

  $ENV:ANDROID\_HOME  
  $ENV:PATH
* Vérifiez version de Node :

  Terminal

  node -v

  Vous devez avoir la version 18 ou plus récent.
* Vérifiez la version de Java :

  Terminal

  java --version

  Vous devez avoir la version 17 ou moins.

## Création d'un projet React Native

Vous êtes maintenant prêts à [apical\_lien\_interne][creer\_un\_nouveau\_projet\_react\_native,créer votre premier projet React Native][/apical\_lien\_interne].

## 2.3 React Native et Docker

Si vous préférez effectuer votre installation React Native dans Docker, et ainsi avoir un environnement propre sans risquer de modifier vos configurations actuelles, suivez les instructions de ce tutoriel : <https://bspalamutcu.medium.com/dockerize-your-react-native-app-5b8020849c92>.