# 19. Utiliser un API

# 19. Utiliser un API

## 19.1 Utiliser fetch avec React Native

L'utilisation de [fetch avec React Native](https://reactnative.dev/docs/network) est très semblable à l'utilisation que l'on en fait en React ou même en JavaScript pur.

Deux syntaxes sont à votre disposition : la syntaxe avec .then() ou celle avec async/await.

## Syntaxe .then() (promesses)

Cette syntaxe est la première qui a été disponible lorsque le mécanisme des promesses a été ajouté à JavaScript dans la version ES2015 (ES6).

Un des avantages de cette syntaxe est qu'elle peut être appelée dans un fonction qui n'est pas asynchrone.

Le fonctionnement de base d'une promesse va comme suit :

React Native

promesse  
.then(...)  
.catch(...)

Voici un exemple d'appel d'API avec fetch et la syntaxe des promesses.

React Native

const appelerApi = () => {  
  fetch(url)  
  .then(response => {  
    if (!response.ok) {  
      // le catch pourra réagir à l'erreur  
      throw new Error(`Erreur API : ${response.status} ${response.statusText}`);  
    }  
    return response.json();   // selon ce que l'API retourne, on pourrait devoir utiliser response.text() ou response.url.  
  }).then(result => {  
    //console.log(result)  
    // faire quelque chose avec les informations générées  
  }).catch(error => {  
    //console.error(error)  
    // réagir en cas d'erreur  
  }).finally(() => {  
    // instructions à faire quand l'appel est terminé ou quand l'appel a échoué  
  });   
 };

## Syntaxe avec async/await

La syntaxe avec async/await est apparue avec JavaScript ES2017 (ES8).

Il s'agit d'un simple sucre syntaxique par-dessus les promesses, qui a l'avantage d'être plus concis.

Si un appel d'API est utilisé dans une fonction, cette fonction doit être asynchrone.

De plus, il faut entourer le fetch d'un try... catch pour récupérer les erreurs potentielles.

React Native

const appelerApi = async () => {  
  try {  
    const response = await fetch(url);  
    if (!response.ok) {  
        throw new Error(`Erreur API : ${response.status} ${response.statusText}`);  
    }  
    const result = await response.json();  
    //console.log(result)  
    // faire quelque chose avec les informations générées  
  } catch (error) {  
    //console.error(error)  
    // réagir en cas d'erreur  
  } finally {  
    // instructions à faire quand l'appel est terminé ou quand l'appel a échoué  
  }  
 };

## Appel de type POST, avec en-tête et corps

Voici un rappel sur la façon d'effectuer une requête de type POST.

J'ai également inclus la technique pour fournir des informations d'en-tête à la requête de même que des informations passées dans le corps.

Syntaxe .then() :

React Native

const appelerApi = () => {  
  fetch(url, {  
    method: 'POST',  
    headers: {  
      'Authorization': ...,  
      'Content-Type': 'application/json',  
    },  
    body: JSON.stringify({  
      ...  
    }),  
  }).then(response => {  
     ...   // même chose qu'avec une requête GET  
  });  
};

Syntaxe async/await :

JavaScript

const appelerApi = async () => {  
  try {  
    const response = await fetch(url, {  
      method: 'POST',  
      headers: {  
        'Authorization': ...,  
        'Content-Type': 'application/json',  
      },  
      body: JSON.stringify({  
        ...  
      }),  
    });  
    ...

 

    const result = await response.json();  
    ...   // même chose qu'avec une requête GET  
  }  
};

## Erreur « TypeError: Network request failed »

Si, sur l'émulateur, vous obtenez l'erreur « TypeError: Network request failed » quand l'application tente d'effectuer la requête API, ceci peut être dû à un problème d'initialisation de l'émulateur.

![Network Request Failed](assets/21-19-utiliser-un-api-55.png)

Une solution consiste à refermer l'émulateur, refermer la fenêtre Metro avec Ctrl + C puis à relancer l'application.

## 19.2 API qui génère des images

Le type d'information retournée par les API qui génèrent des images dépend de la façon dont chaque API est programmée.

Si l'API génère du JSON, son utilisation se fera de la même façon que pour toute autre API.

Mais souvent, les API d'images génèrent directement une image. De façon plus précise, ces API effectuent une redirection pour retourner une image.

La solution la plus simple pour utiliser un tel API est de placer l'URL directement dans une balise <Image>.

React Native

<Image source={{uri: `https://api.dicebear.com/9.x/personas/png?seed=${seed}`}} style={mesStyles.image} />

Si vous tenez à passer par un appel fetch (mais pourquoi vous voudriez faire ça?), il faut utiliser response.url (notez l'absence de parenthèses) afin de récupérer l'URL finale.

Il s'agit souvent de l'URL utilisé pour réaliser le fetch mais parfois, il pourrait avoir été modifié par des redirections.

React Native

const appelerApi = () => {  
  fetch(url)  
  .then(response => {  
    if (!response.ok) {  
      // le catch pourra réagir à l'erreur  
      throw new Error(`Erreur API : ${response.status} ${response.statusText}`);  
    }  
    return response.url;  
  }).then(result => {  
    //console.log(result)   // on verra l'URL  
    // faire quelque chose avec les informations générées  
  }).catch(error => {  
    //console.error(error)  
    // réagir en cas d'erreur  
  }).finally(() => {  
    // instructions à faire quand l'appel est terminé ou quand l'appel a échoué  
  });   
 };

## 19.3 fetch vs Axios

Avant l'arrivée de fetch, aux alentours de 2015, le code JavaScript devait effectuer les requêtes à un API avec [XmlHttpRequests](https://developer.mozilla.org/en-US/docs/Web/API/XMLHttpRequest). C'était un processus plutôt complexe.

Peu de temps après l'arrivée de fetch, aux alentours de 2016, un petit nouveau est entré en piste : la bibliothèque [Axios](https://axios-http.com/docs/intro).

Mais pourquoi installer une bibliothèque tierce pour effectuer un travail qui peut être réalisé avec fetch, considérant que fetch est natif dans tous les navigateurs modernes et dans Node.js (depuis v18)?

Axios offre des fonctionnalités pratiques comme les intercepteurs de requêtes, l'interprétation automatique du JSON (JSON parsing) et une gestion simplifiée des erreurs.

En revanche, il requiert une installation supplémentaire, ce qui augmente légèrement le poids de l'application.

Le choix entre fetch et Axios dépend principalement des préférences personnelles. À vous de choisir!

Sachez qu'en plus d'Axios, d'autres bibliothèques tierces permettent d'effectuer des requêtes à un API en JavaScript, par exemple [Ky](https://github.com/sindresorhus/ky), [Alova](https://github.com/alovajs/alova) et [Redaxios](https://github.com/developit/redaxios) (une version légère d'Axios).

Voici un exemple qui montre une requête API simple avec fetch puis avec Axios.

Dans un premier temps, le code est illustré avec la syntaxe .then().

React Native

fetch('https://reactnative.dev/movies.json')  
.then(response => {  
  if (!response.ok) {  
    throw new Error(`Erreur API : ${response.status} ${response.statusText}`);  
  }  
  return response.json();   
})  
.then(result => {  
  setData(result.movies);  
})  
.catch(error => {  
  console.error(error);  
})  
.finally(() => {  
  ...  
});

React Native

axios.get('https://reactnative.dev/movies.json')  
 .then(response => {  
  setData(response.data.movies);  
 })  
 .catch(error => {  
  // arrivera ici également si la réponse n'est pas ok (code d'état pas entre 200 et 299)  
  console.error(error);  
 })  
 .finally(() => {  
  ...  
 });

Voici ces mêmes extraits de code avec la syntaxe async/await.

React Native

try {  
  // Code de la doc React Native : https://reactnative.dev/docs/network  
  const response = await fetch('https://reactnative.dev/movies.json');  
  if (!response.ok) {  
    throw new Error(`Erreur API : ${response.status} ${response.statusText}`);  
  }  
  const result = await response.json();  
  setData(result.movies);   // met à jour une variable d'état  
} catch (error) {  
  console.error(error);  
 } finally {  
  ...  
 }

React Native

try {  
  const response = await axios.get('https://reactnative.dev/movies.json');  
  setData(response.data.movies);   // met à jour une variable d'état  
 } catch (error) {  
  // arrivera ici également si la réponse n'est pas ok (code d'état pas entre 200 et 299)  
  console.error(error);  
 } finally {  
  ...  
 }