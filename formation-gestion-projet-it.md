# Formation : Piloter un projet IT — du code au cadrage

**Pour qui :** un dev (Python/FastAPI, Django, React) qui sait livrer du code mais veut savoir cadrer, planifier, prioriser, gérer une équipe et parler à un client/sponsor sans se noyer.

**Comment l'utiliser :** chaque module a une partie "comprendre" et une partie "template" à copier-coller directement dans tes projets (CourIA, PySTACK, formation backend, etc.). Ne lis pas ça comme un cours — applique chaque template sur un projet réel dès que tu le lis, sinon ça reste théorique.

---

## Module 1 — Fondamentaux de la gestion de projet IT

### Le cycle de vie d'un projet, version concrète

Peu importe la méthodo, un projet passe toujours par 4 phases. Ce qui change, c'est la taille des boucles.

1. **Cadrage** — Qu'est-ce qu'on construit, pour qui, pourquoi, avec quelles limites. C'est la phase la plus souvent bâclée par les devs qui veulent coder tout de suite. Erreur n°1 en gestion de projet.
2. **Planification** — Découper le travail, estimer, poser un calendrier réaliste, identifier les risques.
3. **Exécution** — Le développement, les livraisons, le suivi d'avancement.
4. **Clôture** — Recette, mise en prod, bilan, ce qu'on garde pour la prochaine fois.

En cycle en V, ces 4 phases sont séquentielles et se font **une fois**. En Agile, elles se répètent en boucle courte (le sprint est un mini cycle de vie complet : on cadre le sprint, on planifie, on exécute, on clôture avec la review).

### Cycle en V vs Agile vs Kanban : quand utiliser quoi

Arrête de penser "Agile = moderne et bien, cycle en V = ringard". Chaque méthodo répond à un contexte.

| Critère | Cycle en V | Scrum (Agile) | Kanban |
|---|---|---|---|
| Le besoin est-il clair dès le départ ? | Oui, figé (cahier des charges validé) | Non, ça va évoluer | Variable |
| Le client peut-il voir le résultat régulièrement ? | Non, souvent qu'à la fin | Oui, toutes les 1-4 semaines | Oui, en continu |
| Contraintes réglementaires fortes (santé, aéronautique, banque) ? | Fréquent — traçabilité exigée | Rare | Rare |
| Taille de l'équipe | Grande, plusieurs équipes | Petite (5-9 personnes) | Variable |
| Type de flux de travail | Projet avec fin définie | Projet avec fin définie, releases itératives | Flux continu (support, maintenance, ops) |
| Exemple concret | Développement d'un logiciel médical certifié | Construction d'un SaaS dont les features évoluent avec les retours utilisateurs | Équipe support qui traite tickets et bugs en continu |

**Ta règle pratique :**
- Besoin flou, tu veux itérer avec des retours → **Scrum**. C'est le cas pour CourIA ou PySTACK : tu ajustes en marchant.
- Flux continu sans "fin de projet" (maintenance, support, contenu de formation qu'on enrichit sans cesse) → **Kanban**.
- Cahier des charges figé, contractuel, avec pénalités de retard → **Cycle en V**. Rare en solo/petite structure, fréquent en prestation pour grands comptes.

Rien n'empêche de mixer : beaucoup d'équipes font du "Scrumban" (backlog priorisé + flux Kanban sans sprints rigides). C'est souvent le plus pertinent pour un solo dev ou une petite équipe.

### Le triangle qualité / coût / délai, appliqué à un vrai projet

Le triangle dit : tu ne peux pas fixer les 3 sommets en même temps. Si tu bouges un sommet, un autre bouge forcément.

Exemple concret sur un projet comme CourIA :
- Le sponsor (toi, ou un client) veut : **livrer en 3 semaines** (délai fixe), **avec génération de cours + auth + paiement** (scope large = qualité/périmètre), **sans budget pour recruter** (coût fixe = toi seul).
- Les 3 sommets sont bloqués → **quelque chose va casser**. Soit tu réduis le scope (paiement en V2), soit tu acceptes un délai plus long, soit tu baisses la qualité (moins de tests, dette technique).

**Ton rôle de chef de projet : rendre ce compromis explicite** au lieu de le subir. Face à une demande "tout, vite, avec ce qu'on a", tu réponds toujours avec un arbitrage clair, jamais par un "oui" silencieux qui te retombera dessus en semaine 3.

### Template — Fiche d'arbitrage triangle QCD

```
PROJET : ___________
DATE : ___________

Contrainte fixe (celle qu'on NE PEUT PAS bouger) : [ ] Délai  [ ] Coût  [ ] Qualité/Scope

Conséquence sur les 2 autres sommets :
- Si délai fixe → scope réduit à : ...
- Si scope fixe → délai réaliste estimé à : ...
- Si coût fixe (pas de ressource supplémentaire) → scope réduit à : ...

Décision validée par : ___________
Ce qui est repoussé en V2 / hors scope : ...
```

---

## Module 2 — Cadrage et spécifications

### Écrire un cahier des charges qui évite les malentendus

Un cahier des charges (CDC) qui rate son objectif, c'est un CDC écrit pour se couvrir plutôt que pour transmettre. Les malentendus viennent presque toujours de 3 trous :

1. **Le "pourquoi" absent.** Tu listes des fonctionnalités sans dire quel problème elles résolvent. Résultat : dès qu'un arbitrage est nécessaire, personne ne sait quoi prioriser.
2. **Le flou sur les limites.** "Le système doit être rapide" ne veut rien dire. "Le temps de génération d'un cours ne doit pas dépasser 15 secondes" est vérifiable.
3. **Les non-dits sur ce qui N'EST PAS inclus.** Un CDC qui ne dit pas ce qui est hors scope se fait déborder en cours de route ("ah mais je pensais que c'était inclus").

**Règle pratique :** chaque fonctionnalité du CDC doit répondre à 3 questions : *pour qui, pour quoi faire, comment on sait que c'est fait (critère vérifiable)*. Si tu ne peux pas répondre aux 3, c'est encore une idée, pas une spec.

### Découper un projet en user stories / tâches estimables

Une user story mal découpée ("En tant qu'utilisateur je veux gérer mes cours") est invérifiable et inestimable. Le découpage doit descendre jusqu'à une tâche qu'un dev peut estimer en heures/jours avec une marge d'erreur raisonnable.

Méthode simple (INVEST) :
- **I**ndépendante — pas besoin d'une autre story pour tester celle-ci
- **N**égociable — c'est une discussion, pas un contrat gravé
- **V**alorisable — apporte une valeur identifiable à l'utilisateur
- **E**stimable — l'équipe peut dire "environ combien de temps"
- **S**mall — assez petite pour tenir dans un sprint (1-3 jours de travail max, sinon redécoupe)
- **T**estable — il existe un moyen concret de vérifier que c'est fait

Exemple de découpage sur CourIA :
- ❌ Trop gros : "Système de génération de cours par IA"
- ✅ Découpé : "Formulaire de saisie du sujet + niveau" / "Appel à l'API Groq et récupération du plan de cours" / "Affichage du plan généré avec les sections" / "Gestion des erreurs si l'API Groq timeout"

### Gestion des parties prenantes

Trois profils reviennent tout le temps, et chacun attend un langage différent :
- **Le client / sponsor** — veut savoir si ça avance et si le budget/délai tient. Ne lui parle jamais de stack technique en premier, parle résultat.
- **L'équipe technique** — a besoin de specs claires et d'un accès direct à toi pour lever les ambiguïtés vite, sinon elle bloque ou elle invente.
- **Les utilisateurs finaux** — s'ils ne sont pas consultés en cours de route, tu livres un truc techniquement parfait mais inutilisé.

**Règle pratique :** identifie qui décide (a le dernier mot), qui doit être consulté (a un avis qui compte), qui doit juste être informé. Ne confonds jamais les 3 — c'est la cause n°1 des blocages "on attendait ta validation".

### Template — Cahier des charges express

```
# Cahier des charges — [Nom du projet]

## 1. Contexte et problème résolu
- Quel problème concret ce projet résout-il ?
- Pour qui (persona précis, pas "les utilisateurs") ?

## 2. Objectifs mesurables
- Objectif 1 : ... (mesurable : ...)
- Objectif 2 : ... (mesurable : ...)

## 3. Fonctionnalités incluses (V1)
| Fonctionnalité | Pour qui | Pour quoi faire | Critère de "fait" |
|---|---|---|---|
| ... | ... | ... | ... |

## 4. Explicitement hors scope (V1)
- ...
- ...

## 5. Contraintes
- Techniques : ...
- Délai : ...
- Budget / ressources : ...

## 6. Parties prenantes
| Nom/rôle | Décide / Consulté / Informé |
|---|---|
| ... | ... |
```

### Template — User story

```
En tant que [persona],
je veux [action],
afin de [bénéfice concret].

Critères d'acceptation :
- [ ] ...
- [ ] ...
- [ ] ...

Estimation : ___ (points ou jours)
Hors scope de cette story : ...
```

---

## Module 3 — Méthodologie Agile en pratique

### Les rituels Scrum : à quoi ils servent VRAIMENT

Le piège classique : faire les rituels par cérémonie sans comprendre leur fonction. Voici ce qu'ils font réellement, pas ce que dit le manuel.

- **Sprint planning** — Ce n'est pas "on liste ce qu'on va faire", c'est **un engagement collectif sur ce qui est réaliste**. Sortie attendue : un sprint goal clair (une phrase) + une liste de stories que l'équipe s'engage à livrer.
- **Daily standup** — Ce n'est PAS un reporting managérial. C'est un outil de **synchronisation entre pairs** pour détecter les blocages tôt. 3 questions : qu'est-ce que j'ai fait, qu'est-ce que je fais aujourd'hui, qu'est-ce qui me bloque. 15 min max, debout, pas de résolution de problème en direct (ça se fait après, en petit comité).
- **Sprint review** — Démo du travail RÉELLEMENT fonctionnel (pas de slides, pas de "ça marche sur ma machine"). Sert à récolter du feedback avant que le mauvais chemin soit trop avancé.
- **Rétrospective** — Le rituel le plus sous-estimé et le plus utile. Sert à améliorer le **processus d'équipe**, pas à juger les personnes. Sans rétro, une équipe répète les mêmes erreurs sprint après sprint.

### Prioriser un backlog : MoSCoW et valeur/effort

**MoSCoW** — classe chaque item en :
- **M**ust have — sans ça, le produit n'a pas de sens
- **S**hould have — important mais pas bloquant pour livrer
- **C**ould have — bonus si le temps le permet
- **W**on't have (cette fois) — explicitement repoussé

**Matrice valeur/effort** — plus visuelle, utile quand tu as trop d'idées et pas assez de temps :

```
Valeur élevée, effort faible  → À FAIRE EN PREMIER (quick wins)
Valeur élevée, effort élevé   → À planifier, découper en plus petit
Valeur faible, effort faible  → Si le temps permet, sans urgence
Valeur faible, effort élevé   → À écarter ou repousser loin
```

Sur PySTACK par exemple : ajouter une fonctionnalité de recherche/filtre est valeur élevée + effort modéré → priorité haute. Ajouter un système de notation communautaire des libs est valeur incertaine + effort élevé (auth, modération) → à repousser.

### Outils concrets et comment les configurer pour un petit projet

Tu n'as pas besoin d'usine à gaz pour un projet solo ou une petite équipe.

- **Trello / Notion (Kanban simple)** — Suffisant pour un projet solo ou 2-3 personnes. Colonnes recommandées : `Backlog` → `À faire (sprint courant)` → `En cours` → `À review` → `Terminé`. Limite le nombre de tâches en "En cours" à 2-3 max par personne (WIP limit) — c'est ce qui évite le multitâche qui ralentit tout.
- **Linear** — Plus adapté dès que tu veux du suivi propre avec cycles (sprints), priorités, et liaison avec GitHub (les commits/PR peuvent fermer automatiquement une tâche). Bon choix si tu veux passer pro sans la lourdeur de Jira.
- **Jira** — Overkill pour un petit projet, mais **tu dois savoir t'en servir** car c'est le standard en entreprise. Différence clé avec Trello : Jira gère nativement les epics (regroupement de stories), les sprints avec vélocité calculée, et les workflows configurables (ex: "Terminé" nécessite une review obligatoire avant de fermer le ticket).

**Config minimale pour démarrer (Trello/Notion/Linear) :**
1. Un board avec les colonnes ci-dessus
2. Une étiquette par type de tâche (bug / feature / dette technique / doc)
3. Une estimation sur chaque carte (même grossière : S/M/L)
4. Un sprint = 1 à 2 semaines pour un petit projet, pas plus (au-delà, le feedback arrive trop tard)

### Template — Sprint planning

```
# Sprint [N] — [Nom du projet]
Dates : du __ au __
Sprint goal (1 phrase) : ...

## Stories engagées
| Story | Estimation | Responsable | Statut |
|---|---|---|---|
| ... | ... | ... | À faire |

## Capacité de l'équipe ce sprint
- Jours dispo par personne : ...
- Absences/congés prévus : ...

## Risques identifiés pour ce sprint
- ...
```

### Template — Rétrospective

```
# Rétro Sprint [N]

## Ce qui a bien marché
- ...

## Ce qui a coincé
- ...

## Actions concrètes pour le sprint prochain (max 3, avec un responsable)
1. Action : ... — Responsable : ... — À vérifier le : ...
2. ...
3. ...
```

---

## Module 4 — Gestion d'équipe et communication

### Répartir les tâches selon les compétences

Trois erreurs fréquentes à éviter :
- **Distribuer par disponibilité plutôt que par compétence** — tu donnes la tâche complexe à qui a du temps libre, pas à qui sait la faire vite et bien. Résultat : retard + qualité en baisse.
- **Ne jamais faire monter en compétence** — si une seule personne touche toujours le backend et une autre toujours le front, tu crées un point de défaillance unique (bus factor = 1).
- **Distribuer trop gros d'un coup** — une tâche assignée sans checkpoint intermédiaire n'est vérifiable qu'à la fin, quand il est trop tard pour corriger.

**Règle pratique :** assigne en fonction de la compétence dominante requise + prévois un checkpoint à mi-parcours sur toute tâche de plus de 2 jours.

### Gérer les retards, blocages techniques, conflits

- **Retard** — Ne jamais attendre la deadline pour le découvrir. Le daily standup existe pour ça : un retard signalé à J+1 se rattrape, signalé à J+5 c'est une crise. Face à un retard confirmé, la question n'est pas "pourquoi t'es en retard" mais "qu'est-ce qu'on coupe ou qu'est-ce qu'on décale".
- **Blocage technique** — Fixe une règle simple dans l'équipe : "bloqué plus de X heures → tu demandes de l'aide, tu ne restes pas seul dessus". Ça évite qu'un dev s'enferme 2 jours sur un problème qu'un pair résout en 20 minutes.
- **Conflit** — Sépare toujours le désaccord technique (sain, à trancher avec des critères objectifs : perf, maintenabilité, délai) du conflit relationnel (à traiter en 1-to-1, jamais devant le groupe). Un chef de projet qui laisse un désaccord technique traîner sans trancher fait perdre du temps à tout le monde.

### Reporting : communiquer l'avancement sans noyer de détails techniques

Le sponsor/client ne veut pas savoir que tu as refactorisé un service Django. Il veut savoir : **on est dans les temps ou pas, qu'est-ce qui a été livré, qu'est-ce qui reste, y a-t-il un risque**.

Structure de reporting qui marche à tous les coups (format "SVR" — Statut / Valeur livrée / Risques) :

```
STATUT GLOBAL : 🟢 Dans les temps / 🟡 Vigilance / 🔴 Risque

VALEUR LIVRÉE CETTE PÉRIODE
- [Fonctionnalité en langage utilisateur, pas technique]

À VENIR
- [3-5 points max]

RISQUES / DÉCISIONS ATTENDUES
- [Ce qui nécessite un arbitrage du sponsor, avec une deadline de décision]
```

### Template — Point d'avancement hebdomadaire

```
# Point d'avancement — [Projet] — Semaine du __

STATUT : 🟢 / 🟡 / 🔴

## Livré cette semaine
- ...

## Prévu la semaine prochaine
- ...

## Points de blocage / décisions attendues
- Blocage : ... → Impact si non résolu : ... → Décision attendue avant le : ...

## Indicateur d'avancement
Stories terminées / prévues ce sprint : __ / __
```

---

## Module 5 — Gestion des risques et qualité

### Identifier les risques avant qu'ils arrivent

Un risque n'est pas un problème — c'est un problème qui n'est pas encore arrivé mais que tu peux anticiper. Deux catégories à checker systématiquement en début de projet :

**Risques techniques**
- Dépendance à une API tierce (ex: Groq pour CourIA) — que se passe-t-il si elle est down, rate-limited, ou change de pricing ?
- Dette technique accumulée sans plan de remboursement
- Absence de tests sur les fonctionnalités critiques (paiement, auth)
- Choix technique non validé à l'échelle (ex: une structure de DB qui tient à 100 users mais pas à 10 000)

**Risques organisationnels**
- Un seul point de compétence sur une brique clé (bus factor)
- Scope qui grossit sans révision du délai ("scope creep")
- Sponsor/client indisponible pour valider → blocages en cascade
- Sur-engagement de l'équipe (vélocité irréaliste)

**Méthode simple : la matrice probabilité × impact**

```
Impact élevé, probabilité élevée   → Traiter maintenant, priorité absolue
Impact élevé, probabilité faible   → Avoir un plan B prêt, surveiller
Impact faible, probabilité élevée  → Accepter et monitorer
Impact faible, probabilité faible  → Ignorer, ne pas sur-investir dessus
```

### Tests, revues de code, definition of done

Une **Definition of Done (DoD)** est un contrat d'équipe : une tâche n'est "terminée" que si elle coche TOUS les critères, pas seulement "le code marche sur ma machine".

Exemple de DoD réaliste pour un projet solo/petite équipe :
- [ ] Code revu (par un pair, ou par toi-même à froid le lendemain si solo)
- [ ] Tests des cas critiques passés (pas 100% de couverture obligatoire, mais les chemins critiques oui)
- [ ] Pas de régression sur les fonctionnalités existantes
- [ ] Documentation minimale à jour (README, docstring sur les fonctions clés)
- [ ] Déployé/testé en environnement proche de la prod

La revue de code n'est pas là pour "attraper les fautes" — c'est un outil de **diffusion de connaissance** (2 personnes connaissent le code au lieu d'une) et de détection précoce des mauvais choix d'architecture, avant qu'ils soient trop coûteux à changer.

### Gérer la dette technique dans un planning

La dette technique n'est un problème que si elle n'est **jamais budgétée**. Règle simple : réserve systématiquement **10 à 20% de chaque sprint** pour du remboursement de dette (refacto, mise à jour de dépendances, nettoyage). Si tu ne le fais pas explicitement, la dette s'accumule silencieusement jusqu'à ralentir tout le développement — et personne ne comprend pourquoi "on n'avance plus".

### Template — Registre des risques

```
# Registre des risques — [Projet]

| Risque | Probabilité (1-3) | Impact (1-3) | Score | Plan d'action | Responsable |
|---|---|---|---|---|---|
| ... | ... | ... | ... | ... | ... |
```

### Template — Definition of Done

```
# Definition of Done — [Projet]

Une tâche est "Done" seulement si :
- [ ] ...
- [ ] ...
- [ ] ...

Une release est "Done" seulement si (en plus des critères ci-dessus) :
- [ ] ...
```

---

## Module 6 — Cas pratique : structurer CourIA en vrai plan de projet

Prenons **CourIA** (plateforme de génération de cours par IA, Django + API Groq) comme cas concret. Voici comment le structurer avec tout ce qu'on a vu.

### Étape 1 — Cadrage express

```
Problème résolu : les enseignants francophones africains passent trop de temps
à préparer des supports de cours et manquent d'outils adaptés à leur contexte.

Objectif V1 mesurable : un enseignant peut générer un plan de cours structuré
en moins de 15 secondes, sur un sujet et un niveau donnés.

Hors scope V1 : paiement, gestion multi-établissements, export PDF stylisé
(repoussés en V2).
```

### Étape 2 — Backlog priorisé (extrait, matrice valeur/effort)

| Story | Valeur | Effort | Priorité |
|---|---|---|---|
| Formulaire saisie sujet + niveau | Élevée | Faible | Sprint 1 |
| Appel API Groq + récupération plan de cours | Élevée | Moyen | Sprint 1 |
| Affichage du cours généré | Élevée | Faible | Sprint 1 |
| Gestion des erreurs / timeout API | Élevée | Faible | Sprint 1 |
| Authentification utilisateur | Moyenne | Moyen | Sprint 2 |
| Historique des cours générés | Moyenne | Moyen | Sprint 2 |
| Export PDF stylisé | Faible | Élevé | Backlog (V2) |
| Paiement / abonnement | Faible (tant que pas d'utilisateurs) | Élevé | Backlog (V2) |

### Étape 3 — Découpage en sprints et jalons

```
Sprint 1 (2 semaines) — Goal : "Un utilisateur peut générer et voir un cours"
→ Jalon : Démo fonctionnelle interne, sans compte utilisateur

Sprint 2 (2 semaines) — Goal : "Un utilisateur peut créer un compte et
retrouver ses cours générés"
→ Jalon : Version testable par 5 enseignants pilotes

Sprint 3 (2 semaines) — Goal : "Stabilisation + retours des pilotes intégrés"
→ Jalon : V1 publique
```

### Étape 4 — Risques identifiés pour CourIA

| Risque | Prob. | Impact | Plan d'action |
|---|---|---|---|
| Rate limit / coût de l'API Groq si usage monte | Moyenne | Élevé | Mettre un cache sur les prompts similaires, monitorer les quotas dès sprint 1 |
| Qualité variable des cours générés selon les sujets | Élevée | Moyen | Tester sur un panel de sujets variés avant la démo pilote, ajuster le prompt système |
| Seul développeur sur le projet (bus factor = 1) | Élevée | Moyen | Documenter les choix d'archi au fil de l'eau (README technique) |
| Pas de retours utilisateurs avant la V1 publique | Moyenne | Élevé | Recruter 5 enseignants pilotes dès le sprint 2, pas après |

### Étape 5 — Métriques de suivi à mettre en place

- **Vélocité** : nombre de stories terminées par sprint (sert à calibrer les sprints suivants, pas à "juger" la performance)
- **Taux de complétion du sprint** : stories engagées vs stories réellement livrées — si <70% sur 2 sprints d'affilée, tu sur-engages, réduis la capacité prévue
- **Temps de génération moyen** (métrique produit) : doit rester sous 15 sec, sinon c'est un risque qui remonte dans le registre
- **Taux d'usage post-génération** (les enseignants reviennent-ils utiliser un 2e cours ?) — indicateur de valeur réelle, pas juste de fonctionnalité livrée

### Ce qu'il faut retenir de ce cas pratique

Le plan de projet n'est pas un document figé qu'on écrit une fois — c'est un outil de décision qu'on met à jour à chaque sprint (backlog qui bouge, risques qui se confirment ou disparaissent, priorités qui changent avec les retours pilotes). Le réflexe à prendre : **avant chaque sprint, tu retournes voir le cadrage et le registre des risques**, pas seulement le backlog.

---

## Pour aller plus loin

Une fois ces bases posées, les compétences qui font vraiment la différence en entreprise :
- Savoir chiffrer un projet (estimation en jours-homme, marge d'incertitude)
- Piloter un budget, pas juste un planning
- Gérer un projet avec plusieurs équipes (dépendances inter-équipes, pas juste inter-tâches)

Ce sont des sujets pour une formation de niveau 2 — commence par appliquer ces 6 modules sur un projet réel (CourIA, PySTACK ou la formation backend) avant d'aller plus loin : la gestion de projet s'apprend en pilotant, pas en lisant.
