# 6. Initiation à React Native

# 6. Initiation à React Native

## 6.1 Structure du projet

Dans le cadre de ce cours, tous les fichiers que vous créerez seront placés dans le dossier src, que vous devez créer à la racine du projet.

Parmi les fichiers que vous coderez, seul le fichier App.tsx demeurera à la racine.

Voici un aperçu de la structure attendue pour le projet. Notez que les fichiers et dossiers d'un projet de base n'ont pas tous été reproduits sur cette image.

![Structure projet React Native](assets/08-6-initiation-react-native-38.png)

## 6.2 Fichiers initiaux du projet

Une fois que vous avez créé votre projet React Native à l'aide de la commande npx @react-native-community/cli init MonProjet, voici les fichiers initiaux générés.

Vous remarquerez que la structure du dossier android ressemble à celle d'un projet Android natif.

De même, celle du dossier ios ressemble à celle d'un projet iOS natif avec UIKit (on le reconnait au fichier .storyboard). Même si nous connaissons plutôt SwiftUI, nous voyons que iOS saura reconnaître les fichiers de ce projet.

Rassurez-vous, le contenu de ces dossiers n'aura pas à être modifié.

Le point de départ du projet est le fichier App.tsx, avec sa fonction App(). C'est là que vous pourrez débuter les modifications au squelette afin de créer votre propre application.

Tous les autres fichiers que vous coderez seront placés dans un dossier src que vous créerez à la racine du projet.

![Fichiers de base React Native](assets/08-6-initiation-react-native-39.png)

## 6.3 Fichier de base de l'application (App.tsx)

Le fichier App.tsx est le fichier de base d'une application React Native.

Au départ, on y retrouve ce code.

Fichier App.tsx (React Native)

/\*\*  
 \* Sample React Native App  
 \* https://github.com/facebook/react-native  
 \*  
 \* @format  
 \*/

 

import { NewAppScreen } from '@react-native/new-app-screen';  
import { StatusBar, StyleSheet, useColorScheme, View } from 'react-native';  
import {  
  SafeAreaProvider,  
  useSafeAreaInsets,  
} from 'react-native-safe-area-context';

 

function App() {  
  const isDarkMode = useColorScheme() === 'dark';

 

  return (  
    <SafeAreaProvider>  
      <StatusBar barStyle={isDarkMode ? 'light-content' : 'dark-content'} />  
      <AppContent />  
    </SafeAreaProvider>  
  );  
}

 

function AppContent() {  
  const safeAreaInsets = useSafeAreaInsets();

 

  return (  
    <View style={styles.container}>  
      <NewAppScreen  
        templateFileName="App.tsx"  
        safeAreaInsets={safeAreaInsets}  
      />  
    </View>  
  );  
}

 

const styles = StyleSheet.create({  
  container: {  
    flex: 1,  
  },  
});

 

export default App;

La moindre modification à ce code sera immédiatement reflétée dans l'émulateur une fois l'application lancée.

Tout ce code peut être effacé.

On y codera plutôt une fonction App() déclarée à l'aide de la [apical\_lien\_interne][comment\_fonctionnent\_les\_fonctions\_flechees,syntaxe des fonctions fléchées][/apical\_lien\_interne].

Dans le return de cette fonction, on ajoutera des composants à l'aide de code [JSX](https://react.dev/learn/writing-markup-with-jsx) afin de préciser ce qui sera affiché.

Fichier App.tsx (React Native)

import React from 'react';

 

const App = () => {  
  return (  
      ...  
  );  
};

 

export default App;

Si vous préférez, il est possible déclarer un composant fonctionnel.

Remarquez qu'ici, j'ai précisé que le type de retour est JSX.Element, c'est-à-dire un composant React Native.

Fichier App.tsx (React Native)

import React from 'react';  
import type {JSX} from 'react';

 

function App(): JSX.Element {  
  return (  
      ...  
  );  
};

 

export default App;

## 6.4  Les composants de React Native

Plusieurs composants sont fournis directement par React Native sans nécessiter d'installation supplémentaire.

Ces composants React Native jouent un rôle qui ressemble à celui des balises HTML en React : ils constituent les blocs de base à partir desquels d'autres composants seront développés pour former l'interface de l'application.

Pour les utiliser, vous devez ajouter l'instruction import { View, Text } from 'react-native'; en prenant soin de lister les composant utilisés.

Dans la littérature, on retrouve deux appellations :

* [Composants de base (basic components)](https://apical.xyz/formations/pageunique/applications_mobiles_avec_react_native20251210101311#basic)
* [Composants du noyau (core components)](https://apical.xyz/formations/pageunique/applications_mobiles_avec_react_native20251210101311#core)

## Composants de base (basic components)

La documentation officielle de React Native mentionne qu'il y a des basic components[1](https://reactnative.dev/docs/components-and-apis#basic-components).

Les composants de base sont les composants de plus bas niveau, par exemple :

* <[apical\_lien\_interne][view,View][/apical\_lien\_interne]>
* <[apical\_lien\_interne][text\_003,Text][/apical\_lien\_interne]>
* <[apical\_lien\_interne][image\_003,Image][/apical\_lien\_interne]>
* <ScrollView>
* <TextInput>
* <[apical\_lien\_interne][pressable,Pressable][/apical\_lien\_interne]>

Voici un exemple d'utilisation de composants de base.

React Native

<View>  
  <Image  
    source={{  
      uri: 'https://apical.xyz/Sourire.png',  
    }}  
    style={mesStyles.imageAccueil}  
  />  
  <Text>Bonjour!</Text>  
 </View>

## Composants du noyau (core components)

Les composants du noyau regroupent l'ensemble des composants fournis par React Native. Ils incluent les composants de base.

Parmi eux, on retrouve également d'autres composants de plus haut niveau, par exemple :

* <[apical\_lien\_interne][button\_003,Button][/apical\_lien\_interne]>
* <[apical\_lien\_interne][flatlist,FlatList][/apical\_lien\_interne]>
* <[apical\_lien\_interne][alert\_002,Alert][/apical\_lien\_interne]>
* <StatusBar>

Pour une liste plus complète : <https://reactnative.dev/docs/components-and-apis>.

## Source

1. « Core Components and APIs - Basic Components ». React Native. <https://reactnative.dev/docs/components-and-apis#basic-components>

## Pour plus d'information

« Fundamental React Native Components You Need To Know About ». Locofy Blog. <https://www.locofy.ai/blog/react-native-components>

« React Native cheatsheet for beginners ». Medium. <https://medium.com/coinmonks/react-native-cheatsheet-for-beginners-ce0559365d68>

## 6.5 Développer nos propres composants

Le composant est l'unité de base d'une vue React Native.

Il existe des [apical\_lien\_interne][composants\_de\_base,composants de base][/apical\_lien\_interne] fournis par React Native. Nous allons développer nos propres composants à partir de ces composants de base.

Il est d'usage de placer chaque composant dans son propre fichier.

Je vous présente deux syntaxes possibles. Les deux syntaxes offrent chacune quelques particularités mais dans l'ensemble, elles sont équivalentes.

Remarquez que dans le return, il n'est pas possible de retourner plus d'un composant. Il est possible de retourner un composant qui en englobe d'autres, ou encore d'entourer plusieurs composants par [apical\_lien\_interne][les\_fragments,des fragments][/apical\_lien\_interne] (<> ... </>).

Nous prendrons l'habitude de placer tous les composants dans le dossier MonProjet/src/components. Vous devez créer le dossier src et son sous-dossier.

Parmi les fichiers que vous aurez modifiés dans votre projet, seul App.tsx demeurera à la racine.

## Syntaxe des fonctions fléchées

Ce premier exemple utilise la [apical\_lien\_interne][comment\_fonctionnent\_les\_fonctions\_flechees,syntaxe des fonctions fléchées][/apical\_lien\_interne].

C'est cette syntaxe qui sera utilisée partout dans les notes de cours.

Fichier MonComposantFleche.tsx (React Native)

import React from 'react';  
import {Text} from 'react-native';  
  
const MonComposantFleche = () => {  
  return (  
    <>  
      <Text>Je suis un composant déclaré avec une fonction fléchée.</Text>  
      ...  
    </>  
  );  
};  
  
export default MonComposantFleche;

## Syntaxe des fonctions

Il est également possible d'utiliser la syntaxe des fonctions. On dira qu'on a un composant fonctionnel.

Fichier MonComposantFonction.tsx (React Native)

import React from 'react';  
import type {JSX} from 'react';  
import {Text} from 'react-native';  
  
function MonComposantFonction(): JSX.Element {  
  return (  
    <>  
      <Text>Je suis un composant déclaré avec une fonction.</Text>  
      ...  
    </>  
  );  
}  
  
export default MonComposantFonction;

## Composants de classe

Il existe une troisième syntaxe qui consite à créer une classe qui hérite de Component. On dira qu'on a un composant de classe.

Cependant, cette syntaxe n'est plus recommandée par l'équipe de React[1](https://react.dev/reference/react/Component) :

> We recommend defining components as functions instead of classes.

React Native

class MonComposantClasse extends Component {  
  constructor(props) {  
    super(props);  
  }

 

  render() {  
    return (  
      <>  
        <Text>Je suis un composant déclaré en tant que classe.</Text>  
        ...  
      </>  
    );  
  }  
}

 

export default MonComposantClasse;

## Utiliser un composant

Dans un cas comme dans l'autre, pour utiliser ce composant, vous pouvez utiliser ceci, par exemple dans le fichier App.tsx.

Fichier App.tsx (React Native)

import React from 'react';  
import {View} from 'react-native';  
import MonComposantFleche from './src/components/MonComposantFleche';  
import MonComposantFonction from './src/components/MonComposantFonction';  
  
const App = () => {  
  return (  
    <View>  
      <MonComposantFleche />  
      <MonComposantFonction />  
    </View>  
  );  
};  
  
export default App;

## Source

1. « Component ». React. <https://react.dev/reference/react/Component>

## Pour plus d'information

« React Fundamentals ». React Native. <https://reactnative.dev/docs/intro-react>

« Comparing different types of functions used in react-native ». Medium. <https://mithusha-s.medium.com/comparing-different-types-of-functions-used-in-react-native-f1f988c838bc>

« React functional components: const vs. function ». dev.to. <https://dev.to/ugglr/react-functional-components-const-vs-function-2kj9>

« React.ReactNode vs JSX.Element vs React.ReactElement ». Total Typescript. <https://www.totaltypescript.com/jsx-element-vs-react-reactnode>

« Const vs. Function For React Functional Components ». Plain English. <https://plainenglish.io/blog/const-vs-function-for-react-functional-components>

« Is There Any Reason to Still Use React Class Components? ». Medium. <https://medium.com/geekculture/is-there-any-reason-to-still-use-react-class-components-9b6a1e6aa9ef>

## 6.6 Simples accolades ou doubles accolades?

Parfois, le code React Native comprend des accolades. Ceci signifie qu'il faut évaluer du code JavaScript.

JavaScript

<Text>{props.nom}</Text>

Dans d'autres cas, on retrouve des doubles-accolades.

* Les accolades extérieures indiquent qu'il faut évaluer du code JavaScript.
* Les accolades intérieures représentent un objet JavaScript. On le reconnaît à ses paires propriété: valeur.

React Native

<Image source={{ uri: 'https://apical.xyz/medias/fr/LogoApical.png' }} ... />

## 6.7 Les fragments (<>...</> ou <React.Fragment>...</React.Fragment>)

Dans certains blocs React Native, il n'est pas possible d'avoir plus d'un composant.

C'est le cas notamment dans le return d'un composant.

Le code qui suit ne compilera pas et il générera l'erreur « Les expressions JSX doivent avoir un élément parent. ».

React Native

return (  
  <Text>{ami.prenom}</Text>  
  <Text>{ami.nomFamille}</Text>  
 );

Pour régler ce problème, on peut entourer les composants dans un autre composant plus englobant.

React Native

return (  
  <View>  
    <Text>{ami.prenom}</Text>  
    <Text>{ami.nomFamille}</Text>  
  </View>  
 );

Il est aussi possible de l'entourer de [fragments](https://react.dev/reference/react/Fragment), c'est-à-dire des balises qui permettent de grouper des composants sans ajouter de composant et sans impact visuel.

Dans leur forme la plus simple, les fragments sont représentés par les symboles <> et </>.

React Native

return (  
  <>  
    <Text>{ami.prenom}</Text>  
    <Text>{ami.nomFamille}</Text>  
  </>  
);

On obtiendra le même résultata avec ceci :

React Native

return (  
  <React.Fragment>  
    <Text>{ami.prenom}</Text>  
    <Text>{ami.nomFamille}</Text>  
  </React.Fragment>  
);

Cette dernière syntaxe permet d'ajouter des propriétés au fragment, par exemple la clé unique (attribut key) pour chaque itération.

React Native

{amis.map((ami) => (  
  <React.Fragment key={ami.id}>  
    <Text>{ami.prenom}</Text>  
    <Text>{ami.nomFamille}</Text>  
  </React.Fragment>  
))}

## 6.8 Ajouter des styles

Les styles dans React Native ressemblent beaucoup aux règles CSS en Web.

Les bonnes pratiques veulent que les styles soient déclarés dans des fichiers distincts et importés dans le fichier du composant.

Contrairement aux classes CSS, qui utilisent généralement la [apical\_lien\_interne][casse\_kebab\_la-casse-kebab,casse kebab][/apical\_lien\_interne] (tout en minuscules et traits d'union entre les mots), le nom d'un style React Native sera écrit en [apical\_lien\_interne][Casse\_chameau\_laCasseChameau,casse chameau][/apical\_lien\_interne] (débute par minuscule et majuscule à chaque changement de mot).

Nous prendrons l'habitude de placer tous les fichiers de styles dans le dossier MonProjet/src/styles.

Remarquez que le fichier de style portera l'extension .ts (TypeScript) et non .tsx (TypeScript qui contient du JSX).

Le nom du fichier utilisera la casse chameau donc il débutera par une minuscule puisque les fichiers dont le nom débute par une majuscule sont généralement des composants.

Fichier mesStyles.ts (React Native)

import {StyleSheet} from 'react-native';  
  
const mesStyles = StyleSheet.create({  
  conteneur: {  
    alignItems: 'center',  
  },  
  imageAccueil: {  
    width: 200,  
    height: 200,  
  },  
});  
  
export {mesStyles};

Fichier MonComposant.tsx

import React from 'react';  
import {View, Image, Text} from 'react-native';  
import { mesStyles } from '../styles/mesStyles';

 

const MonComposant = () => {  
  return (  
    <View style={mesStyles.conteneur}>  
      <Image  
        source = {{  
          uri: 'https://apical.xyz/logoApical.png',  
        }}  
        style={mesStyles.imageAccueil}  
      />  
    </View>  
  );  
};  
  
export default MonComposant;

## Affecter plusieurs classes CSS à un composant

Dans le cas où un composant doit être affecté par plus d'une classe CSS, il faut placer les classes CSS dans un tableau.

React Native

<View style={[mesStyles.conteneur, mesStyles.accueil]}>  
  ...  
</View>

## Pour plus d'information

« Style ». React Native. <https://reactnative.dev/docs/style>

« React Native Styling: Structure for Style Organization  ». Thoughtbot. <https://thoughtbot.com/blog/structure-for-styling-in-react-native>

## 6.9 Propriétés

Les propriétés, appelées props, sont des valeurs de base que l'on peut passer à un composant. Elles sont immuables, c'est-à-dire que leur valeur ne peut pas être modifiée.

Pour recevoir et utiliser une propriété, le composant utilise cette syntaxe :

React Native

type MonComposantProps = {  
  texte: string;  
};  
  
const MonComposant = (props: MonComposantProps) => {  
  return (  
    <>  
      <Text>{props.texte}</Text>  
      ...  
    </>  
  );  
};

Plutôt que d'utiliser le mot-clé type pour définir les propriétés, il est possible d'utiliser interface. Bien qu'il existe [quelques différences fonctionnelles](https://blog.logrocket.com/types-vs-interfaces-typescript/) entre ces deux syntaxes, elles peuvent toutes deux être utilisées dans le présent contexte.

React Native

interface MonComposantProps {  
  texte: string;  
}

## Utilisation

Pour fournir une valeur aux propriétés, la syntaxe ressemble à celle des attributs HTML.

JavaScript

const App = () => {  
  return (  
    <MonComposant texte="Ça marche!" />  
  );  
};

## Propriétés par défaut

Il est possible de définir une valeur par défaut pour les propriétés.

De cette façon, si aucune valeur n'est fournie lors de l'utilisation du composant, c'est la valeur par défaut qui sera utilisée.

React Native

type MonComposantProps = {  
  texte: string;  
};

 

 

 

const defaultProps: MonComposantProps = {  
  texte: 'À venir...',  
};  
  
const MonComposant = (props: MonComposantProps) => {  
  return (  
    <>  
      <Text>{props.texte}</Text>  
      ...  
    </>  
  );  
};  
  
MonComposant.defaultProps = defaultProps;

## Pour plus d'information

« Props ». React Native. <https://reactnative.dev/docs/props>

« Mastering TypeScript: A Guide to Choosing Between ‘type’ and ‘interface’ ». Medium. <https://levelup.gitconnected.com/mastering-typescript-a-guide-to-choosing-between-type-and-interface-c31d3527693b>

## 6.10 Variables d'état (useState)

Les variables d'état React Native, appelées states, permettent de rafraîchir l'écran dès que leur valeur change.

React Native

import React, {useState} from 'react';  
import {Text, Button} from 'react-native';  
  
const MonComposant = () => {  
  const [uneValeur, setUneValeur] = useState('Valeur initiale');  
  
  return (  
    <>  
      <Text>{uneValeur}</Text>  
      <Button  
        onPress={() => {  
          setUneValeur('Nouvelle valeur!');  
        }}  
        title="Changer la valeur"  
      />  
    </>  
  );  
};

## Variable d'état qui peut être nulle

Pour indiquer à TypeScript qu'une variable d'état peut être nulle, on précisera son type comme suit :

React Native

const [nombre, setNombre] = useState<number | null>(0);

## Variable d'état initialisée à partir d'une propriété

Parfois, un composant recevra une valeur dans une propriété dans le but de modifier cette valeur.

On pourra à ce moment modifier une variable d'état à partir de cette propriété :

React Native

const [texte, setTexte] = useState(props.texteOriginal);

## La fonction de modification d'état est asynchrone

Attention : il ne faut jamais utiliser une variable d'état immédiatement après avoir modifié sa valeur car cette opération est asynchrone.

On peut contourner ce problème en travaillant avec une variable locale.

Plutôt que de faire ceci :

React Native

setQuantite(...);  
  
// ici, on n'a aucune garantie que la variable est changée  
if (quantite == 3) {  
  ...  
}

il faut faire :

React Native

const quantiteLocale = ...  
  
setQuantite(quantiteLocale);  
  
if (quantiteLocale == 3) {  
  ...  
}

Une autre technique consiste à utiliser un [apical\_lien\_interne][reagir\_seulement\_lorsqu\_une\_variable\_d\_etat\_a\_change\_de\_valeur,useEffect][/apical\_lien\_interne] qui prendra effet quand cette variable d'état aura effectivement changé de valeur.

React Native

const MonComposant = () => {  
  ...

 

  useEffect(() => {  
    if (quantite == 3) {  
      ...  
    }  
  }, [quantite]);

 

  return (  
    <>  
      ...  
      <Button  
        onPress={() => {  
          setQuantite(...);  
        }}  
        title="Incrémenter"  
      />  
    </>  
  );  
};

## Pour plus d'information

« Sharing State Between Components ». React. <https://react.dev/learn/sharing-state-between-components>