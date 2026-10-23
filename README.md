# D2B : Design to Build

> Méthode de développement produit assistée par IA. Une personne seule, ou une très petite équipe, conçoit et livre un produit de qualité production en orchestrant trois rôles IA complémentaires.

**Version :** 2.0 (octobre 2026), anciennement ADD (Agent-Driven Development)
**Auteur :** Pierre Verfaillie · [pvfl.fr](https://pvfl.fr)

---

## En une phrase

On **conçoit** avec un outil de design, on **challenge** avec un LLM conversationnel, on **construit** avec un agent de code en terminal, et l'humain **décide**.


---

## 1. Les quatre rôles

| Rôle | Outil de référence | Responsabilité | Ne fait pas |
|---|---|---|---|
| **Concevoir** | Claude Design | Maquettes desktop et mobile, design system (tokens, typo, couleurs), états (vide, erreur, chargement), écrans admin | N'écrit pas le code de production |
| **Challenger** | LLM conversationnel (Claude, dans un projet dédié) | Cadrage, arbitrages, revue des plans et des rapports de l'agent, rédaction des consignes, vérification des faits, garde-fous sécurité et légaux | Ne code pas, ne décide pas à la place de l'humain |
| **Construire** | Claude Code (CLI) | Scaffold, code, migrations, tests, build, déploiement, documentation | Ne tranche pas seul les arbitrages de marque, légaux ou chiffrés : il les remonte |
| **Décider** | L'humain (product owner) | Vision, validation des plans, arbitrages, actions irréversibles, secrets | Ne colle jamais un secret dans une conversation |

Les outils sont interchangeables. Les **rôles**, eux, ne le sont pas : c'est leur séparation qui fait la qualité.

---

## 2. Le flow

```
 Cadrer ──► Concevoir ──► Planifier ──► Construire ──► Vérifier ──► Revoir ──► Consigner
 (Challenger) (Design)     (Build + OK     (Build, par    (Build)      (Challenger  (Build)
                            humain)         lot)                        + humain)
                                 ▲                                          │
                                 └────────────── lot suivant ◄──────────────┘
```

| Étape | Entrée | Sortie |
|---|---|---|
| **0. Cadrer** | Une idée, un besoin | Brief, stack, règles non négociables, `CLAUDE.md` initial |
| **1. Concevoir** | Brief | Maquette exportée dans `docs/design/` |
| **2. Planifier** | `CLAUDE.md` + maquette | Plan du lot (arborescence, composants, décisions), **validé avant tout code** |
| **3. Construire** | Plan validé | Code du lot, poussé sur une branche ou sur `main` |
| **4. Vérifier** | Code | Build, lint, typage, tests, Lighthouse, HTML servi, console propre |
| **5. Revoir** | Rapport de fin de lot | Liste d'arbitrages tranchés par l'humain, avec l'aide du Challenger |
| **6. Consigner** | Arbitrages | `docs/sessions/AAAA-MM-JJ.md`, `docs/decisions.md`, README et `CLAUDE.md` mis à jour |

---

## 3. Artefacts du repo

```
CLAUDE.md               Source de vérité : mission, stack, règles, arborescence, conventions
_inbox/                 Dépôt brut de l'humain (images, exports). L'agent range puis vide. Ignoré par git
docs/
  design/               Maquettes et exports de design (référence visuelle versionnée)
  decisions.md          Journal des décisions : quoi, pourquoi, alternatives écartées
  sessions/             Une synthèse par session : fait, écarts, à trancher, prochaine étape
.env.example            Noms des variables, jamais les valeurs
```

**Règle d'or :** si une décision n'est pas écrite dans le repo, elle n'existe pas pour la session suivante.

---

## 4. Les réflexes

1. **Plan avant code.** Corriger un plan coûte 30 secondes, corriger du code coûte une session.
2. **Lot livré à 100 %.** Pas de demi-fonctionnalité en production : un lot est fini, testé et documenté, ou il n'est pas livré.
3. **L'agent remonte, l'humain tranche.** Tout écart avec la maquette ou le plan, tout choix de marque, tout chiffre affiché, toute mention légale est signalé, jamais décidé en silence.
4. **Le Challenger relit chaque rapport.** Un second regard qui n'a pas écrit le code repère les angles morts : duplication de contenu, performance, sécurité, conformité.
5. **Vérifier plutôt que deviner.** Une adresse légale, un prix ou une version de dépendance se sourcent ; ils ne s'inventent pas.
6. **Sécurité par défaut fermé.** Accès refusé tant qu'il n'est pas explicitement accordé (RLS, grants, inscriptions fermées). Le middleware n'est jamais la seule barrière.
7. **Les secrets restent hors des conversations** et de l'historique du terminal ; ils vont directement dans les fichiers d'environnement ou le gestionnaire de secrets.
8. **Une seule source de vérité.** Un prix, un texte ou un lien est défini à un seul endroit et lu partout ailleurs.
9. **Actions irréversibles = humain.** DNS, suppression de données, publication, paiement : l'agent prépare, l'humain exécute ou confirme.

---

## 5. Anti-patterns

| Anti-pattern | Conséquence | Correctif D2B |
|---|---|---|
| Coder directement depuis une idée | Architecture improvisée, refonte au lot 2 | Cadrer, concevoir, planifier |
| Lancer l'agent sans `CLAUDE.md` | Chaque session repart de zéro | `CLAUDE.md` avant la première ligne de code |
| Laisser l'agent arbitrer seul | Choix de marque ou légaux faits par défaut | Section « à trancher » obligatoire dans chaque rapport |
| Maquette jointe au chat, jamais versionnée | Référence perdue après la session | Maquette dans `docs/design/` |
| Accepter le rapport de l'agent sans relecture | Écarts silencieux en production | Revue par le Challenger |
| Optimiser un score de labo au détriment du produit | Identité sacrifiée pour un chiffre simulé | Mesurer sur les vrais utilisateurs avant d'arbitrer |
| Dater une migration à la main | Migration antérieure au dernier état appliqué, que l'outil refuse d'insérer | Horodatage réel à la création, renumérotation avant le push, jamais de `--include-all` |

---

## 6. Modèles de consignes

**Lancement d'un lot (à l'agent)**
```
Lis CLAUDE.md et docs/design/.
Lot N : [périmètre]. Hors périmètre : [liste].
Avant de coder, présente ton plan (arborescence, composants, décisions) et attends ma validation.
En fin de lot : build, lint et typage verts, vérifications [liste], synthèse dans docs/sessions/,
et une section « À trancher » pour chaque écart ou choix que tu n'as pas pu faire seul.
```

**Revue d'un rapport (au Challenger)**
```
Voici le rapport de fin de lot de l'agent. Évalue le plan et les écarts :
ce qui est solide, ce qui est risqué (SEO, perf, sécurité, légal, cohérence de marque),
ce que je dois trancher. Rédige ensuite la réponse à transmettre à l'agent.
```

**Retour d'arbitrage (à l'agent)**
```
Arbitrages : 1. [...] 2. [...]
Règles à ajouter à CLAUDE.md : [...]
Ensuite : [lot ou étape suivante].
```

---

## 7. Quand l'utiliser, et ses limites

**Adapté à :** sites et applications web ou mobiles, MVP, outils internes, prototypes à faire passer en production, portage d'un design vers du code.

**Limites :**
- La méthode accélère la production ; elle ne remplace ni un audit de sécurité, ni une validation UX avec de vrais utilisateurs, ni un avis juridique.
- La qualité dépend des réflexes de la personne qui pilote : l'outillage seul ne suffit pas.
- Pour un produit qui exige plusieurs expertises pointues en parallèle, D2B se combine avec des experts humains ; il ne les remplace pas.

---

## Historique

| Version | Date | Changement |
|---|---|---|
| 1.x | 2025-2026 | ADD (Agent-Driven Development) : workflow à deux Claude, stratégie et exécution |
| 2.0 | Octobre 2026 | Renommée D2B (Design to Build) : ajout du rôle Concevoir, formalisation du flow, des artefacts et des réflexes |
