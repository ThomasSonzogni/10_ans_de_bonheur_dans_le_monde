# Compte-rendu — Réunion de découverte (R1)
## Projet « Bonheur des pays — visualisation pour le rapport RSE »

---

**Date** : 12 mai 2026
**Durée** : 45 minutes
**Lieu** : Visio Teams

**Participants**
- Élise Marchand — Directrice RSE, Lumea Group (commanditaire)
- Karim Benali — Chargé de communication RSE
- [Moi] — Analyste BI / Chef de projet data, Direction Data & Performance

**Objet** : Cadrer la demande initiale, comprendre le besoin et les attentes.

---

## 1. Demande initiale exprimée

> « *On voudrait un dashboard qui montre le classement des pays les plus heureux, qu'on puisse mettre dans notre rapport RSE et sur l'intranet. Quelque chose de visuel, joli, qui parle aux gens. On a vu que le World Happiness Report sortait chaque année, on aimerait s'appuyer là-dessus.* »
> — Élise Marchand

**Échéance évoquée** : « *avant la fin du trimestre, idéalement pour la prochaine publication du rapport extra-financier* ».

---

## 2. Questions posées et éléments découverts

### 2.1 Sur l'usage et l'audience

**Q : À qui ce dashboard est-il destiné ? Qui va le consulter ?**
Réponse : « *Au début on pensait à tout le monde — les investisseurs via le rapport, les collaborateurs via l'intranet. En vrai, je ne sais pas qui irait cliquer dessus régulièrement.* »

**Q : Qu'est-ce qu'un utilisateur doit pouvoir faire après avoir consulté ce dashboard ?**
Long silence. Puis : « *Bonne question. On n'avait pas vraiment réfléchi à ça. On voulait juste illustrer notre engagement RSE.* »

**→ Constat** : la demande mélange deux usages très différents — **un visuel statique pour communication externe** (rapport annuel) et **un outil interactif pour usage interne** (intranet). Les deux n'appellent pas la même solution.

### 2.2 Sur le lien avec l'activité de Lumea

**Q : Quel est le lien entre le bonheur mondial et l'activité de Lumea Group ?**
Réponse : « *Honnêtement, c'est un peu indirect. On a 23 sites dans 17 pays, donc on touche à plein de réalités. Mais c'est vrai qu'afficher le classement des pays sans le relier à nous… ça fait un peu déconnecté.* »

**Q : Y a-t-il un sujet RH/social interne qui vous préoccupe et que cette donnée pourrait éclairer ?**
Réponse longue, plus animée : « *Ah oui, en fait Karim a fait remonter quelque chose. Nos enquêtes de satisfaction collaborateurs annuelles montrent des écarts énormes entre pays — notre site de Singapour est à 8.2/10, celui de Madrid à 5.4. On ne sait pas si c'est notre management qui est mauvais à Madrid ou si les Espagnols sont juste plus exigeants ou moins satisfaits en général. On galère à objectiver.* »

**→ Découverte clé** : il existe un **vrai besoin métier latent** non exprimé dans la demande initiale.

### 2.3 Sur le problème réel à résoudre

**Q : Si on pouvait répondre précisément à une question avec ce projet, quelle serait-elle ?**
Réponse (après réflexion) : « *Est-ce que nos collaborateurs sont plus heureux ou moins heureux que la moyenne de leur pays ? Et où devons-nous concentrer nos efforts en priorité ?* »

**Q : Cette information, à quoi servirait-elle concrètement ?**
Réponse : « *À nourrir notre plan d'action RSE / QVT. Aujourd'hui on saupoudre les budgets bien-être de manière égale entre sites. On gagnerait beaucoup à cibler là où il y a un vrai écart négatif vs le contexte local.* »

**Q : Ce serait utilisé à quelle fréquence ?**
Réponse : « *L'enquête interne est annuelle, le WHR aussi. Donc une vraie analyse une fois par an, mais une consultation trimestrielle pour le suivi des plans d'action serait utile.* »

---

## 3. Reformulation du besoin réel (validée en séance)

> « **Lumea Group souhaite disposer d'un outil de pilotage permettant de comparer la satisfaction de ses collaborateurs par implantation à un benchmark externe (bonheur du pays), afin d'identifier les sites en sur ou sous-performance et de prioriser les plans d'action RSE/QVT.** »

**Validation orale** : Élise et Karim confirment que cette reformulation correspond beaucoup mieux à ce qu'ils cherchent vraiment.

> « *C'est exactement ça, on n'avait juste pas réussi à le formuler comme ça. Ce qu'on demandait au départ, c'était la partie visible de l'iceberg.* » — Élise Marchand

---

## 4. Ce qui a évolué entre la demande et le besoin

| Dimension | Demande initiale | Besoin réel découvert |
|-----------|------------------|----------------------|
| **Objet** | Classement décoratif des pays | Comparaison interne vs externe |
| **Audience** | Tout le monde (flou) | Direction RSE, DRH, Top management |
| **Usage** | Illustrer l'engagement RSE | Prioriser les budgets QVT |
| **Format** | Visuel statique pour rapport PDF | Outil interactif + capacité d'export |
| **Fréquence** | Une fois par an | Annuelle (analyse) + trimestrielle (suivi) |
| **Décision attendue** | Aucune | Identifier 3 sites prioritaires/an |
| **Mesure du succès** | Non défini | Plan d'action ciblé déployé |

---

## 5. Données disponibles évoquées

- **Externe** : World Happiness Report (2015-2025), public et gratuit
- **Externe** : indicateurs Banque Mondiale, V-Dem (à confirmer pertinence)
- **Interne** : enquête annuelle de satisfaction collaborateurs (Karim Benali fournira le fichier — 23 sites × 5 ans d'historique)

**Point d'attention soulevé** : confidentialité des données internes. À traiter en phase de cadrage technique (RGPD, anonymisation, niveau d'agrégation, RLS).

---

## 6. Prochaines étapes convenues

| # | Action | Responsable | Échéance |
|---|--------|-------------|----------|
| 1 | Étude de faisabilité technique : sources, architectures, charge | Moi | + 1 semaine |
| 2 | Préparer 2-3 options d'architecture à présenter | Moi | + 1 semaine |
| 3 | Fournir le fichier d'enquête interne anonymisé | Karim Benali | + 3 jours |
| 4 | Réunion R2 de cadrage et choix de l'option | Tous | + 10 jours |
| 5 | Inviter le DRH groupe à la R2 | Élise Marchand | + 5 jours |

---

## 7. Décisions prises

✅ Le projet est **maintenu** mais son périmètre est **redéfini** par rapport à la demande initiale.
✅ Le DRH groupe sera **associé** comme co-sponsor (le besoin est mixte RSE/RH).
✅ Une **étude de faisabilité préalable** est nécessaire avant tout engagement de planning ferme.
⏸ Le format de restitution (intranet, rapport PDF, Power BI Service…) sera tranché à l'issue de la R2.

---

## 8. Points à clarifier avant la R2

- Volumétrie exacte de l'enquête interne (combien de répondants par site/an ?)
- Existence d'un référentiel pays groupe (codes ISO ? noms officiels ?)
- Contraintes IT : Power BI Service déjà déployé ? droits d'accès ?
- Budget temps disponible côté métier pour les itérations

---

*Compte-rendu rédigé le 12 mai 2026, validé par retour mail d'Élise Marchand le 13 mai 2026.*
