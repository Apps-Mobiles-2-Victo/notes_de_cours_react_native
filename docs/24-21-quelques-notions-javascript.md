# 21. Quelques notions JavaScript

# 21. Quelques notions JavaScript

## 21.1 Les tableaux avec React Native

Prenon l'exemple d'une application React Native qui doit afficher les valeurs d'un tableau dont voici la structure.

React Native

interface Ami {  
  id: number;  
  prenom: string;  
  nomFamille: string;  
}  
  
...  
  
const amis: Ami[] = [  
  {id: 1, prenom: 'Annie', nomFamille: 'Gagnon'},

 

  {id: 2, prenom: 'Justin', nomFamille: 'Bellemare'},  
  {id: 3, prenom: 'Aurélie', nomFamille: 'Dubuc'},  
];

Pour afficher les valeurs d'un tel tableau à l'aide d'une boucle, vous pouvez utiliser la méthode [map()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/map).

React Native

{amis.map((ami, i) => (  
  <React.Fragment key={i}>  
    <Text>{ami.prenom}</Text>  
    <Text>{ami.nomFamille}</Text>  
  </React.Fragment>  
 ))}

## Fragments pour spécifier la clé unique

Remarquez l'utilisation [apical\_lien\_interne][les\_fragments,des fragments][/apical\_lien\_interne], c'est-à-dire les balises <React.Fragment></React.Fragment>.  Elles sont l'équivalent de <></> mais elles permettent d'ajouter des propriétés.

Ceci est requis ici puisque si vous ne spécifiez pas de clé unique (attribut key) pour chaque itération, vous obtiendrez ce message d'erreur au bas de l'émulateur : « Each child in a list should have a uniaque "key" prop. ».

![Each child in a list should have a uniaque ](assets/24-21-quelques-notions-javascript-57.png)

Si on n'avait qu'un composant à afficher dans la boucle, il aurait été possible de lui passer directement l'attribut key.

React Native

{amis.map((ami, i) => (  
  <Text key={i}>{ami.prenom}</Text>  
 ))}

## Si la valeur de i n'est pas toujours associée au même élément

React Native a besoin de connaître en tout temps à quel élément la clé est associée. L'utilisation de i fonctionnera dans la majorité des cas.

Mais si le tableau devait être trié, par exemple après l'ajout d'un élément, la valeur de i ne permettrait plus de faire le suivi avec les valeurs originales. Ceci pourrait entraîner des problèmes de performance ou des incohérences dans l'affichage puisque React Native ne pourra plus gérer correctement l'état.

Dans un tel cas, on devra utiliser une valeur qui identifie chaque élément de façon unique, par exemple son identifiant s'il existe.

React Native

{amis.map((ami) => (  
  <React.Fragment key={ami.id}>  
    <Text>{ami.prenom}</Text>  
    <Text>{ami.nomFamille}</Text>  
  </React.Fragment>  
))}

En fait, lorsqu'on a accès à une donnée qui identifie chaque élément de façon unique, on devrait toujours l'utiliser comme clé plutôt que de travailler avec i.

## Ajouter un élément à la fin d'un tableau

Le code qui suit permet d'ajouter un élément à la fin d'un tableau existant.

React Native

setDonnees((donnees) => [...donnees, nouvelleDonnee]);

Voici un extrait de code qui fait la même chose mais de façon plus explicite (explications tirées de <https://react.dev/learn/updating-arrays-in-state>) :

React Native

setDonnees((donnees) =>  
  // remplace le tableau  
  [  
    // avec un nouveau tableau  
    ...donnees, // qui comprend les éléments originaux  
    {id: null, ...}, // et un nouvel élément à la fin  
  ],  
);

## 21.2 Les littéraux de gabarits

En JavaScript, un littéral de gabarit (en anglais : template literal) et une chaîne de caractère dans laquelle une expression peut être introduite et correctement interprétée.

On utilise les guillemets obliques (backticks : `) pour entourer un littéral de gabarit.

JavaScript

const nom = 'Annie';  
 const libelle = `Mon nom est ${nom}.`;   // libellé vaut « Mon nom est Annie. »

À l'intérieur du littéral de gabarit, il faut entourer d'accolades la variable qui doit être interprétée et faire précéder le tout par un signe $.

On peut comparer les littéraux de gabarits JavaScript aux chaînes entourées d'apostrophes en PHP : les deux permettent d'interpréter une variable à l'intérieur de la chaîne.

Cependant, les littéraux de gabarits vont plus loin : ils permettent d'exécuter des fonctions, ce qui n'est pas permis dans une chaîne PHP entourée d'apostrophes.

JavaScript

const libellé = `J'ai ${getAge("1993/06/04")} ans.`;

## Pour plus d'information

« Littéraux de gabarits ». MDN. <https://developer.mozilla.org/fr/docs/Web/JavaScript/Reference/Litt%C3%A9raux_gabarits>

## 21.3 La copie de tableaux JavaScript - par référence, superficielle, profonde

## Copie par référence

Lorsqu'on copie un tableau  JavaScript dans un autre tableau à l'aide de l'opérateur d'assignation, on obtient deux variables qui pointent sur les mêmes cases mémoire.

Autrement dit, toute modification au premier tableau se reflète dans le second tableau.

JavaScript

const tableauOriginal = ['Table', 'Chaise', 'Bureau'];  
const copieReference = tableauOriginal;  
tableauOriginal[1]= 'Lampe';  
console.log(copieReference[1]);   // la copie est également modifiée : 'Lampe'

## Copie superficielle (shallow copy)

Il est souvent préférable de créer une copie du tableau plutôt que de simplement faire pointer le second tableau sur les cases mémoire du premier.

La première technique que je vous présente permet d'effectuer une copie superficielle, c'est-à-dire que les éléments du second tableau seront des copies de celles du premier. Par contre, on dit que la copie est superficielle puisque si les tableaux ont des éléments qui sont des objets, ces éléments dans le second tableau seront des références à ceux du premier tableau.

Voyons d'abord un exemple sans élément objet. Les deux tableaux seront alors complètement distincts.

JavaScript

const tableauOriginal = ['Table', 'Chaise', 'Bureau'];  
const copieSuperficielle = tableauOriginal.slice(0);  
tableauOriginal[1]= 'Lampe';  
console.log(copieSuperficielle[1]);   // la copie est inchangée : 'Chaise'

Maintenant, faisons le test avec des éléments objet.

JavaScript

const tableauOriginal = [  
    {id: 1, description: 'Table'},  
    {id: 2, description: 'Chaise'},  
    {id: 3, description: 'Bureau'},  
];  
  
const copieSuperficielle = tableauOriginal.slice(0);  
tableauOriginal[1].description = 'Lit';  
console.log(copieSuperficielle[1]);   // l'élément objet de la copie est également modifié :   
                                      // {id: 2, description: 'Lit'}

La copie superficielle peut également être réalisée à l'aide de l'[opérateur de décomposition](https://fr.legacy.reactjs.org/docs/jsx-in-depth.html#spread-attributes) (spread operator).

JavaScript

const tableauOriginal = [  
    {id: 1, description: 'Table'},  
    {id: 2, description: 'Chaise'},  
    {id: 3, description: 'Bureau'},  
];  
  
const copieSuperficielle = {...tableauOriginal};  
tableauOriginal[1].description = 'Lit';  
console.log(copieSuperficielle[1]);   // l'élément objet de la copie est également modifié :   
                                      // {id: 2, description: 'Lit'}

## Copie profonde (deep copy)

Avec une copie profonde, tous les éléments du second tableau sont distincts de ceux du premier, même les éléments qui sont des objets.

La technique la plus intéressante pour réaliser une copie profonde consiste à utiliser [structuredClone()](https://developer.mozilla.org/en-US/docs/Web/API/Window/structuredClone).

JavaScript

const tableauOriginal = [  
    {id: 1, description: 'Table'},  
    {id: 2, description: 'Chaise'},  
    {id: 3, description: 'Bureau'},  
];  
  
const copieProfonde = structuredClone(tableauOriginal);  
tableauOriginal[1].description = 'Lit';  
console.log(copieProfonde[1]);   // l'élément objet de la copie demeure inchangé :   
                                 // {id: 2, description: 'Chaise'}

Les deux objets sont vraiment distincts peu importe le niveau de profondeur des éléments objet.

JavaScript

const tableauOriginal = [  
    {id: 1, description: 'Table', categorie: {code: 'a', description: 'Catégorie A'}},  
    {id: 2, description: 'Chaise', categorie: {code: 'b', description: 'Catégorie B'}},  
    {id: 3, description: 'Bureau', categorie: {code: 'c', description: 'Catégorie C'}},  
];  
  
const copieProfonde = structuredClone(tableauOriginal);  
  
tableauOriginal[1].categorie = {code: 'd', description: 'Catégorie D'};  
  
console.log(copieProfonde[1]);   // l'élément objet de la copie demeure inchangé :   
                                // {id: 2, description: 'Chaise', categorie": {code: 'b', description: "Catégorie B'}}

La méthode structuredClone() est largement supportée depuis 2021, mais pas dans tous les environnements. Par exemple, lors de mes tests en React Native en 2025, elle n'était pas implantée.

Dans le code produit avant son arrivée, et dans les environnements où structuredClone() n'est pas implantée, les développeurs utilisent cette astuce pour effectuer un travail à peu près équivalent.

JavaScript

const tableauOriginal = [  
    {id: 1, description: 'Table', categorie: {code: 'a', description: 'Catégorie A'}},  
    {id: 2, description: 'Chaise', categorie: {code: 'b', description: 'Catégorie B'}},  
    {id: 3, description: 'Bureau', categorie: {code: 'c', description: 'Catégorie C'}},  
];  
  
const copieProfonde = JSON.parse(JSON.stringify(tableauOriginal));  
tableauOriginal[1].categorie = {code: 'd', description: 'Catégorie D'};  
console.log(copieProfonde[1]);   // l'élément objet de la copie demeure inchangé :   
                                 // {id: 2, description: 'Chaise', categorie": {code: 'b', description: "Catégorie B'}}

## 21.4 Comment fonctionnent les fonctions fléchées

Les fonctions fléchées (arrow functions), vous connaissez?

Ce concept existe dans différents langages, par exemple JavaScript, Java, Kotlin, Python, PHP.

Dans certains langage, on les appelle plutôt expressions lambda.

Je vous fait une démonstration ici en JavaScript mais les concepts peuvent être étendus aux autres langages si on y apporte quelques ajustements, notamment au niveau de la syntaxe.

Dans cette fiche :

* [Syntaxe de base](https://apical.xyz/formations/pageunique/applications_mobiles_avec_react_native20251210101311#syntaxe)
* [Quelques raccourcis](https://apical.xyz/formations/pageunique/applications_mobiles_avec_react_native20251210101311#raccourcis)
* [Instruction return](https://apical.xyz/formations/pageunique/applications_mobiles_avec_react_native20251210101311#return)
* [Utilisation](https://apical.xyz/formations/pageunique/applications_mobiles_avec_react_native20251210101311#utilisation)
  + [Exemple avec boucle](https://apical.xyz/formations/pageunique/applications_mobiles_avec_react_native20251210101311#boucle)
  + [Exemple avec fetch](https://apical.xyz/formations/pageunique/applications_mobiles_avec_react_native20251210101311#fetch)
* [Quelques différences](https://apical.xyz/formations/pageunique/applications_mobiles_avec_react_native20251210101311#differences)
  + [Déclaration avant utilisation](https://apical.xyz/formations/pageunique/applications_mobiles_avec_react_native20251210101311#declaration)
  + [this](https://apical.xyz/formations/pageunique/applications_mobiles_avec_react_native20251210101311#this)

## Syntaxe de base

Une fonction fléchée, c'est une façon différente, plus concise de déclarer une fonction.

Plutôt que de déclarer une fonction régulière comme ceci :

JavaScript

function faireQuelqueChose() {  
  console.log('Test!');  
}

on déclarera une fonction fléchée comme cela :

JavaScript

const faireQuelqueChose = () => {  
  console.log('Test!');  
}

Remarquez :

* la fonction est assignée à une variable
* il pourrait y avoir des paramètres entre les parenthèses
* la flèche (=>) indique le début du code à exécuter

Dans le cas où la fonction attend un ou plusieurs paramètres, on aura plutôt ceci.

Fonction régulière :

JavaScript

function faireQuelqueChose(unParametre) {  
  console.log(`Paramètre: ${unParametre}!`);  
}

Fonction fléchée :

JavaScript

const faireQuelqueChose = (unParametre) => {  
  console.log(`Paramètre: ${unParametre}!`);  
}

Oui mais la fonction fléchée dans cet exemple n'est pas plus concise que la fonction régulière.

Pour donner un exemple vraiment équivalent, la fonction régulière aurait été elle aussi assignée à une variable. On voit que la syntaxe fléchée est plus concise dans ce contexte.

JavaScript

const faireQuelqueChose = function(unParametre) {  
  console.log(`Paramètre: ${unParametre}!`);  
}

## Quelques raccourcis

Avec une fonction fléchée, il est possible d'omettre les parenthèses alentour du paramètre lorsqu'il n'y en a qu'un seul.

JavaScript

const faireQuelqueChose = unParametre => {  
  console.log(`Paramètre: ${unParametre}!`);  
}

Il est également possible d'omettre les accolades lorsqu'il n'y a qu'une seule instruction et qu'il ne s'agit pas d'un return.

JavaScript

const faireQuelqueChose = unParametre => console.log(`Paramètre: ${unParametre}!`);

Remarquez que plusieurs développeurs préfèrent toujours utiliser les accolades et les parenthèses puisqu'elles contribuent à clarifier le code.

## Instruction return

Lorsque la fonction doit faire un return et qu'elle ne comprend qu'une seule instruction, il n'est pas requis d'écrire le return. Il est implicite.

Version avec return explicite :

JavaScript

const doubler = nombre => { return nombre \* 2 };

Version avec return implicite :

JavaScript

const doubler = nombre => nombre \* 2;

Si on écrit le return, il faut absolument utiliser les accolades même s'il n'y a qu'une seule instruction.

JavaScript

const doubler = nombre => return nombre \* 2;  
console.log(doubler(3)); // Unexpected token 'return'

Et si on désire retourner une valeur et qu'on utilise les accolades, il faut absolument écrire le mot return.

JavaScript

const doubler = nombre => { nombre \* 2 }  
console.log(doubler(3)); // undefined car il y a des accolades sans return

## Utilisation

Que la fonction ait été définie de façon régulière ou en tant que fonction fléchée, elle sera appelée de la même façon.

JavaScript

faireQuelqueChose('Allô!');  
const resultat = doubler(3);

### Exemple avec boucle

On pourrait définir et appeler la fonction fléchée directement comme fonction de rappel (callback function) dans une boucle.

On n'aura alors pas besoin de l'assigner à une variable.

JavaScript

const valeurs = ['A', 'B', 'C'];

 

valeurs.map((unParametre) => {  
  console.log(`Paramètre: ${unParametre}!`);  
})

Sans les fonctions fléchées, le code est moins élégant.

JavaScript

valeurs.map(function(unParametre) {  
  console.log(`Paramètre: ${unParametre}!`);  
})

### Exemple avec fetch

Lorsqu'on réalise un appel AJAX avec fetch, les fonctions fléchées sont particulièrement intéressantes.

JavaScript

fetch('https://...', {  
  method: 'POST',  
  headers: {  
    'Content-Type': 'application/json',  
  },  
  body: JSON.stringify({  
    // ...  
  }),  
 }).then(response => {  
  if (!response.ok) {  
    // le catch pourra réagir à l'erreur  
    throw new Error(`Erreur API : ${response.status} ${response.statusText}`);  
  }  
  return response.json();  
 }).then(result => {  
  console.log(result.message);  
 }).catch(error => {  
  console.log(error);  
 }).finally(() => {  
  // ...  
 });

Sans les fonctions fléchées, le code est ici aussi moins élégant.

JavaScript

fetch('https://...', {  
  method: 'POST',  
  headers: {  
    'Content-Type': 'application/json',  
  },  
  body: JSON.stringify({  
    // ...  
  }),  
 }).then(function(response) {  
  if (!response.ok) {  
    // le catch pourra réagir à l'erreur  
    throw new Error(`Erreur API : ${response.status} ${response.statusText}`);  
  }  
  return response.json();   
 }).then(function(result) {  
  console.log(result.message);  
 }).catch(function(error) {  
  console.log(error);  
 }).finally(function() {  
  // ...  
 });

## Quelques différences

Voyons maintenant quelques différences entre les fonctions régulières et les fonctions fléchées.

### Déclaration avant utilisation

Une fonction régulière peut être appelée dans le haut d'un fichier puis déclarée plus bas.

JavaScript

faireQuelqueChose('Appel avant déclaration');   // ok

 

function faireQuelqueChose(unParametre) {  
  console.log(`Paramètre: ${unParametre}!`);  
}

Avec les fonctions fléchées, il faut absolument déclarer la fonction avant de l'appeler.

JavaScript

faireQuelqueChose('Appel avant déclaration');   // Uncaught ReferenceError: Cannot access 'faireQuelqueChose' before initialization  
  
const faireQuelqueChose = unParametre => {  
  console.log(`Paramètre: ${unParametre}!`);  
}

Par contre, ce code React fonctionne puisque le contenu du hook useEffect n'est exécuté que lorsque le composant est chargé.

JavaScript

useEffect(() => {  
  faireQuelqueChose('ok dans useEffect');  
 }, []);  
  
const faireQuelqueChose = unParametre => {  
  console.log(`Paramètre: ${unParametre}!`);  
}

### this

Dans une fonction régulière, le mot-clé this fait référence au contexte d'exécution.

Par exemple, dans un gestionnaire d'événement, il s'agit de l'élément du DOM qui a déclenché l'événement.

JavaScript

class MaClasse {  
  constructor() {  
    this.bouton = document.getElementById('bouton');  
    this.compteur = 0;       
  }  
  
  maMethode() {      
    this.bouton.addEventListener('click', function() {  
      this.compteur++;   // this est l'élément DOM sur lequel on a cliqué. Ceci n'a aucun effet sur la propriété compteur de la classe.  
    });  
  }  
}  
  
let test = new MaClasse();  
test.maMethode();

Dans une fonction fléchée, le mot-clé this fait référence au contexte dans lequel la fonction a été créée.

Par exemple, à l'intérieur d'une classe, il s'agit de l'instance de la classe.

JavaScript

class MaClasse {  
  constructor() {  
    this.bouton = document.getElementById('bouton');  
    this.compteur = 0;       
  }

 

  maMethode() {  
    this.element.addEventListener('click', () => {  
      this.compteur++;    // this est l'instance de la classe. Ceci fonctionne correctement.  
    });  
  }  
}  
  
let test = new MaClasse();  
test.maMethode();

Voici un exemple que j'ai vécu il y a quelques années dans un programme réel.

Avant de réaliser que les fonctions fléchées pourraient régler mon problème avec this, j'avais développé une astuce pour contourner ce problème.

Ce code fait partie d'une application WordPress qui utilise la bibliothèque [three.js](https://threejs.org/) pour afficher une image 3D.

JavaScript

class MonGestionnaireImages {  
  ...  
  chargerImage( urlImage ) {  
    let referenceAThis = this;   // ici, this est l'instance de la classe

 

    let textureLoader = new THREE.TextureLoader();

 

    textureLoader.load( urlImage,  
       function ( texture ) {  
        var spriteMaterial = new THREE.SpriteMaterial( { map: texture } );

 

        referenceAThis.objet = new THREE.Sprite( spriteMaterial );   // ici, this serait undefined car c'est le code interne de la classe THREE.TextureLoader qui exécute la fonction de rappel  
         ...  
      },  
      ...  
    );

 

  }  
}

Le code serait beaucoup plus élégant avec une fonction fléchée :

JavaScript

class MonGestionnaireImages {  
  ...  
  chargerImage( urlImage ) {  
    let textureLoader = new THREE.TextureLoader();

 

    textureLoader.load( urlImage,  
      (texture) => {  
        var spriteMaterial = new THREE.SpriteMaterial( { map: texture } );  
        this.objet = new THREE.Sprite( spriteMaterial );   // ici, this est l'instance de la classe  
      },  
      ...  
    );   
  }  
}

Selon vous, les parenthèses alentour de texture sont-elles obligatoires?

## Pour plus d'information

« How to Use JavaScript Arrow Functions – Explained in Detail ». Free Code Camp. <https://www.freecodecamp.org/news/javascript-arrow-functions-in-depth/>