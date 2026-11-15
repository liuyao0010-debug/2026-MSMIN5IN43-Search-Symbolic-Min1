# Tester des validateurs avec SMT Strings et expressions régulières

## Groupe

**Groupe : G01**

| Membre | Contribution principale |
|---|---|
| Membre 1 | Spécifications SMT et génération automatique des données |
| Membre 2 | Implémentation des validateurs et harness de test |
| Membre 3 | Intégration, analyse et évaluation expérimentale |

## Sujet

- **Identifiant du catalogue :** D3
- **Axe :** D — Logique SAT/SMT
- **Difficulté :** 3/5
- **Technologies principales :** Python, Z3 Strings, expressions régulières (`re`).

### Problématique

Les validateurs de chaînes de caractères sont largement utilisés dans les applications informatiques pour vérifier des données telles que les adresses e-mail, les identifiants, les plaques d'immatriculation ou les numéros respectant un format particulier.

Cependant, les expressions régulières et les fonctions de validation écrites manuellement peuvent contenir des erreurs. Certains formats valides peuvent être rejetés, tandis que des formats invalides peuvent être acceptés.

L'objectif de ce projet est d'utiliser la logique SMT sur les chaînes de caractères, notamment avec Z3, pour spécifier formellement les contraintes de validation et générer automatiquement des exemples valides et invalides.

Ces exemples seront ensuite utilisés pour tester des validateurs réels développés en Python et identifier d'éventuelles divergences entre les spécifications théoriques et les implémentations.

### Objectifs

Le projet vise à :

1. Définir au moins quatre spécifications de validation à l'aide de Z3 Strings.
2. Utiliser des contraintes de longueur, de contenu et des expressions régulières.
3. Générer automatiquement des chaînes satisfaisant ou violant les spécifications.
4. Tester des validateurs réels avec les données générées.
5. Comparer les résultats attendus et les résultats obtenus.
6. Identifier et documenter les divergences éventuelles.
7. Étudier les limites de la théorie des chaînes de caractères dans Z3, notamment la décidabilité et la complexité.

### MVP (Minimum Viable Product)

La première version fonctionnelle du projet devra permettre de :

- Définir au moins quatre formats de chaînes à valider.
- Décrire leurs contraintes avec Z3.
- Générer des exemples valides et invalides.
- Exécuter les validateurs Python sur les exemples générés.
- Comparer automatiquement les résultats des spécifications SMT et des validateurs.
- Afficher un résumé des tests et des divergences détectées.

Les formats étudiés seront définis par le groupe. Les exemples de départ envisagés sont les adresses e-mail, les identifiants, les plaques d'immatriculation françaises et les numéros de sécurité sociale synthétiques.

Aucune donnée personnelle réelle ne sera utilisée.

## Architecture

Le projet sera organisé autour de quatre composants principaux.

### 1. Spécifications SMT

Chaque format sera décrit à l'aide de contraintes sur les chaînes de caractères dans Z3.

Les contraintes pourront notamment porter sur :

- La longueur minimale et maximale.
- Les caractères autorisés.
- La présence de certains caractères ou sous-chaînes.
- La structure générale du format.
- Les expressions régulières compatibles avec les fonctionnalités de Z3.

### 2. Génération automatique

À partir des spécifications, Z3 sera utilisé pour rechercher des chaînes satisfaisant ou violant certaines contraintes.

Les exemples générés seront ensuite classés selon les propriétés attendues.

### 3. Validateurs réels et tests

Des validateurs indépendants seront implémentés en Python, principalement à l'aide du module `re`.

Un harness de test automatisé exécutera ces validateurs sur les données générées et comparera leurs résultats avec les attentes définies par les spécifications SMT.

### 4. Analyse des résultats

Les résultats seront regroupés afin de mesurer le comportement des validateurs.

L'analyse portera notamment sur :

- Le nombre de chaînes générées.
- Le nombre de cas valides et invalides.
- Le nombre de cas acceptés ou rejetés correctement.
- Les divergences entre les spécifications et les implémentations.
- Les limites et difficultés rencontrées lors de l'utilisation de Z3 Strings.

## Installation

### Prérequis

- Python 3.10 ou version supérieure.
- Git.
- Visual Studio Code (recommandé).
- pip.

### Cloner le dépôt

```bash
git clone https://github.com/liuyao0010-debug/2026-MSMIN5IN43-Search-Symbolic-Min1.git
cd 2026-MSMIN5IN43-Search-Symbolic-Min1/groupe-g01
```

### Créer un environnement virtuel

Sous Windows :

```powershell
python -m venv .venv
.venv\Scripts\Activate.ps1
```

Sous Linux ou macOS :

```bash
python3 -m venv .venv
source .venv/bin/activate
```

### Installer les dépendances

```bash
pip install -r requirements.txt
```

Le fichier `requirements.txt` sera complété avec les dépendances effectivement utilisées dans le projet.

## Utilisation

L'utilisation prévue du projet suivra les étapes suivantes :

1. Charger les spécifications SMT.
2. Générer des chaînes de caractères valides et invalides.
3. Exécuter les validateurs Python.
4. Comparer les résultats.
5. Afficher ou exporter le rapport expérimental.

**Commande prévue :**

```bash
python src/main.py
```

Cette commande sera disponible une fois le point d'entrée principal implémenté.

## Tests

Les tests automatisés seront développés avec `pytest`.

Pour installer l'outil :

```bash
pip install pytest
```

Pour exécuter l'ensemble des tests :

```bash
pytest
```

Les tests couvriront notamment :

- La cohérence des spécifications SMT.
- La génération de chaînes valides et invalides.
- Le comportement des validateurs Python.
- La comparaison entre les résultats attendus et obtenus.
- Les cas limites et les divergences potentielles.

## Résultats et évaluation

Les expériences permettront d'évaluer la capacité de Z3 à générer des exemples utiles pour tester des validateurs de chaînes de caractères.

Les résultats seront analysés selon plusieurs critères :

| Critère | Description |
|---|---|
| Couverture | Nombre et diversité des cas de test générés |
| Validité | Conformité des exemples aux spécifications |
| Détection | Nombre de divergences identifiées |
| Performance | Temps nécessaire à la génération et à la validation |
| Robustesse | Comportement sur les cas limites |

Les résultats expérimentaux, les exemples intéressants et les éventuelles erreurs détectées seront présentés dans un rapport et/ou un notebook explicatif.

## Limites et plan B

### Limites

L'utilisation de Z3 Strings présente plusieurs difficultés :

- Certaines expressions régulières ne sont pas directement compatibles entre Z3 et Python.
- La résolution de contraintes complexes peut nécessiter un temps de calcul important.
- Certaines requêtes peuvent retourner `unknown` lorsque le solveur ne parvient pas à déterminer une réponse.
- Les contraintes SMT doivent représenter correctement les formats étudiés pour que les résultats soient pertinents.

Il est donc nécessaire de distinguer les limites du solveur des erreurs présentes dans les validateurs testés.

### Plan B

Si certaines contraintes sont trop complexes à résoudre :

- Simplifier les spécifications tout en conservant les propriétés essentielles.
- Réduire la complexité des expressions régulières.
- Utiliser des contraintes de longueur et de contenu plus simples.
- Limiter les expériences à des formats représentatifs.
- Documenter les cas non résolus et leurs causes.

## Répartition du travail

Le projet est divisé en trois parties complémentaires.

### Membre 1 — Spécifications SMT et génération des données

- Développer les spécifications déclaratives avec Z3 Strings.
- Implémenter les contraintes de longueur, de contenu et d'expressions régulières.
- Développer la génération automatique d'entrées valides et invalides.
- Prendre en charge deux validateurs.
- Tester et documenter le fonctionnement du générateur.

### Membre 2 — Implémentation des validateurs et harness de test

- Développer les validateurs réels avec Python `re` ou des parseurs.
- Prendre en charge deux validateurs.
- Implémenter le harness de test automatisé.
- Comparer les résultats des spécifications SMT et des implémentations réelles.
- Identifier, reproduire et documenter les divergences détectées.

### Membre 3 — Intégration, analyse et évaluation expérimentale

- Intégrer les différents composants dans une application cohérente.
- Développer le point d'entrée et le processus global d'exécution.
- Définir et calculer les métriques expérimentales.
- Produire des rapports et des visualisations des résultats.
- Étudier les limites de la théorie des chaînes dans Z3, notamment la décidabilité et la complexité.

### Collaboration

Les trois membres participeront aux tests d'intégration, à la revue de code, à la documentation et à la préparation de la soutenance.

Les contributions individuelles seront suivies à travers les commits Git et les Pull Requests.

## Licence et références

### Références

- [Dépôt du cours CoursIA](https://github.com/jsboige/CoursIA)
- [Notebook Z3 — Strings et Regex avec Python](https://github.com/jsboige/CoursIA/blob/main/MyIA.AI.Notebooks/SymbolicAI/SMT/Z3-API/Z3-04-Strings-Regex-Python.ipynb)
- [Notebook Z3 — Tactics](https://github.com/jsboige/CoursIA/blob/main/MyIA.AI.Notebooks/SymbolicAI/SMT/Z3-API/Z3-03-Tactics-Python.ipynb)
- Czerwinski et al., *Strings in SMT* (2021).
- [Documentation officielle de Z3](https://microsoft.github.io/z3guide/)

### Licence

Le projet est réalisé dans le cadre du cours MSMIN5IN43 — Intelligence Artificielle II à l'EPF.

Les conditions de réutilisation du code suivent la licence du dépôt pédagogique principal.