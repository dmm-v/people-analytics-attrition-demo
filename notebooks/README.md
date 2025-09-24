# Notebooks du projet Attrition OPM

Ce dossier regroupe les notebooks Jupyter qui documentent et démontrent les étapes clés du pipeline de traitement des données du projet **Analyse de l'attrition des employés fédéraux**.

Chaque notebook combine code, explications et résultats intermédiaires pour assurer transparence et reproductibilité.

---

## Contenu

1. **[01_concat_data & 02_data_cleaning.ipynb](Notebook_hr_analytics.ipynb)**  
   - Fusion des tables brutes (2015–2019 et 2020–2024)  
   - Suppression des doublons  
   - Nettoyage des données (valeurs manquantes, formats, types)  
   - Harmonisation des catégories d’âge  
   - Export du dataset complet `DTagy_Clean.csv` puis du dataset final `DTagy_Clean_Final.csv`

2. **sample_data**  
   - Création d’un échantillon de **500 premières lignes** de la table enrichie  
   - Fichier disponible ici : [`hr_analytics2_sample.csv`](../data/data/hr_analytics2_sample.csv)  
   - Contient **toutes les colonnes finales** utilisées dans Power BI et pour les modèles de Machine Learning  
   - Étape conçue pour la **démonstration** et la **reproductibilité** du pipeline

---

## Ordre d’exécution

1. Lancer `01_concat_data.ipynb` pour produire le dataset complet.  
2. Lancer `02_data_cleaning.ipynb` pour obtenir le dataset final nettoyé.  
3. Lancer `03_sample_data.ipynb` pour générer l’échantillon public (`data/hr_analytics2_sample.csv`).

---

## Technologies

- Python 3.x  
- Pandas, NumPy  
- Jupyter Notebook

---

## Notes

- Les fichiers complets sont stockés dans `data/data/` (non tous publiés sur GitHub pour des raisons de taille/confidentialité).
- Les notebooks peuvent être exécutés indépendamment si les fichiers nécessaires sont déjà présents.
- Pour plus de détails sur les fichiers de données, voir le [README du dossier `data`](../data/README.md).

---

