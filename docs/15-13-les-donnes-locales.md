# 13. Les données locales

# 13. Les données locales

## 13.1 Modèle pour définir la structure des données

Quand une application React Native a besoin d'une base de données SQLite, elle doit définir un modèle qui indique la structure de chacune des tables.

Les modèles seront placés dans le dossier src/models à la racine du projet.

Fichier src/models/Categorie.ts

export interface Categorie {  
  id: number | null;  
  description: string;  
}

L'interface d'un modèle sera utilisée avec [apical\_lien\_interne][rendre\_une\_variable\_d\_etat\_disponible\_a\_tous\_les\_enfants,le hook useContext][/apical\_lien\_interne] afin qu'une variable d'état qui l'implémente puisse être déclarée dans le plus proche parent et être disponible dans tous les enfants.

## 13.2 Interagir avec SQLite dans une application React Native

## Installation

Pour installer le paquet SQLite pour React Native :

Terminal

cd /chemin/MonProjet

 

npm install --save react-native-sqlite-storage  
npm install --save @types/react-native-sqlite-storage

Il y a des paquets supplémentaires à installer si vous produisez des applications pour iOS mais pas si vous visez Android.

Concentrons-nous sur Android pour l'instant.

### Problème avec jcenter()

Avant d'aller plus loin, essayez de lancer l'application dans l'émulateur.

Si vous obtenez une erreur du genre « A problem occurred evaluating project ':react-native-sqlite-storage'.> Could not find method jcenter() for arguments [] on repository container », vous devez éditer le fichier MonProjet/node\_modules/react-native-sqlite-storage/platforms/android/build.gradle. Vous devez retirer l'instruction jcenter() et la remplacer par  mavenCentral().

Le problème est que JCenter, un gestionnaire de dépendances, [a été fermé en 2024](https://medium.com/@nikhilkhant/jcenters-shutdown-key-alternatives-every-developer-should-know-7499dffa2cdd). Il faut donc le remplacer par un autre gestionnaire de dépendances comme MavenCentral.

Attention : ces modifications seront écrasées si vous [apical\_lien\_interne][recreer\_le\_dossier\_node\_modules,réinstallez les paquets de l'application][/apical\_lien\_interne].

Fichier MonProjet/node\_modules/react-native-sqlite-storage/platforms/android/build.gradle

buildscript {  
    repositories {  
        google()  
        mavenCentral()  
    }

 

    dependencies {  
        ...  
    }  
}  
...

Note : si le problème n'est pas corrigé par les développeurs de react-native-sqlite-storage, il faudra penser à remplacer ce paquet dans vos prochains projets par un paquet dont la maintenance est plus à jour, par exemple [react-native-nitro-sqlite](https://github.com/margelo/react-native-nitro-sqlite).

Au moment d'écrire ces lignes (novembre 2025), [la dernière mise à jour de react-native-sqlite-storage](https://github.com/andpor/react-native-sqlite-storage) datait d'octobre 2021.

## Utilisation

Il est d'usage de coder toutes les requêtes à la base de données dans un même fichier.

Comme pour toutes les requêtes dans une base de données, il faut utiliser des requêtes préparées en SQLite lorsque la requête comprend des variables qui proviennent de l'usager.

Fichier src/services/dbService.ts (React Native)

// inspiré de https://blog.logrocket.com/using-sqlite-with-react-native/  
import {  
  enablePromise,  
  openDatabase,  
  SQLiteDatabase,  
} from 'react-native-sqlite-storage';  
import {Categorie} from './models/Categorie';

 enablePromise(true);  
  
 // cette fonction sera utilisée à chaque fois qu'un composant a besoin de se brancher à la base de données  
export const getDBConnection = async () => {  
  return openDatabase({name: 'mabd.db', location: 'default'});  
};  
  
 export const createTable = async (db: SQLiteDatabase) => {  
  const query = `CREATE TABLE IF NOT EXISTS categories(  
    id INTEGER PRIMARY KEY AUTOINCREMENT,  
    description TEXT NOT NULL  
  );`;  
  
   await db.executeSql(query);  
};  
  
 export const getCategories = async (db: SQLiteDatabase): Promise<Categorie[]> => {  
  try {  
    const categories: Categorie[] = [];  
    // resultat est un tableau de ResultSet  
    const resultat = await db.executeSql(  
      'SELECT id, description FROM categories ...',  
    );  
  
    resultat.forEach(result => {  
      for (let index = 0; index < result.rows.length; index++) {  
        categories.push(result.rows.item(index));  
      }  
    });  
  
    return categories;  
  
  } catch (error) {  
    console.error(error);  
    throw Error('Impossible de retrouver les catégories.');  
  }  
};  
  
 export const ajouterCategorie = async (db: SQLiteDatabase, categorie: Categorie) => { ... };

Pour effectuer une requête préparée :

Fichier src/services/dbService.ts (React Native)

const resultat = await db.executeSql('SELECT id, description FROM categories WHERE id = ?', [id]);

Pour retrouver l'identifiant d'un enregistrement nouvellement ajouté :

Fichier src/services/dbService.ts (React Native)

const resultat = await db.executeSql(requeteInsertion, [donneeChamp1, donneeChamp2, donneeChamp3]);  
 const id = resultat[0].insertId;

Pour initialiser la base de données :

Fichier App.tsx

const App = () => {  
  const [categories, setCategories] = useState<Categorie[]>([]);

   // inspiré de https://blog.logrocket.com/using-sqlite-with-react-native/  
  // \*\*\* initialisation de la BD - crée la table si elle n'existe pas et ajoute des données initiales si elle est vide.  
  // useCallback permet de mettre une fonction en cache entre deux rendus  
  const loadDataCallback = useCallback(async () => {  
    try {  
      const categoriesInitiales = [  
        {id: null, description: 'Catégorie 1'},  
        {id: null, description: 'Catégorie 2'},  
        {id: null, description: 'Catégorie 3'},  
      ];   
      const db = await getDBConnection();  
      await createTable(db);  
      const categoriesBD = await getCategories(db);  
  
      if (categoriesBD.length) {  
        setCategories(categoriesBD);  
      } else {  
        await saveCategories(db, categoriesInitiales);  
        setCategories(categoriesInitiales);  
      }  
    } catch (error) {  
      console.error(error);  
      ...  
    }  
  }, []);  
  
   useEffect(() => {  
    loadDataCallback();  
  }, [loadDataCallback]);  
  
  ...  
  return (  
    // si la liste des données a été initialisée dans ce composant,   
    // on peut la rendre disponible aux composants enfants à l'aide du [apical\_lien\_interne][rendre\_une\_variable\_d\_etat\_disponible\_a\_tous\_les\_enfants,hook useContext][/apical\_lien\_interne]  
    ...  
  );  
};

Pour effectuer une requête à la base de données :

MonComposant.tsx (React Native)

const MonComposant = ({navigation}: Props) => {  
  ...  
  
  // inspiré de https://blog.logrocket.com/using-sqlite-with-react-native/  
  // le nom de cette fonction doit être différent de celui de la fonction définie dans dbService.ts

   const ajouter = async () => {  
    try {  
      // enregistre la catégorie dans la BD  
      const db = await getDBConnection();  
      await ajouterCategorie(db, {id: null, description: description});  
  
       // ajuste la variable d'état de la liste de catégories  
      ...  
  
    } catch (error) {  
      console.error(error);  
      ...  
    }  
  };  
  
  return (  
    ...  
  );  
};

## Pour plus d'information

« Using SQLite with React Native  ». Log Rocket. <https://blog.logrocket.com/using-sqlite-with-react-native/>

## 13.3 Quand plusieurs composants doivent travailler avec la même base de données

Quelques idées :

* Pour chaque composant, se brancher à la BD avant de faire son travail. C'est d'ailleurs l'approche utilisée par <https://blog.logrocket.com/using-sqlite-with-react-native/> (il y a une ligne const db = await getDBConnection(); dans chaque fonction qui a besoin de la BD).
* Stocker la référence à la BD [apical\_lien\_interne][rendre\_une\_variable\_d\_etat\_disponible\_a\_tous\_les\_enfants,dans un contexte][/apical\_lien\_interne].
* Passer la référence à la BD dans chaque route.

Souvent, la liste d'items devra elle aussi être utilisée par plusieurs composants.

Cette fois, l'option de la route ne fonctionnera pas puisque depuis react-navigation 5, passer une fonction de rappel en paramètre à une route pourrait causer des problèmes (voir <https://stackoverflow.com/questions/60114496/passing-function-as-a-param-in-react-navigation-5>).

Quelques idées :

* Stocker la liste [apical\_lien\_interne][rendre\_une\_variable\_d\_etat\_disponible\_a\_tous\_les\_enfants,dans un contexte][/apical\_lien\_interne].
* Dans la route qui mène au composant dans lequel la liste a été déclarée comme variable d'état (c'est la source unique de vérité, single source of truth), recevoir les nouvelles valeurs en paramètre à partir du composant qui a modifié un élément. La liste sera donc toujours mise à jour directement dans la source unique de vérité.

## Pour plus d'information

« How To Use React.Context To Pass Database Reference To Child Components? ». Naroju. <https://www.naroju.com/how-to-use-react-context-to-pass-database-reference-to-child-components/>

« How to use React Context with TypeScript ». LogRocket. <https://blog.logrocket.com/how-to-use-react-context-typescript/>

« Passing function as a param in react-navigation 5 ». Stack Overflow. <https://stackoverflow.com/questions/60114496/passing-function-as-a-param-in-react-navigation-5>