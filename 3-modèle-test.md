## Modèle de test - Méthodes de Test et de Validation du Logiciel

### Héritage

Il est important de prendre en compte que les tests qui sont fait pour les parents ne sont pas suffisants pour les enfants. Il faut donc faire des **nouveaux tests pour les enfants**.

#### Modèle de test abstrait

Ce modèle permet de créer une suite de tests qui peut être réutilisée entre les descendants.

**Règles de test abstrait**

1. Un test abstrait doit avoir des cas de test qui ne peuvent pas être remplacés et il doit y avoir une méthode de abstract factory qui créer des instances de la classe à tester.

2. Écrire une classe de test concrète pour chaque implémentation de l'interface (ou classe abstraite). La classe de test concrète doit étendre la classe de test abstraite et implémenter la méthode factory. On peut ajouter plus de test à la nouvelle classe de test concrète qui sont spécifiques à sa mise en oeuvre.

#### Mocking

Le mocking est une technique qui permet de remplacer un objet par un objet simulé (mock) qui a le même comportement que l'objet original.

Pourquoi rédiger des Mock/Fakes/Stubs ? 

- Pour des parties du système qui ne sont pas implémentés.
- La simulation de dispositifs matériels.

*Attention*: Ne pas faire un stub qui fait toute la logique de l'objet mais uniquement simuler le comportement de l'objet (sinon ça sert juste à rien...).

<hr>

# Différences entre Mock, Stub et Fake

Les termes **mock**, **stub**, et **fake** sont couramment utilisés en développement logiciel et dans les tests unitaires pour décrire différents types d'objets de test. Ils permettent de remplacer des composants réels pour tester une unité de code en isolation. Voici leurs différences :

## 1. Stub
- **Usage** : Un stub est un objet qui fournit des réponses prédéfinies pour des appels de méthodes lors des tests.
- **But** : Utilisé pour remplacer une dépendance dont le comportement est simple et prévisible.
- **Exemple d’utilisation** : Si vous testez une méthode qui dépend d’un service externe, un stub peut retourner une réponse spécifique sans appeler réellement le service.

```js
const serviceStub = {
    getData: () => 'data statique'
};
```

## 2. Mock
- **Usage** : Un mock est un objet qui enregistre des informations sur la manière dont il a été utilisé (quelles méthodes ont été appelées, avec quels arguments) et permet de vérifier les interactions.
- **But** : Utilisé pour vérifier si certaines méthodes ont été appelées ou non, et avec les bons paramètres.
- **Exemple d’utilisation** : Lors de tests, vous pouvez vérifier qu’une méthode a été appelée un certain nombre de fois ou avec certains arguments.

```js
const mockService = {
    getData: jest.fn()
};

// Interaction dans le test
mockService.getData();

// Vérification
expect(mockService.getData).toHaveBeenCalledTimes(1);
```

## 3. Fake
- **Usage** : Un fake est un objet qui a une implémentation simple mais fonctionnelle, qui simule le vrai comportement d'un composant sans en être une version complète.
- **But** : Utilisé lorsqu'une implémentation réelle est nécessaire pour le test, mais que la version complète serait trop complexe ou non disponible.
- **Exemple d’utilisation** : Un fake pourrait être une base de données en mémoire qui stocke des données temporairement au lieu d’utiliser une vraie base de données.

```js
class InMemoryDatabase {
    constructor() {
        this.data = [];
    }

    insert(record) {
        this.data.push(record);
    }

    getAll() {
        return this.data;
    }
}

const fakeDB = new InMemoryDatabase();
```

## Différences clés
- **Stub** : Fournit des réponses prédéfinies pour les tests.
- **Mock** : Permet de vérifier des interactions entre composants (e.g., méthodes appelées, arguments).
- **Fake** : Remplace un composant avec une implémentation légère mais fonctionnelle.

<hr>

### Drivers

Un driver permet de remplacer un humain qui effectuerai des actions sur le logiciel. Ex: Selenium, Dear Imgui Test engine, etc.

### MaDUM (Minimal Data-member Usage Matrix)

On veut catégoriser les méthodes afin de déterminer dans quel ordre exécuter les tests.

1. **Tester les rapporteurs (getters)** en premier : ils ne devraient pas avoir d'impact sur l'état de l'objet.
2. **Tester les constructeurs** ensuite : ils sont un passage obligé pour l'utilisation de l'objet.
3. **Tester les transformateurs** ensuite : ce sont eux qui changent l'objet et qui sont donc susceptibles de l'amener à un état incohérent.
4. **Tester les autres** à n'importe quel moment : une méthode autre peut être testée de manière indépendante des autres méthodes.

Le MaDUM (Minimal Data-member Usage Matrix) est une méthode utilisée pour mieux comprendre comment les attributs d'une classe interagissent avec ses méthodes, dans le cadre d'un développement orienté objet. Voici comment l'utiliser et l'appliquer étape par étape :

### Objectif
MaDUM aide à identifier comment chaque attribut est manipulé par les méthodes d'une classe. Cela permet de détecter des problèmes d'architecture comme le couplage fort, les violations d'encapsulation, et autres problèmes de gestion d'état.

### Étapes pour utiliser MaDUM

1. **Identification des Attributs et Méthodes :**
   - Recensez tous les attributs (variables membres) et méthodes de la classe que vous souhaitez analyser.

2. **Construction de la Matrice :**
   - Créez une matrice où chaque ligne représente un attribut et chaque colonne représente une méthode.
   - Pour chaque cellule de la matrice, déterminez comment la méthode utilise l'attribut correspondant.

3. **Classification des Usages :**
   - **C (Construction)** : La méthode initialise ou est un constructeur qui initialise l'attribut.
   - **R (Rapport)** : La méthode lit ou accède simplement à l'attribut (getters).
   - **T (Transformation)** : La méthode modifie l'attribut (setters).
   - **O (Autre)** : La méthode utilise l'attribut de manière différente (par exemple, une méthode de calcul complexe sans modification directe).

4. **Remplissage de la Matrice :**
   - Analysez chaque méthode et notez dans la matrice le type d'interaction qu'elle a avec chaque attribut. Par exemple, si une méthode modifie un attribut, notez "T" dans la cellule correspondante.

5. **Analyse des Résultats :**
   - Examinez la matrice pour déceler des patterns problématiques, comme des attributs trop souvent modifiés (peut indiquer un couplage excessif) ou rarement accédés de manière sécurisée (violation potentielle de l'encapsulation).

### Application des Tests avec MaDUM

Pour appliquer les tests de manière efficace selon l'approche MaDUM, voici l'ordre recommandé :

1. **Test des Rapporteurs (Getters) :**
   - Commencez par tester les méthodes qui accèdent aux champs sans les modifier. Cela garantit qu’elles n’impactent pas l’état de l’objet.

2. **Test des Constructeurs :**
   - Testez ensuite les constructeurs pour s’assurer qu’ils initialisent l’objet correctement. Chaque attribut doit être vérifié pour s'assurer de sa bonne initialisation.

3. **Test des Transformateurs (Setters) :**
   - Poursuivez avec les méthodes qui modifient les attributs. Vérifiez qu’elles mènent l’objet vers un état valide après leur exécution.

4. **Test des Méthodes Autres :**
   - Finalement, testez les autres méthodes à votre convenance. Ces méthodes doivent être évaluées pour leur logique individuelle et impact global.

Pour appliquer efficacement des tests à l'aide de la méthode MaDUM, il est important d'adopter une approche structurée qui tient compte des différents types de méthodes et de leur interaction avec les attributs de la classe. Voici un guide détaillé pour réaliser ces tests :

### Étapes pour Test MaDUM

1. **Préparation des Tests :**
   - Identifiez tous les attributs et méthodes de la classe.
   - Remplissez la matrice MaDUM pour avoir une vue d'ensemble de l'interaction entre chaque attribut et méthode.

2. **Tester les Méthodes de Rapport (Getters) :**
   - **Objectif :** Vérifier que les getters retournent les valeurs correctes des attributs sans les modifier.
   - **Stratégie :**
     - Pour chaque getter, initialisez l'objet avec des valeurs définies pour ses attributs.
     - Utilisez le getter pour récupérer la valeur de l'attribut et validez qu'elle correspond à celle attendue.
     - Exemple : Si un objet `Voiture` a un attribut `couleur` et une méthode `getCouleur()`, après avoir initialisé `Voiture` avec `couleur = "rouge"`, vérifiez que `getCouleur()` retourne `"rouge"`.

3. **Tester les Constructeurs :**
   - **Objectif :** S'assurer que les constructeurs initialisent l'objet à un état cohérent et valide.
   - **Stratégie :**
     - Créez des instances de l'objet en utilisant divers scénarios d'initialisation.
     - Vérifiez que chaque attribut est correctement initialisé à sa valeur prévue.
     - Exemple : Pour un constructeur de `Voiture` qui initialise `couleur`, `modèle`, assurez-vous que ces attributs sont correctement définis après l'instanciation.

4. **Tester les Méthodes de Transformation (Setters) :**
   - **Objectif :** Assurer que les setters modifient correctement les attributs tout en respectant les règles métiers.
   - **Stratégie :**
     - Initialisez un objet et modifiez ses attributs à l'aide des setters.
     - Vérifiez que les setters changent les valeurs des attributs comme attendu et que l'état de l'objet reste valide après leurs appels.
     - Exemple : Si `Voiture` a `setCouleur(c)`, modifiez la couleur et validez le changement avec `getCouleur()`.

5. **Tester les Méthodes Autres :**
   - **Objectif :** Vérifier le comportement logique de ces méthodes, souvent dépendent directement ou indirectement des attributs.
   - **Stratégie :**
     - Testez chaque méthode pour s'assurer qu'elle produit les résultats attendus.
     - Incluez des tests pour voir comment ces méthodes interagissent avec les getters et setters, surtout si elles dépendent d'eux pour fonctionner.
     - Exemple : Une méthode `calculerVitesse()` devrait être testée pour sa sortie correcte donnée une certaine configuration d'attributs.

### Considérations supplémentaires

- **Tests de Boundaries et Edge Cases** : Pour chaque test, incluez des scénarios de frontières pour vérifier que l'application peut gérer des valeurs extrêmes sans erreur.
- **Mocking et Stubbing** : Utilisez des techniques de simulation pour tester des méthodes qui dépendent de composants externes ou d'autres classes.
- **Automatisation** : Intégrez ces tests dans une suite de tests automatisés pour garantir leur exécution régulière à chaque modification du code.

En suivant ces étapes, MaDUM aide non seulement à structurer et prioriser les tests, mais aussi à assurer que la classe fonctionne correctement et de manière fiable dans différents scénarios d'utilisation.

### Conclusion

L'approche MaDUM fournit une manière structurée et systématique de comprendre et tester l'utilisation des attributs d'une classe. Elle aide à identifier les problèmes potentiels dans le code source, facilitant ainsi la maintenance et la robustesse du logiciel. Ce processus nécessite de la rigueur, mais il offre des bénéfices significatifs en termes de qualité du code et de son évolutivité.

<hr>

### Contexte et Limites de la Couverture de Code
- La couverture de code vérifie quelles parties du code ont été exécutées par des tests, sans garantir que les erreurs sont identifiées.
- Cela signifie que même une couverture de code de 100 % ne prouve pas que tous les défauts ont été détectés.

### Introduction aux Tests de Mutation
- Les tests de mutation introduisent intentionnellement des défauts, appelés mutants, dans le code pour évaluer l'efficacité des tests.
- L'objectif est de voir si les tests actuels peuvent détecter ces changements délibérément introduits dans le code.

### Processus de Test de Mutation
- **Création de Mutants :** On crée des versions modifiées du programme original, chacune contenant un défaut spécifique.
- **Exécution des Tests :** Les tests existants sont exécutés sur ces mutants.
- **Évaluation :** Si les tests échouent pour un mutant, il est "tué", confirmant l'efficacité des tests. Si un mutant "survit", cela peut indiquer un problème avec les tests.

### Types de Mutants
1. **Mutants Équivalents :** Ceux-ci se comportent de manière identique au code original, rendant leur détection impossible à travers des tests. Ils représentent un défi particulier car ils ne sont pas révélateurs d'insuffisances de la suite de tests.
2. **Mutants Incomplets :** Ceux qui survivent parce que les tests ne couvrent pas suffisamment le code ou ne ciblent pas correctement les défauts.
3. **Mutants Mort-nés ("Stillborn"):** Défauts syntaxiques immédiatement éliminés par le compilateur.
4. **Mutants Triviaux:** Défauts évidents rapidement détectés par des tests de base.

### Exemples de Mutations
- **Changer d’Opérateurs Logiques :** Remplacer `&&` par `||`.
- **Modifier des Conditions :** Transformer `a > b` en `a >= b`.
- **Altérer des Opérateurs Arithmétiques :** Substituer `+` par `-`.
- **Modifier des Constantes :** Échanger la valeur de `3` avec `0`.
- **Changer d’Variables :** Utiliser une autre variable de même type à la place.
- **Modifier des Opérateurs d'Incrémentation :** Passer de `i++` à `i--`.
- **Changer de Négation :** Passer de `a` à `-a`.

### Calcul et Analyse
- **Score de Mutation :** Ce score est calculé en fonction du nombre de mutants tués par rapport au total, souvent ajusté pour tenir compte des mutants équivalents :
  - Score = 100 × (Mutants tués) / (Mutants totaux - Mutants équivalents)

### Avantages et Considérations
- **Complémentarité :** Les tests de mutation devraient être utilisés parallèlement aux techniques de test conventionnelles pour améliorer la robustesse de la suite de tests.
- **Calcul Intensif :** La génération et le test de nombreux mutants peuvent être coûteux en ressources et en temps.
- **Ciblage Clair :** Cette approche aide à donner une direction claire aux développeurs, en mettant en évidence les parties du code moins bien testées.

### Conclusion
Les tests de mutation fournissent une approche rigoureuse pour évaluer la qualité des suites de tests en introduisant des défauts artificiels et mesurant la capacité des tests à les détecter. Malgré les défis, notamment liés aux mutants équivalents et aux ressources nécessaires, ils représentent un outil puissant pour améliorer l'efficacité des tests.
