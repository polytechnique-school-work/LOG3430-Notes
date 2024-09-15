## Test en boîte blanche - Méthodes de Test et de Validation du Logiciel

### Graphe de flot de contrôle (CLG)

Graphe orienté, utilisé pour représenter le flux de contrôle d'un programme.

**Exemple** :

![alt text](image.png)

## Type de couverture

**Couverture de tous les chemins** : Couvre tous les nœuds du CLG. Sauf que c'est très long et coûteux en temps, voir impossible pour des programmes complexes.

**Couverture des instructions** : Couvre toutes les instructions du programme (en gros c'est notre 100% code coverage).

**Couverture des branches** : Couvre toutes les branches du programme.

Branch coverage = # de branches couvertes / # de branches totales

**Couverture des conditions (ou de décisions)** : Couvre toutes les conditions du programme.

Condition coverage = # de conditions couvertes (true/false) / # de conditions totales

**MC/DC (Multiple Condition/Decision Coverage)** : Couvre toutes les combinaisons de conditions. En gros, on fixe toutes les variables sauf une et on vérifie que toutes les combinaisons sont testées.

### Hiérarchie des tests

![alt text](image-1.png)

## Graph de flot de données (DLG)

Se concentre sur les points où les entrées et les sorties sont utilisées.

![alt text](image-2.png)
