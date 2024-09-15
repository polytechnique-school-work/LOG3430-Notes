## Introduction - Méthodes de Test et de Validation du Logiciel

**1. Introduction aux Méthodes de Test**
- Les tests visent à augmenter la confiance dans le bon fonctionnement du logiciel. Ils permettent de détecter les défauts et de vérifier la conformité du logiciel par rapport aux exigences.
- **Exemple** : Un test pour vérifier que l'utilisateur peut se connecter à une application avec des identifiants valides.

**2. Types de Tests**
- **Tests Unitaires** : Vérifient les unités individuelles de code, telles que des fonctions ou des méthodes en isolation. Ils se focalisent sur le comportement attendu des petites unités de code.
  - **Exemple** : Tester une fonction `additionner` qui prend deux nombres en entrée et retourne leur somme.
- **Tests d'Intégration** : Vérifient les interactions entre plusieurs unités combinées. Ils identifient les problèmes d'interface entre les différentes unités.
  - **Exemple** : Tester une méthode `soumettreCommande` qui appelle des fonctions pour vérifier le stock, calculer le total et enregistrer la commande.
- **Tests Fonctionnels (ou de Système)** : Vérifient que le système complet exécute les fonctionnalités selon les attentes spécifiées dans les exigences.
  - **Exemple** : Tester l'ensemble du processus d'achat sur un site de commerce électronique, de la sélection des produits au paiement.
- **Tests d'Acceptation** : Validés par les utilisateurs finaux pour s'assurer que le logiciel répond parfaitement à leurs besoins et attentes.
  - **Exemple** : Tester une application par des utilisateurs finaux pour vérifier qu'elle remplit toutes les exigences de leur cahier des charges.

**3. Stratégies de Test**
- **Boîte Blanche** : Basée sur le code source, cette stratégie examine la structure interne du logiciel pour concevoir des cas de test. Elle permet de vérifier des chemins spécifiques du code.
  - **Exemple** : Tester toutes les branches d'une condition if-else pour s'assurer que chaque chemin est correctement exécuté.
- **Boîte Noire** : Basée sur les spécifications du logiciel, cette stratégie conçoit des cas de test sans aucune connaissance interne du code. Les tests se basent sur les fonctionnalités attendues.
  - **Exemple** : Tester les différentes entrées possibles dans un formulaire pour valider que le traitement des données respecte les spécifications.

**4. Automatisation des Tests**
- **Importance de l'automatisation** : Elle permet d'exécuter des tests répétitifs et complexes de manière efficace et en un temps réduit.
  - **Exemple** : Utiliser un script automatisé pour exécuter des tests de régression à chaque nouvelle version du logiciel.
- **Outils et Frameworks** : Utilisation d'outils comme Docker et Docker Compose pour créer des environnements de test standardisés et isolés.
  - **Exemple** : Conteneuriser une application dans Docker afin de garantir des conditions de test identiques à chaque exécution.

**5. Oracles de Test**
- **Oracle de Test** : Un oracle de test est utilisé pour déterminer si le résultat d'un test est correct. Il compare les résultats obtenus avec les résultats attendus.
  - **Exemple** : Un test qui vérifie si la fonction `calculerRemise` retourne 10% de remise pour des achats dépassant 100€.
- **Types d'Oracles** :
  - **Spécifiés** : Utilisent des exigences formelles pour juger de l'exactitude.
    - **Exemple** : Comparer la sortie d’une fonction avec les valeurs spécifiées dans un document de requis.
  - **Dérivés** : Utilisent les artefacts existants du projet, comme des versions antérieures du logiciel, pour dériver les oracles.
    - **Exemple** : Utiliser la sortie d’une version précédente d'un programme comme référence pour une nouvelle version.
  - **Implicites** : Vérifient des propriétés générales comme l'absence de plantage ou de fuites mémoire.
    - **Exemple** : Tester un programme pour s’assurer qu’il ne plante pas avec des entrées invalides.
  - **Humains** : Les jugements sont faits manuellement par des testeurs humains.
    - **Exemple** : Un testeur humain vérifie visuellement le rendu graphique d'une interface utilisateur.

**6. Problèmes et Solutions Liées aux Oracles de Test**
- **Problème d'Oracle** : Il est difficile de vérifier automatiquement les résultats pour de nombreux tests, surtout avec des sorties complexes ou non déterministes.
- **Techniques pour Réduire le Coût de Vérification** : 
  - **Réduction des Suites de Tests** : Enlever des tests redondants pour éviter de tester plusieurs fois la même fonctionnalité.
    - **Exemple** : Éliminer plusieurs tests unitaires qui couvrent la même logique de code.
  - Qualitative : Générer des tests qui sont logiques et compréhensibles pour faciliter l'évaluation.
    - **Exemple** : Utiliser des profils utilisateurs pour générer des scénarios de test plus réalistes.

**7. Méthodes de Tests Avancées**
- **Tests de Mutations** : Injectent des défauts artificiels dans le logiciel pour voir si les tests existants les détectent.
  - **Exemple** : Modifier une condition dans le code (`>` en `<`) et vérifier si les tests détectent le changement.
- **Fuzzing** : Génère des entrées aléatoires pour découvrir des anomalies implicites, utilisé surtout pour la sécurité.
  - **Exemple** : Injecter des chaînes aléatoires dans une fonction de manipulation de texte pour détecter des défaillances inattendues.
- **Tests Métamorphiques** : Utilisent des propriétés métamorphiques entre les tests pour générer des résultats attendus.
  - **Exemple** : Si une fonction de tri est correcte, alors ses résultats devraient être les mêmes si les mêmes données sont triées plusieurs fois.

**8. Localisation des Défauts**
- Utilisation de données historiques pour identifier et localiser les défauts dans le logiciel. Cela inclut l'analyse des défaillances passées pour prédire et prévenir les futurs défauts.
  - **Exemple** : Analyser les logs historiques pour déterminer les modules les plus susceptibles de contenir des bogues.

En résumé, les tests de logiciels sont cruciaux pour détecter les défauts et s'assurer que le logiciel répond aux attentes. Les différentes stratégies et niveaux de tests, l'importance de l'automatisation, et les techniques pour gérer les oracles et les tests avancés permettent de créer des processus de tests efficaces et fiables.

### Définitions et Différences : Erreur, Défaut, Défaillance

**Erreur (Error)** :
- **Définition** : Une action humaine incorrecte.
- **Exemple** : Un développeur code une boucle infinie par erreur.

**Défaut (Fault, Bug)** :
- **Définition** : Manifestation concrète d'une erreur dans le code ou la conception.
- **Exemple** : Un bogue fait que la fonction de remise retourne toujours zéro.

**Défaillance (Failure)** :
- **Définition** : Mauvais fonctionnement du logiciel causé par l'exécution d'un défaut.
- **Exemple** : Le système plante lorsque la remise est appliquée.

### Relations
- **Erreur** crée un **défaut**.
- **Défaut** conduit à une **défaillance** lorsque exécuté.

### Schéma
```plaintext
Erreur (Error) → Défaut (Fault, Bug) → Défaillance (Failure)
```

### Exemple Concret
1. **Erreur** : Mauvaise interprétation d'une spécification.
2. **Défaut** : Calcul incorrect de la remise.
3. **Défaillance** : Montant de remise erroné observé par l'utilisateur.

### Importance
- **Défaut** latent peut causer des **défaillances** graves (ex. : Gotofail d'Apple en 2014 causant un défaut de SSL).

En résumé, une **erreur** humaine engendre un **défaut**, et l'exécution de ce défaut cause une **défaillance** dans le logiciel.