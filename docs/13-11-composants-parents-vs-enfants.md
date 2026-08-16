# 11. Composants parents vs enfants

# 11. Composants parents vs enfants

## 11.1 Changer la valeur d'une variable d'état à partir d'un composant enfant

Le flux de l'information circule généralement du composant parent vers ses composants enfants.

Dans certains cas, cependant, ce flux peut être inversé c'est-à-dire qu'un composant enfant peut modifier une valeur qui devrait affecter le composant parent.

Pour que ce soit possible, le composant parent doit passer en paramètre au composant enfant une référence à la fonction du composant parent qui devra être exécutée. Dans cet exemple, le paramètre s'appelle onClick (il aurait pu porter n'importe quel autre nom) et il contient une référence à la fonction faireQuelqueChose définie dans le parent.

Fichier App.tsx (React Native)

const App = () => {  
  ...  
  
  const faireQuelqueChose = (): void => {  
    ...;   // on pourrait modifier la valeur d'une variable d'état ici  
  };  
  
  return (  
    <MonComposantEnfant ... onClick={faireQuelqueChose}/>  
  );  
};

 

 

 

export default App;

Dans le composant enfant, un appel à cette fonction pourra être réalisé au moment opportun.

Fichier MonComposantEnfant.tsx (React Native)

type MonComposantEnfantProps = {  
  ...  
  onClick(): void;  
};  
  
const MonComposantEnfant = (props: MonComposantEnfantProps) => {  
  return (  
    <TouchableOpacity onPress={() => props.onClick()}>  
      ...  
    </TouchableOpacity>  
  );  
};  
  
export default MonComposantEnfant;