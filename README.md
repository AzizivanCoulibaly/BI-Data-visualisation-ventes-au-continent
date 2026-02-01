# BI-Data-visualisation-ventes-au-continent

## Projet : Consolidation et analyse de 4 millions de lignes de ventes multi-continents (2019–2022)  

### Données brutes  
[🌐 Télécharger le dataset complet](https://drive.google.com/drive/folders/1wVMY45d3gs_bTIdUYqQ7uSHOWxGzJt9-?usp=share_link)


### Problème rencontré  
- Données brutes sous forme de fichiers texte dispersés (un fichier par continent : Afrique, Europe, Asie, Amérique)  
- Table de correspondance pays–continent séparée (2 colonnes : Pays, Continent)  
- Volume total de données : **4 millions de lignes** → limite technique d’Excel (1 million de lignes max)  
- Fichiers lourds et éparpillés, mais nécessité de connecter les ventes aux continents pour l’analyse
- Colonne pays non standardisé à cause des caractéres d'écriture Majuscule/minuscule

---

### Étapes de traitement  

**Importation des données (Power Query)**  
- Importation depuis un dossier contenant les 4 fichiers texte (ventes 2019–2022 par continent)  
- Importation de la table pays–continent (2 colonnes : Pays, Continent)  

**Combinaison et nettoyage (Power Query)**  
- Combinaison des 4 tables de ventes (“Afrique”, “Europe”, “Asie”, “Amérique”) → structure identique (Date, Pays, Qte, Prix unitaire)
- Formatage des dates et des montants (devise normalisée)  
- Standardisation des noms de pays (première lettre en majuscule)  
- Transformation de la table pays–continent :  
  - Standardisation des pays (première lettre en majuscule)  
  - Promotion de la première ligne comme en-tête  
##### Nettoyage des données  
 ---
![Nettoyage des données brutes](https://github.com/AzizivanCoulibaly/BI-Data-Visualisation-Analyse-de-ventes-/blob/c65ccb32c8211001890bcd3ff88ebf8a807e5114/Nettoyage%20%26%20transformation%20%26%20combinaison%20des%20fichiers.JPG)
 ---
![Nettoyage pays-continent](
https://raw.githubusercontent.com/azizivancoulibaly/BI-Data-Visualisation-Analyse-de-ventes-/Images/Nettoyage%20table%20pays-continent.JPG)

**Chargement dans Power Pivot**  
- Les données ( 4M de lignes) sont **chargées uniquement en connexion** puis ** Ajouter au modèle de donnée* pour éviter de saturer Excel  
- Les tables utilisées dans le modèle :  
  - Table de faits = Ventes consolidées  
  - Table de dimension = Pays–Continent 
##### Chargement dans Power Pivo
![Chargement dans Power Pivo](https://github.com/AzizivanCoulibaly/AZIZ-COULIBALY/blob/a7ddde83e127e2a109102c9e1e76cc18256fb176/Images/Charger%20au%20mode%CC%80le%20de%20donne%CC%81e%20power%20pivot.JPG
)
**Table calendrier (Power Pivot)**  
- Création d’une table calendrier indépendante pour gérer le temps efficacement  
- Étendue : 2019 → 2030 (anticipation des années futures)  
- Évite d’ajouter des colonnes calculées dans la table de faits; une nouvelle colonne implique qu'elle s'étende sur 4 million de ligne
- Ajout d'une colonne semestre pour affiner les analyses
##### Table Calendrier
![Table Calendrier](https://github.com/AzizivanCoulibaly/AZIZ-COULIBALY/blob/b06678e3036d517e3cb03a2472ae2e39ad90de49/Images/Ajout%20colonne%20semestre.JPG)

**Modélisation relationnelle**  
- Table centrale : **Ventes 2019-2022**  
  - Connectée à la **Table Date** (clé = Date)  
  - Connectée à la **Table Pays–Continent** (clé = Pays)  
##### Modélisation des données
![Modélisation des données](https://github.com/AzizivanCoulibaly/AZIZ-COULIBALY/blob/0aea8fe9e45bf766d3d50ea07db427a8012b6225/Images/Mode%CC%80le%20de%20donne%CC%81e.JPG)

**Création de mesures (DAX)**  
- `CA = SUMX(Ventes, Ventes[Qte] * Ventes[Prix unitaire])`  
- `CA N-1 = CALCULATE([CA], DATEADD(Date[Date], -1, YEAR))`  
- `Ecart = [CA] - [CA N-1]`  
- `Part continent = DIVIDE([CA], CALCULATE([CA], ALL(PaysContinent[Continent])))`  

**Analyse (Excel & Power BI)**  
- Analyse via Tableaux Croisés Dynamiques (Excel) et réponse aux problématiques métiers (15 Questions)
##### Questions et réponses  
[🌐 Accéder aux analyses excel](https://drive.google.com/drive/folders/1wVMY45d3gs_bTIdUYqQ7uSHOWxGzJt9-?usp=share_link)
![Q1,Q2,Q3](https://github.com/AzizivanCoulibaly/AZIZ-COULIBALY/blob/efcaea532f643369413a1be0b04e082a5e31d6cd/Images/WhatsApp%20Image%202025-09-30%20at%2019.06.49.jpeg)
![Q4,Q5](https://github.com/AzizivanCoulibaly/AZIZ-COULIBALY/blob/9e5b3c568634c028adad28bfbcc43a6aef8b31eb/Images/WhatsApp%20Image%202025-09-30%20at%2019.06.54.jpeg)
![Q6,Q7](https://github.com/AzizivanCoulibaly/AZIZ-COULIBALY/blob/6f7af6fb0a34cf88f00e237b6e5783b538629ec6/Images/WhatsApp%20Image%202025-09-30%20at%2019.06.56.jpeg)
![Q8,Q9](https://github.com/AzizivanCoulibaly/AZIZ-COULIBALY/blob/6f7af6fb0a34cf88f00e237b6e5783b538629ec6/Images/WhatsApp%20Image%202025-09-30%20at%2019.06.58.jpeg)
![Q10,Q11](https://github.com/AzizivanCoulibaly/AZIZ-COULIBALY/blob/6f7af6fb0a34cf88f00e237b6e5783b538629ec6/Images/WhatsApp%20Image%202025-09-30%20at%2019.06.59.jpeg)
![Q12,Q13](https://github.com/AzizivanCoulibaly/AZIZ-COULIBALY/blob/6f7af6fb0a34cf88f00e237b6e5783b538629ec6/Images/WhatsApp%20Image%202025-09-30%20at%2019.07.00.jpeg)
![Q14,Q15](https://github.com/AzizivanCoulibaly/AZIZ-COULIBALY/blob/6f7af6fb0a34cf88f00e237b6e5783b538629ec6/Images/WhatsApp%20Image%202025-09-30%20at%2019.07.00-2.jpeg)

Visualisations dans Power BI : histogrammes, cartes, Treemap, Filtre
[🌐 Accéder au visuel](https://drive.google.com/drive/folders/1wVMY45d3gs_bTIdUYqQ7uSHOWxGzJt9-?usp=share_link)
![Power BI visuel](https://github.com/AzizivanCoulibaly/AZIZ-COULIBALY/blob/162b5f6011e88797ffc6c686e0ff1d9b0b2ce1a0/Images/Vente%20au%20continent.PNG)
![Power BI visuel](https://github.com/AzizivanCoulibaly/AZIZ-COULIBALY/blob/3092e31028e92708d3f7b9b110787dd19fc25eed/Images/Vente%20au%20continent%20Asie_stylo%20et%20chaussure.PNG)

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


