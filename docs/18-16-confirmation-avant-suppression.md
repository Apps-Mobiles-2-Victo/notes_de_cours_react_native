# 16. Confirmation avant suppression

# 16. Confirmation avant suppression

## 16.1 <Alert>

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