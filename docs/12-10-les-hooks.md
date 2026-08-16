# 10. Les hooks

# 10. Les hooks

## 10.1 Qu'est-ce qu'un hook React Native?

Dans React et React Native, les hooks sont un mécanisme qui permet de suivre l'état d'un composant de type fonction (functional component).

Parmi les hooks les plus utilisés, notons :

* [apical\_lien\_interne][variables\_d\_etat,useState][/apical\_lien\_interne]: permet de définir une variable d'état
* [apical\_lien\_interne][rendre\_une\_variable\_d\_etat\_disponible\_a\_tous\_les\_enfants,useContext][/apical\_lien\_interne]: permet de définir une variable partagée entre différents composants
* [apical\_lien\_interne][reagir\_seulement\_lorsqu\_une\_variable\_d\_etat\_a\_change\_de\_valeur,useEffect][/apical\_lien\_interne]: permet de réagir à un changement de valeur
* [useRef](https://react.dev/reference/react/useRef) : permet de définir une variable qui n'est pas utilisée dans le rendu
* [apical\_lien\_interne][memoriser\_des\_informations\_longues\_a\_calculer\_usememo,useMemo][/apical\_lien\_interne]: permet de mémoriser des informations longues à calculer.
* [apical\_lien\_interne][optimiser\_les\_performances\_usecallback,useCallback][/apical\_lien\_interne]: permet de mettre une fonction en cache entre deux rendus
* [apical\_lien\_interne][effectuer\_une\_action\_quand\_un\_composant\_gagne\_le\_focus,useFocusEffect][/apical\_lien\_interne]: permet d'effectuer du code lorsqu'un écran prend le focus (fonctionne avec React Navigation)

## Pour plus d'information

« Built-in React Hooks ». React. <https://react.dev/reference/react/hooks>

« React Hooks cheat sheet: Best practices with examples ». LogRocket. <https://blog.logrocket.com/react-hooks-cheat-sheet-solutions-common-problems/>

## 10.2 Effectuer un traitement lors du chargement du composant (useEffect sans dépendances)

Le hook [useEffect](https://react.dev/reference/react/useEffect) permet d'effectuer un traitement lorsqu'une variable change de valeur.

Mais si on ne fournit aucune variable en paramètre, le traitement n'aura lieu qu'une seule fois, lorsque le composant vient de se charger.

React Native

const MonComposant = () => {  
  ...  
  
  useEffect(() => {  
    ...   // ce code sera exécuté une fois, lorsque le composant sera chargé  
  }, []);  
  
  return (  
    ...  
  );  
};

## 10.3 Réagir seulement lorsqu'une variable d'état a changé de valeur (useEffect)

Le hook [useEffect](https://react.dev/reference/react/useEffect) permet d'effectuer un traitement seulement lorsqu'une variable change de valeur.

React Native

useEffect(() => {  
  ...   // Ce code sera effectué seulement lorsque maVariable change de valeur.  
}, [maVariable]);

Il sera utile notamment pour effectuer un traitement qui requiert que le changement de valeur soit effectivement fait.

React Native

setMaVariable('nouvelle valeur');   // la nouvelle valeur ne sera effective que lors du prochain rendu  
...  
useEffect(() => {  
  ...   // Ici, on sait que le changement de valeur a eu lieu.  
}, [maVariable]);

## 10.4 Appeler une fonction asynchrone dans un useEffect

Si vous utilisez le hook useEffect pour appeler une fonction asynchrone lorsqu'une variable change de valeur, vous obtiendrez l'erreur « Les expressions 'await' sont autorisées uniquement dans les fonctions asynchrones et aux niveaux supérieurs des modules. ».

React Native

useEffect(() => {  
  const db = await getDBConnection();  
  ...  
}, [id]);

Une des techniques pour régler ce problème consiste à définir une fonction asynchrone dans le useEffect et immédiatement appeler cette fonction.

React Native

useEffect(() => {  
  // pour appeler une fonction asynchrone dans un useEffect, il faut déclarer une fonction async qui fait le await puis appeler cette fonction.  
  // autres pistes de solutions : https://stackoverflow.com/questions/53332321/react-hook-warnings-for-async-function-in-useeffect-useeffect-function-must-ret  
  async function retrouverItem() {  
    ...  
    const db = await getDBConnection();  
    ...

 

  }

 

  retrouverItem();  
 }, [id]);

Je vous propose une autre syntaxe que vous pourriez rencontrer mais qui est moins facile à comprendre pour un débutant.

Il s'agit de déclarer une fonction anonyme et de l'appeler immédiatement grâce aux [fonctions immédiatement invoquées](https://developer.mozilla.org/en-US/docs/Glossary/IIFE) (IIFE, Immediately Invoked Function Expression).

Notez que cet acronyme se prononce « iffy ».

Dans le contexte actuel, la IIFE est asynchrone mais il est possible d'avoir une IIFE sans le mot-clé async.

React Native

useEffect(() => {  
  (async () => {  
    ...  
  })();   // ces parenthèses sont ce qui fait que la fonction est immédiatement invoquée  
 }, [id]);

## 10.5 Rendre une variable d'état disponible à tous les enfants (useContext)

Le hook [useContext](https://react.dev/reference/react/useContext) permet de définir une variable qui sera disponible pour tous les enfants.

Pour l'utiliser avec TypeScript, il faut d'abord définir le type d'information qu'il permet de passer. De façon générale, il s'agira d'une variable d'état et de la fonction qui permet de la modifier.

Ce type peut être défini dans le même fichier que la définition d'une interface.

Fichier src/models/Categorie.ts

export interface Categorie {  
  id: number | null;  
  description: string;  
}

 

export type CategorieContextType = {  
  categories: Categorie[];  
  setCategories: (categories: Categorie[]) => void;  
};

La définition du contexte sera effectuée dans le composant le plus proche parent de ceux qui en ont besoin.

Dans cet exemple, j'ai utilisé le composant de base de l'application.

Fichier App.tsx (React Native)

...  
import {Categorie,CategorieContextType} from "./models/Categorie";  
  
export const CategoriesContext = React.createContext<CategorieContextType | null>(null);  
  
const App = () => {  
  const [categories, setCategories] = useState<Categorie[]>([]);  
  ...  
  return (  
    <CategoriesContext.Provider value={{categories, setCategories}}>  
      {/\* Les composant qui sont ici auront accès au contexte \*/}  
    </CategoriesContext.Provider>  
  );  
};

Dans un composant enfant, on retrouvera le contexte comme suit :

Composant enfant (Reat Native)

const ComposantEnfant = () => {  
  const {categories, setCategories} = React.useContext(CategoriesContext) as CategorieContextType;  
  ...   // il est désormais possible d'utiliser la variable categories et de la modifier à l'aide de setCategories.  
};

## Pour plus d'information

«  createContext ». React. <https://react.dev/reference/react/createContext>

« How to use React Context with TypeScript ». LogRocket. <https://blog.logrocket.com/how-to-use-react-context-typescript/>

## 10.6 Mémoriser des informations longues à calculer (useMemo)

Le hook [useMemo](https://react.dev/reference/react/useMemo) permet de mettre en cache le résultat de calculs entre deux rendus.

## Pour plus d'information

« Understanding useMemo and useCallback ». Josh Comeau. <https://www.joshwcomeau.com/react/usememo-and-usecallback/>

« How to Work with useMemo in React – with Code Examples ». Free Code Camp. <https://www.freecodecamp.org/news/how-to-work-with-usememo-in-react/>

« You Might Not Need an Effect ». React.dev. <https://react.dev/learn/you-might-not-need-an-effect>

## 10.7 Mettre une fonction en cache entre deux rendus (useCallback)

Le hook [useCallback](https://react.dev/reference/react/useCallback) permet de mettre une fonction en cache afin d'optimiser les performances de l'application.

Il est souvent utilisé en combinaison avec d'autres hooks, par exemple [apical\_lien\_interne][effectuer\_une\_action\_quand\_un\_composant\_gagne\_le\_focus,useFocusEffect][/apical\_lien\_interne]. Ceci assure que le code de la fonction mise en cache ne sera pas réexécuté à chaque rafraîchissement d'écran. On précisera dans quelles conditions il faut réexécuter ce code.

## Pour plus d'information

« Understanding useMemo and useCallback ». Josh Comeau. <https://www.joshwcomeau.com/react/usememo-and-usecallback/>

« Optimizing React Native Performance with useCallback ». Medium. <https://viniciuspetrachin.medium.com/optimizing-react-native-performance-with-usecallback-bdb6d801c9cf>

## 10.8 Effectuer une action quand un composant gagne le focus (useFocusEffect)

Dans une application qui utilise React Navigation, le hook [useFocusEffect](https://reactnavigation.org/docs/use-focus-effect/) permet d'effectuer une action seulement si le composant a le focus.

Ce hook doit être utilisé en combinaison avec le hook [useCallback](https://react.dev/reference/react/useCallback) sinon, le code sera exécuté à chaque fois que l'écran est rafraîchi.

De façon plus précise, le code sera exécuté seulement si le composant a le focus ET que la dépendance associée au hook useCallback change de valeur.

Dans le cas où useCallback n'a aucune dépendance (lorqu'on retrouve [] comme second paramètre), le code sera exécuté lorsque le composant gagne le focus.

React Native

useFocusEffect(  
  useCallback(() => {  
    // code à exécuter à chaque fois que le composant gagne le focus, par exemple retrouver des informations dans la BD.  
  
    return () => {  
      // code à exécuter à chaque fois que le composant perd le focus, par exemple effectuer des tâches de nettoyage.  
    };  
  }, [])  
 );

## Pour plus d'information

« useFocusEffect ». React Navigation. [https://reactnavigation.org/docs/use-focus-effect/](https://reactnavigation.org/docs/use-focus-effect/#:~:text=The%20effect%20will%20run%20whenever%20the%20dependencies%20passed%20to%20React.useCallback%20change)