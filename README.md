# BI-Data-visualisation-ventes-au-continent

## Projet : Consolidation et analyse de 4 millions de lignes de ventes multi-continents (2019–2022)  

### Données brutes  
[🌐 Télécharger le dataset complet](https://drive.google.com/drive/folders/1wVMY45d3gs_bTIdUYqQ7uSHOWxGzJt9-?usp=share_link)

### Contexte & Besoin
- Analyse de ventes spontanées pour identifier tendances et optimisations business.
- Conception de dashboards intuitifs pour une prise de décision rapide.
- Priorité à la performance des rapports et requêtes, avec fiabilité des données (données historiques, pas de besoin en temps réel).
- Mesures DAX complexes pour analyses temporelles et comparatives.
- Lien business : Inspiré de mon expérience retail chez Baccarat, où j'analysais la performance commercial .
- Fiabilité des données
- Mesure DAX complexe, Analyse temporelle


### Problème rencontré  
- Données brutes en fichiers texte dispersés (un par continent : Afrique, Europe, Asie, Amérique).
- Table de correspondance pays–continent séparée (colonnes : Pays, Continent).
- Volume massif : **4 millions de lignes** – dépasse limite Excel (1M lignes max).
- Fichiers lourds/éparpillés ; besoin de lier ventes à continents pour analyses.
- Noms de pays non standardisés (maj/min, accents), risque d'erreurs

---

### Étapes de traitement  

**Importation des données (Power Query)**  
- Import depuis dossier contenant 4 fichiers texte (ventes 2019-2022 par continent).
- Import de la table pays–continent.  
[Imgur](https://imgur.com/ryrRvzw)
[Imgur](https://imgur.com/uxEA3LL)

**Combinaison et nettoyage (Power Query)**  
- Combinaison des 4 tables de ventes (“Afrique”, “Europe”, “Asie”, “Amérique”) → structure identique (Date, Pays, Qte, Prix unitaire)
- Formatage dates/montants (devise unifiée).
- Standardisation pays (première lettre majuscule, trim espaces).
- Pour table pays–continent : Promotion en-têtes, standardisation similaire.

##### Nettoyage des données  
 -
  [Imgur](https://imgur.com/Sfa9BqP) 
 ---
 [Imgur](https://imgur.com/XZTw9XK) 

**Chargement des requêtes dans Power Pivot**  
- Données chargées en mode connexion pour gérer 4M lignes sans saturer Excel.
- Ajout au modèle sémantique : Table faits = Ventes consolidées ; Dimension = Pays-Continent.
  
##### Chargement des requêtes dans Power Pivot
[Imgur](https://imgur.com/qDMLg6c) 

**Table calendrier (Power Pivot)**  
- Création indépendante (2019–2030) pour scalabilité et performance (évite colonnes calculées sur 4M lignes).
- Colonnes ajoutées : Semestre, Trimestre, pour analyses affinées.

##### Table Calendrier
[Imgur](https://imgur.com/2bqZsAC)
[Imgur](https://imgur.com/8ywt3ZP)

**Modélisation relationnelle**  
- Modèle de donnée : Modèle semantique
- Schéma en étoile (Star Schema) : Faits (Ventes) reliés à Dimensions (Calendar via Date ; Pays-Continent via Pays).
- Cardinalité : 1:N (dimensions vers table de faits).
- Filtrage : Sens unique pour efficacité.
- Vision governance : Prêt pour RLS (ex. : filtrer par continent pour accès sécurisé en équipe).
  
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
Nous avons repris le même modèle de donnée précédent (Voir Modélisation Power Pivot) ainsi que tous ces caractéristiques
  
Certaines interractions ont été modifié volontairement de sorte à ce que les visuels concernés soient dissociées de certains filtres afin de préserver une lecture stratégique globale .
En effet, Le graphique représentant l'Evolution du CA au fil des mois est indépendant du filtre "Mois" car cela nous permet de conserver une vision complète des tendances temporelles tout en garantissant une analyse de la dynamique globale du business.
Aussi, la treemap utilisée pour visualisation la repartition total du CA par catégorie de produits (En pourcentage) est indépendante du filtre catégorie de Articles pour les mêmes raisons
[Imgur](https://imgur.com/bh6xBVN) 
[Imgur](https://imgur.com/a/q9NiSE9)
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
- Maîtrise volumétrie importante via Power Query/Pivot.
- Expertise modélisation multi-tables et calendrier optimisé.
- Approche scalable (2030-ready) + lien data-business.
- Préparation PL-300 : Couvre 80 % skills (ajout governance pour full coverage).
  
### Améliorations Futures
- Migration Power BI Service : Workspaces, apps, scheduled refreshes, RLS réel pour collaboration sécurisée.
- Intégration sources live (ex. : API pour ventes récentes).


