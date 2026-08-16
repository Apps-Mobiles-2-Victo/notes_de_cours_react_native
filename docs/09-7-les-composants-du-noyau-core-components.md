# 7. Les composants du noyau (core components)

# 7. Les composants du noyau (core components)

## 7.1 <View>

Le composant [<View>](https://reactnative.dev/docs/view) est un conteneur qui permet notamment de spécifier une mise en forme.

Ce composant a un fonctionnement semblable à celui de la balise <div> en HTML.

React Native

import React from 'react';

 

const App = () => {  
  return (  
    <View style={mesStyles.conteneur}>  
      ...  
    </View>  
  );  
};

 

export default App;

## 7.2 <Text>

Le composant [<Text>](https://reactnative.dev/docs/text) permet d'afficher un simple texte à l'écran.

React Native

<Text>Bonjour!</Text>

Pour forcer un saut de ligne :

React Native

<Text>Bonjour{'\n'}tout le monde!</Text>

Pour afficher une variable :

React Native

<Text>{maVariable}</Text>

Il est possible de lui ajouter [apical\_lien\_interne][ajouter\_des\_styles,des styles][/apical\_lien\_interne].

React Native

<Text style={styles.titre}>Mon titre</Text>

## 7.3 <Image>

Le composant [<Image>](http://reactnative.dev/docs/image) permet d'afficher une image locale ou encore une image à partir d'un URL.

Attention : contrairement à SwiftUI et à Jetpack Compose, il n'y a pas d'icônes en React Native. Pour faire l'équivalent, on travaillera avec de petites images.

React Native

<Image source={require('../images/home.png')} />

Dans le cas d'une image chargée à partir d'un URL, il faut absolument spécifier sa taille [apical\_lien\_interne][ajouter\_des\_styles,à l'aide d'un style][/apical\_lien\_interne] puisque React Native n'est pas en mesure de connaître sa taille avant que le téléchargement soit complété.

React Native

<Image source={{ uri: 'https://apical.xyz/medias/fr/LogoApical.png' }} style={styles.logo} />

## Pour plus d'information

« Image Style Props ». React Native. <https://reactnative.dev/docs/image-style-props>

## 7.4 <Button>

Dans React Native, le composant [<Button>](https://reactnative.dev/docs/button), comme son nom l'indique, permet de créer un bouton cliquable.

Il a l'avantage d'ajuster l'apparence selon la plateforme cible : Android ou iOS. De plus, il est très simple à utiliser.

Par contre, il n'est pas possible de modifier son apparence sauf pour modifier sa couleur.

React Native

<Button  
  onPress={...}  
  title="Je suis un Button"  
  color="#2acfcaff"  
 />

Voici l'apparence du bouton sur chacune des plateformes.

|  |  |  |
| --- | --- | --- |
| Button sur Android |  | Button sur iOS |
| Androïd |  | iOS |

Si vous désirez créer un bouton avec une apparence personnalisée, vous devez vous tourner vers d'autres composants, par exemple <[apical\_lien\_interne][pressable,Pressable][/apical\_lien\_interne]>.

React Native

<Pressable  
  style={mesStyles.bouton}  
  onPress={...}>  
  <Text style={mesStyles.texteBouton} adjustsFontSizeToFit>Cliquez ici</Text>  
 </Pressable>

Il existe également le composant <TouchableOpacity> mais React Native suggère que ce composant pourrait disparaître[1](https://reactnative.dev/docs/touchableopacity).

> If you're looking for a more extensive and future-proof way to handle touch-based input, check out the Pressable API.

## Source

1. «  TouchableOpacity ». React Native. <https://reactnative.dev/docs/touchableopacity>

## Pour plus d'information

« Buttons on React Native: Understanding the difference and which one to use ». Medium. <https://gabrielvrl.medium.com/buttons-on-react-native-understanding-the-difference-and-which-one-to-use-756760140119>

## 7.5 <Pressable>

Le composant [<Pressable>](https://reactnative.dev/docs/pressable) permet de réagir à un clic sur n'importe quoi à l'écran.

Ici, on réagit à un clic sur du texte :

React Native

<Pressable  
  style={mesStyles.bouton}  
  onPress={...}>  
  <Text style={mesStyles.texteBouton} adjustsFontSizeToFit>Cliquez ici</Text>  
 </Pressable>

Ici, on réagit à un clic sur une image :

React Native

<Pressable onPress={...}>  
  <Image source={require('../images/home.png')} style={mesStyles.icone} />  
 </Pressable>

## Effet sur le clic

Fait intéressant : il est possible de modifier le style au moment du clic pour améliorer l'expérience utilisateur.

React Native

<Pressable  
  onPress={...}  
  style={({ pressed }) => [  
    mesStyles.boutonBase,  
    pressed && mesStyles.boutonPresse  
  ]}  
 >

Les classes CSS pourraient ressembler à ceci. Le bouton sera plus foncé au moment de cliquer dessus.

Fichier mesStyles.ts (React Native)

boutonBase: {  
  paddingVertical: 10,  
  paddingHorizontal: 20,  
  borderRadius: 8,  
  backgroundColor: 'rgb(23, 72, 128)',  
 },  
 boutonPresse: {  
  backgroundColor: 'rgb(11, 45, 80)',  
 },

Il peut également être intéressant de jouer avec l'opacité.

Fichier mesStyles.ts (React Native)

boutonPresse: {  
  opacity: 0.2,  
 },

## 7.6 <FlatList>

Le composant [<FlatList>](https://reactnative.dev/docs/flatlist.html) est intéressant pour afficher une liste d'items en colonne ou en quadrillé.

Pour lister des données dont le type comprend un identifiant :

React Native

<FlatList  
  contentContainerStyle={styles.listedonnees}  
  numColumns={1}  
  data={donnees}  
  renderItem={({item}) => <Text>{item.nom}</Text>}  
/>

Si les données ne possèdent pas d'identifiant :

React Native

<FlatList  
  contentContainerStyle={styles.listedonnees}  
  numColumns={1}  
  data={donnees}  
  keyExtractor={(item, i) => String(i)}  
  renderItem={({item}) => <Text>{item.nom}</Text>}  
/>

Si on a besoin de connaître l'index de chaque item :

React Native

<FlatList  
  contentContainerStyle={styles.listedonnees}  
  numColumns={1}  
  data={donnees}  
  renderItem={({item, index}) => <Text>{index}: {item.nom}</Text>}  
/>

## 7.7 <Alert>

Avec React Native, [Alert](https://reactnative.dev/docs/alert) permet d'afficher un popup qui permet par exemple d'afficher un message ou de demander une confirmation.

React Native

const confirmerSuppression = (description: string) => {  
  Alert.alert('Confirmation', `Désirez-vous vraiment supprimer l'item ${description}?`, [  
    {  
      text: 'Non',  
      style: 'cancel',  
    },  
    {  
      text: 'Oui',  
      onPress: () => {  
       // procéder à la suppression  
      }  
    },  
  ]);  
 };  
return (  
  ...  
  <Button  
    title="Supprimer"  
    onPress={() => {  
      confirmerSuppression(...)  
    }}  
  />  
  ...  
);