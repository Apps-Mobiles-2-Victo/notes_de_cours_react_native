# 8. Rendu conditionnel

# 8. Rendu conditionnel

## 8.1 Afficher un élément React Native seulement si une condition est remplie (&&)

L'opérateur && (ET logique) peut être utilisé pour effectuer un affichage seulement si une condition est vraie.

Le prinicipe est le suivant : avec un ET logique, si la première condition est fausse, il est inutile de tenter d'évaluer ce qui suit car le résultat sera forcément faux.

Donc, l'affichage qui suit le ET ne sera pas réalisé si la condition est fausse.

React Native

{uneVariable &&  
  <Text>Ceci est affiché seulement si uneVariable est à true.</Text>  
}

## 8.2 Afficher une chose ou une autre selon une condition

L'opérateur ternaire peut être utilisé pour afficher une chose ou une autre selon une condition.

Attention : contrairement à l'affichage conditionnel avec un ET logique qui nécessite deux caractères &, il n'y a qu'un seul caractère ? ici.

React Native

{uneVariable ?  
  <Text>La condition est remplie.</Text>  
:  
  <Text>La condition n'est pas remplie.</Text>  
}