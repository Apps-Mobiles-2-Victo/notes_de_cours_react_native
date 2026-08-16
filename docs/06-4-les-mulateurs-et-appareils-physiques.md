# 4. Les émulateurs et appareils physiques

# 4. Les émulateurs et appareils physiques

## 4.1 Travailler avec les émulateurs

Puisque React Native permet de tester une application à l'aide d'un émulateur installé par Android Studio, il est intéressant de mieux comprendre la mécanique derrière cette fonctionnalité.

Par défaut, quand on lance une application, React Native CLI (Command Line Interface), en collaboration avec ADB (Android Debug Bridge), recherche un émulateur actif.

* S'il n'y en a aucun, il en active un et lance l'application dans cet émulateur.
* S'il y en a un seul, il lance l'application dans cet émulateur.
* S'il y en a plusieurs, tout dépend de votre environnement. Il pourrait, par exemple, être nécessaire de refermer manuellement les émulateurs afin d'en garder un seul actif.

La technique la plus simple pour s'assurer que l'application soit lancée sur l'émulateur désiré consiste à en créer un seul dans Android Studio.

Mais si vous désirez conserver plus d'un émulateur, le fait de connaître les techniques de cette fiche vous permettra de lancer l'application sur l'émulateur de votre choix.

Dans cette fiche :

* [Liste des émulateurs](https://apical.xyz/formations/pageunique/applications_mobiles_avec_react_native20251210101311#liste)
* [Activer un émulateur](https://apical.xyz/formations/pageunique/applications_mobiles_avec_react_native20251210101311#activer)
* [Lister les émulateurs actifs](https://apical.xyz/formations/pageunique/applications_mobiles_avec_react_native20251210101311#actif)
* [Retrouver le nom d'un émulateur à partir de son identifiant](https://apical.xyz/formations/pageunique/applications_mobiles_avec_react_native20251210101311#nom)
* [Lancer l'application dans un émulateur](https://apical.xyz/formations/pageunique/applications_mobiles_avec_react_native20251210101311#lancer)
* [Refermer un émulateur](https://apical.xyz/formations/pageunique/applications_mobiles_avec_react_native20251210101311#refermer)
* [Émulateur hors de l'écran](https://apical.xyz/formations/pageunique/applications_mobiles_avec_react_native20251210101311#ecran)

## Liste des émulateurs

Pour connaître les émulateurs créés avec Android Studio (actifs ou non) :

Terminal

C:\Users\MonNom\AppData\Local\Android\Sdk\emulator\emulator -list-avds

On voit le nom des émulateurs.

Résultat à l'écran

PS C:\Users\MonNom> C:\Users\MonNom\AppData\Local\Android\Sdk\emulator\emulator -list-avds  
Medium\_Phone\_API\_36.0  
Pixel\_9a

Autre technique : chaque émulateur aura un dossier à son nom sous C:\Users\MonNom\.android\avd.

## Activer un émulateur

Pour activer un émulateur en particulier :

Terminal

C:\Users\MonNom\AppData\Local\Android\Sdk\emulator\emulator -avd Pixel\_9a

L'émulateur apparaîtra à l'écran.

## Lister les émulateurs actifs

Cette commande liste les émulateurs actifs en fournissant leur identifiant :

Terminal

adb devices

Dans cet écran, on voit qu'il y a un téléphone physique branché à l'ordinateur (35051jegro08333) de même que deux émulateurs actifs (emulator-5554 et emulator-5556).

Résultat à l'écran

PS C:\Users\MonNom> adb devices  
List of devices attached  
35051jegro08333  device  
emulator-5554    device  
emulator-5556    device

## Retrouver le nom d'un émulateur à partir de son identifiant

Pour vérifier le nom de l'émulateur qui correspond à un identifiant :

Terminal

adb -s emulator-5554 emu avd name

Résultat à l'écran

PS C:\Users\MonNom> adb -s emulator-5554 emu avd name  
Pixel\_9a

 

OK

## Lancer l'application dans un émulateur

Lorsque vous avez activé l'émulateur désiré, vous pouvez lancer l'application dans cet émulateur à l'aide de la commande habituelle à condition qu'il n'y ait qu'un seul émulateur actif.

Terminal

cd /chemin/MonProjet  
npx react-native run-android

Dans le cas où il y a plusieurs émulateurs actifs (et que votre environnement est capable de travailler dans ces conditions – ce qui n'est pas toujours le cas), il est possible de forcer l'utilisation d'un émulateur en particulier :

Terminal

cd /chemin/MonProjet  
npx react-native run-android --device=emulator-5554

## Refermer un émulateur

Il est possible de refermer un émulateur de différentes façons :

* en cliquant sur le X dans le coin supérieur droit de l'émulateur;
* en appuyant sur Ctrl + C dans la fenêtre Terminal qui a lancé l'émulateur;
* à l'aide de la commande suivante, en changeant emulator-5554 pour l'identifiant de l'émulateur :

  Terminal

  adb -s emulator-5554 emu kill

## Émulateur hors de l'écran

Si l'émulateur apparaît partiellement hors de l'écran ou, pire encore, complètement en dehors de l'écran, il est possible de modifier ce comportement à l'aide d'un fichier de configuration.

* Dans l'explorateur de fichiers, rendez-vous dans le dossier C:\Users\MonNom\.android\avd.
* Cliquez sur le dossier qui porte le nom de l'émulateur en problème.
* À l'aide d'un éditeur de texte brut, par exemple Geany, éditez le fichier emulator-user.ini.
* Modifiez les coordonnées pour afficher l'émulateur à l'intérieur de la fenêtre. Dans cet exemple, je le place dans le coin supérieur gauche de l'écran.

  Fichier emulator-user.ini

  window.x = 0  
  window.y = 0  
  ...

## 4.2 Exécuter une application React Native sur un téléphone Android

Il est possible de tester une application React Native sur un téléphone Android plutôt que d'utiliser un émulateur.

* Sur le téléphone, assurez-vous que les options de développeur sont activées : Paramètres (Settings) / À propos du téléphone (About phone) / Cliquer 7 fois sur Numéro de version (Build number).

  ou

  Paramètres (Settings) / Dans la zone de recherche, entrer Options pour les développeurs (Developer options) / Taper sur l'option pour l'activer.
* Activez le débogage USB : Paramètres (Settings) / Dans la zone de recherche, entrer Options pour les développeurs (Developer options) / Activer Débogage USB (USB debugging).
* Brancher le téléphone à l'ordinateur via USB.
* Vérifiez que le téléphone est reconnu en lançant cette commande dans une fenêtre Terminal à partir du dossier de l'application :

  Terminal

  adb devices
* Un seul périphérique doit être actif dans cette commande. Si un émulateur est listé sans la mention offline, il faut absolument le refermer avant de poursuivre.
* Lancez votre application normalement, elle apparaîtra sur le téléphone.

## Pour plus d'information

« Running On Device ». React Native. <https://reactnative.dev/docs/running-on-device?os=windows&platform=android>