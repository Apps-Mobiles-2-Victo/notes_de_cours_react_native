# 3. Création et lancement d'un projet React Native

## 3.1 Créer et lancer un projet React Native avec l'image Docker

Pour créer un nouveau projet React Native dans `examples/` avec cette image,
fournir son nom en argument ou le saisir lorsque le script le demande :

```sh
./scripts/create-react-native-project-wsl.sh MonProjet
# ou
./scripts/create-react-native-project-wsl.sh
```

Le cache npm est conservé dans le volume Docker `npm-cache`.

## Projet de test Hello World Android

Un petit projet React Native utilisant Gradle se trouve dans
[examples/android-hello-world](../examples/android-hello-world). Il affiche
« Bonjour Android ! » et inclut les bibliothèques natives pour les architectures
`x86_64`.

Depuis la racine du dépôt, construire l’APK autonome (la variante `release`
embarque le bundle JavaScript, sans serveur Metro) dans l’image Docker :

```sh
./scripts/build-android-app-wsl.sh android-hello-world
# ou, sans argument, le script demande le nom du projet
```

Le script conserve les caches Gradle et Android dans les volumes Docker
`gradle-home-cache` et `android-sdk-cache`. Les versions du NDK et des Build-Tools téléchargées pendant le build sont ainsi réutilisées aux exécutions suivantes.

Le premiers builds peuvent être longs, mais les suivants seront beaucoup plus rapides.

Le fichier produit est
`examples/<Nom Projet>/android/app/build/outputs/apk/release/app-release.apk`.

Assurez-vous que l’AVD d’Android Studio est démarré et joinable depuis WSL avant de lancer l’APK. Pour tester, exécuter dans WSL :

```sh
./scripts/list-android-devices-wsl.sh
```

Ensuite lancer l’APK sur l’AVD d’Android Studio depuis WSL :

```sh
./scripts/run-android-app-wsl.sh android-hello-world
# ou, sans argument, le script demande le nom du projet
```

Le script utilise le serveur ADB Windows à l’adresse de passerelle WSL et au
port `5037`. Pour plusieurs AVD, choisir celui à utiliser avec
`ANDROID_SERIAL=emulator-5554 ./scripts/run-android-app-wsl.sh android-hello-world`.

L’APK de test est signé avec la clé de débogage du modèle React Native ; ne pas
l’utiliser pour une publication.

L’ancienne procédure classique de création et de lancement avec la CLI est
conservée dans [la fiche de référence](05-3-création-et-lancement-dun-projet-react-native-reference.md).

## 3.2 Environnement Web pour tester une application React Native

[Expo Snack](https://snack.expo.dev/) est un environnement de développement Web qui permet de développer et de tester des applications React Native directement dans le navigateur.

Un exemple d'utilisation est donné dans ce tutoriel : <https://www.freecodecamp.org/news/react-native-networking-api-requests-using-fetchapi/>

## 3.3 Expo

[Expo](https://expo.dev/) est un cadre d'application qui facilite la création d'applications React Native. Il permet de tester les applications dans son interface Expo GO.

Cet environnement convient pour la grande majorité des applications React Native.

Notez qu'il ne s'agit pas d'un simple environnement de développement. Les applications créées avec Expo auront une structure différentes de celles créées directement avec React Native CLI.

Il faut savoir que certains modules pourraient ne pas être disponibles dans Expo. Aussi, les applications seront plus lourdes que celles développées avec React Native CLI.

## Pour plus d'information

« Expo vs React Native CLI: Key Differences Explained ». Swovo. <https://swovo.com/blog/expo-vs-react-native/>

« Should you use Expo or Bare React Native? ». Medium. <https://medium.com/@andrew.chester/should-you-use-expo-or-bare-react-native-8dd400f4a468>

« Why Choose React Native Expo Over React Native CLI in 2025 ». Dev.to. <https://dev.to/singhvishal802/why-choose-react-native-expo-over-react-native-cli-in-2025-1087>