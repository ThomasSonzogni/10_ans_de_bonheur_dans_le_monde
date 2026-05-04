# Procès-verbal de recette
## Projet « Pilotage bien-être collaborateurs vs benchmark pays »

---

**Date de recette** : 18 juin 2026
**Lieu** : Salle de réunion + visio
**Durée** : 1h30

**Participants**
- Élise Marchand — Sponsor RSE (recetteuse)
- DRH Groupe — Co-sponsor (recetteur)
- Camille R. — Utilisatrice pilote (RH régional Europe)
- [Moi] — Maîtrise d'œuvre

---

## 1. Objet de la recette

Vérifier que le rapport Power BI livré répond aux objectifs et critères de succès définis dans le cahier des charges du 22 mai 2026.

**Périmètre testé** :
- 5 pages du rapport (Accueil + 4 pages thématiques)
- Modèle de données et mesures DAX
- Navigation et expérience utilisateur
- Gestion des droits d'accès (RLS)
- Documentation utilisateur

---

## 2. Méthode de recette

La recette s'est déroulée en 3 phases successives :

**Phase 1 — Tests fonctionnels** (30 min)
Vérification que chaque visuel répond aux questions définies dans le cahier des charges. Le chef de projet présente, les recetteurs valident.

**Phase 2 — Test utilisateur libre** (30 min)
L'utilisatrice pilote (Camille R.) navigue librement dans le rapport sans aide, en commentant à voix haute. Test de l'intuitivité.

**Phase 3 — Vérification des critères de succès** (30 min)
Revue des 6 critères du cahier des charges, point par point.

---

## 3. Tests fonctionnels — résultats

| # | Test | Résultat attendu | Résultat obtenu | Statut |
|---|------|------------------|-----------------|--------|
| T1 | Carte mondiale 2024 | Pays nordiques en vert, Afghanistan en rouge | Conforme | ✅ |
| T2 | Top 10 page 1 | Finlande en tête | Conforme | ✅ |
| T3 | Filtre année | Tous les visuels réagissent au slicer | Conforme | ✅ |
| T4 | Évolution mondiale | Visible dip en 2020 (COVID) | Conforme + bande grise visible | ✅ |
| T5 | Navigation accueil → pages | Boutons fonctionnels avec hover | Conforme | ✅ |
| T6 | Navigation page → accueil | Bouton retour visible et actif | Conforme | ✅ |
| T7 | Page atypiques — Israël | Surperformance ~+1,95 | +1,95 conforme | ✅ |
| T8 | Page démocratie | Démocraties pleines > autocraties | Écart de +2,3 points | ✅ |
| T9 | Mesure `Catégorie Bonheur` | Affichage avec emojis 🟢🟡🟠🔴 | Conforme | ✅ |
| T10 | Mise en forme conditionnelle | Dégradé visible sur les écarts | Conforme | ✅ |

---

## 4. Test utilisateur libre — observations

**Profil** : Camille R., RH régional Europe, jamais utilisé Power BI auparavant.

**Consigne** : « Ouvre le rapport et trouve-moi les pays européens qui sous-performent. »

**Observations** :
- ✅ Trouve l'accueil immédiatement, lit le pavé d'introduction
- ✅ Hésite 5 secondes entre les 4 boutons, finit par choisir « Pays atypiques » (intuitif)
- ✅ Filtre sur le continent Europe sans hésitation
- ✅ Identifie le tableau « Moins heureux que la moyenne de leur continent »
- ⚠️ Cherche pendant 30 secondes comment revenir à l'accueil (le bouton retour est petit)
- ✅ Comprend le score d'écart sans explication

**Verbatim utilisatrice** :
> « *Le format question dans le titre des pages, c'est très clair. On sait tout de suite ce qu'on va trouver.* »
> « *La page atypiques est celle qui m'aidera le plus dans mon travail. Je l'aurais mise en premier dans la navigation.* »

---

## 5. Critères de succès du cahier des charges — revue

| # | Critère | Cible | Atteint à la livraison ? |
|---|---------|-------|---------------------------|
| 1 | Taux d'adoption ≥ 80% à 6 mois | Mesurable à 6 mois | À mesurer (livraison T0) |
| 2 | 3 sites prioritaires identifiés au COMEX RSE | Validation COMEX | À mesurer (prochain COMEX) |
| 3 | Délai mise à jour ≤ 5 jours après publication WHR | Logs d'actualisation | Process documenté ✅ |
| 4 | Satisfaction utilisateur ≥ 4/5 à 3 mois | Mini-sondage | À mesurer |
| 5 | ≥ 2 visuels dans le rapport extra-financier | Rapport publié | Process d'export validé ✅ |
| 6 | Plan d'action RSE/QVT déployé | Validation DRH N+1 | À mesurer |

**Constat** : 4 critères sur 6 ne sont mesurables qu'à 3-6 mois. Les 2 critères à valider à la livraison sont **conformes**. Le bilan définitif aura lieu en septembre 2026.

---

## 6. Anomalies relevées et traitement

| # | Anomalie | Sévérité | Traitement | Statut |
|---|----------|----------|------------|--------|
| A1 | Bouton retour Accueil trop petit (40×40) | Mineure | Agrandi à 50×50 | ✅ Corrigé en séance |
| A2 | Tooltip carte mondiale sans le score N-1 | Mineure | Ajouté info-bulle personnalisée | ✅ Corrigé en séance |
| A3 | Slicer continent ne se réinitialise pas après navigation | Cosmétique | À traiter en v1.1 | ⏸ Reporté |

**Aucune anomalie bloquante.** Le rapport est validé pour mise en production.

---

## 7. Points d'attention / réserves émises

**Par les sponsors** :
- ✅ Acceptation du **scope v1**.
- ⚠️ Demande explicite : à 3 mois, **mesure d'usage et bilan d'adoption** (déjà prévu).
- ⚠️ Volonté d'élargir en **v2** à d'autres dimensions (engagement, turnover).

**Par l'utilisatrice pilote** :
- ⚠️ Souhaite une **session de formation collective** avant déploiement large (prévue le 25 juin).

---

## 8. Décision finale

✅ **Le projet est accepté en l'état.**

Les anomalies mineures sont corrigées en séance. L'anomalie cosmétique (A3) est reportée à une éventuelle v1.1 sans impact bloquant.

La **mise en production est autorisée** pour le 25 juin 2026, en lien avec la session de formation utilisateurs.

---

## 9. Engagements post-livraison

| Engagement | Responsable | Échéance |
|------------|-------------|----------|
| Session de formation utilisateurs (45 min) | Moi | 25 juin |
| Support de niveau 1 pendant 3 mois | Moi | Jusqu'au 25 sept. |
| Mini-sondage satisfaction à 3 mois | Moi + Élise | 25 sept. |
| Bilan d'adoption à 3 mois | Moi | 25 sept. |
| Décision v1.1 / v2 | COPIL | À 6 mois |

---

## 10. Signatures

| Fonction | Nom | Date | Validation |
|----------|-----|------|------------|
| Sponsor RSE | Élise Marchand | 18/06/2026 | ✅ |
| Co-sponsor RH | DRH Groupe | 18/06/2026 | ✅ |
| Utilisatrice pilote | Camille R. | 18/06/2026 | ✅ |
| Maîtrise d'œuvre | [Moi] | 18/06/2026 | ✅ |

---

*PV rédigé le 18 juin 2026, validé en séance par signature électronique des participants.*
