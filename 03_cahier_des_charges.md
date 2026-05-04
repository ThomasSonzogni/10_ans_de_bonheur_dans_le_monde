# Cahier des charges
## Projet « Pilotage bien-être collaborateurs vs benchmark pays »

---

**Version** : 1.0 — validée
**Date** : 22 mai 2026 (réunion R2)
**Maîtrise d'ouvrage** : Élise Marchand, Directrice RSE — Lumea Group
**Co-sponsor** : DRH Groupe
**Maîtrise d'œuvre** : [Moi], Analyste BI / Chef de projet data

**Statut** : co-construit en séance, validé à l'issue de la R2.

---

## 1. Contexte et enjeu

Lumea Group, multinationale présente dans 17 pays via 23 implantations, observe des écarts importants de satisfaction collaborateurs entre sites sans pouvoir objectiver leur origine. Les budgets RSE/QVT sont actuellement répartis de manière uniforme, sans ciblage des sites en réelle difficulté.

**Hypothèse stratégique** : une partie des écarts de satisfaction interne reflète le contexte national (bonheur du pays). Identifier les sites qui sur ou sous-performent **par rapport à leur contexte** permettra de mieux cibler les actions et d'augmenter l'impact des budgets QVT.

---

## 2. Objectifs

### 2.1 Objectif stratégique
Améliorer le ciblage et l'efficacité des plans d'action RSE/QVT en s'appuyant sur une comparaison interne vs benchmark externe.

### 2.2 Objectifs opérationnels
- Disposer d'un outil de pilotage interactif accessible à la Direction RSE, au DRH et à leurs équipes
- Identifier annuellement **les 3 implantations prioritaires** pour un plan d'action ciblé
- Suivre trimestriellement l'évolution des écarts
- Alimenter le rapport extra-financier avec des données objectivées (sortie visuelle exportable)

---

## 3. Périmètre

### 3.1 Inclus dans le projet (v1)

✅ Rapport Power BI publié sur Power BI Service (Option B retenue)
✅ Sources externes : World Happiness Report, Banque Mondiale, V-Dem
✅ Source interne : enquête de satisfaction collaborateurs Lumea (5 ans d'historique)
✅ Référentiel pays unifié (ISO 3 lettres)
✅ Row-Level Security (RLS) selon périmètre RH régional
✅ Documentation utilisateur + session de formation
✅ Fonctions d'export visuel pour le rapport RSE annuel

### 3.2 Exclu du projet (v1)

❌ Données individuelles (le projet reste au niveau site agrégé)
❌ Sites < 10 répondants (filtrés pour confidentialité)
❌ Modèles prédictifs / IA
❌ Intégration au datawarehouse Lumea (envisagée en v2 si succès)
❌ Multilingue (FR uniquement en v1)
❌ Mobile dédié (Power BI Mobile suffisant en v1)

### 3.3 Hors périmètre — explicitement écarté

🚫 Comparaison nominative inter-managers
🚫 Lien individuel salaire / satisfaction (RGPD + sensibilité sociale)
🚫 Toute donnée non strictement nécessaire au cas d'usage

---

## 4. Critères de succès

Critères quantifiés validés par les sponsors :

| # | Critère | Cible | Modalité de mesure |
|---|---------|-------|--------------------|
| 1 | Taux d'adoption — utilisateurs cibles ayant consulté ≥ 1×/trimestre | ≥ 80% à 6 mois | Audit Power BI Service |
| 2 | Identification de sites prioritaires lors du COMEX RSE annuel | 3 sites | Compte-rendu COMEX |
| 3 | Délai entre publication du WHR annuel et mise à jour du rapport | ≤ 5 jours ouvrés | Logs d'actualisation |
| 4 | Satisfaction utilisateur (enquête à 3 mois) | ≥ 4/5 | Mini-sondage 5 questions |
| 5 | Présence de visuels issus de l'outil dans le rapport extra-financier | ≥ 2 visuels | Rapport publié |
| 6 | Plan d'action RSE/QVT ciblé déployé sur les sites identifiés | Oui/Non | Validation DRH N+1 |

**Le projet sera considéré comme un succès si au moins 4 des 6 critères sont atteints à 6 mois.**

---

## 5. Engagements réciproques

### 5.1 Engagements du sponsor (Direction RSE + DRH)

- Mettre à disposition la donnée enquête interne dans les délais convenus
- Désigner un référent métier joignable sous 48h
- Participer aux points d'avancement (5 jalons prévus)
- Valider chaque livrable de phase dans un délai ≤ 3 jours ouvrés
- Faciliter l'accès aux interlocuteurs IT et RGPD si nécessaire
- Communiquer l'outil aux utilisateurs cibles à la livraison

### 5.2 Engagements du chef de projet / MOE

- Respecter le planning convenu (cf. § 7) ou alerter sous 48h en cas de dérive
- Tenir un journal de projet partagé (décisions, ajustements)
- Organiser les points de validation à chaque jalon
- Livrer une documentation complète permettant la reprise par un tiers
- Former les utilisateurs cibles à l'usage de l'outil (1 session)
- Assurer un support de niveau 1 pendant les 3 premiers mois

### 5.3 Engagements partagés

- Communication transparente sur les difficultés rencontrées
- Décisions documentées et tracées (compte-rendus systématiques)
- Adaptation du périmètre possible mais formalisée (avenant)

---

## 6. Architecture technique retenue

**Option B** : rapport Power BI publié sur Power BI Service.

**Sources de données**
- Externes (publiques) : WHR, Banque Mondiale, V-Dem — chargées via CSV puis via API en v1.5 si pertinent
- Interne : enquête satisfaction Lumea — fichier Excel sur SharePoint RH avec passerelle Power BI

**Modélisation** : modèle en étoile, table de faits par source, dimensions partagées (Pays, Calendrier).

**Sécurité**
- Authentification Azure AD existante
- Row-Level Security par région RH
- Pas de stockage hors tenant Lumea

**Fréquence d'actualisation**
- WHR : annuelle (mars) après publication
- Banque Mondiale, V-Dem : annuelle
- Enquête interne : trimestrielle

---

## 7. Planning et jalons

| Jalon | Date cible | Livrable | Validation |
|-------|------------|----------|------------|
| J0 — Cahier des charges | 22 mai | Ce document | ✅ Sponsors |
| J1 — Lot 1 : préparation des données | 28 mai | Modèle de données + qualité validée | Sponsor + moi |
| J2 — Lot 2 : socle de mesures et KPI | 4 juin | Mesures DAX + tests cohérence | Sponsor + moi |
| J3 — Lot 3 : maquettes visuelles | 11 juin | 4 pages prototypes | Sponsor + 1 utilisateur |
| J4 — Lot 4 : finalisation et recette | 18 juin | Rapport complet, RLS, doc utilisateur | Recette formelle |
| J5 — Mise en production & formation | 25 juin | Publication + 1 session formation | DRH + RSE |
| J6 — Bilan à 3 mois | 25 sept. | Rapport d'adoption + axes d'évolution | Comité de pilotage |

**Points hebdomadaires** : 30 minutes le mercredi, sponsors + moi.
**Format** : statut (🟢🟠🔴) sur chaque lot + risques + décisions à prendre.

---

## 8. Gouvernance

### 8.1 Comité de pilotage (COPIL)
- Élise Marchand (Sponsor RSE)
- DRH Groupe (Co-sponsor)
- Représentant DSI (consultatif)
- Moi (rapporteur)

**Fréquence** : à chaque jalon majeur (J3, J5, J6).

### 8.2 Comité opérationnel
- Karim Benali (référent métier)
- Moi
- Représentant équipe RH régionale (utilisatrice pilote)

**Fréquence** : hebdomadaire.

### 8.3 Décisionnaires

| Type de décision | Décideur |
|------------------|----------|
| Périmètre fonctionnel | Sponsor (Élise) |
| Architecture technique | Moi (en accord DSI) |
| Sécurité / RGPD | DRH + RGPD |
| Ajustement planning | Moi (avec alerte sponsor) |
| Avenant au périmètre | COPIL |

---

## 9. Gestion des changements

Toute évolution du périmètre ou du planning fera l'objet :
- D'une **alerte écrite** dans les 48h
- D'une **fiche d'écart** documentant impact, alternatives, recommandation
- D'une **décision tracée** dans le journal de projet

**Principe** : on n'élargit pas le scope sans formaliser. On ne réduit pas le scope sans le justifier.

---

## 10. Annexes

- Annexe 1 : Compte-rendu R1 du 12 mai 2026
- Annexe 2 : Note de faisabilité du 18 mai 2026
- Annexe 3 : Modèle de données prévisionnel (à joindre J1)
- Annexe 4 : Charte RGPD enquête interne (fournie par DPO)

---

## 11. Signatures

| Fonction | Nom | Date | Validation |
|----------|-----|------|------------|
| Sponsor RSE | Élise Marchand | 22/05/2026 | ✅ |
| Co-sponsor RH | DRH Groupe | 22/05/2026 | ✅ |
| Chef de projet / MOE | [Moi] | 22/05/2026 | ✅ |

---

*Document co-construit en séance R2 le 22 mai 2026. Toute modification ultérieure devra faire l'objet d'un avenant validé par les signataires.*
