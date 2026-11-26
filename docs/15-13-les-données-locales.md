# 12. Les données locales

## 12.1 Modèle pour définir la structure des données

Un type TypeScript permet de décrire les données manipulées par l'application. Il ne crée pas la table SQLite : celle-ci sera créée séparément avec une requête SQL.

Les modèles seront placés dans le dossier `src/models` à la racine du projet.

```ts title="src/models/Categorie.ts"
export type Categorie = {
  id: number | null;
  description: string;
};
```

Ici, `id` peut être `null` pour représenter une catégorie qui n'a pas encore été enregistrée. SQLite attribuera un identifiant lors de l'insertion.

Une liste de catégories peut notamment être partagée entre composants à l'aide du [hook useContext](12-10-les-hooks.md#use-context).

## 12.2 Interagir avec SQLite dans une application React Native { #sqlite }

SQLite est une base de données locale : les données sont conservées sur l'appareil entre les lancements de l'application. Pour y accéder, nous utiliserons `react-native-nitro-sqlite`.

## Créer un service de base de données

Regroupons les requêtes dans `src/services/dbService.ts`. La fonction `open` ouvre une base existante ou crée le fichier s'il n'existe pas. Nous conservons une seule connexion pour la réutiliser : ouvrir plusieurs fois la connexion par défaut avec le même nom provoque une erreur.

```ts title="src/services/dbService.ts"
import {open} from 'react-native-nitro-sqlite';
import type {Categorie} from '../models/Categorie';

let connexion: ReturnType<typeof open> | undefined;

export const getDBConnection = () => {
  if (!connexion) {
    connexion = open({name: 'mabd.db'});
  }
  return connexion;
};

export const initialiserBD = async () => {
  const db = getDBConnection();

  await db.transaction(async tx => {
    await tx.executeAsync(`
      CREATE TABLE IF NOT EXISTS categories (
        id INTEGER PRIMARY KEY AUTOINCREMENT,
        description TEXT NOT NULL
      )
    `);

    const resultat = await tx.executeAsync<Categorie>(
      'SELECT id, description FROM categories LIMIT 1',
    );

    if (resultat.rows.length === 0) {
      for (const description of ['Catégorie 1', 'Catégorie 2', 'Catégorie 3']) {
        await tx.executeAsync(
          'INSERT INTO categories (description) VALUES (?)',
          [description],
        );
      }
    }
  });
};

export const getCategories = async (): Promise<Categorie[]> => {
  const resultat = await getDBConnection().executeAsync<Categorie>(
    'SELECT id, description FROM categories ORDER BY id',
  );
  return resultat.rows._array;
};

export const ajouterCategorie = async (description: string): Promise<Categorie> => {
  const resultat = await getDBConnection().executeAsync(
    'INSERT INTO categories (description) VALUES (?)',
    [description],
  );

  if (resultat.insertId == null) {
    throw new Error("L'identifiant de la catégorie insérée est indisponible.");
  }

  return {id: resultat.insertId, description};
};
```

`executeAsync` retourne une promesse : utilisez `await` pour attendre le résultat. Les lignes d'une requête `SELECT` se trouvent dans `resultat.rows._array`. Le type `<Categorie>` décrit la forme attendue, mais ne valide pas les données retournées.

La transaction regroupe la création de la table et l'ajout des catégories initiales. Elle évite aussi de les insérer deux fois si l'initialisation est lancée plusieurs fois en même temps. Si une opération échoue, la transaction est annulée. Les catégories initiales sont ajoutées lorsque la table est vide, y compris si toutes les catégories ont été supprimées.

Les `?` sont des paramètres : leurs valeurs sont fournies séparément dans un tableau. Utilisez cette technique pour les valeurs provenant de l'utilisateur, plutôt que de les insérer directement dans la chaîne SQL. Après un `INSERT`, `insertId` contient l'identifiant généré lorsqu'il est disponible.

## Initialiser la base et afficher les catégories

Dans cet exemple complet, `App` initialise la base, charge les catégories et permet d'en ajouter une. Après l'insertion, la catégorie retournée contient son identifiant et peut être ajoutée à l'état.

```tsx title="App.tsx"
import React, {useEffect, useState} from 'react';
import {Button, Text, TextInput, View} from 'react-native';
import type {Categorie} from './src/models/Categorie';
import {
  ajouterCategorie,
  getCategories,
  initialiserBD,
} from './src/services/dbService';

const App = () => {
  const [categories, setCategories] = useState<Categorie[]>([]);
  const [description, setDescription] = useState('');
  const [prete, setPrete] = useState(false);
  const [enregistrement, setEnregistrement] = useState(false);
  const [erreur, setErreur] = useState<string | null>(null);

  useEffect(() => {
    let actif = true;

    const charger = async () => {
      try {
        await initialiserBD();
        const categoriesBD = await getCategories();
        if (actif) {
          setCategories(categoriesBD);
          setPrete(true);
        }
      } catch (error) {
        console.error('Erreur lors du chargement des catégories', error);
        if (actif) {
          setErreur('Impossible de charger les catégories.');
        }
      }
    };

    void charger();
    return () => {
      actif = false;
    };
  }, []);

  const ajouter = async () => {
    const texte = description.trim();
    if (!texte) {
      setErreur('Entrez une description.');
      return;
    }

    setEnregistrement(true);
    setErreur(null);
    try {
      const categorie = await ajouterCategorie(texte);
      setCategories(liste => [...liste, categorie]);
      setDescription('');
    } catch (error) {
      console.error("Erreur lors de l'ajout de la catégorie", error);
      setErreur("Impossible d'ajouter la catégorie.");
    } finally {
      setEnregistrement(false);
    }
  };

  return (
    <View>
      <Text>Catégories</Text>
      {categories.map(categorie => (
        <Text key={categorie.id}>{categorie.description}</Text>
      ))}
      <TextInput
        value={description}
        onChangeText={setDescription}
        placeholder="Nouvelle catégorie"
        editable={prete && !enregistrement}
      />
      <Button
        title="Ajouter"
        onPress={ajouter}
        disabled={!prete || enregistrement}
      />
      {erreur && <Text>{erreur}</Text>}
    </View>
  );
};

export default App;
```

La variable `actif` évite de modifier l'état si le composant est démonté pendant le chargement. Les erreurs sont affichées et consignées dans la console.

## Pour plus d'information

« Getting Started ». Nitro SQLite. <https://sqlite.margelo.com/docs>

« Parameters and results ». Nitro SQLite. <https://sqlite.margelo.com/docs/guides/parameters-and-results>

## 12.3 Quand plusieurs composants doivent travailler avec la même base de données

Dans l'exemple ci-dessus, les fonctions du service réutilisent la même connexion. Les composants peuvent appeler ces fonctions sans ouvrir chacun une nouvelle connexion.

La connexion reste ouverte pendant l'utilisation de l'application. Si vous choisissez de la fermer avec `close()`, attendez d'abord la fin des opérations en cours et ne réutilisez pas la connexion fermée. Consultez la [documentation sur le cycle de vie des connexions](https://sqlite.margelo.com/docs/guides/database-lifecycle) pour les cas plus avancés.

Souvent, la liste de catégories doit elle aussi être utilisée par plusieurs composants. Quelques possibilités :

* Stocker la liste et ses fonctions de modification [dans un contexte](12-10-les-hooks.md#use-context).
* Conserver la liste dans un composant parent et transmettre les données et les fonctions nécessaires à ses enfants.

Évitez de passer une connexion ou une fonction de rappel dans les paramètres de navigation : ces paramètres devraient être sérialisables.

## Pour plus d'information

« How to use React Context with TypeScript ». LogRocket. <https://blog.logrocket.com/how-to-use-react-context-typescript/>

« Passing parameters to routes ». React Navigation. <https://reactnavigation.org/docs/params/>
