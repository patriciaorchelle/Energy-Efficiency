# Energy-Efficiency



# Performance énergétique des bâtiments

## **1. Contexte du projet**

**Dataset : Energy Efficiency (UCI / Kaggle).**

L’efficacité énergétique des bâtiments est un enjeu majeur dans le contexte actuel de réduction des consommations. Ce projet utilise un dataset tabulaire bien connu, centré sur la prédiction des **charges de chauffage et de refroidissement**, à partir des caractéristiques structurelles.

Cibles à prédire :

* **Heating Load (Y1)** → besoin de chauffage (kWh/m²).
* **Cooling Load (Y2)** → besoin de refroidissement (kWh/m²).


## **2. Description du dataset**

* **Taille** : 768 échantillons.
* **Type** : tabulaire, propre, aucune donnée manquante.

###  Variables explicatives (X)

1. Relative Compactness
2. Surface Area
3. Wall Area
4. Roof Area
5. Overall Height
6. Orientation (catégorielle → encodée en One-Hot)
7. Glazing Area
8. Glazing Area Distribution (catégorielle → encodée en One-Hot)

### Variables cibles (Y)

* Y1 = **Heating Load (HL)**
* Y2 = **Cooling Load (CL)**



## **3. Objectif du projet**

* Détection et suppression des outliers (**IQR + Z-score**).
* Étude des corrélations (**Pearson & Spearman**) et visualisation via **heatmap**.
* Prétraitement des données :

  * **Encodage One-Hot** pour les variables catégorielles.
  * **Standardisation** des variables explicatives (un seul scaler partagé pour Y1 et Y2).
* Analyse de multicolinéarité via **Variance Inflation Factor (VIF)**.
* Construction et comparaison de modèles de Machine Learning :

  * Régressions de base (Linear, Ridge, Lasso, ElasticNet).
  * Modèles avancés (RandomForest, GradientBoosting, XGBoost).
* Évaluation des modèles avec :

  * Validation simple.
  * **Cross-Validation**.
  * **RandomizedSearchCV**.
  * **GridSearchCV**.
* Comparaison finale des performances dans un tableau synthétique.
* **Prédiction finale** sur de nouveaux échantillons avec les meilleurs modèles retenus pour chaque cible.



## **4. Pipeline détaillé**

### 4.1 Prétraitement des données

* **Encodage One-Hot** de *Orientation* et *Glazing Area Distribution*.
* **StandardScaler** appliqué sur toutes les caractéristiques (un seul scaler pour Y1 et Y2).

### 4.2 Détection et traitement des outliers

* Méthodes utilisées : **IQR** et **Z-score**.
* Nettoyage des données avant modélisation.

### 4.3 Analyse des corrélations

* Corrélations **Pearson** et **Spearman** entre X et {Y1, Y2}.
* Visualisation : **heatmap seaborn**.

### 4.4 Multicolinéarité (VIF)

* Vérification de la redondance entre variables explicatives.
* Conservation uniquement des variables avec VIF acceptable (< 10).

### 4.5 Modélisation

#### Étapes réalisées pour Y1 et Y2 séparément :

1. **Évaluation simple des modèles** (sans CV).
2. **Cross-Validation** → scores moyens (R², RMSE, MAE).
3. **RandomizedSearchCV** → recherche aléatoire des hyperparamètres.
4. **GridSearchCV** → recherche exhaustive sur grille restreinte.



## **5. Résultats comparatifs**

* Tableau final regroupant les performances des 3 modèles (RF, GB, XGB) pour chaque cible, et pour les 4 stratégies d’évaluation :

  * Sans CV.
  * CV simple.
  * RandomizedSearchCV.
  * GridSearchCV.

**Conclusion :**

* Pour **Y1**, le meilleur modèle est **XGBoost (CV simple)**.
* Pour **Y2**, le meilleur modèle est **XGBoost (GridSearchCV)**.



## **6. Prédictions finales**

* Sélection des **meilleurs modèles entraînés** pour chaque cible.
* Génération de **10 nouveaux exemples** (valeurs des 8 features).
* Application du **scaler** puis prédiction avec :

  * `best_model_y1` pour Y1.
  * `best_model_y2` pour Y2.
* Construction d’un **DataFrame final** affichant :

  * Les caractéristiques de chaque exemple.
  * Les prédictions **Heating Load (Y1)** et **Cooling Load (Y2)**.



## **7. Prochaines pistes**

* Ajouter des modèles **plus avancés** (LightGBM, CatBoost, NN).
* Étudier l’importance des variables (**feature importance / SHAP**).
* Déployer une **API Flask/FastAPI** pour prédictions en temps réel.
* Créer une **interface web (Streamlit/Shiny)** pour utilisateurs non techniques.
