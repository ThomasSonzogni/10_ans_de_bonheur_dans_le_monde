# Plan de déploiement & onboarding
## Bonheur mondial — Pilotage QVT/RSE Lumea Group

---

**Date du document** : 20 juin 2026
**Date de mise en production** : 25 juin 2026
**Auteur** : [Moi], Chef de projet data
**Audience** : équipe RSE, DRH, équipe IT, utilisateurs cibles

---

## 1. Objectif du document

Ce document décrit comment le rapport Power BI sera **techniquement déployé** et **fonctionnellement adopté** par les utilisateurs cibles.

Le succès d'un projet BI ne se mesure pas à la livraison du fichier, mais à son **usage régulier**. Ce plan formalise les actions pour passer de « rapport livré » à « outil utilisé ».

---

## 2. Volet technique — Déploiement

### 2.1 Architecture cible

```
[Sources externes : WHR, Banque Mondiale, V-Dem]
                  ↓ (chargement Python initial puis CSV propres)
[SharePoint RSE — dossier dédié]
                  ↓ (passerelle Power BI Gateway)
[Power BI Service — espace de travail "RSE Pilotage"]
                  ↓
[Application Power BI — accès via app.powerbi.com ou Power BI Mobile]
                  ↓
[Utilisateurs : RSE, DRH, RH régionaux, Top management]
```

### 2.2 Étapes de mise en production

| # | Étape | Responsable | Date |
|---|-------|-------------|------|
| 1 | Création de l'espace de travail Power BI dédié | DSI + Moi | 21 juin |
| 2 | Publication du .pbix depuis Power BI Desktop | Moi | 24 juin |
| 3 | Configuration de la passerelle de données | DSI | 24 juin |
| 4 | Configuration des actualisations programmées | Moi | 24 juin |
| 5 | Mise en place de la Row-Level Security (RLS) | Moi + DRH | 24 juin |
| 6 | Création du package « App » Power BI | Moi | 24 juin |
| 7 | Tests d'accès avec 3 utilisateurs pilotes | Moi + Camille R. | 25 juin matin |
| 8 | Diffusion à tous les utilisateurs cibles | Moi + Élise | 25 juin après-midi |

### 2.3 Configuration des actualisations

| Source | Fréquence | Mode | Délai max |
|--------|-----------|------|-----------|
| WHR | Annuelle (mars) | Manuel | J+5 ouvrés après publication |
| Banque Mondiale | Annuelle | Automatique (API en v1.5) | J+1 |
| V-Dem | Annuelle | Manuel | J+10 ouvrés |
| Enquête interne Lumea | Trimestrielle | Automatique (SharePoint) | J+1 |

### 2.4 Gestion des accès et sécurité (RLS)

| Rôle | Périmètre visible | Cas d'usage |
|------|-------------------|-------------|
| RSE Direction | Tous les pays + tous les sites Lumea | Pilotage global |
| DRH Groupe | Tous les pays + tous les sites | Pilotage RH global |
| RH régional Europe | Tous les pays + sites Lumea Europe uniquement | Plan d'action local |
| RH régional APAC | Tous les pays + sites Lumea APAC uniquement | Plan d'action local |
| Top management | Tous les pays + tous les sites en lecture seule | Reporting |

⚠️ **Principe de moindre privilège** : un utilisateur ne voit que ce dont il a besoin pour son rôle. Validé par le DPO.

### 2.5 Plan de continuité

| Situation | Plan d'action |
|-----------|---------------|
| Erreur d'actualisation | Notification email automatique → diagnostic dans la journée |
| Source externe indisponible | Affichage de la dernière donnée disponible avec indicateur visuel |
| Bug bloquant en production | Rollback vers version N-1 (sauvegarde mensuelle du .pbix) |
| Départ du chef de projet | Documentation complète permet la reprise par un autre profil data |

---

## 3. Volet humain — Onboarding utilisateurs

### 3.1 Parcours d'onboarding

L'erreur classique d'un projet BI réussi techniquement mais peu adopté : **livrer sans accompagner**. Le plan d'adoption est aussi important que le déploiement technique.

```
Avant      → Communication d'attente (J-7)
Jour J     → Session de formation collective (45 min)
J+15       → Email de relance + mini-tutoriel
J+30       → Premier point d'usage (groupe pilote)
J+90       → Mini-sondage de satisfaction
J+90       → Bilan d'adoption (cf. document de bilan)
```

### 3.2 Communication de pré-lancement (J-7)

📧 Email envoyé par Élise Marchand (Sponsor) à tous les utilisateurs cibles.

**Contenu type** :

> *Bonjour,*
>
> *Le 25 juin, vous aurez accès à un nouvel outil de pilotage : « Bonheur mondial — Lumea ». Il vous permettra de comprendre comment la satisfaction de nos collaborateurs se compare au contexte de chaque pays, pour mieux orienter nos plans d'action QVT.*
>
> *Une session de formation de 45 minutes est organisée le 25 juin à 14h (visio). Elle est ouverte à tous, fortement recommandée.*
>
> *À très bientôt,*
> *Élise Marchand, Direction RSE*

### 3.3 Session de formation (45 min)

**Date** : 25 juin 2026, 14h-14h45
**Format** : Visio Teams + replay disponible
**Public** : tous les utilisateurs cibles (~25 personnes)

**Plan détaillé** :

| Temps | Contenu | Format |
|-------|---------|--------|
| 0-5' | Pourquoi cet outil ? Le besoin métier | Présentation |
| 5-10' | Démo navigation : page d'accueil et 4 pages | Démo en direct |
| 10-25' | Cas d'usage concret : « Identifier les sites prioritaires » | Exercice guidé |
| 25-35' | Astuces : filtres, drill-through, exports | Démo |
| 35-45' | Q&R | Discussion |

**Support** : slide deck de 15 slides + replay vidéo + tutoriel écrit (4 pages).

### 3.4 Documentation utilisateur

Trois niveaux de documentation, accessibles depuis le rapport :

**Niveau 1 — Quick start (1 page)** :
- Comment accéder
- Les 5 fonctions principales
- Qui contacter en cas de problème

**Niveau 2 — Tutoriel (4 pages)** :
- Parcours guidé d'utilisation
- Cas d'usage concrets (3 scénarios)
- FAQ courte

**Niveau 3 — README technique** :
- Documentation pour la DSI et les futurs repreneurs
- Modèle de données, choix techniques, axes d'évolution

### 3.5 Support utilisateur

| Niveau | Qui | Quand | Comment |
|--------|-----|-------|---------|
| N1 (questions d'usage) | Moi | Pendant 3 mois post-livraison | Canal Teams dédié `#rse-pilotage` |
| N2 (problèmes techniques) | Moi + DSI | À la demande | Ticket Jira |
| N3 (évolutions fonctionnelles) | COPIL | Trimestriel | Réunion |

📌 **Engagement de support** : réponse aux questions N1 sous 24h ouvrées pendant les 3 premiers mois.

### 3.6 Champion users

Identification de **3 « champion users »** qui auront un rôle de relais :
- Camille R. (RH régional Europe) — utilisatrice pilote du projet
- Un référent APAC (à désigner)
- Un référent Amériques (à désigner)

Leur rôle :
- Premier point de contact local pour les questions
- Remontée des retours utilisateurs au chef de projet
- Aide à la diffusion de bonnes pratiques d'usage

---

## 4. Mesure du succès du déploiement

### 4.1 Indicateurs d'usage à suivre

Power BI Service permet d'auditer l'usage. Les indicateurs suivants sont collectés mensuellement :

| Indicateur | Source | Cible |
|------------|--------|-------|
| Nombre d'utilisateurs uniques / mois | Power BI Audit Logs | ≥ 80% à 6 mois |
| Nombre de sessions / mois | Power BI Audit Logs | En croissance T1 → T2 |
| Page la plus consultée | Power BI Audit Logs | Cohérent avec besoin métier |
| Temps moyen de session | Power BI Audit Logs | ≥ 3 min (lecture vs survol) |
| Tickets de support | Canal Teams | < 5 / mois après J+30 |

### 4.2 Indicateurs de valeur

Au-delà de l'usage, mesurer la **valeur produite** :

- Citation de l'outil dans le rapport extra-financier annuel
- Nombre de sites identifiés comme prioritaires en COMEX RSE
- Plans d'action QVT déployés sur la base de l'analyse
- Évolution du score interne sur les sites priorisés (à 12 mois)

---

## 5. Communication continue

| Action | Fréquence | Canal |
|--------|-----------|-------|
| Newsletter RSE — point sur l'outil | Mensuelle | Email |
| Témoignages utilisateurs | Trimestrielle | Intranet |
| Démo en COMEX RSE | Trimestrielle | Présentiel |
| Bilan d'usage et roadmap | Semestrielle | COPIL |

---

## 6. Risques d'adoption identifiés

| Risque | Probabilité | Mitigation |
|--------|-------------|------------|
| Faible utilisation post-formation | Moyenne | Champion users + relances ciblées |
| Mauvaise interprétation des écarts | Moyenne | Tooltips explicites + slide de cadrage en formation |
| Outil perçu comme « jugement » des managers | Élevée | Communication soigneuse : outil d'aide, pas de notation |
| Évolution méthodologique du WHR | Faible | Veille annuelle + adaptation si besoin |

⚠️ **Le risque majeur est le risque humain** : un manager local pourrait percevoir un score interne bas de son site comme une évaluation de sa performance. La communication initiale doit être très claire sur le fait que **l'outil sert à allouer des budgets, pas à juger des personnes**.

---

## 7. Annexes

- A1 : Slide deck de formation (15 slides)
- A2 : Tutoriel utilisateur (4 pages)
- A3 : README technique (cf. document associé)
- A4 : Charte de support utilisateur
- A5 : Modèle d'email de communication (J-7)

---

*Document validé par Élise Marchand (Sponsor) et le DRH Groupe le 22 juin 2026.*
