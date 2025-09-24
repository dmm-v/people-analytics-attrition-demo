# Données du projet

Ce dossier documente les données utilisées pour l'analyse de l'attrition et l'alimentation du dashboard Power BI, ainsi que pour les modèles de Machine Learning.

Les données proviennent de l'**Office of Personnel Management (OPM, USA)** et sont intégrées dans **Snowflake**.

Elles sont issues de deux jeux de données distincts :

- Années fiscales 2015 à 2019
- Années fiscales 2020 à 2024

Ces jeux de données concernent le départ des employés fédéraux et servent de base à la préparation, à l'analyse et à la modélisation.

---

## Structure relationnelle (Snowflake)

### Table principale
- **Nom** : `hr_data.employee_data`
- **Contenu** : enregistrements individuels (identifiant anonymisé, variables RH, salaires, codes de séparation, etc.).

### Tables de métadonnées jointes
Les clés de la table principale permettent de relier les tables de référence suivantes :

| Clé dans `employee_data` | Table de métadonnées | Rôle |
|--------------------------|----------------------|------|
| `EFDATE`                 | `hr_data.metadat_employee_DateSep` | Date de séparation |
| `AGELVL`                 | `hr_data.metadat_employee_AgeLevel` | Groupe d’âge et valeur |
| `EDLVL`                  | `hr_data.metadat_employee_FormLevl` | Niveau d’éducation |
| `SEP`                    | `hr_data.metadat_employee_Separation` | Code de séparation |
| `LOSLVL`                 | `hr_data.metadat_employee_TempsService` | Catégorie de temps de service |
| `OCC`                    | `hr_data.metadat_employee_Occupation` | Type de poste |
| `PATCO`                  | `hr_data.metadat_employee_CatgOccupation` | Catégorie d’occupation |
| `SALLVL`                 | `hr_data.metadat_employee_SalaryLvl` | Niveau de salaire |
| `WORKSCH`                | `hr_data.metadat_employee_TypeContrat` | Type de contrat |

---

## Table consolidée

Une table finale est créée dans Snowflake pour centraliser les données :

- **Nom** : `bruts.hr_data.Hr_Analytics2`
- **Méthode** : jointures `LEFT JOIN` entre `employee_data` et les tables de métadonnées ci‑dessus.
- **Colonnes calculées** :
  - `Separation_Category` : classification des codes `SEP` en **Volontaire**, **Involontaire**, **Autre**, ou **Non spécifié**.

Cette table est directement utilisée dans **Power BI** et pour les modèles de **Machine Learning**.

---

## Source publique OPM

### Année 2015
- **Données (ZIP)** : [Télécharger](https://www.opm.gov/data/datasets/Files/611/004f4b85-fe4f-4e7d-bd7b-2d23033570cb.zip)
- **Documentation (PDF)** : [Lire](https://www.opm.gov/data/datasets/Files/611/0e9d0a58-7601-4215-99c6-fc63274f7511.pdf)

### Année 2020
- **Données (ZIP)** : [Télécharger](https://www.opm.gov/data/datasets/Files/652/21d85c40-bd35-4f20-9359-281a31af39b1.zip)
- **Documentation (PDF)** : [Lire](https://www.opm.gov/data/datasets/Files/652/f17bf96e-e96f-429e-9606-eac534ce6c9b.pdf)

---

## Contenu local (pour tests)

Ce dossier contient un **échantillon anonymisé** issu de la **table enrichie** `bruts.hr_data.Hr_Analytics2` :

- [`hr_analytics2_sample.csv`](data/hr_analytics2_sample.csv) — 500 premières lignes de la table enrichie, avec toutes les colonnes finales utilisées dans Power BI et ML.
- [`data_dictionary.xlsx`](data/data_dictionary.xlsx) — dictionnaire des variables (noms, types, description, origine).


**Origine de l’échantillon** :  
Ce fichier a été généré à partir des données OPM USA (voir section *Source publique*), enrichies dans Snowflake via le script de création de `Hr_Analytics2`, puis exportées depuis Power BI et réduites à 500 lignes avec `pandas`.

---

## Confidentialité
- Les fichiers inclus ici sont **anonymisés** et légers.
- Aucune donnée nominative ou sensible n’est publiée.

---

## Format et conventions
- **Format** : CSV (UTF‑8, séparateur `,`), première ligne = noms de colonnes.
- **Nommage colonnes** : clair et explicite (ex. `Age_Group`, `Salary_Level`, `Separation_Category`).

---

## Utilisation rapide
```python
import pandas as pd

# Charger l'échantillon enrichi
df = pd.read_csv("data/hr_analytics2_sample.csv")

# Exemple : filtrer les départs volontaires
df_volontaire = df[df["Separation_Category"] == "Volontaire"]
