# Tester des validateurs avec SMT Strings et expressions régulières

## Groupe

**Groupe : G01**

| Membre | Contribution principale |
|---|---|
| Membre 1 | Spécifications SMT et génération automatique des données |
| Membre 2 | Implémentation des validateurs Python et tests différentiels |
| Membre 3 | Intégration, analyse et évaluation expérimentale |

## Sujet

- **Identifiant du catalogue :** D3
- **Axe :** D — Logique SAT/SMT
- **Difficulté :** 3/5
- **Technologies principales :** Python, Z3 Strings, expressions régulières (`re`), pytest.

## 1. Problématique

Les validateurs de chaînes de caractères sont largement utilisés dans les applications informatiques pour vérifier des données telles que les adresses e-mail, les identifiants, les plaques d'immatriculation ou les numéros respectant un format particulier.

Cependant, les expressions régulières et les fonctions de validation écrites manuellement peuvent contenir des erreurs. Certains formats valides peuvent être rejetés, tandis que des formats invalides peuvent être acceptés.

L'objectif de ce projet est d'utiliser la logique SMT sur les chaînes de caractères, notamment avec Z3, pour spécifier formellement les contraintes de validation et générer automatiquement des exemples valides et invalides.

Ces exemples seront ensuite utilisés pour tester des validateurs réels développés en Python et identifier d'éventuelles divergences entre les spécifications théoriques et les implémentations.

### Exemple de divergence

Considérons un identifiant soumis aux contraintes suivantes :

- Longueur comprise entre 6 et 10 caractères.
- Premier caractère obligatoirement alphabétique.
- Utilisation exclusive de lettres et de chiffres.

| Entrée | Résultat attendu | Résultat du validateur |
|---|---|---|
| `ABC123` | Valide | Accepté |
| `123ABC` | Invalide | Rejeté |
| `AB@123` | Invalide | Accepté (exemple de bug) |

Dans ce dernier cas, le validateur accepte une chaîne qui ne respecte pas la spécification formelle. Ce type de divergence constitue l'un des principaux problèmes étudiés dans ce projet.

## 2. Objectifs

Le projet vise à :

- Définir au moins quatre spécifications de validation à l'aide de Z3 Strings.
- Utiliser des contraintes de longueur, de contenu et des expressions régulières.
- Générer automatiquement des chaînes satisfaisant ou violant les spécifications.
- Tester des validateurs réels avec les données générées.
- Comparer les résultats attendus et les résultats obtenus.
- Identifier les faux positifs et les faux négatifs.
- Mesurer les performances et la couverture des tests.
- Étudier les limites pratiques de la théorie des chaînes de caractères dans Z3, notamment les temps de résolution et les résultats `unknown`.

### Types de divergences étudiés

| Type | Spécification SMT | Validateur Python | Interprétation |
|---|---|---|---|
| Faux positif (False Positive) | Invalide | Accepté | Une entrée invalide est acceptée à tort. |
| Faux négatif (False Negative) | Valide | Rejeté | Une entrée valide est rejetée à tort. |
| Vrai positif (True Positive) | Valide | Accepté | Une entrée valide est correctement acceptée. |
| Vrai négatif (True Negative) | Invalide | Rejeté | Une entrée invalide est correctement rejetée. |

## 3. MVP (Minimum Viable Product)

La première version fonctionnelle du projet devra permettre de :

1. Définir au moins quatre formats de chaînes à valider.
2. Décrire leurs contraintes à l'aide de Z3.
3. Générer automatiquement des exemples valides et invalides.
4. Exécuter les validateurs Python sur les exemples générés.
5. Comparer automatiquement les résultats SMT et Python.
6. Identifier et compter les divergences.
7. Afficher un résumé des tests et des résultats expérimentaux.

Les formats étudiés seront définis par le groupe. Les exemples envisagés sont :

- Adresses e-mail.
- Identifiants.
- Plaques d'immatriculation françaises.
- Numéros de sécurité sociale synthétiques, uniquement pour l'étude de leur format.

Aucune donnée personnelle réelle ne sera utilisée.

## 4. Architecture

Le projet sera organisé autour de quatre composants principaux.

### 4.1. Spécifications SMT

Chaque format sera décrit à l'aide de contraintes sur les chaînes de caractères dans Z3.

Les contraintes pourront notamment porter sur :

- La longueur minimale et maximale.
- Les caractères autorisés.
- La présence de certains caractères ou sous-chaînes.
- La structure générale du format.
- Les expressions régulières compatibles avec les fonctionnalités de Z3.

### 4.2. Génération automatique

À partir des spécifications, Z3 sera utilisé pour rechercher des chaînes satisfaisant ou violant certaines contraintes.

La génération des exemples invalides pourra notamment s'appuyer sur une mutation ciblée des chaînes valides :

- Modification de la longueur.
- Introduction de caractères interdits.
- Suppression d'un caractère obligatoire.
- Modification de la structure attendue.

Chaque exemple généré sera associé à son format et à son résultat attendu.

### 4.3. Validateurs Python et tests différentiels

Des validateurs indépendants seront implémentés en Python, principalement à l'aide du module `re`.

Un harness de test automatisé exécutera ces validateurs sur les données générées.

Les résultats seront comparés aux attentes définies par les spécifications SMT afin d'identifier :

- Les faux positifs.
- Les faux négatifs.
- Les cas correctement acceptés.
- Les cas correctement rejetés.

### 4.4. Analyse des résultats

Les résultats seront regroupés afin de mesurer le comportement des validateurs.

L'analyse portera notamment sur :

- Le nombre total de chaînes générées.
- Le nombre de cas valides et invalides.
- Le nombre de faux positifs et de faux négatifs.
- Le taux de divergence.
- Les temps de génération, de résolution et de validation.
- Les limites et difficultés rencontrées lors de l'utilisation de Z3 Strings.

### 4.5. Architecture globale

Le processus général du projet suivra le flux suivant :

```text
        Spécifications SMT (Z3)
                  |
                  v
       Génération automatique
          de chaînes de test
                  |
          +-------+-------+
          |               |
          v               v
    Résultat attendu   Chaînes générées
       par SMT               |
                             v
                   Validateurs Python
                             |
                             v
                    Tests différentiels
                             |
                             v
                    Analyse des résultats
                             |
                             v
                  Rapports et visualisations
```

### 4.6. Format des données échangées

Afin de faciliter l'intégration des différents composants, les données de test utiliseront un format commun, par exemple JSON ou JSONL.

Exemple :

```json
{
  "format": "identifier",
  "input": "ABC123",
  "expected": true,
  "violated_constraint": null
}
```

Exemple de chaîne invalide :

```json
{
  "format": "identifier",
  "input": "AB@123",
  "expected": false,
  "violated_constraint": "invalid_character"
}
```

Chaque composant devra respecter cette structure commune afin de permettre une communication cohérente entre le générateur, les validateurs et le module d'analyse.

## 5. Installation

### 5.1. Prérequis

- Python 3.10 ou version supérieure.
- Git.
- pip.
- Visual Studio Code (recommandé).

### 5.2. Cloner le dépôt

```bash
git clone https://github.com/liuyao0010-debug/2026-MSMIN5IN43-Search-Symbolic-Min1.git

cd 2026-MSMIN5IN43-Search-Symbolic-Min1/groupe-g01
```

### 5.3. Créer un environnement virtuel

**Sous Windows (PowerShell) :**

```powershell
python -m venv .venv
.venv\Scripts\Activate.ps1
```

**Sous Linux ou macOS :**

```bash
python3 -m venv .venv
source .venv/bin/activate
```

### 5.4. Installer les dépendances

```bash
pip install -r requirements.txt
```

Les principales dépendances prévues sont :

- `z3-solver` : résolution des contraintes SMT sur les chaînes.
- `pytest` : exécution des tests automatisés.
- `pandas` : traitement et analyse des résultats.
- `matplotlib` : visualisation des résultats.

Le fichier `requirements.txt` sera maintenu à jour avec les dépendances effectivement utilisées.

## 6. Utilisation

Le point d'entrée principal prévu pour lancer le projet est :

```bash
python src/main.py
```

Il permettra d'exécuter l'ensemble du processus :

1. Chargement des spécifications SMT.
2. Génération des chaînes de caractères.
3. Exécution des validateurs Python.
4. Comparaison des résultats.
5. Analyse des divergences.
6. Génération du rapport expérimental.

Les instructions définitives d'exécution seront mises à jour au fur et à mesure de l'implémentation.

## 7. Tests

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
- L'intégration des différents composants.

Chaque module devra disposer de tests adaptés à ses responsabilités.

## 8. Résultats et évaluation

Les expériences permettront d'évaluer la capacité de Z3 à générer des exemples utiles pour tester des validateurs de chaînes de caractères.

Les résultats seront analysés selon les critères suivants :

| Critère | Description |
|---|---|
| Couverture | Nombre et diversité des cas de test générés. |
| Validité | Conformité des exemples aux spécifications SMT. |
| Détection | Nombre de faux positifs et de faux négatifs identifiés. |
| Taux de divergence | Proportion de résultats différents entre SMT et Python. |
| Performance | Temps de génération, de résolution et de validation. |
| Robustesse | Comportement sur les cas limites et les entrées inhabituelles. |

Le taux de divergence sera notamment calculé selon la formule :

\[
Taux\ de\ divergence =
\frac{Nombre\ de\ divergences}{Nombre\ de\ tests\ comparables}
\times 100
\]

Les résultats `unknown` de Z3 seront comptabilisés séparément et ne seront pas considérés comme des entrées valides ou invalides.

Les résultats expérimentaux, les exemples intéressants et les éventuelles erreurs détectées seront présentés dans un rapport et/ou un notebook explicatif.

## 9. Limites et plan B

### 9.1. Limites

L'utilisation de Z3 Strings présente plusieurs difficultés :

- Certaines expressions régulières ne sont pas directement compatibles entre Z3 et Python.
- La théorie des chaînes de caractères peut entraîner des temps de résolution importants pour certaines contraintes.
- Certaines requêtes peuvent retourner `unknown` lorsque le solveur ne parvient pas à déterminer une réponse.
- Les contraintes SMT doivent représenter correctement les formats étudiés pour que les résultats soient pertinents.
- Les différences de sémantique entre les expressions régulières de Z3 et Python peuvent produire des divergences qui ne correspondent pas nécessairement à des bugs des validateurs.

Il sera donc nécessaire de définir un sous-ensemble commun d'expressions régulières et de distinguer les limites du solveur des erreurs présentes dans les validateurs testés.

### 9.2. Plan B

Si certaines contraintes sont trop complexes à résoudre :

- Simplifier les spécifications tout en conservant les propriétés essentielles.
- Réduire la complexité des expressions régulières.
- Utiliser des contraintes de longueur et de contenu plus simples.
- Limiter les expériences à des formats représentatifs.
- Documenter les cas non résolus et leurs causes.
- Utiliser des mutations ciblées pour compléter les jeux de tests lorsque certaines générations SMT sont difficiles à obtenir.

## 10. Répartition du travail

Le projet est réparti entre trois membres, chacun étant responsable d'un composant technique principal.

### Membre 1 — Spécifications SMT et génération automatique

- Définir les spécifications formelles des quatre formats étudiés avec Z3 Strings.
- Implémenter les contraintes de longueur, de caractères autorisés et de structure.
- Utiliser les expressions régulières compatibles avec Z3.
- Développer un générateur automatique de chaînes valides et invalides.
- Implémenter une génération par mutation ciblée pour produire des cas invalides.
- Vérifier la cohérence des spécifications avec des tests unitaires.
- Documenter les contraintes SMT et les limites rencontrées.

**Livrables :** spécifications SMT, générateur automatique, jeux de données de test et résultats attendus.

### Membre 2 — Validateurs Python et tests différentiels

- Développer les quatre validateurs indépendants en Python, principalement à l'aide du module `re`.
- Implémenter le harness de test automatisé.
- Exécuter les validateurs sur les chaînes générées.
- Comparer les résultats obtenus avec les résultats attendus des spécifications SMT.
- Identifier les faux positifs et les faux négatifs.
- Reproduire, analyser et documenter les divergences détectées.
- Développer les tests unitaires des validateurs et du harness.

**Livrables :** validateurs Python, harness de test, module de comparaison et documentation des divergences.

### Membre 3 — Intégration, expérimentation et analyse

- Définir les interfaces communes entre les différents composants.
- Intégrer les spécifications SMT, le générateur et les validateurs.
- Développer le point d'entrée principal et le processus global d'exécution.
- Définir et calculer les métriques expérimentales.
- Mesurer les temps de génération, de résolution et de validation.
- Produire des rapports et des visualisations des résultats.
- Étudier les limites pratiques de Z3 Strings, notamment les résultats `unknown`, la complexité et les temps de résolution.
- Rédiger la synthèse des résultats expérimentaux.

**Livrables :** application intégrée, point d'entrée principal, rapports expérimentaux, visualisations et synthèse des résultats.

### Collaboration

Les trois membres participeront aux tests d'intégration, aux revues de code, à la validation des résultats expérimentaux et à la préparation de la soutenance.

Les interfaces entre les composants et le format des données échangées seront définis dès le début du développement afin de faciliter l'intégration.

Les contributions individuelles seront suivies à travers les commits Git et les Pull Requests.

## 11. Licence et références

### Références

- Dépôt pédagogique du cours CoursIA.
- Notebook Z3 — Strings et Regex avec Python.
- Notebook Z3 — Tactics.
- Czerwinski et al., *Strings in SMT* (2021).
- [Documentation officielle de Z3](https://microsoft.github.io/z3guide/).

### Licence

Le projet est réalisé dans le cadre du cours MSMIN5IN43 — Intelligence Artificielle II à l'EPF.

Les conditions de réutilisation du code seront précisées en fonction de la licence applicable au dépôt pédagogique principal.
