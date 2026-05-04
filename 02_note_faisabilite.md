# Note de faisabilité technique
## Projet « Pilotage bien-être collaborateurs vs benchmark pays »

---

**Version** : 1.0
**Date** : 18 mai 2026
**Auteur** : [Moi], Analyste BI / Chef de projet data
**Destinataires** : Élise Marchand (RSE), DRH Groupe, Karim Benali

**Statut** : à valider lors de la R2 du 22 mai 2026.

---

## 1. Objectif de cette note

Suite à la R1 du 12 mai, cette note présente :
- L'analyse de faisabilité du besoin reformulé
- 3 options d'architecture envisageables, avec leurs avantages, inconvénients et coûts
- Une recommandation argumentée
- Les points qui devront être tranchés en R2

L'objectif est de **fournir au commanditaire les éléments pour faire un choix éclairé**, pas de lui imposer une solution.

---

## 2. Rappel du besoin (validé en R1)

> Disposer d'un outil de pilotage permettant de comparer la satisfaction des collaborateurs Lumea par implantation à un benchmark externe (bonheur du pays), pour identifier les sites en sur/sous-performance et prioriser les plans d'action RSE/QVT.

---

## 3. Analyse de faisabilité

### 3.1 Données — disponibilité

| Source | Statut | Couverture | Risque |
|--------|--------|------------|--------|
| World Happiness Report 2015-2025 | ✅ Disponible (public) | 150+ pays, 11 ans | Faible |
| Banque Mondiale (PIB, espérance de vie) | ✅ Disponible (public) | Couverture mondiale | Données 2025 partielles |
| V-Dem (démocratie, corruption) | ✅ Disponible (public, inscription) | 180+ pays, 11 ans | Couverture incomplète sur petits États |
| Enquête satisfaction Lumea | ✅ Confirmé par Karim | 23 sites × 5 ans | Volumétrie répondants à confirmer |

**Conclusion** : toutes les données nécessaires sont disponibles. Aucun bloqueur.

### 3.2 Données — qualité

- **Référentiel pays** : risque d'incohérence entre noms de pays (« USA », « United States », etc.). **Mitigation** : utiliser le **code ISO 3 lettres** comme clé universelle, créer une table de correspondance.
- **Granularité** : WHR et benchmarks externes sont au niveau pays/an. L'enquête interne est au niveau site/an. Un pays peut contenir plusieurs sites Lumea — modélisation à prévoir.
- **Données manquantes 2025** : certains indicateurs Banque Mondiale ne sont pas encore publiés. Stratégie de repli : utiliser la dernière valeur disponible avec un indicateur visuel de fraîcheur.

### 3.3 Données — confidentialité (point sensible)

L'enquête interne contient des données qui pourraient être sensibles si on descend trop bas en granularité.

**Règles proposées** (à valider RGPD/IT) :
- Agrégation minimale au site (pas d'individu)
- Suppression des sites < 10 répondants pour éviter ré-identification
- Pas de stockage cloud externe (Power BI Service tenant Lumea uniquement)
- Row-Level Security : accès complet pour la Direction RSE et le DRH, accès limité au périmètre régional pour les RH régionaux

### 3.4 Compétences & charge

- Sur le périmètre data : compétences disponibles en interne (Power Query, DAX, modélisation)
- Sur le périmètre déploiement : dépendance à l'équipe IT pour publication Power BI Service et gestion des droits
- **Charge estimée** : entre 5 et 15 jours-homme selon l'option retenue (cf. comparatif ci-dessous)

---

## 4. Trois options d'architecture

### Option A — Rapport Power BI standalone (léger)

**Principe** : un fichier .pbix unique, données chargées en local, distribution par mail / SharePoint.

```
[CSV WHR + WB + V-Dem + Enquête] → Power BI Desktop (.pbix) → Diffusion fichier
```

**Avantages**
- Mise en œuvre rapide (5 j/h)
- Aucune dépendance IT lourde
- Maîtrise totale du périmètre par l'équipe RSE
- Coût marginal nul (licences existantes)

**Inconvénients**
- Pas d'actualisation automatique → mise à jour manuelle annuelle
- Pas de contrôle d'accès fin (qui peut ouvrir le fichier ?)
- Difficile à gouverner si le fichier circule
- Pas adapté à un usage trimestriel par plusieurs équipes
- Données internes potentiellement copiées sur des postes individuels

**Recommandé si** : POC, usage très restreint (3-4 personnes), budget zéro.

---

### Option B — Rapport publié sur Power BI Service avec actualisation programmée (recommandée)

**Principe** : .pbix publié sur le tenant Power BI de Lumea, sources connectées via passerelle, RLS activée, accès via app Power BI.

```
[Sources ext. via API/CSV] ──┐
                              ├─→ Dataset Power BI Service ──→ Rapport publié (RLS)
[Enquête interne SharePoint]─┘                                  ↓
                                                          Utilisateurs cibles
```

**Avantages**
- Actualisation automatique programmée (annuelle pour WHR, trimestrielle pour enquête)
- Contrôle des accès via Azure AD
- Row-Level Security : chaque RH régional voit son périmètre
- Audit des consultations (mesure d'adoption possible)
- Mise à jour centralisée — pas de version qui traîne
- Déploiement progressif possible (groupe pilote puis généralisation)

**Inconvénients**
- Charge supérieure : 8-10 j/h (configuration passerelle, RLS, droits)
- Dépendance à l'équipe IT pour la passerelle de données
- Nécessite licence Power BI Pro pour les utilisateurs (déjà déployée chez Lumea ✅)
- Délai supplémentaire pour la mise en production (~ 1 semaine IT)

**Recommandé si** : usage régulier, multi-utilisateurs, gouvernance importante.

---

### Option C — Solution complète avec entrepôt de données et automatisation full

**Principe** : intégration des sources dans le datawarehouse Lumea, modèle sémantique partagé, rapport Power BI sur dataset certifié, alertes automatiques.

```
[Sources externes API] ──→ Pipeline ETL ──→ Datawarehouse ──→ Dataset certifié
[Enquête HRIS]        ──→ (Azure DF/dbt) ──→  (Snowflake)  ──→ ↓
                                                            Rapport Power BI + alertes Teams
```

**Avantages**
- Données réutilisables par d'autres projets (ex : DRH pour pilotage RH, contrôle de gestion pour analyse coûts)
- Industrialisation complète : alertes Teams si un site décroche
- Actualisation quasi temps réel
- Modèle sémantique partagé, gouvernance forte
- Évolutif vers d'autres sources (engagement, turnover, formation)

**Inconvénients**
- Charge importante : 12-15 j/h
- Mobilisation forte de la Data Engineering team
- Coût d'infra (stockage, compute datawarehouse)
- Délai 4-6 semaines minimum
- Surdimensionné pour un usage initial RSE seulement

**Recommandé si** : intention de pérenniser et étendre l'outil au-delà du périmètre RSE.

---

## 5. Tableau comparatif synthétique

| Critère | Option A | Option B (recommandée) | Option C |
|---------|:--------:|:----------------------:|:--------:|
| Délai de mise en œuvre | ~1 semaine | ~3 semaines | ~6 semaines |
| Charge interne (j/h) | 5 | 8-10 | 12-15 |
| Coût infra | 0 | ~0 (existant) | Modéré |
| Actualisation auto | ❌ | ✅ | ✅ |
| Contrôle d'accès fin (RLS) | ❌ | ✅ | ✅ |
| Mesure d'adoption | ❌ | ✅ | ✅ |
| Évolutivité | Faible | Moyenne | Forte |
| Couverture du besoin reformulé | Partielle | **Complète** | Surdimensionnée |
| Risque RGPD | Élevé | Maîtrisé | Maîtrisé |

---

## 6. Recommandation

**Je recommande l'Option B** pour les raisons suivantes :

1. **Couvre exactement le besoin** exprimé en R1 : usage trimestriel, multi-utilisateurs, gouvernance des données internes, mesure d'adoption.
2. **Ratio valeur/effort optimal** : ~10 jours pour une solution pérenne et gouvernée.
3. **Réutilise l'existant** : licences Power BI déjà en place chez Lumea, pas de nouveau coût.
4. **Conforme RGPD** : RLS et audit répondent aux contraintes de confidentialité de l'enquête interne.
5. **Permet de tester avant d'industrialiser** : si le besoin se confirme et s'étend, on pourra basculer vers l'Option C dans 12-18 mois sans repartir de zéro (le modèle sémantique sera réutilisable).

L'Option A est sous-dimensionnée : elle ne permet pas le suivi trimestriel ni le contrôle d'accès demandés.
L'Option C est surdimensionnée : elle anticipe des usages non confirmés à ce stade. **Stratégie « start small, scale later » plus prudente.**

---

## 7. Points à trancher en R2

| # | Sujet | Décideur | Importance |
|---|-------|----------|:----------:|
| 1 | Validation de l'option d'architecture | Sponsor + DRH | 🔴 |
| 2 | Périmètre des accès (qui voit quoi via RLS ?) | DRH + RGPD | 🔴 |
| 3 | Format des exports pour le rapport extra-financier | RSE + Communication | 🟠 |
| 4 | Critères de succès quantifiés | Sponsor | 🔴 |
| 5 | Niveau d'implication du DRH (co-sponsor ou contributeur ?) | DRH | 🟠 |
| 6 | Calendrier — date butoir réaliste | Sponsor + Moi | 🔴 |
| 7 | Composition du comité de pilotage | Sponsor | 🟢 |

---

## 8. Risques projet identifiés

| Risque | Probabilité | Impact | Plan de mitigation |
|--------|:-----------:|:------:|---------------------|
| Faible volumétrie de répondants sur certains sites | Moyenne | Moyen | Seuil minimum + flag « non significatif » |
| Délai IT pour passerelle plus long que prévu | Moyenne | Élevé | Démarrer la demande IT en parallèle de la R2 |
| Sur-interprétation des écarts par les managers | Élevée | Moyen | Disclaimer + accompagnement DRH |
| Décrochage du sponsor (turnover, autres priorités) | Faible | Élevé | Co-sponsoring DRH, points mensuels |
| Évolution méthodologique du WHR rendant les comparaisons N-1 difficiles | Faible | Moyen | Documentation source + alertes en cas de rupture |

---

## 9. Annexe — Sources documentaires consultées

- World Happiness Report : worldhappiness.report
- Banque Mondiale (WDI) : data.worldbank.org
- V-Dem Institute : v-dem.net/data
- Référentiel ISO-3166 : github.com/lukes/ISO-3166-Countries-with-Regional-Codes

---

*Document préparé pour la R2 du 22 mai 2026.*
