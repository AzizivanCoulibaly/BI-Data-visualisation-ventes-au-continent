# BI-Data-visualisation-ventes-au-continent

## Projet : Consolidation et analyse de 4 millions de lignes de ventes multi-continents (2019–2022)  

### Données brutes  
[🌐 Télécharger le dataset complet](https://drive.google.com/drive/folders/1wVMY45d3gs_bTIdUYqQ7uSHOWxGzJt9-?usp=share_link)

### Contexte & Besoin
- Analyse de ventes Spontanés
- Conception de Dashboard intuitif
- Performance du rapport et des requêtes
- Fiabilité des données mais aucune necessité de les avoir en temps réel
- Mesure DAX complexe, Analyse temporelle


### Problème rencontré  
- Données brutes sous forme de fichiers texte dispersés et stocké en local (un fichier par continent : Afrique, Europe, Asie, Amérique)  
- Table de correspondance pays–continent séparée (2 colonnes : Pays, Continent)  
- Volume total de données : **4 millions de lignes** → limite technique d’Excel (1 million de lignes max)  
- Fichiers lourds et éparpillés, mais nécessité de connecter les ventes aux continents pour l’analyse
- Colonne pays non standardisé à cause des caractéres d'écriture Majuscule/minuscule

---

### Étapes de traitement  

**Importation des données (Power Query)**  
- Importation à partir  (dossier contenant 4 fichiers texte, ventes 2019:2022 par continent)  
- Importation de la table pays–continent (2 colonnes : Pays, Continent)  
[Imgur](https://imgur.com/ryrRvzw)
[Imgur](https://imgur.com/uxEA3LL)

**Combinaison et nettoyage (Power Query)**  
- Combinaison des 4 tables de ventes (“Afrique”, “Europe”, “Asie”, “Amérique”) → structure identique (Date, Pays, Qte, Prix unitaire)
- Formatage des dates et des montants (devise normalisée)  
- Standardisation des noms de pays (première lettre en majuscule)  
- Transformation de la table pays–continent :  
  - Standardisation des pays (première lettre en majuscule)  
  - Promotion de la première ligne comme en-tête  
##### Nettoyage des données  
 -
  [Imgur](https://imgur.com/Sfa9BqP) 
 ---
 [Imgur](https://imgur.com/XZTw9XK) 

**Chargement des requêtes dans Power Pivot**  
- Les données ( 4M de lignes) sont **chargées uniquement en connexion** puis ** Ajouter au modèle de donnée* pour éviter de saturer Excel  
- Les tables utilisées dans le modèle :  
  - Table de faits = Ventes consolidées  
  - Table de dimension = Pays–Continent 
##### Chargement des requêtes dans Power Pivot
[Imgur](https://imgur.com/qDMLg6c) 

**Table calendrier (Power Pivot)**  
- Création d’une table calendrier indépendante pour gérer le temps efficacement  
- Étendue : 2019 → 2030 (anticipation des années futures)  
- Évite d’ajouter des colonnes calculées dans la table de faits; une nouvelle colonne implique qu'elle s'étende sur 4 million de ligne
- Ajout d'une colonne semestre pour affiner les analyses
##### Table Calendrier
[Imgur](https://imgur.com/2bqZsAC)
[Imgur](https://imgur.com/8ywt3ZP)

**Modélisation relationnelle**  
- Table centrale : **Ventes 2019-2022**  
  - Connectée à la **Table Date** (clé = Date)  
  - Connectée à la **Table Pays–Continent** (clé = Pays)  
##### Modélisation des données
[Imgur](https://imgur.com/0dGIAjd)

**Création de mesures (DAX)**  
- `CA = SUMX(Ventes, Ventes[Qte] * Ventes[Prix unitaire])`  
- `CA N-1 = CALCULATE([CA], DATEADD(Date[Date], -1, YEAR))`  
- `Ecart = [CA] - [CA N-1]`  
- `Part continent = DIVIDE([CA], CALCULATE([CA], ALL(PaysContinent[Continent])))`  

**Analyse (Excel)**  
- Analyse via Tableaux Croisés Dynamiques (Excel) et réponse aux problématiques métiers (15 Questions)
##### Questions et réponses (Métier) 
Q1,Q2,Q3 [Imgur](https://imgur.com/gN7z9k1) 
Q4, Q5 [Imgur](https://imgur.com/Ypr2t7s)
Q6, Q7 [Imgur](https://imgur.com/9aJNIlW)
Q8, Q9 [Imgur](https://imgur.com/fpWWSCC)
Q10, Q11 [Imgur](https://imgur.com/uTb7K3t) 
Q12, Q13 [Imgur](https://imgur.com/cam2UTu) 
Q14, Q15 [Imgur](https://imgur.com/VlLU4Xz)

[🌐 Accéder aux analyses excel](https://drive.google.com/drive/folders/1wVMY45d3gs_bTIdUYqQ7uSHOWxGzJt9-?usp=share_link)

**Visualisations Power BI** : histogrammes, cartes, Treemap, Filtre
- Mode de connexion : Import (les données sont stockées de manière local à partir de fichiers excel,de plus ce mode de connexion favorise la performance du rapport sur que le besoin exprimé ne necessite pas de donnée en temps réel)
- Modèle de donnée : Modèle semantique
- Type de modèle : Star Schema
  
Certaines interractions ont été modifié volontairement de sorte à ce que les visuels concernés soient dissociées de certains filtres afin de préserver une lecture stratégique globale .
En effet, Le graphique représentant l'Evolution du CA au fil des mois est indépendant du filtre "Mois" car cela nous permet de conserver une vision complète des tendances temporelles tout en garantissant une analyse de la dynamique globale du business .
Aussi, la treemap utilisée pour visualisation la repartition total du CA par catégorie de produits (En pourcentage) est indépendante du filtre catégorie de Articles pour les mêmes raisons
[Imgur](https://imgur.com/bh6xBVN) 
[Imgur](https://imgur.com/idxYSQY)
[🌐 Accéder au visuel](https://drive.google.com/drive/folders/1wVMY45d3gs_bTIdUYqQ7uSHOWxGzJt9-?usp=share_link)

---

### Résultats quantitatifs  
- Consolidation de **4 millions de lignes** dans un modèle robuste et exploitable  
- Réduction du temps de préparation : de plusieurs heures manuelles à quelques minutes automatisées  
- Suivi par continent, pays et période possible en temps réel  

### Résultats qualitatifs  
- Visualisations intuitives permettant une comparaison claire entre continents  
- Modèle extensible : ajout possible de nouvelles années ou continents sans refonte complète  
- Adoption facilitée grâce à la disponibilité des données dans **Excel (TCD)** et **Power BI (dashboards interactifs)**  

### Résultats personnels  
- Maîtrise du traitement de **volumétrie importante** (4M de lignes) grâce à Power Query + Power Pivot  
- Expérience dans la **modélisation multi-tables** et la création d’une table calendrier optimisée  
- Développement d’une approche analytique orientée “scalabilité” (anticipation des années futures jusqu’en 2030)  
- Renforcement de ma capacité à relier la donnée brute à des **indicateurs business pertinents**  


