10 ans de bonheur dans le monde
Analyse Power BI du bonheur mondial entre 2015 et 2025, à partir de 4 sources de données croisées.
Afficher l'image
Afficher l'image
Afficher l'image

Aperçu
Le projet croise quatre sources publiques pour analyser le bonheur mondial sur 10 ans, avec un focus sur les pays qui s'écartent du modèle attendu de leur continent.
Le rapport Power BI est organisé en 5 pages :
PageQuestion répondueAccueilHub de navigationVue globaleOù en est le bonheur dans le monde ?FacteursQu'est-ce qui rend un pays heureux ?DémocratieLa démocratie rend-elle plus heureux ?Pays atypiquesQui s'écarte du modèle attendu ?

💡 Captures d'écran à insérer dans assets/


Sources de données
SourceContenuCouvertureWorld Happiness ReportScore de bonheur + 6 sous-facteurs159 pays · 2015-2025Banque Mondiale (WDI)PIB, PIB/hab PPA, chômage, espérance de vie213 paysV-Dem Institute v165 indices de démocratie et corruption180+ paysISO-3166Référentiel pays + régions249 entrées
Périmètre commun aux 3 sources principales : 158 pays.

Architecture
[CSV bruts]  →  [Notebook Python]  →  [CSV propres]  →  [Power BI]
                  - Nettoyage              - dim_pays.csv
                  - Harmonisation ISO      - fact_happiness.csv
                  - Filtrage 2015-2025     - fact_economy.csv
                                            - fact_democracy.csv
Modèle Power BI en étoile : 2 dimensions (dim_pays, dim_calendrier) et 3 tables de faits, jointures via code ISO 3 lettres.

Structure du repo
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

Stack technique

Python (pandas) pour l'ETL
Power Query (M) pour le raffinement et la table calendrier
DAX pour ~25 mesures (time intelligence, virtual tables, mesures composées)
Power BI Desktop pour le rapport et la mise en page


Reproduire le projet
bash# Cloner le repo
git clone https://github.com/ThomasSonzogni/10_ans_de_bonheur_dans_le_monde.git
cd 10_ans_de_bonheur_dans_le_monde

# Télécharger les sources brutes (cf. liens ci-dessus) dans data/raw/

# Lancer le notebook de nettoyage
jupyter notebook notebooks/nettoyage.ipynb

# Ouvrir le rapport
# powerbi/10_ans_de_bonheur.pbix dans Power BI Desktop
# Adapter les chemins dans Power Query si besoin

Limites

Données 2024-2025 partielles sur Banque Mondiale et V-Dem : utilisation de la dernière valeur disponible
Corrélations affichées ≠ causalité
WHR est une enquête perçue : biais culturels possibles
3 territoires non-ISO traités spécifiquement : Kosovo conservé (XKX), North Cyprus et Somaliland exclus


Licence

Code : MIT
Données : sources publiques, soumises à leurs licences respectives


Auteur
Thomas Sonzogni

📧 Contact : thomas.paul.sonzogni@gmail.com
💼 LinkedIn : www.linkedin.com/in/thomas-sonzogni

---

*Si ce projet t'intéresse ou si tu veux échanger sur la démarche, n'hésite pas à ouvrir une issue ou à me contacter directement.*
