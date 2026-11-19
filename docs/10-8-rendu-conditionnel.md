# 7. Rendu conditionnel

## 7.1 Afficher un élément React Native seulement si une condition est remplie (&&)

L'opérateur && (ET logique) peut être utilisé pour effectuer un affichage seulement si une condition est vraie.

Le principe est le suivant : avec un ET logique, si la première condition est fausse, il est inutile de tenter d'évaluer ce qui suit car le résultat sera forcément faux.

Donc, l'affichage qui suit le ET ne sera pas réalisé si la condition est fausse.

```tsx title="React Native"
{uneVariable &&  
  <Text>Ceci est affiché seulement si uneVariable est à true.</Text>  
}
```

## 7.2 Afficher une chose ou une autre selon une condition

L'opérateur ternaire peut être utilisé pour afficher une chose ou une autre selon une condition.

Attention : contrairement à l'affichage conditionnel avec un ET logique qui nécessite deux caractères &, il n'y a qu'un seul caractère ? ici.

```tsx title="React Native"
{uneVariable ?  
  <Text>La condition est remplie.</Text>  
:  
  <Text>La condition n'est pas remplie.</Text>  
}
```

# Exercice 2

Affichez une liste d'images disponibles à partir du Web ainsi qu'une courte description pour chacune.

1. Dans un premier temps, ces images seront contenues dans un tableau codé en dur dans votre application.
1. Prenez soin de bien découper votre application en petits composants.
1. Ajoutez des styles pour que l'apparence soit acceptable.
1. Faites en sorte qu'un clic sur une image affiche cette image en plus gros.