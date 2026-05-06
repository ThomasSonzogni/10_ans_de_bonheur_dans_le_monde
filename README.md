# 10 ans de bonheur dans le monde
 
Analyse Power BI du bonheur mondial entre 2015 et 2025, à partir de 4 sources de données croisées.
 
![Power BI](https://img.shields.io/badge/Power%20BI-F2C811?style=flat&logo=powerbi&logoColor=black)
![Python](https://img.shields.io/badge/Python-3776AB?style=flat&logo=python&logoColor=white)
![DAX](https://img.shields.io/badge/DAX-4B0082?style=flat&logoColor=white)
 
---
 
## Aperçu
 
Le projet croise quatre sources publiques pour analyser le bonheur mondial sur 10 ans, avec un focus sur les pays qui s'écartent du modèle attendu de leur continent.
 
Le rapport Power BI est organisé en 5 pages :
 
| Page | Question répondue |
|------|-------------------|
| Accueil | Hub de navigation |
| Vue globale | Où en est le bonheur dans le monde ? |
| Facteurs | Qu'est-ce qui rend un pays heureux ? |
| Démocratie | La démocratie rend-elle plus heureux ? |
| Pays atypiques | Qui s'écarte du modèle attendu ? |
 
> 💡 *Captures d'écran à insérer dans `assets/`*
 
---
 
## Sources de données
 
| Source | Contenu | Couverture |
|--------|---------|------------|
| [World Happiness Report](https://worldhappiness.report) | Score de bonheur + 6 sous-facteurs | 159 pays · 2015-2025 |
| [Banque Mondiale (WDI)](https://data.worldbank.org) | PIB, PIB/hab PPA, chômage, espérance de vie | 213 pays |
| [V-Dem Institute v16](https://v-dem.net/data) | 5 indices de démocratie et corruption | 180+ pays |
| [ISO-3166](https://github.com/lukes/ISO-3166-Countries-with-Regional-Codes) | Référentiel pays + régions | 249 entrées |
 
Périmètre commun aux 3 sources principales : **158 pays**.
 
---
 
## Architecture
 
```
[CSV bruts]  →  [Notebook Python]  →  [CSV propres]  →  [Power BI]
                  - Nettoyage              - dim_pays.csv
                  - Harmonisation ISO      - fact_happiness.csv
                  - Filtrage 2015-2025     - fact_economy.csv
                                            - fact_democracy.csv
```
 
Modèle Power BI en étoile : 2 dimensions (`dim_pays`, `dim_calendrier`) et 3 tables de faits, jointures via code ISO 3 lettres.
 
---
 
## Structure du repo
 
```
10_ans_de_bonheur_dans_le_monde/
├── README.md
├── data/
│   └── clean/                       # CSV nettoyés exportés pour Power BI
├── notebooks/
│   └── nettoyage.ipynb              # Pipeline ETL Python
├── powerbi/
│   └── 10_ans_de_bonheur.pbix       # Rapport Power BI
├── docs/
│   └── Démarche_projet.md           # Document de démarche projet
└── assets/                          # Captures d'écran
```
 
---
 
## Stack technique
 
- **Python** (pandas) pour l'ETL
- **Power Query (M)** pour le raffinement et la table calendrier
- **DAX** pour ~25 mesures (time intelligence, virtual tables, mesures composées)
- **Power BI Desktop** pour le rapport et la mise en page
---
 
## Reproduire le projet
 
```bash
# Cloner le repo
git clone https://github.com/ThomasSonzogni/10_ans_de_bonheur_dans_le_monde.git
cd 10_ans_de_bonheur_dans_le_monde
 
# Télécharger les sources brutes (cf. liens ci-dessus) dans data/raw/
 
# Lancer le notebook de nettoyage
jupyter notebook notebooks/nettoyage.ipynb
 
# Ouvrir le rapport
# powerbi/10_ans_de_bonheur.pbix dans Power BI Desktop
# Adapter les chemins dans Power Query si besoin
```
 
---
 
## Limites
 
- Données 2024-2025 partielles sur Banque Mondiale et V-Dem : utilisation de la dernière valeur disponible
- Corrélations affichées ≠ causalité
- WHR est une enquête perçue : biais culturels possibles
- 3 territoires non-ISO traités spécifiquement : Kosovo conservé (XKX), North Cyprus et Somaliland exclus
---
 
## Licence
 
- Code : MIT
- Données : sources publiques, soumises à leurs licences respectives
---
 
## Auteur
 
**Thomas Sonzogni**

📧 Contact : thomas.paul.sonzogni@gmail.com
💼 LinkedIn : www.linkedin.com/in/thomas-sonzogni

---

*Si ce projet t'intéresse ou si tu veux échanger sur la démarche, n'hésite pas à ouvrir une issue ou à me contacter directement.*
