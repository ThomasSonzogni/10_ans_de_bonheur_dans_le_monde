# Journal de projet
## Bonheur mondial — Pilotage QVT/RSE Lumea Group

---

**Auteur** : [Moi], Chef de projet data
**Période** : 22 mai → 25 juin 2026
**Format** : 1 entrée par jalon ou décision significative
**Objectif** : tracer les décisions, ajustements et incidents pour assurer la transparence et permettre la reprise du projet par un tiers.

---

## J0 — 22 mai 2026 — Cadrage validé

✅ Cahier des charges signé par sponsors (Élise Marchand RSE, DRH Groupe).
🎯 Démarrage Lot 1 (préparation des données) prévu pour le 23 mai.

**Décision** : architecture validée = Option B (Power BI Service avec actualisation programmée).

---

## J0+1 — 23 mai 2026 — Démarrage technique

📥 Récupération des sources :
- WHR 2005-2025 : téléchargé depuis worldhappiness.report
- Banque Mondiale (4 indicateurs) : DataBank, en CSV
- V-Dem v16 : téléchargé après inscription gratuite
- ISO-3166 : récupéré depuis le repo GitHub de référence

⚠️ **Point d'attention** : le fichier V-Dem fait ~600 Mo brut avec des centaines de colonnes. Décision : filtrer dès le chargement Python via `usecols=` pour ne charger que les 5 indicateurs retenus. Cela ramène le DataFrame à quelques Mo en mémoire.

---

## J0+2 — 24 mai 2026 — Préparation des données (Python)

Création du notebook `nettoyage.ipynb` avec une cellule par source.

**Décision technique** : utiliser le **code ISO 3 lettres** comme clé universelle de jointure plutôt que le nom de pays. Les noms varient trop entre sources (USA / United States / United States of America).

**Incident résolu** : 20 pays du WHR sans correspondance ISO. Investigation détaillée :
- 4 cas de format de nom officiel (Netherlands, Bolivia, Iran, Venezuela)
- 9 cas de variantes mineures (Republic of Korea, DR Congo, etc.)
- 4 cas de renommages (Swaziland → Eswatini, etc.)
- 3 territoires non-ISO : Kosovo (ajouté manuellement avec code de facto XKX), North Cyprus et Somaliland (exclus avec traçabilité)

📄 **Décision tracée** : Kosovo conservé avec le code XKX (justification : présent dans la plupart des analyses comparatives internationales). Les 2 autres territoires sont documentés dans le README.

---

## J0+3 — 25 mai 2026 — Factorisation et exports

Code refactorisé en fonction `clean_wb_indicator()` pour les 4 fichiers Banque Mondiale (gain ~60 lignes de code).

**Décision** : exporter les fichiers en `utf-8-sig` pour assurer la compatibilité Power BI sur les caractères spéciaux (Türkiye, Côte d'Ivoire).

**Vérification couverture** : 158 pays présents dans les 3 sources principales, 1 pays uniquement dans WHR, 55 dans WB sans WHR (petits États non couverts par Gallup). Ce constat est documenté dans le notebook et le README.

---

## J1 — 28 mai 2026 — Lot 1 validé

✅ Modèle de données + qualité validée par sponsors.

📊 Démo en réunion hebdo : présentation du nettoyage, des règles d'harmonisation, et de la couverture finale.

**Retour sponsor** : « *Le travail de réconciliation des référentiels pays est plus important qu'on ne l'imaginait. Bien vu d'avoir documenté les exclusions.* » (Élise Marchand)

🎯 Démarrage Lot 2 (modèle Power BI + mesures DAX).

---

## J1+1 — 29 mai 2026 — Modélisation Power BI

Import des 4 CSV nettoyés. Création de `dim_calendrier` en M (table 2015-2025 avec colonnes `decade` et `periode` pour le storytelling COVID).

**Modèle en étoile** :
- 2 dimensions : `dim_pays`, `dim_calendrier`
- 3 faits : `fact_happiness`, `fact_economy`, `fact_democracy`
- Toutes les relations en cardinalité 1:N, direction simple

**Bonne pratique appliquée** : masquage des colonnes `country_code`, `country_name` et `year` côté faits (les utilisateurs doivent passer par les dimensions).

---

## J1+2 — 30 mai 2026 — Premières mesures DAX

Création de la table `_Mesures` (vide, organisation pure UX) et écriture des 8 mesures de base : `Score Bonheur`, `Nb Pays`, `PIB par hab Moyen`, etc.

**Incident résolu** : erreur `AVERAGE` sur la colonne `gdp_per_capita_ppp` qui était typée Texte au lieu de Nombre décimal. Corrigé en Power Query (changement de type explicite + remplacement des valeurs vides par null).

📌 **Leçon retenue** : forcer les types numériques en Python avant export pour éviter le souci à l'avenir.

---

## J2 — 4 juin 2026 — Lot 2 validé

✅ Toutes les mesures de base testées et cohérentes (score moyen 5,52 plausible, top pays Finlande, etc.).

📊 Création des mesures avancées : time intelligence (`Score Bonheur N-1`, `Évolution Bonheur`, `Moyenne Mobile 3 ans`), classement (`Rang Bonheur`), catégorisation (`Catégorie Bonheur`).

🎯 Démarrage Lot 3 (maquettes visuelles).

---

## J2+1 — 5 juin 2026 — Architecture des pages

Décision majeure suite à un échange avec le sponsor : transformer le rapport en **expérience utilisateur** plutôt qu'en série de pages indépendantes.

**Structure adoptée** :
- 1 page d'accueil (hub avec storytelling et 4 boutons de navigation)
- 4 pages thématiques répondant chacune à une question

**Justification** : un dashboard est consulté différemment selon les profils. La page d'accueil sert de point d'entrée commun et oriente l'utilisateur selon sa question du moment.

---

## J3 — 11 juin 2026 — Lot 3 validé

✅ 4 pages prototypes présentées aux sponsors et à un utilisateur pilote (RH régional Europe).

**Retour utilisateur pilote** : « *La page atypiques est très parlante, on voit immédiatement où concentrer les efforts. La page démocratie est plus académique, on l'utilisera moins.* »

**Décision** : maintenir les 4 pages mais réorganiser l'ordre des boutons (Vue globale → Facteurs → Atypiques en priorité, Démocratie en bonus).

---

## J3+1 — 12 juin 2026 — Incident technique majeur

🐛 **Bug détecté** sur la mesure `Bonheur Attendu (régression PIB)`. La régression linéaire complète en DAX donnait des valeurs identiques au score réel pour la plupart des pays (problème de contexte de filtre).

**Diagnostic** : 2h passées à essayer de corriger via `ALL`, `ALLEXCEPT`, `REMOVEFILTERS`. La complexité du calcul empêche un fonctionnement fiable.

**Décision pragmatique** : pivoter vers une **mesure plus simple** = écart à la moyenne du continent. Avantages :
- Plus simple à expliquer aux utilisateurs métier
- Plus robuste techniquement
- Répond à la même question business : « ce pays sur ou sous-performe-t-il par rapport à son contexte ? »

📌 **Communication sponsor** : note rapide envoyée à Élise pour expliquer le pivot. Validation immédiate.

---

## J3+2 — 13 juin 2026 — Page atypiques refondue

Mesures `Bonheur Attendu (continent)` et `Écart au modèle` corrigées.

**Test sur tableau** : Israël en tête des surperformances (+1,95), Hong Kong en tête des sous-performances. Résultats cohérents et immédiatement parlants.

📊 Mise en forme conditionnelle (dégradé rouge-blanc-vert) sur le nuage de points et les tableaux.

---

## J3+3 — 16 juin 2026 — UX et finalisation

Travail de finition :
- Page d'accueil avec 4 boutons de navigation (états Par défaut + Au survol)
- Bouton retour Accueil sur chaque page
- Slicers d'année et de continent harmonisés
- Titres dynamiques sur chaque visuel (titre + sous-titre)
- Bande verticale grise sur 2020-2021 (« COVID-19 ») dans le graphique d'évolution

**Décision UX** : les titres des pages sont formulés comme des **questions** (« Où en est le bonheur dans le monde ? ») plutôt que comme des intitulés (« Vue d'ensemble »). Effet « article » plutôt que « dashboard ».

---

## J4 — 18 juin 2026 — Lot 4 validé / Recette

✅ Recette formelle conduite avec sponsors + 1 utilisateur pilote :
- 6 critères de succès du cahier des charges revus
- Tests fonctionnels (filtres, navigation, drill-down)
- Tests utilisateurs (parcours libre 15 min)

**Anomalies relevées** : 2 mineures, corrigées dans la journée.

📄 PV de recette signé. Cf. document associé.

---

## J5 — 25 juin 2026 — Mise en production

📦 Publication sur Power BI Service (espace de travail RSE Lumea).

**Configuration RLS** : règles définies avec le DRH (RH régional voit son périmètre, Direction RSE voit tout).

**Actualisation programmée** :
- WHR : annuelle, manuelle (le fichier change de structure parfois)
- Banque Mondiale : annuelle, automatique via API (en projet pour v1.5)
- V-Dem : annuelle, manuelle
- Enquête interne : trimestrielle, automatique via SharePoint

🎓 **Session de formation** organisée : 45 min, 12 participants. Support de formation diffusé avec le rapport.

---

## Décisions tracées (résumé)

| # | Décision | Justification | Date |
|---|----------|---------------|------|
| D1 | Code ISO 3 comme clé de jointure | Stabilité vs noms de pays variables | 24/05 |
| D2 | Kosovo conservé (XKX) | Présent dans analyses comparatives internationales | 24/05 |
| D3 | Refactorisation en fonction Python | Lisibilité, gain de 60 lignes | 25/05 |
| D4 | Page d'accueil avec navigation | Améliorer UX pour profils mixtes | 05/06 |
| D5 | Pivot régression → écart au continent | Robustesse technique + clarté business | 12/06 |
| D6 | Titres en questions plutôt qu'intitulés | Effet « article » plutôt que « dashboard » | 16/06 |

---

## Risques apparus en cours de projet

| Risque | Survenu ? | Impact réel | Mitigation appliquée |
|--------|-----------|-------------|----------------------|
| Données 2024-2025 partielles | Oui | Moyen | Mesures avec dernière valeur disponible + indicateur de fraîcheur |
| Noms de pays divergents | Oui | Élevé | Investigation détaillée, mapping manuel documenté |
| Problème typage CSV → Power BI | Oui | Faible | Conversion explicite Power Query |
| Bug régression DAX | Oui (non anticipé) | Moyen | Pivot vers mesure plus simple |
| Décrochage sponsor | Non | — | Co-sponsoring DRH a sécurisé l'engagement |

---

*Journal tenu en continu, dernière mise à jour le 25 juin 2026.*
