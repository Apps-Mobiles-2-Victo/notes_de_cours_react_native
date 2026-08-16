# 15. Formulaire React Native

# 15. Formulaire React Native

## 15.1 La validation

Dans sa plus simple expression, la validation d'un formulaire React Native consiste à vérifier la valeur d'une variable d'état à l'un de ces moments :

* lorsque cette valeur change;
* lorsque le contrôle perd le focus;
* lors de la soumission du formulaire.

Il est possible d'utiliser des composants spécialisés à cet effet, par exemple [react-hook-form](https://www.react-hook-form.com/), [Yup](https://www.npmjs.com/package/yup) ou [Formik](https://formik.org/). Cependant, effectuer la validation sans utiliser de composant externe vous aidera à mieux comprendre le processus.

## Valider un <TextInput> avec onEndEditing

Pour un composant <TextInput>, l'attribut [onEndEditing](https://reactnative.dev/docs/textinput#onendediting) est tout indiqué pour effectuer la validation. Il permet de déterminer quelle fonction sera exécutée lorsque l'usager aura terminé la saisie, c'est-à-dire lorsque le composant perdra le focus.

Il existe également un attribut [onBlur](https://reactnative.dev/docs/textinput#onblur) mais il pourrait ne pas donner correctement accès à la valeur saisie. C'est pourquoi onEndEditing est préférable pour effectuer la validation.

Quant à l'attribut [onChangeText](https://reactnative.dev/docs/textinput#onchangetext), il est appelé à chaque caractère entré ou effacé alors il causerait l'exécution du code de validation trop fréquemment.

Pour afficher les erreurs de validation, une technique intéressante consiste à initialiser une variable qui dit si la valeur entrée est valide ou non et à afficher un message seulement si la valeur n'est pas valide.

React Native

const [nom, setNom] = useState('');  
  
// la valeur null évite d'afficher le message avant que la validation n'ait eu lieu  
const [nomValide, setNomValide] = useState<boolean | null>(null);  
  
const validerNom = () => {  
  setNomValide(nom.length < 100);  
 };

 

return (  
  ...  
  <TextInput  
    value={nom}  
    onChangeText={setNom}  
    onEndEditing={validerNom}  
  />  
  {nomValide !== null && !nomValide && (  
    <Text style={styles.messageErreur}>  
      Le nom doit ne doit pas comprendre plus de 100 caractères.  
    </Text>  
  )}  
  ...  
);

## 15.2 Configurer le bon clavier virtuel

Le choix du clavier qui apparaît quand on clique sur un élément de formulaire est important pour améliorer l'expérience utilisateur.

Par exemple, pour saisir une valeur numérique :

React Native

<TextInput  
  value={age}  
  onChangeText={setAge}  
  keyboardType="numeric"  
  ...  
 />

Les différents types de claviers sont listés ici : <https://reactnative.dev/docs/0.75/textinput#keyboardtype>

Pour avoir un aperçu de chacun, consultez ce site : <https://www.lefkowitz.me/visual-guide-to-react-native-textinput-keyboardtype-options/>

## 15.3 Cacher le clavier

Dans un formulaire React Native, lorsque l'usager clique sur le bouton de soumission, il est intéressant de cacher le clavier virtuel afin de rendre tout l'espace disponible.

Voici une technique simple pour y arriver : entourer l'écran ou une partie de l'écran dans une balise <ScrollView keyboardShouldPersistTaps='never'>.

Le clavier disparaîtra dès que l'usager clique en dehors d'un TextInput, que ce soit sur le fond de l'écran ou encore sur un bouton.

React Native

<View style={styles.conteneur}>  
  <Text style={styles.titre}>Mon application</Text>  
  <ScrollView keyboardShouldPersistTaps='never'>  
    ...  
    <TextInput ... />

 

    <Button  
      onPress={() => {  
        ...  
      }}  
      title="Cliquer"  
    />  
  </ScrollView>  
 </View>