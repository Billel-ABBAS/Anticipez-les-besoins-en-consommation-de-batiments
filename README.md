# Étapes du Projet : Prédiction des Émissions de CO2 et de la Consommation d'Énergie

##  **1. Introduction**
- **Contexte** : 
  - La ville de Seattle vise la neutralité carbone d'ici 2050.
  - Le projet s'inscrit dans une initiative pour réduire les émissions de gaz à effet de serre dans les bâtiments non résidentiels, secteur clé en termes de consommation d'énergie.
- **Problématique** :
  - Face à des données incomplètes, il est crucial de développer des modèles prédictifs pour estimer la consommation d'énergie et les émissions de CO2.
- **Objectif** :
  - Fournir des estimations précises pour orienter les politiques énergétiques et les décisions de rénovation.

---

##  **2. Vue d'ensemble des données**
- **Source des données** : Dataset 2016 des bâtiments non résidentiels de Seattle.
- **Caractéristiques** :
  - **3376 lignes**, **46 variables** (21 qualitatives et 25 quantitatives).
  - Données sur la localisation, la taille, l'utilisation et les performances énergétiques des bâtiments.
- **Variables cibles** :
  - **Consommation d'énergie totale** : `SiteEnergyUse(kBtu)`.
  - **Émissions de CO2** : `TotalGHGEmissions`.

---

##  **3. Nettoyage des données**
### Suppressions et filtrages :
- **Colonnes redondantes ou inutiles** :
  - Exemple : `Electricity(kWh)`, `NaturalGas(therms)`, `DefaultData`, etc.
- **Lignes supprimées** :
  - Doublons.
  - Valeurs manquantes dans les cibles.
  - Valeurs aberrantes (exemple : nombre d'étages corrigé pour un bâtiment ayant des erreurs).

### Imputation :
- **Colonnes imputées** :
  - `ZipCode` (à partir de la colonne `Address`).
  - `SecondLargestPropertyUseType` et `ThirdLargestPropertyUseType` (basé sur d'autres colonnes pertinentes).

---

##  **4. Feature Engineering**
### Création de nouvelles variables :
1. **Âge du bâtiment** : `BuildingAge` (à partir de `YearBuilt`).
2. **Proportions de surface** :
   - Exemple : `Building(s)_Proportion = PropertyGFABuilding(s) / PropertyGFATotal`.
3. **Transformation logarithmique** :
   - Variables avec skewness élevée, comme `SiteEnergyUse(kBtu)` et `TotalGHGEmissions`.

### Suppression des variables en cas de fuite de données (data leakage) :
- Exemple : `SiteEUI(kBtu/sf)`, `SourceEUI(kBtu/sf)`.

---

##  **5. Analyse exploratoire**
- **Corrélations significatives** :
  - `SiteEUI(kBtu/sf)` et `SourceEUI(kBtu/sf)` : 0.94.
  - `SiteEUI(kBtu/sf)` et `GHGEmissionsIntensity` : 0.75.
- **Impact des variables** :
  - Exemple : `PrimaryPropertyType` sur `Electricity(kBtu)`.

---

##  **6. Modélisation et évaluation**
### Modèles testés :
1. **Régressions linéaires** :
   - Ridge, Lasso.
2. **Arbres de décision et ensembles** :
   - Random Forest, Gradient Boost, XGBoost, CatBoost.

### Optimisation :
- **Validation croisée** via `GridSearchCV` et `RandomizedSearchCV`.
- Meilleurs hyperparamètres identifiés pour CatBoost :
  - `learning_rate`, `depth`, `iterations`, etc.

### Résultats :
- **Consommation d'énergie** :
  - CatBoost - **R² Test** : 0.798, **RMSE Test** : 0.589.
- **Émissions de CO2** :
  - CatBoost - **R² Test** : 0.734, **RMSE Test** : 0.786.

---

##  **7. Analyse des résultats**
### Importance des variables :
1. **Consommation d'énergie** :
   - Variables influentes : `GFA Per Floor`, `Electricity Use Category`.
2. **Émissions de CO2** :
   - Variables influentes : `NaturalGasUseCategory_BuildingType`, `PropertyGFABuilding(s)_log`.

### Influence de l'ENERGY STAR Score :
- L'intégration de cette variable n'a pas amélioré significativement les performances.

---

##  **8. Déploiement et améliorations futures**
### Actions envisagées :
1. **Déploiement** :
   - Création d'une API pour le modèle.
   - Hébergement sur un service cloud.
2. **Collecte de nouvelles données** :
   - Données plus récentes et intégration avec des historiques météo.
3. **Améliorations techniques** :
   - Exploration de nouveaux algorithmes.
   - Ajustements sur le prétraitement.

---

##  **9. Conclusion**
- Le modèle CatBoost a montré les meilleures performances pour les deux cibles.
- Les résultats obtenus contribuent à la stratégie de neutralité carbone de Seattle, offrant des outils fiables pour la prise de décision.