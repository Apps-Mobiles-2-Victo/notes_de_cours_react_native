# 14. Accéder à la base de données de l'émulateur

# 14. Accéder à la base de données de l'émulateur

## 14.1 Base de données de l'émulateur Android

Lorsque vous développez une application Android avec React Native, c'est Android Studio qui est responsable de l'émulateur.

C'est le même logiciel qui permet de voir la base de données présente sur l'émulateur et ce, même si l'application a été lancée depuis une fenêtre Terminal plutôt que directement dans l'application.

Pour accéder à la base de données, suivez [apical\_lien\_interne][database\_inspector\_pour\_voir\_la\_base\_de\_donnees\_dans\_l\_emulateur,les instructions pour accéder à Database Inspector][/apical\_lien\_interne].

Si vous désirez effectuer une copie de la base de données ou encore la supprimer de l'émulateur, vous devez connaître son namespace, au format d'un nom de domaine en format inverse.

C'est React Native qui s'est chargé de donner un nom par défaut. Cette information est disponible dans le fichier MonProjet/android/app/build.gradle.

Fichier build.gradle

...

 

android {  
  ndkVersion rootProject.ext.ndkVersion  
  buildToolsVersion rootProject.ext.buildToolsVersion  
  compileSdk rootProject.ext.compileSdkVersion

 

  namespace "com.monprojet"  
  defaultConfig {  
    applicationId "com.monprojet"  
    minSdkVersion rootProject.ext.minSdkVersion  
    targetSdkVersion rootProject.ext.targetSdkVersion  
    versionCode 1  
    versionName "1.0"  
 }  
 ...  
}

Une fois cette information connue, vous pouvez appliquer la technique pour retrouver les fichiers de la base de données dans Android Studio : « [apical\_lien\_interne]voir\_les\_fichiers\_stockes\_sur\_un\_emulateur\_android[/apical\_lien\_interne] ».