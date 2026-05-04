# 10 ans de bonheur dans le monde

> Projet Power BI explorant les déterminants du bonheur mondial entre 2015 et 2025, à partir de 4 sources de données croisées.
> Cas d'usage fictif : aider la Direction RSE d'un groupe CAC40 à objectiver son pilotage QVT en croisant satisfaction interne et benchmark externe.

![Power BI](https://img.shields.io/badge/Power%20BI-F2C811?style=flat&logo=powerbi&logoColor=black)
![Python](https://img.shields.io/badge/Python-3776AB?style=flat&logo=python&logoColor=white)
![DAX](https://img.shields.io/badge/DAX-4B0082?style=flat&logoColor=white)
![Statut](https://img.shields.io/badge/Statut-Termin%C3%A9-success)
![Licence](https://img.shields.io/badge/Donn%C3%A9es-Publiques-blue)

---

## 🎯 Pourquoi ce projet ?

Ce projet est un **portfolio data + gestion de projet** réalisé en 7-8 heures pour démontrer :

- **Compétences techniques** : ETL Python, modélisation en étoile, DAX intermédiaire/avancé, UX Power BI
- **Démarche projet** : cadrage, faisabilité, cahier des charges, recette, déploiement, bilan
- **Storytelling data** : 5 pages avec navigation interactive et fil narratif clair

Le scénario est volontairement réaliste : un commanditaire fictif (Direction RSE de **« Lumea Group »**, CAC40) demande au départ « un classement des pays heureux pour le rapport extra-financier ». Le cadrage révèle un **besoin caché** plus pertinent : comprendre comment la satisfaction des collaborateurs sur les 23 sites du groupe se compare au contexte de chaque pays, pour mieux cibler les budgets QVT.

---

## 📊 Aperçu du rapport

> 💡 *Captures d'écran à insérer ici*

| Page | Question business |
|------|-------------------|
| 🏠 Accueil | Hub de navigation avec storytelling |
| 🌍 Vue globale | Où en est le bonheur dans le monde ? |
| 📊 Facteurs | Qu'est-ce qui rend un pays heureux ? |
| 🗳️ Démocratie | La démocratie rend-elle plus heureux ? |
| 🎯 Atypiques | Qui s'écarte de la moyenne de son continent ? |

---

## 🗂️ Sources de données

| Source | Contenu | Couverture |
|--------|---------|------------|
| [World Happiness Report](https://worldhappiness.report) | Score de bonheur + 6 sous-facteurs | 159 pays · 2015-2025 |
| [Banque Mondiale (WDI)](https://data.worldbank.org) | PIB, PIB/hab PPA, chômage, espérance de vie | 213 pays |
| [V-Dem Institute v16](https://v-dem.net/data) | 5 indices de démocratie et corruption | 180+ pays |
| [ISO-3166](https://github.com/lukes/ISO-3166-Countries-with-Regional-Codes) | Référentiel pays + régions | 249 entrées |

**Périmètre cœur** : 158 pays présents dans les 3 sources principales.

---

## 🏗️ Architecture technique

### Pipeline de données

```
[CSV bruts]  →  [Notebook Python]  →  [CSV propres]  →  [Power BI]
                  - Nettoyage              - dim_pays.csv
                  - Harmonisation ISO      - fact_happiness.csv
                  - Filtrage 2015-2025     - fact_economy.csv
                  - Factorisation          - fact_democracy.csv
```

### Modèle en étoile

```
                ┌──────────────────┐
                │   dim_calendrier │
                └────────┬─────────┘
                         │
        ┌────────────────┼────────────────┐
        │                │                │
┌───────▼──────┐  ┌──────▼─────┐  ┌──────▼───────┐
│ fact_happin. │  │ fact_econ. │  │ fact_democ.  │
└───────┬──────┘  └──────┬─────┘  └──────┬───────┘
        │                │                │
        └────────────────┼────────────────┘
                         │
                ┌────────▼────────┐
                │    dim_pays     │
                └─────────────────┘
```

3 tables de faits, 2 dimensions partagées, jointures via code ISO 3 lettres.

---

## 📁 Structure du repo

```
10_ans_de_bonheur_dans_le_monde/
├── data/
│   ├── raw/                      # Sources brutes (non versionnées si trop lourdes)
│   └── clean/                    # CSV nettoyés, exportés pour Power BI
│       ├── dim_pays.csv
│       ├── fact_happiness.csv
│       ├── fact_economy.csv
│       └── fact_democracy.csv
├── notebooks/
│   └── nettoyage.ipynb           # Pipeline ETL Python documenté
├── powerbi/
│   └── 10_ans_de_bonheur.pbix    # Rapport Power BI complet
├── docs/                         # Pack de documents projet
│   ├── 01_note_de_cadrage.md
│   ├── 02_README.md              # README technique du projet
│   ├── 03_CR_R1_decouverte.md
│   ├── 04_note_faisabilite.md
│   ├── 05_cahier_des_charges.md
│   ├── 06_journal_de_projet.md
│   ├── 07_PV_recette.md
│   ├── 08_plan_deploiement_onboarding.md
│   └── 09_bilan_3_mois_roadmap.md
└── README.md                     # Ce fichier
```

---

## 🧠 Démarche projet

Ce repo n'est pas qu'un fichier .pbix : il documente l'**ensemble du cycle de vie projet**, du premier échange avec le commanditaire au bilan d'usage à 3 mois.

| Phase | Document associé |
|-------|------------------|
| Découverte du besoin | [CR R1](docs/03_CR_R1_decouverte.md) |
| Étude de faisabilité | [Note de faisabilité](docs/04_note_faisabilite.md) |
| Cadrage formel | [Cahier des charges](docs/05_cahier_des_charges.md) |
| Suivi d'exécution | [Journal de projet](docs/06_journal_de_projet.md) |
| Validation livrable | [PV de recette](docs/07_PV_recette.md) |
| Mise en production | [Plan de déploiement & onboarding](docs/08_plan_deploiement_onboarding.md) |
| Mesure de la valeur | [Bilan à 3 mois](docs/09_bilan_3_mois_roadmap.md) |

Cette approche illustre une **double casquette analyste BI / chef de projet** : la qualité technique compte, mais c'est l'adoption et la valeur business produite qui font le succès.

---

## 🛠️ Stack technique

**Préparation des données** (Python)
- pandas pour la manipulation
- Factorisation en fonction réutilisable pour les indicateurs Banque Mondiale
- Harmonisation des codes ISO 3 sur les 4 sources

**Modélisation et visualisation** (Power BI Desktop)
- Power Query (langage M) pour le raffinement final
- Modèle en étoile avec dimensions partagées
- ~25 mesures DAX :
  - Mesures de base (moyennes, comptages)
  - Time intelligence (N-1, moyenne mobile 3 ans)
  - Classements (RANKX)
  - Catégorisation conditionnelle (SWITCH)
  - Mesures contextuelles avec virtual tables (`ALLEXCEPT`)

**Mesure phare du projet** : `Écart au modèle`, qui calcule pour chaque pays son écart à la moyenne de bonheur de son continent. Permet d'identifier les pays qui sur ou sous-performent leur contexte régional.

---

## 🎓 Ce que ce projet illustre

✅ **Réconciliation multi-sources** : 4 référentiels pays harmonisés via code ISO, gestion documentée des cas particuliers (Kosovo, territoires non-ISO)

✅ **Pragmatisme technique** : pivot d'une régression linéaire complexe vers une mesure plus simple (écart à la moyenne du continent) après diagnostic de bug DAX

✅ **Storytelling data** : navigation par questions plutôt que par intitulés, fil narratif d'une page à l'autre

✅ **Pensée produit** : page d'accueil avec parcours utilisateur, plus d'un dashboard juxtaposant des visuels

✅ **Maturité projet** : cadrage en 2 réunions, traçabilité des décisions, mesure d'usage et de valeur

---

## 🔄 Reproduire le projet

### Pré-requis
- Python 3.9+ avec pandas
- Power BI Desktop (version récente)

### Étapes
```bash
# 1. Cloner le repo
git clone https://github.com/[ton-username]/10_ans_de_bonheur_dans_le_monde.git
cd 10_ans_de_bonheur_dans_le_monde

# 2. Télécharger les sources brutes (cf. liens section "Sources")
# Les placer dans data/raw/

# 3. Lancer le notebook de nettoyage
jupyter notebook notebooks/nettoyage.ipynb

# 4. Ouvrir le rapport Power BI
# Ouvrir powerbi/10_ans_de_bonheur.pbix dans Power BI Desktop
# Mettre à jour les chemins dans Power Query si nécessaire
# Cliquer "Actualiser"
```

---

## 📈 Limites connues

- **Données 2024-2025 partielles** sur Banque Mondiale et V-Dem : les mesures utilisent la dernière valeur disponible.
- **Causalité non démontrée** : les corrélations affichées ne prouvent pas de lien causal. Disclaimer présent dans le rapport.
- **WHR est une enquête perçue** : biais culturels possibles dans la déclaration de bonheur entre pays.
- **Petits États non couverts** par V-Dem (~20 pays) : exclus de la page Démocratie.

---

## 📜 Licence et données

- Code (notebooks, fichiers Power BI) : MIT
- Données : sources publiques, citées en début de README, soumises à leurs licences respectives

---

## 👤 Auteur

Projet réalisé dans le cadre d'un test technique et d'une démarche portfolio, mettant en avant un profil **Data Analyst / BI Analyst avec sensibilité chef de projet**.

📧 Contact : [ton-email]
💼 LinkedIn : [ton-profil]

---

*Si ce projet t'intéresse ou si tu veux échanger sur la démarche, n'hésite pas à ouvrir une issue ou à me contacter directement.*
