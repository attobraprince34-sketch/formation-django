# Formation complète — Software Engineering professionnel avec l’IA

> **Objectif :** passer progressivement de « développeur capable de coder une application » à **Software Engineer capable de concevoir, développer, tester, sécuriser, déployer, maintenir et faire évoluer une application professionnelle**, tout en utilisant l’IA comme outil d’ingénierie.

---

## Comment utiliser cette formation

Cette formation est volontairement orientée **projet, décisions et pratique**.

La règle centrale est :

```text
Comprendre
  ↓
Décider
  ↓
Concevoir
  ↓
Implémenter
  ↓
Tester
  ↓
Sécuriser
  ↓
Reviewer
  ↓
Déployer
  ↓
Observer
  ↓
Améliorer
```

L’IA peut intervenir à presque toutes les étapes, mais elle ne remplace ni le raisonnement, ni la validation, ni la responsabilité technique.

### Règle de travail avec l’IA

```text
Contexte
+ Contraintes
+ Comportement attendu
+ Exigences techniques
+ Critères d’acceptation
= meilleure sortie IA
```

Puis :

```text
Demander
→ analyser
→ générer
→ vérifier
→ tester
→ corriger
→ sécuriser
→ intégrer
```

---

# Parcours global

| Module | Sujet | Livrable principal |
|---|---|---|
| 1 | Penser comme un Software Engineer | Software Engineering Checklist |
| 2 | Analyse du besoin | Cahier des charges + user stories |
| 3 | Architecture | Architecture + ADR |
| 4 | API REST | API Design |
| 5 | PostgreSQL | Modèle de données |
| 6 | Backend professionnel | Structure FastAPI/Django |
| 7 | Frontend professionnel | Architecture React/TypeScript |
| 8 | Authentification | Modèle auth/RBAC |
| 9 | Sécurité | Threat model + Security Review |
| 10 | Tests | Testing Strategy |
| 11 | Code Review | PR Review |
| 12 | Git | Workflow d’équipe |
| 13 | CI/CD | Pipeline |
| 14 | Docker | Environnements reproductibles |
| 15 | Performance | Plan de mesure et optimisation |
| 16 | Observabilité | Monitoring + incident response |
| 17 | Résilience | Stratégie de tolérance aux pannes |
| 18 | IA pour développer | Workflow AI-assisted |
| 19 | Prompt engineering | Bibliothèque de prompts |
| 20 | Vérification du code IA | AI Code Verification |
| 21 | Projet complet | SaaS de formation |
| 22 | Développement itératif | Features livrées une par une |
| 23 | Debugging | Root Cause Analysis |
| 24 | Décisions techniques | Decision Records |
| 25 | Documentation | Documentation professionnelle |
| 26 | Audit final | Professional Software Audit |

---

# MODULE 1 — PENSER COMME UN SOFTWARE ENGINEER

## Objectifs

À la fin du module, tu dois savoir :

- distinguer coder et développer un logiciel ;
- identifier les contraintes avant d’écrire du code ;
- raisonner en termes de qualité, risques et compromis ;
- comprendre pourquoi l’architecture, les tests, la sécurité et l’observabilité font partie du produit ;
- utiliser une checklist avant de commencer un projet.

## 1. Coder ≠ développer un logiciel

Coder consiste principalement à transformer une intention en instructions exécutables.

Développer un logiciel implique beaucoup plus :

```text
Comprendre le problème
→ définir le comportement
→ choisir une architecture
→ modéliser les données
→ implémenter
→ tester
→ sécuriser
→ déployer
→ observer
→ maintenir
```

Une application peut fonctionner aujourd’hui et être pourtant mauvaise.

Exemple :

```python
@app.post("/users")
def create_user(data):
    # tout dans la route
    ...
```

Le problème n’est pas forcément que le code ne fonctionne pas. Le problème peut être :

- absence de validation ;
- accès direct à la base ;
- logique métier mélangée à HTTP ;
- erreurs non structurées ;
- absence de tests ;
- absence de contrôle d’autorisation ;
- comportement difficile à faire évoluer.

### Réflexe senior

Avant de demander « comment coder cette fonctionnalité ? », demande :

1. Quel problème résout-elle ?
2. Qui l’utilise ?
3. Quel comportement est attendu ?
4. Quelles sont les contraintes ?
5. Que se passe-t-il en cas d’échec ?
6. Quelles données sont sensibles ?
7. Comment vais-je vérifier que cela fonctionne ?
8. Comment cela évoluera-t-il ?

## 2. Les qualités d’un logiciel professionnel

### Maintenabilité

Un autre développeur doit pouvoir comprendre et modifier le système.

### Fiabilité

Le système doit avoir un comportement prévisible même lorsque quelque chose échoue.

### Sécurité

Les utilisateurs ne doivent pas pouvoir réaliser des actions auxquelles ils n’ont pas droit.

### Performance

Le système doit répondre dans des délais acceptables pour le besoin réel.

### Scalabilité

Le système doit pouvoir absorber l’évolution prévue sans architecture inutilement complexe.

### Observabilité

Après déploiement, l’équipe doit pouvoir comprendre ce qui se passe.

## 3. Les compromis sont normaux

Il n’existe pas d’architecture universellement meilleure.

Un monolithe peut être excellent.

Des microservices peuvent être nécessaires dans certains contextes et catastrophiques dans d’autres.

Le raisonnement professionnel est :

```text
Problème
→ contraintes
→ options
→ avantages
→ inconvénients
→ risques
→ coût
→ décision
```

## Utilisation de l’IA

### Mauvais prompt

```text
Construis-moi une architecture SaaS moderne.
```

### Meilleur prompt

```text
Contexte :
Je construis un SaaS B2B de gestion de formations.

Contraintes :
- équipe de 2 développeurs ;
- Python/FastAPI ;
- PostgreSQL ;
- React/TypeScript ;
- budget limité ;
- première version avec quelques milliers d’utilisateurs.

Objectif :
Propose 3 architectures possibles.

Pour chacune :
- avantages ;
- inconvénients ;
- complexité opérationnelle ;
- risques ;
- coût de maintenance.

Ne génère aucun code.
Termine par une recommandation argumentée.
```

### Vérification

Ne prends jamais la réponse IA comme décision finale. Vérifie :

- les hypothèses ;
- les contraintes ;
- les dépendances ;
- les conséquences opérationnelles ;
- les coûts ;
- les risques.

## Checklist — Software Engineering

```markdown
# Software Engineering Checklist

## Produit
- [ ] Le problème est clairement défini
- [ ] Les utilisateurs sont identifiés
- [ ] Le comportement attendu est documenté
- [ ] Les critères d'acceptation existent

## Architecture
- [ ] Les responsabilités sont identifiées
- [ ] Les dépendances sont explicites
- [ ] Les décisions importantes sont documentées
- [ ] Les compromis sont connus

## Données
- [ ] Modèle de données défini
- [ ] Contraintes identifiées
- [ ] Transactions nécessaires identifiées
- [ ] Index nécessaires étudiés

## Sécurité
- [ ] Données sensibles identifiées
- [ ] Authentification définie
- [ ] Autorisation définie
- [ ] Menaces principales identifiées

## Qualité
- [ ] Stratégie de tests
- [ ] Linting
- [ ] Type checking
- [ ] Code review

## Production
- [ ] Configuration
- [ ] Logs
- [ ] Monitoring
- [ ] Alerting
- [ ] Backup
- [ ] Rollback
```

## Exercice

Tu dois créer une application permettant à une entreprise de gérer ses employés.

Avant tout code, écris :

- utilisateurs ;
- objectifs ;
- fonctionnalités ;
- contraintes ;
- risques ;
- données sensibles ;
- questions ouvertes.

### Challenge

Explique pourquoi tu choisirais un monolithe, un modular monolith ou des services séparés.

---

# MODULE 2 — ANALYSE DU BESOIN ET CONCEPTION

## Objectifs

Transformer une idée vague en spécifications exploitables.

## De l’idée au système

Exemple :

> « Je veux une plateforme de formation en ligne. »

Cette phrase ne suffit pas.

On doit produire :

```text
Business requirements
→ Functional requirements
→ Non-functional requirements
→ User stories
→ Acceptance criteria
→ Technical requirements
```

## 1. Questions de découverte

### Utilisateurs

- Qui utilise le système ?
- Qui paie ?
- Qui administre ?
- Qui crée le contenu ?
- Existe-t-il plusieurs organisations ?

### Fonctionnalités

- Que peut faire un étudiant ?
- Que peut faire un instructeur ?
- Que peut faire un administrateur ?
- Quelles actions sont critiques ?

### Contraintes

- réglementation ?
- volume ?
- disponibilité ?
- budget ?
- délai ?
- intégrations externes ?

### Ambiguïtés

Une bonne question de clarification vaut souvent mieux que 500 lignes de code.

## 2. User story

Format :

```text
En tant que [acteur]
Je veux [action]
Afin de [valeur]
```

Exemple :

```text
En tant qu'étudiant,
je veux reprendre une formation là où je me suis arrêté,
afin de continuer mon apprentissage sans perdre ma progression.
```

## 3. Acceptance criteria

Utilise des critères observables.

```text
Étant donné que l'étudiant est inscrit,
quand il ouvre la formation,
alors la dernière leçon non terminée est affichée.

Étant donné qu'il termine une leçon,
quand l'opération réussit,
alors sa progression est persistée.
```

## Template — Cahier des charges

```markdown
# Cahier des charges

## 1. Contexte

## 2. Problème

## 3. Objectifs

## 4. Utilisateurs

## 5. Fonctionnalités

## 6. Hors périmètre

## 7. Contraintes

## 8. Exigences non fonctionnelles

## 9. Données

## 10. Intégrations externes

## 11. Sécurité

## 12. Risques

## 13. Critères de succès

## 14. Questions ouvertes

## 15. Hypothèses
```

## Utilisation de l’IA

Demande à l’IA de jouer le rôle d’un analyste critique :

```text
Voici mon besoin.

Ne propose pas de code.

1. Identifie les ambiguïtés.
2. Liste les hypothèses implicites.
3. Pose les questions qui pourraient changer l'architecture.
4. Identifie les acteurs.
5. Identifie les cas limites.
6. Identifie les risques.
7. Sépare les exigences fonctionnelles des exigences non fonctionnelles.
```

## Erreur fréquente

Commencer par :

```text
Crée-moi les modèles SQLAlchemy.
```

avant de savoir exactement ce que le produit doit faire.

---

# MODULE 3 — ARCHITECTURE LOGICIELLE

## Objectifs

Savoir choisir une architecture proportionnée au problème.

## Progression

```text
Monolith
→ Modular Monolith
→ Services séparés
→ Microservices
```

La complexité opérationnelle augmente généralement avec la distribution.

## Monolithe

Une application déployée comme une unité.

Avantages :

- simplicité ;
- déploiement facile ;
- transactions simples ;
- debugging plus simple ;
- faible coût opérationnel.

Inconvénients :

- frontières parfois moins strictes ;
- scaling moins granulaire ;
- risque de gros bloc si mal structuré.

## Modular Monolith

Un seul déploiement, mais des modules métier clairement séparés.

Exemple :

```text
app/
├── auth/
├── users/
├── courses/
├── enrollments/
├── payments/
└── notifications/
```

C’est souvent un excellent point de départ.

## Microservices

Chaque service possède une responsabilité et une autonomie plus importantes.

À considérer lorsqu’il existe de vraies raisons :

- équipes indépendantes ;
- besoins de scaling très différents ;
- frontières métier fortes ;
- exigences de déploiement indépendantes ;
- contraintes organisationnelles.

Ne pas utiliser des microservices uniquement parce que cela paraît « professionnel ».

## Couplage et cohésion

Bon système :

```text
forte cohésion
+
faible couplage
```

## Clean Architecture

Idée fondamentale :

> Les détails techniques ne doivent pas dicter le cœur métier.

Une organisation possible :

```text
domain/
application/
infrastructure/
interfaces/
```

Ne transforme toutefois pas cette idée en religion architecturale.

## Template ADR

```markdown
# ADR-XXX — [Titre]

## Status
Proposed | Accepted | Rejected | Superseded

## Context
Quel problème devons-nous résoudre ?

## Decision
Quelle décision prenons-nous ?

## Options considered

### Option A
Pros:
Cons:

### Option B
Pros:
Cons:

## Decision rationale
Pourquoi cette option ?

## Risks

## Consequences

## Revisit conditions
Quand devrons-nous réévaluer cette décision ?
```

## Challenge

Choisis une architecture pour :

1. Portfolio personnel.
2. SaaS B2B de 2 développeurs.
3. Système bancaire fortement réglementé.
4. Plateforme mondiale avec équipes autonomes.

Justifie chaque décision.

---

# MODULE 4 — CONCEPTION D’UNE API PROFESSIONNELLE

## Objectifs

Construire des APIs cohérentes, prévisibles et sécurisées avec FastAPI.

## Ressources

Une API REST expose des ressources.

Exemple :

```text
GET    /courses
POST   /courses
GET    /courses/{course_id}
PATCH  /courses/{course_id}
DELETE /courses/{course_id}
```

## Status codes

Utilise les codes pour communiquer le résultat.

Exemples :

```text
200 OK
201 Created
204 No Content
400 Bad Request
401 Unauthorized
403 Forbidden
404 Not Found
409 Conflict
422 Unprocessable Entity
429 Too Many Requests
500 Internal Server Error
```

## Validation

Les données entrantes doivent être validées avant d'atteindre la logique métier.

FastAPI + Pydantic facilitent ce travail.

## Erreurs cohérentes

Évite :

```json
{"error": "nope"}
```

pour une route et :

```json
{"message": "something failed"}
```

pour une autre.

Définis une convention.

Exemple :

```json
{
  "error": {
    "code": "COURSE_NOT_FOUND",
    "message": "Course not found",
    "details": {}
  }
}
```

## Pagination

Évite de renvoyer potentiellement des millions de lignes.

```text
GET /courses?page=2&page_size=20
```

Pour certains systèmes à gros volume, une pagination par curseur peut être préférable.

## Idempotence

Une requête répétée ne doit pas provoquer plusieurs effets indésirables lorsqu’une opération doit être idempotente.

Cas typique : paiement ou création d’une ressource à partir d’un événement pouvant être répété.

## API Design Checklist

```markdown
# API Design Checklist

- [ ] Noms de ressources cohérents
- [ ] Méthodes HTTP adaptées
- [ ] Status codes cohérents
- [ ] Validation des entrées
- [ ] Erreurs structurées
- [ ] Pagination
- [ ] Filtering
- [ ] Sorting
- [ ] Authentification
- [ ] Autorisation
- [ ] Rate limiting si nécessaire
- [ ] Idempotency si nécessaire
- [ ] Versioning si nécessaire
- [ ] Documentation OpenAPI
- [ ] Tests API
- [ ] Données sensibles non exposées
```

## Exercice

Conçois l’API de :

```text
Users
Courses
Lessons
Enrollments
Progress
Payments
```

Ne code pas encore.

---

# MODULE 5 — BASES DE DONNÉES ET POSTGRESQL

## Objectifs

Savoir concevoir une base avant de l’optimiser.

## Modèle initial

Pour le SaaS :

```text
users
organizations
courses
lessons
enrollments
progress
payments
notifications
```

Relations possibles :

```text
Organization 1──N Users
Course 1──N Lessons
User N──N Course via Enrollment
Enrollment 1──N Progress
User 1──N Payment
```

## Contraintes

Une base de données doit protéger l’intégrité.

Exemples :

- `PRIMARY KEY`
- `FOREIGN KEY`
- `UNIQUE`
- `NOT NULL`
- `CHECK`

Ne place pas toute l’intégrité uniquement dans Python.

## Index

Un index peut accélérer certaines lectures, mais il a un coût :

- stockage ;
- maintenance ;
- coût lors des écritures.

N’ajoute pas des index « au cas où ».

## N+1

Mauvais scénario :

```text
1 requête pour les cours
+
1 requête par cours pour les leçons
```

Pour 1 000 cours :

```text
1001 requêtes
```

Il faut apprendre à observer les requêtes réellement exécutées.

## Transactions

Une transaction protège un ensemble logique d’opérations.

Exemple :

```text
Créer paiement
+
enregistrer enrollment
+
enregistrer événement
```

Si la cohérence exige que ces opérations réussissent ensemble, réfléchis à leur frontière transactionnelle.

## EXPLAIN

Utilise les outils de PostgreSQL pour comprendre les plans d’exécution.

Question professionnelle :

> « Pourquoi cette requête est lente ? »

et non :

> « Quel index puis-je ajouter ? »

## Database Design Checklist

```markdown
# Database Design Checklist

- [ ] Entités identifiées
- [ ] Relations définies
- [ ] Clés primaires
- [ ] Clés étrangères
- [ ] Contraintes
- [ ] Index justifiés
- [ ] Transactions identifiées
- [ ] Migrations
- [ ] Audit fields
- [ ] Stratégie de suppression
- [ ] Volumétrie estimée
- [ ] Requêtes critiques identifiées
- [ ] N+1 étudié
- [ ] EXPLAIN utilisé lorsque nécessaire
- [ ] Connection pooling configuré
- [ ] Backup
- [ ] Restore testé
```

---

# MODULE 6 — BACKEND PROFESSIONNEL

## Architecture indicative

```text
app/
├── main.py
├── config/
├── api/
│   ├── routes/
│   └── dependencies/
├── domain/
├── application/
├── infrastructure/
│   ├── database/
│   └── external/
└── tests/
```

Cette structure n’est pas obligatoire. Elle doit servir les responsabilités.

## Où mettre la logique métier ?

Évite :

```python
@app.post("/courses")
def create_course(...):
    # validation HTTP
    # SQL
    # règles métier
    # email
    # logging
    # ...
```

Préférable :

```text
HTTP
 ↓
Application/service
 ↓
Domain logic
 ↓
Repository / infrastructure
```

## SOLID

Apprends les principes pour détecter les problèmes, pas pour multiplier les classes.

### Single Responsibility

Une unité de code devrait avoir une responsabilité cohérente.

### Dependency Inversion

Le cœur métier ne devrait pas dépendre inutilement des détails techniques.

## KISS

Commence simple.

## YAGNI

Ne construis pas aujourd’hui une abstraction uniquement parce qu’elle pourrait être utile dans trois ans.

## DRY

Évite la duplication de **connaissance**, pas simplement les lignes qui se ressemblent.

## Design patterns

Apprends à reconnaître les problèmes :

- Strategy : comportement interchangeable ;
- Adapter : interface incompatible ;
- Factory : construction complexe ;
- Repository : isolation d’un mécanisme de persistance lorsque cela apporte une vraie valeur ;
- Dependency Injection : inversion et composition des dépendances.

---

# MODULE 7 — FRONTEND PROFESSIONNEL AVEC REACT + TYPESCRIPT

## Objectifs

Construire un frontend lisible et prévisible.

## Séparation

Un composant ne devrait pas nécessairement :

- gérer toute la logique métier ;
- appeler directement plusieurs APIs ;
- transformer toutes les données ;
- gérer toutes les erreurs ;
- contenir des centaines de lignes.

Organisation indicative :

```text
src/
├── app/
├── features/
│   ├── auth/
│   ├── courses/
│   └── progress/
├── components/
├── services/
├── hooks/
├── types/
└── tests/
```

## Client state vs server state

Question essentielle :

> Cette donnée appartient-elle réellement à l’interface, ou vient-elle du serveur ?

Ne mets pas automatiquement toutes les données dans un store global.

## États à gérer

Une interface professionnelle pense à :

```text
idle
loading
success
empty
error
unauthorized
offline / degraded
```

## TypeScript

Les types doivent réduire les erreurs, pas devenir une bureaucratie.

Évite le :

```ts
any
```

lorsqu’un type utile peut être défini.

## React Architecture Checklist

```markdown
# React Architecture Checklist

- [ ] Features identifiées
- [ ] Composants raisonnablement petits
- [ ] State minimal
- [ ] Server state séparé
- [ ] Appels API centralisés
- [ ] Types explicites
- [ ] Loading states
- [ ] Empty states
- [ ] Error states
- [ ] Auth UI
- [ ] Authorization UI
- [ ] Tests
- [ ] Accessibilité considérée
```

---

# MODULE 8 — AUTHENTIFICATION ET AUTORISATION

## Authentication vs Authorization

```text
Authentication
= Qui es-tu ?

Authorization
= As-tu le droit de faire cela ?
```

Un utilisateur authentifié peut parfaitement être interdit d’une action.

## RBAC

Exemple :

```text
Admin
Instructor
Student
```

Mais un rôle seul ne suffit pas toujours.

Exemple :

```text
Instructor A
→ peut modifier ses propres cours

Instructor A
→ ne peut pas modifier les cours de Instructor B
```

Il existe alors une notion de propriété/permission.

## Sessions, JWT et cookies

Le choix dépend de l’architecture.

Questions :

- navigateur ou application mobile ?
- besoin de révocation ?
- durée de session ?
- stockage ?
- protection CSRF ?
- architecture frontend/backend ?

### JWT dans localStorage

Ce n’est pas une règle absolue « toujours interdit », mais stocker un token sensible dans un emplacement accessible au JavaScript augmente l’impact potentiel d’une compromission XSS.

Il faut donc raisonner sur :

- XSS ;
- cookies `HttpOnly` ;
- `Secure` ;
- `SameSite` ;
- CSRF ;
- rotation ;
- expiration ;
- révocation.

## Passwords

Ne stocke jamais les mots de passe en clair.

Utilise un mécanisme de hashing conçu pour les mots de passe, avec paramètres adaptés.

## Security Checklist

```markdown
# Authentication & Authorization Checklist

- [ ] Passwords hashés correctement
- [ ] Sessions/token expirants
- [ ] Rotation/révocation étudiée
- [ ] Cookies sécurisés si utilisés
- [ ] CSRF étudié
- [ ] RBAC
- [ ] Ownership checks
- [ ] Password reset sécurisé
- [ ] Email verification
- [ ] MFA si nécessaire
- [ ] Brute-force protection
- [ ] Rate limiting
- [ ] Secrets hors du code
- [ ] Logs sans tokens/mots de passe
```

---

# MODULE 9 — SÉCURITÉ APPLICATION

## Objectifs

Apprendre à penser :

```text
Attaquant
→ défenseur
→ test
→ prévention
```

## Threat modeling

Pour chaque fonctionnalité :

```text
Actifs
↓
Acteurs
↓
Trust boundaries
↓
Menaces
↓
Impacts
↓
Mitigations
↓
Tests
```

## Vulnérabilités à connaître

- Injection ;
- XSS ;
- CSRF ;
- SSRF ;
- Broken Access Control ;
- mauvaises configurations ;
- secrets exposés ;
- upload non sécurisé ;
- erreurs trop bavardes ;
- dépendances vulnérables ;
- brute force.

## Exemple : Broken Access Control

Supposons :

```http
GET /courses/123
```

Le serveur vérifie seulement que l’utilisateur est connecté.

Mais le cours appartient à une autre organisation.

Le bug est une absence de contrôle d’autorisation.

Le correctif n’est pas :

> « cacher le bouton dans React ».

Le serveur doit vérifier le droit réel.

## Security Review Template

```markdown
# Security Review

## Scope

## Assets

## Authentication

## Authorization

## Input validation

## Output handling

## Data protection

## Secrets

## Dependencies

## File uploads

## Network requests

## Logging

## Rate limiting

## Threats identified

### [SEC-001]
Severity:
Description:
Impact:
Root cause:
Detection:
Mitigation:
Regression test:

## Residual risks
```

## Exercice

On te donne :

```python
@app.get("/users/{user_id}")
def get_user(user_id: int, current_user=Depends(get_user)):
    return repository.get(user_id)
```

Trouve au moins trois questions de sécurité à poser avant d’accepter ce code.

---

# MODULE 10 — TESTS AUTOMATISÉS

## Pyramide pratique

```text
        E2E
      /     \
 Integration
    /       \
 Unit tests
```

Ce n’est pas une loi mathématique. Le bon équilibre dépend du système.

## Unit tests

Testent une unité avec un environnement contrôlé.

## Integration tests

Vérifient que plusieurs composants fonctionnent réellement ensemble.

## API tests

Vérifient le contrat HTTP.

## E2E

Vérifient un parcours utilisateur important.

## Que tester ?

Teste en priorité :

- règles métier ;
- permissions ;
- comportements critiques ;
- erreurs importantes ;
- cas limites ;
- intégrations critiques.

Ne teste pas inutilement l’implémentation interne.

## Coverage

```text
Coverage ≠ qualité
```

100 % de couverture ne garantit pas :

- les bons assertions ;
- la sécurité ;
- les bons scénarios métier ;
- l’absence de bugs.

## Testing Strategy Template

```markdown
# Testing Strategy

## Scope

## Critical business flows

## Unit tests

## Integration tests

## API tests

## E2E tests

## Security tests

## Regression strategy

## Test data

## Environments

## CI execution

## Definition of Done
```

## Exercice

Écris la stratégie de tests pour :

> Un étudiant termine une leçon et sa progression doit être enregistrée exactement une fois.

Pense à :

- succès ;
- double requête ;
- requête concurrente ;
- utilisateur non autorisé ;
- leçon inexistante ;
- base indisponible.

---

# MODULE 11 — CODE REVIEW

## Objectif

Ne pas reviewer uniquement le style.

Ordre recommandé :

```text
Correctness
→ Security
→ Architecture
→ Data
→ Error handling
→ Performance
→ Tests
→ Maintainability
→ Style
```

## Mauvaise review

> « Je préfère mettre cette fonction dans utils.py. »

## Bonne review

> « Cette logique est utilisée par deux flux métier mais son comportement dépend actuellement de l’HTTP. Je proposerais de la déplacer vers la couche application afin qu’elle soit testable sans requête HTTP. »

## Pull Request Review Checklist

```markdown
# PR Review Checklist

## Fonctionnel
- [ ] Comportement attendu respecté
- [ ] Edge cases

## Architecture
- [ ] Responsabilités cohérentes
- [ ] Pas de couplage inutile

## Sécurité
- [ ] Authorization
- [ ] Validation
- [ ] Secrets
- [ ] Données sensibles

## Données
- [ ] Transactions
- [ ] Migrations
- [ ] Index

## Tests
- [ ] Tests utiles
- [ ] Régression couverte

## Production
- [ ] Logs
- [ ] Monitoring
- [ ] Rollback
```

---

# MODULE 12 — GIT ET WORKFLOW PROFESSIONNEL

## Commits

Un commit doit représenter un changement compréhensible.

Exemple :

```text
feat: add course enrollment endpoint
fix: prevent duplicate enrollment
test: cover enrollment authorization
```

## Git Flow vs Trunk Based

### Git Flow

Peut convenir à certains processus avec releases structurées.

### Trunk Based Development

Favorise des changements courts intégrés fréquemment.

La question n’est pas :

> « Quelle stratégie est la meilleure ? »

mais :

> « Quelle stratégie correspond à notre équipe, notre fréquence de livraison et notre pipeline ? »

## Workflow conseillé

```text
Issue
↓
Branch
↓
Small commits
↓
Push
↓
CI
↓
PR
↓
Review
↓
Merge
↓
Deploy
```

## Git Workflow Guide

```markdown
# Git Workflow

## Branch naming

feature/
fix/
refactor/
chore/

## Commit conventions

feat:
fix:
refactor:
test:
docs:
chore:

## Pull Requests

- contexte
- problème
- solution
- tests
- risques
- screenshots si UI
- migration si nécessaire
```

---

# MODULE 13 — CI/CD

## Pipeline

```text
git push
↓
lint
↓
type checking
↓
tests
↓
security checks
↓
build
↓
deploy staging
↓
validation
↓
production
```

## CI

La CI vérifie automatiquement la qualité du changement.

## CD

Le déploiement devient reproductible.

## Environnements

```text
Development
Testing
Staging
Production
```

Ils doivent être suffisamment similaires pour éviter :

> « Cela fonctionne sur ma machine. »

## Migrations

Une migration en production est un changement critique.

Questions :

- backward compatible ?
- rollback ?
- données existantes ?
- durée ?
- verrouillage ?
- ordre du déploiement ?

## CI/CD Checklist

```markdown
# CI/CD Checklist

- [ ] Lint
- [ ] Type checking
- [ ] Unit tests
- [ ] Integration tests
- [ ] Security checks
- [ ] Build
- [ ] Artifact
- [ ] Environment separation
- [ ] Secrets management
- [ ] Migration strategy
- [ ] Deployment strategy
- [ ] Rollback
- [ ] Logs
- [ ] Release tracking
```

---

# MODULE 14 — DOCKER ET ENVIRONNEMENTS

## Concepts

```text
Dockerfile → image → container
```

Un container est une instance d’exécution d’une image.

## Multi-stage build

Objectif :

- image plus petite ;
- séparation build/runtime ;
- réduction de la surface inutile.

## Docker Compose

Très utile pour un environnement local :

```text
api
postgres
redis
worker
frontend
```

## Configuration

Ne hardcode pas :

```python
DATABASE_URL = "postgres://..."
```

Préférable :

```text
Environment
↓
Configuration
↓
Application
```

## Health checks

Un processus qui répond à une requête HTTP n’est pas nécessairement fonctionnel.

Distinguons :

```text
liveness
readiness
```

## Environnements

### Development

Optimisé pour le feedback.

### Testing

Optimisé pour des tests reproductibles.

### Staging

Proche de la production.

### Production

Sécurité, disponibilité, observabilité et contrôle.

---

# MODULE 15 — PERFORMANCE ET SCALABILITÉ

## Règle

> Mesurer avant d’optimiser.

## Méthode

```text
Symptôme
↓
Mesure
↓
Profiling
↓
Hypothèse
↓
Modification
↓
Mesure
↓
Validation
```

## Sources fréquentes

- requêtes SQL ;
- N+1 ;
- calcul CPU ;
- appels externes ;
- contention ;
- I/O ;
- mémoire ;
- sérialisation ;
- frontend trop lourd.

## Caching

Redis peut être utile, mais le cache introduit une nouvelle question :

> Quelle est la stratégie d’invalidation ?

Un cache incorrect peut être pire qu’une absence de cache.

## Async

`async` n’accélère pas magiquement tout code Python.

Il est particulièrement utile pour certaines opérations I/O lorsqu’elles sont réellement non bloquantes et correctement utilisées.

## Queues

Utilise une queue lorsque le travail :

- est long ;
- peut être asynchrone ;
- doit être réessayé ;
- ne doit pas bloquer la requête utilisateur.

Exemples :

```text
Email
PDF
Webhook processing
Analytics
Notifications
```

## Scaling

```text
Vertical
→ Horizontal
→ composants spécialisés
```

N’introduis pas un système distribué avant d’avoir identifié le besoin.

---

# MODULE 16 — OBSERVABILITÉ ET PRODUCTION

## Les quatre piliers pratiques

```text
Logs
Metrics
Traces
Alerts
```

## Structured logging

Préférable :

```json
{
  "event": "payment_failed",
  "user_id": "...",
  "payment_id": "...",
  "request_id": "..."
}
```

à une longue chaîne impossible à filtrer.

## Correlation ID

Permet de suivre une requête à travers plusieurs composants.

```text
Frontend
→ API
→ worker
→ provider externe
```

## Incident

Symptôme :

> « L’application est lente depuis dix minutes. »

Ne réponds pas immédiatement :

> « Augmentons les serveurs. »

Méthode :

```text
Quel périmètre ?
↓
Quel changement récent ?
↓
Tous les utilisateurs ?
↓
Quelle route ?
↓
Latence ou erreurs ?
↓
DB ?
↓
External API ?
↓
CPU ?
↓
Mémoire ?
↓
Queue ?
```

## Production Incident Checklist

```markdown
# Incident Checklist

## Detection
- [ ] Heure
- [ ] Symptôme
- [ ] Impact

## Investigation
- [ ] Logs
- [ ] Metrics
- [ ] Traces
- [ ] Recent deploys
- [ ] Database
- [ ] External dependencies

## Mitigation
- [ ] Rollback
- [ ] Feature flag
- [ ] Degradation
- [ ] Scaling

## Recovery
- [ ] Service restored
- [ ] Data integrity checked

## Postmortem
- [ ] Root cause
- [ ] Contributing factors
- [ ] Corrective actions
```

---

# MODULE 17 — GESTION DES ERREURS ET RÉSILIENCE

## Objectif

Une dépendance externe finira par :

- tomber ;
- ralentir ;
- retourner une erreur ;
- répondre partiellement.

## Timeout

Toute dépendance distante devrait avoir un comportement de timeout approprié.

Sans timeout, une ressource peut rester bloquée inutilement.

## Retry

Un retry n’est pas toujours sûr.

Il faut réfléchir à :

- idempotence ;
- nombre de tentatives ;
- backoff ;
- jitter ;
- types d’erreurs.

## Circuit breaker

Permet d’éviter de continuer à frapper une dépendance déjà défaillante.

## Fallback

Exemple :

```text
Recommendations API indisponible
→ afficher recommandations précédemment connues
```

## Graceful degradation

L’application ne doit pas nécessairement tout arrêter parce qu’une fonctionnalité secondaire est indisponible.

## Exemple

Paiement externe indisponible :

```text
Utilisateur
↓
Payment API
↓
timeout
↓
statut "payment_pending"
↓
retry asynchrone
```

plutôt que :

```text
timeout
↓
500
↓
état incohérent
```

---

# MODULE 18 — IA COMME OUTIL DE DÉVELOPPEMENT

## Objectif

Utiliser l’IA pour augmenter le raisonnement, pas le remplacer.

## Bons usages

L’IA peut aider à :

- explorer des options ;
- résumer un codebase ;
- proposer une implémentation ;
- générer un squelette ;
- écrire des tests ;
- expliquer une erreur ;
- rechercher des edge cases ;
- préparer une review ;
- documenter ;
- proposer un refactoring.

## Mauvais usage

```text
Construis toute mon application.
```

Puis copier-coller sans comprendre.

## Workflow recommandé

```text
Human defines problem
↓
AI explores
↓
Human decides
↓
AI implements small unit
↓
Human reviews
↓
Tests
↓
Security review
↓
Integration
```

## Prompt d’architecture

```text
Tu es un Software Architect.

Contexte :
[contexte]

Objectif :
[objectif]

Contraintes :
[contraintes]

Architecture actuelle :
[architecture]

Analyse :
1. problèmes
2. options
3. trade-offs
4. risques
5. recommandation

Ne génère pas de code.
```

## Prompt d’implémentation

```text
Voici l'architecture validée :

[architecture]

Feature :
[feature]

Acceptance criteria :
[criteria]

Contraintes :
[constraints]

Implémente uniquement cette feature.

Avant le code :
- liste les hypothèses ;
- signale les risques ;
- propose les fichiers à modifier.

Après le code :
- explique les changements ;
- fournis les tests ;
- liste les points à vérifier manuellement.
```

---

# MODULE 19 — PROMPT ENGINEERING POUR LE CODE

## Principe

Un bon prompt technique ressemble à un ticket bien spécifié.

## Structure

```text
Role
Context
Current state
Goal
Constraints
Inputs
Expected behavior
Acceptance criteria
Output format
Verification requirements
```

## Debugging

```text
Voici le symptôme :
...

Environnement :
...

Comportement attendu :
...

Comportement observé :
...

Logs :
...

Changements récents :
...

Ne donne pas immédiatement le correctif.

1. Formule des hypothèses.
2. Classe-les par probabilité.
3. Demande les preuves nécessaires.
4. Propose des expériences de diagnostic.
5. Puis propose le correctif.
6. Ajoute un test de régression.
```

## Security review

```text
Analyse ce code comme un Security Engineer.

Ne suppose pas que le code est sûr.

Cherche :
- authentication
- authorization
- injection
- XSS
- CSRF
- SSRF
- validation
- secrets
- erreurs
- race conditions
- exposition de données
- dépendances

Pour chaque problème :
Severity
Root cause
Exploitability
Impact
Mitigation
Regression test
```

## Refactoring

```text
Analyse ce code sans le réécrire immédiatement.

Identifie :
1. problèmes de conception ;
2. responsabilités mélangées ;
3. duplication ;
4. couplage ;
5. complexité ;
6. risques de régression.

Puis propose un refactoring minimal et explique pourquoi.
```

---

# MODULE 20 — VÉRIFICATION DU CODE GÉNÉRÉ PAR IA

## Règle fondamentale

> Une IA peut générer du code plausible mais incorrect.

Le code peut :

- compiler ;
- sembler propre ;
- avoir des tests ;
- être faux malgré tout.

## Méthode

```text
AI Generated Code
↓
Understand
↓
Review
↓
Test
↓
Security Review
↓
Benchmark if necessary
↓
Integrate
```

## Vérification logique

Question :

> Est-ce que le code réalise réellement le comportement demandé ?

## Vérification sécurité

Question :

> Que se passe-t-il si l’utilisateur est malveillant ?

## Vérification edge cases

Toujours examiner :

- entrée vide ;
- valeur maximale ;
- valeur minimale ;
- duplication ;
- concurrence ;
- timeout ;
- dépendance indisponible ;
- utilisateur non autorisé ;
- données inexistantes.

## Vérification dépendances

Ne laisse pas l’IA introduire une bibliothèque sans raison.

Vérifie :

- maintenance ;
- licence ;
- sécurité ;
- compatibilité ;
- nécessité réelle.

## Exercice

Demande à une IA une implémentation d’un endpoint d’inscription.

Puis construis toi-même une checklist de vérification avant de l’intégrer.

---

# MODULE 21 — PROJET COMPLET : SAAS DE GESTION DE FORMATIONS

## Objectif

Construire une application réaliste de bout en bout.

## Fonctionnalités

```text
Authentication
Users
Roles
Organizations
Courses
Lessons
Enrollment
Progress tracking
Payments
Notifications
Dashboard
Admin
Analytics
```

## Stack cible

```text
Frontend:
React + TypeScript

Backend:
Python + FastAPI

Database:
PostgreSQL

Cache:
Redis

Background jobs:
Celery/RQ ou équivalent

Tests:
Pytest + Playwright

Container:
Docker

CI/CD:
GitHub Actions
```

## Étapes

```text
1. Product discovery
2. Requirements
3. Architecture
4. Threat model
5. Database design
6. API design
7. Frontend architecture
8. Backlog
9. Implementation
10. Tests
11. Security
12. CI/CD
13. Deployment
14. Monitoring
15. Documentation
```

## MVP recommandé

Ne construis pas immédiatement :

- analytics avancées ;
- microservices ;
- système de recommandations ;
- architecture événementielle complexe.

Commence par :

```text
Auth
→ Courses
→ Lessons
→ Enrollment
→ Progress
```

Puis ajoute progressivement le reste.

---

# MODULE 22 — DÉVELOPPEMENT PAR ITÉRATIONS

## Workflow

Pour chaque feature :

```text
Feature
↓
Design
↓
Acceptance criteria
↓
Implementation
↓
Tests
↓
Review
↓
Security check
↓
Commit
```

## Exemple : Enrollment

### 1. Feature

Un étudiant peut s’inscrire à un cours.

### 2. Acceptance criteria

```text
- utilisateur authentifié ;
- cours existant ;
- utilisateur autorisé ;
- inscription unique ;
- réponse cohérente ;
- opération testée.
```

### 3. Questions

- inscription payante ou gratuite ?
- peut-on se désinscrire ?
- une inscription expirée existe-t-elle ?
- que se passe-t-il en cas de double clic ?
- plusieurs organisations peuvent-elles partager un cours ?

### 4. Design

```text
POST /courses/{course_id}/enrollment
```

### 5. Data

Contrainte possible :

```text
UNIQUE(user_id, course_id)
```

### 6. Test

Vérifier notamment la répétition de la requête.

## Règle

Une feature n’est pas « terminée » lorsque le code est écrit.

Elle est terminée lorsque :

```text
Comportement
+ tests
+ sécurité
+ review
+ documentation nécessaire
+ CI
```

sont satisfaits.

---

# MODULE 23 — DEBUGGING PROFESSIONNEL

## Méthode

```text
Symptom
↓
Hypotheses
↓
Evidence
↓
Isolation
↓
Root cause
↓
Fix
↓
Regression test
```

## Ne pas deviner

Mauvaise méthode :

```text
Ça doit être Redis.
```

Bonne méthode :

```text
Observation :
latence élevée uniquement sur GET /courses.

Hypothèse :
requête SQL coûteuse.

Expérience :
observer les requêtes et leur durée.

Résultat :
requête N+1.

Root cause :
chargement des relations dans une boucle.

Fix :
stratégie de chargement appropriée.

Regression :
test qui vérifie le comportement et éventuellement le nombre de requêtes.
```

## Bugs à pratiquer

- race condition ;
- N+1 ;
- état frontend incorrect ;
- transaction ;
- token expiré ;
- migration ;
- Docker ;
- CI ;
- performance ;
- memory leak ;
- exceptions.

## Challenge

Ne cherche pas le correctif en premier.

Pour chaque bug, produis :

```markdown
## Symptom

## Hypotheses

## Evidence

## Experiments

## Root cause

## Fix

## Regression test
```

---

# MODULE 24 — ARCHITECTURE ET DÉCISIONS TECHNIQUES

## Cadre de décision

```text
Problem
↓
Options
↓
Pros
↓
Cons
↓
Risks
↓
Cost
↓
Complexity
↓
Decision
↓
Reason
```

## Exemple : PostgreSQL vs MongoDB

Ne réponds pas :

> PostgreSQL est meilleur.

Analyse :

- structure des données ;
- relations ;
- transactions ;
- requêtes ;
- contraintes ;
- expertise équipe ;
- opérations ;
- évolution prévue.

## REST vs GraphQL

Questions :

- qui consomme l’API ?
- les besoins de données sont-ils très variables ?
- quelle complexité opérationnelle ?
- quel besoin de caching ?
- qui possède le contrat ?

## Sessions vs JWT

Questions :

- type de client ;
- révocation ;
- rotation ;
- stockage ;
- sécurité navigateur ;
- architecture.

## Celery vs background tasks

Questions :

- durée ;
- retry ;
- persistance ;
- volume ;
- scheduling ;
- monitoring ;
- tolérance aux pannes.

## Règle

Une décision technique doit pouvoir être expliquée à un autre ingénieur.

---

# MODULE 25 — DOCUMENTATION PROFESSIONNELLE

## README professionnel

```markdown
# Project Name

## Overview

## Features

## Architecture

## Tech stack

## Requirements

## Installation

## Environment variables

## Local development

## Tests

## Database

## Migrations

## API

## Deployment

## Monitoring

## Troubleshooting

## Security

## Contributing

## ADRs

## Changelog

## License
```

## Documentation d’architecture

Elle doit répondre :

- quels sont les composants ?
- pourquoi existent-ils ?
- comment communiquent-ils ?
- quelles sont les dépendances ?
- quelles sont les limites ?

## Documentation API

Documente :

- endpoints ;
- auth ;
- payloads ;
- erreurs ;
- pagination ;
- exemples ;
- limites.

## Environment variables

```markdown
| Variable | Obligatoire | Description | Exemple |
|---|---|---|---|
| DATABASE_URL | Oui | Connexion PostgreSQL | ... |
| REDIS_URL | Oui | Connexion Redis | ... |
| JWT_SECRET | Oui | Secret de signature | ... |
```

Ne mets jamais de secret réel dans la documentation.

---

# MODULE 26 — PROJET FINAL : PROFESSIONAL SOFTWARE AUDIT

## Objectif

Auditer l’application sous plusieurs perspectives.

```text
Senior Developer
Tech Lead
Software Architect
Security Engineer
QA Engineer
DevOps Engineer
Product Manager
```

## Audit Senior Developer

Chercher :

- bugs ;
- duplication ;
- complexité ;
- lisibilité ;
- dette technique ;
- responsabilités mal séparées.

## Audit Tech Lead

Chercher :

- cohérence globale ;
- maintenabilité ;
- décisions ;
- risques ;
- capacité d’évolution.

## Audit Architect

Chercher :

- couplage ;
- frontières ;
- dépendances ;
- choix technologiques ;
- complexité distribuée.

## Audit Security Engineer

Chercher :

- auth ;
- authorization ;
- injection ;
- XSS ;
- CSRF ;
- SSRF ;
- secrets ;
- uploads ;
- rate limiting ;
- logs.

## Audit QA

Chercher :

- couverture des flux critiques ;
- edge cases ;
- tests de régression ;
- tests d’intégration ;
- E2E.

## Audit DevOps

Chercher :

- CI/CD ;
- Docker ;
- secrets ;
- déploiement ;
- rollback ;
- health checks ;
- monitoring ;
- backups.

## Audit Product

Chercher :

- fonctionnalités réellement utiles ;
- UX ;
- incohérences métier ;
- risques business ;
- priorités.

## Format final

```markdown
# PROFESSIONAL SOFTWARE AUDIT

## Executive summary

## Critical findings

## High findings

## Medium findings

## Low findings

## Architecture assessment

## Security assessment

## Testing assessment

## Performance assessment

## DevOps assessment

## Documentation assessment

## Product assessment

## Technical debt

## Prioritized remediation plan

### P0
...

### P1
...

### P2
...

## Recommended next steps
```

---

# PROJET FINAL — PLAN D’EXÉCUTION COMPLET

## Phase 1 — Product discovery

Livrables :

```text
Problem statement
Personas
Business goals
Scope
Out of scope
Risks
Open questions
```

### Question

Quel est le minimum de produit permettant de vérifier que la solution apporte de la valeur ?

---

## Phase 2 — Requirements

Livrables :

```text
Functional requirements
Non-functional requirements
User stories
Acceptance criteria
```

---

## Phase 3 — Architecture

Livrables :

```text
Context diagram
Container/component view
Technology choices
ADR
```

Architecture initiale recommandée :

```text
                 React / TypeScript
                         |
                         v
                    FastAPI API
                    /    |    \
                   /     |     \
                  v      v      v
            PostgreSQL  Redis  External APIs
                         |
                         v
                  Background jobs
                         |
                         v
                    Monitoring
```

Cette architecture doit rester évolutive, mais simple tant que le besoin ne justifie pas davantage.

---

## Phase 4 — Threat model

Identifier :

```text
Assets
Actors
Trust boundaries
Threats
Impact
Mitigation
```

Exemples :

```text
Compte utilisateur compromis
→ accès aux données

IDOR / Broken Access Control
→ accès à un cours d'une autre organisation

Webhook falsifié
→ état de paiement incorrect

Upload malveillant
→ risque applicatif
```

---

## Phase 5 — Database design

Produire :

```text
ERD
Tables
Relations
Constraints
Indexes
Migrations
```

Question obligatoire pour chaque table :

> « Quelles sont les invariants que la base doit garantir ? »

---

## Phase 6 — API design

Documenter avant l’implémentation :

```text
POST /auth/login
POST /auth/refresh
GET /courses
POST /courses
GET /courses/{id}
POST /courses/{id}/enrollment
GET /me/progress
PATCH /lessons/{id}/progress
```

Pour chaque endpoint :

```text
Authentication
Authorization
Input
Output
Errors
Side effects
Idempotency
Tests
```

---

## Phase 7 — Frontend architecture

Définir :

```text
Routes
Features
Components
API client
Auth state
Server state
Forms
Validation
Error handling
```

---

## Phase 8 — Backlog

Découper en petites features :

```text
F001 — Project bootstrap
F002 — User registration
F003 — Login
F004 — Course creation
F005 — Course listing
F006 — Lesson management
F007 — Enrollment
F008 — Progress
F009 — Notifications
F010 — Payments
F011 — Admin
F012 — Analytics
```

Chaque feature doit avoir :

```text
Description
Acceptance criteria
Dependencies
Risks
Implementation plan
Tests
```

---

# DEFINITION OF DONE

Une feature n’est pas terminée simplement parce que « ça marche ».

```markdown
# Definition of Done

- [ ] Requirements compris
- [ ] Acceptance criteria validés
- [ ] Architecture cohérente
- [ ] Code implémenté
- [ ] Tests ajoutés
- [ ] Edge cases étudiés
- [ ] Security review effectuée
- [ ] Logs nécessaires ajoutés
- [ ] Documentation mise à jour
- [ ] CI verte
- [ ] Code reviewed
- [ ] Migration validée si nécessaire
- [ ] Déploiement possible
- [ ] Rollback compris
```

---

# BIBLIOTHÈQUE DE PROMPTS RÉUTILISABLES

## 1. Analyse de besoin

```text
Analyse le besoin suivant comme un Product Manager expérimenté.

[besoin]

Ne génère aucun code.

Identifie :
- utilisateurs ;
- objectifs ;
- fonctionnalités ;
- contraintes ;
- ambiguïtés ;
- risques ;
- hypothèses ;
- questions de clarification.

Sépare explicitement :
- functional requirements ;
- non-functional requirements.
```

## 2. Architecture

```text
Agis comme un Software Architect.

Contexte :
[contexte]

Contraintes :
[contraintes]

Propose 2 à 4 architectures.

Pour chaque option :
- description ;
- avantages ;
- inconvénients ;
- complexité ;
- coût ;
- risques ;
- capacité d'évolution.

Termine par une recommandation argumentée.

Ne génère aucun code.
```

## 3. Database review

```text
Review ce modèle PostgreSQL.

Contexte :
[contexte]

Analyse :
- normalisation ;
- contraintes ;
- relations ;
- indexes ;
- transactions ;
- volumétrie ;
- risques N+1 ;
- migrations ;
- audit fields.

Identifie les problèmes par priorité.

Ne propose une optimisation que si elle est justifiée.
```

## 4. API review

```text
Review cette API comme un senior backend engineer.

Vérifie :
- REST semantics ;
- validation ;
- errors ;
- status codes ;
- authorization ;
- pagination ;
- idempotency ;
- rate limiting ;
- sensitive data ;
- documentation ;
- tests.

Donne :
Problem
Why
Risk
Recommendation
Test
```

## 5. Code review

```text
Review ce code.

Ordre :
1. correctness
2. security
3. architecture
4. data
5. error handling
6. performance
7. tests
8. maintainability
9. style

Ne commente pas le style si un problème plus important existe.
```

## 6. Debugging

```text
Je veux diagnostiquer ce problème sans sauter directement au correctif.

Symptom:
...

Expected:
...

Observed:
...

Environment:
...

Logs:
...

Recent changes:
...

Formule d'abord plusieurs hypothèses.
Pour chaque hypothèse, donne l'expérience permettant de la confirmer ou de l'écarter.
```

## 7. Security

```text
Effectue une Security Review.

Cherche activement :
- authentication
- authorization
- IDOR
- injection
- XSS
- CSRF
- SSRF
- secrets
- uploads
- rate limiting
- brute force
- sensitive data exposure
- insecure error handling

Pour chaque finding :
Severity
Evidence
Impact
Root cause
Mitigation
Regression test
```

## 8. Tests

```text
À partir de cette fonctionnalité :

[feature]

Construis une stratégie de tests.

Inclure :
- happy path ;
- edge cases ;
- authorization ;
- failure modes ;
- concurrency/idempotency si pertinent ;
- integration tests ;
- E2E si nécessaire.

Évite les tests qui vérifient seulement l'implémentation interne.
```

## 9. Documentation

```text
Transforme ces informations en documentation professionnelle.

Projet :
...

Audience :
développeur qui rejoint le projet.

Inclure :
- overview ;
- architecture ;
- setup ;
- environment variables ;
- tests ;
- migrations ;
- deployment ;
- troubleshooting ;
- security ;
- contribution.
```

---

# CHECKLIST GLOBALE — AVANT PRODUCTION

```markdown
# Production Readiness Checklist

## Product
- [ ] Requirements validés
- [ ] Critical user flows validés

## Architecture
- [ ] Architecture documentée
- [ ] ADR importantes
- [ ] Failure modes étudiés

## Database
- [ ] Schema validé
- [ ] Constraints
- [ ] Index
- [ ] Migrations
- [ ] Backup
- [ ] Restore testé

## API
- [ ] Validation
- [ ] Errors
- [ ] Authentication
- [ ] Authorization
- [ ] Rate limiting
- [ ] Documentation

## Frontend
- [ ] Loading states
- [ ] Empty states
- [ ] Error states
- [ ] Authorization
- [ ] Validation

## Security
- [ ] Threat model
- [ ] Secrets
- [ ] Headers
- [ ] CORS
- [ ] CSRF si pertinent
- [ ] XSS
- [ ] Injection
- [ ] SSRF
- [ ] Access control
- [ ] Uploads

## Tests
- [ ] Unit
- [ ] Integration
- [ ] API
- [ ] E2E
- [ ] Security
- [ ] Regression

## Code quality
- [ ] Lint
- [ ] Type checking
- [ ] Review
- [ ] No debug code

## CI/CD
- [ ] CI
- [ ] Build
- [ ] Security checks
- [ ] Staging
- [ ] Production
- [ ] Rollback

## Docker
- [ ] Multi-stage si pertinent
- [ ] Non-root user si pertinent
- [ ] Health checks
- [ ] Configuration externe
- [ ] Images maîtrisées

## Observability
- [ ] Structured logs
- [ ] Metrics
- [ ] Traces si pertinent
- [ ] Alerts
- [ ] Correlation IDs
- [ ] Error tracking

## Operations
- [ ] Runbook
- [ ] Incident checklist
- [ ] Backup
- [ ] Disaster recovery
- [ ] On-call ownership
```

---

# CHECKLIST — AVANT CHAQUE FEATURE

```markdown
# Feature Checklist

## Compréhension
- [ ] Problème compris
- [ ] User story claire
- [ ] Acceptance criteria

## Design
- [ ] Impact architecture
- [ ] Impact database
- [ ] Impact API
- [ ] Impact frontend
- [ ] Dependencies

## Risques
- [ ] Security
- [ ] Performance
- [ ] Concurrency
- [ ] Failure modes

## Implementation
- [ ] Petit changement
- [ ] Code typé lorsque pertinent
- [ ] Abstractions justifiées

## Validation
- [ ] Unit tests
- [ ] Integration tests
- [ ] E2E si nécessaire
- [ ] Security check
- [ ] Manual verification

## Livraison
- [ ] Code review
- [ ] Documentation
- [ ] CI
- [ ] Commit
```

---

# CHECKLIST — REVUE D’UNE SORTIE IA

```markdown
# AI Output Review

## Compréhension
- [ ] Je comprends tout le code proposé
- [ ] Les hypothèses sont explicites

## Correctness
- [ ] Le comportement correspond au besoin
- [ ] Les edge cases sont couverts

## Security
- [ ] Authorization
- [ ] Validation
- [ ] Secrets
- [ ] Sensitive data
- [ ] External input

## Architecture
- [ ] Respecte l'architecture
- [ ] Pas d'abstraction inutile
- [ ] Pas de couplage inutile

## Dependencies
- [ ] Bibliothèques nécessaires
- [ ] Versions compatibles
- [ ] Risques étudiés

## Tests
- [ ] Tests utiles
- [ ] Tests négatifs
- [ ] Tests de sécurité si pertinent

## Performance
- [ ] Pas de requêtes inutiles
- [ ] Pas de boucle coûteuse évidente
- [ ] Pas d'optimisation prématurée

## Final
- [ ] Lint
- [ ] Type check
- [ ] Tests
- [ ] Review humaine
```

---

# PROGRESSION PÉDAGOGIQUE RECOMMANDÉE

Ne cherche pas à mémoriser tout le contenu.

Pour chaque concept :

```text
Comprendre
↓
Voir un exemple
↓
Faire un exercice
↓
Faire une erreur
↓
Analyser l'erreur
↓
Refaire
↓
Appliquer au projet
```

## Règle de progression

Si tu peux copier une solution mais que tu ne peux pas expliquer :

- pourquoi elle existe ;
- quelles alternatives existaient ;
- quels sont ses risques ;
- comment la tester ;
- comment la sécuriser ;

alors tu ne maîtrises pas encore réellement la notion.

---

# MODE DE TRAVAIL AVEC L’IA

## Niveau 1 — Assistant

L’IA explique.

Tu décides.

## Niveau 2 — Pair programmer

L’IA propose.

Tu reviews.

## Niveau 3 — Reviewer

L’IA critique ton travail.

Tu choisis les corrections.

## Niveau 4 — Tech Lead simulé

L’IA challenge :

- architecture ;
- risques ;
- sécurité ;
- dette ;
- tests ;
- production.

## Niveau 5 — Ingénieur augmenté

Tu gardes la responsabilité de :

```text
Problem framing
Decision making
Risk assessment
Validation
Integration
Ownership
```

L’IA accélère :

```text
Exploration
Drafting
Boilerplate
Test generation
Documentation
Analysis
```

---

# EXERCICES RÉCURRENTS

## Architecture challenge

> Tu dois construire un SaaS avec 2 développeurs et un budget limité. Quelle architecture choisis-tu ? Pourquoi ?

## Security challenge

> Un utilisateur connecté peut modifier une ressource appartenant à un autre utilisateur. Où se situe le vrai problème ?

## Code review challenge

> Une route FastAPI contient 180 lignes, cinq appels SQL et deux appels externes. Quels problèmes cherches-tu ?

## Debugging challenge

> Le endpoint passe de 100 ms à 4 secondes lorsque la base contient beaucoup de données. Quelles hypothèses testes-tu ?

## Refactoring challenge

> Une fonction contient validation, SQL, email et logique métier. Comment la découpes-tu sans créer 15 classes inutiles ?

## AI challenge

> L’IA propose 300 lignes de code et 20 nouvelles dépendances pour une feature de 30 lignes. Acceptes-tu ? Pourquoi ?

---

# CRITÈRES DE MATURITÉ

## Niveau 1 — Développeur fonctionnel

Tu sais :

- coder ;
- utiliser un framework ;
- construire une fonctionnalité.

## Niveau 2 — Développeur structuré

Tu sais :

- découper ;
- tester ;
- utiliser Git ;
- organiser le code.

## Niveau 3 — Software Engineer

Tu sais :

- concevoir ;
- raisonner sur les trade-offs ;
- sécuriser ;
- tester ;
- diagnostiquer.

## Niveau 4 — Senior Engineer

Tu sais :

- anticiper les risques ;
- simplifier ;
- reviewer ;
- faire évoluer une architecture ;
- prendre des décisions sous contraintes.

## Niveau 5 — Tech Lead

Tu sais :

- cadrer le problème ;
- aligner produit et technique ;
- faire émerger les décisions ;
- réduire les risques ;
- guider d’autres développeurs ;
- arbitrer complexité, coût, qualité et délai.

---

# RÈGLES D’OR

## 1. Comprendre avant de coder

```text
Problem first.
Code second.
```

## 2. La simplicité est une fonctionnalité

Une architecture simple est plus facile à :

- tester ;
- déployer ;
- comprendre ;
- monitorer ;
- maintenir.

## 3. Ne pas optimiser prématurément

Mesure d’abord.

## 4. Ne pas sécuriser uniquement à la frontière frontend

Le serveur doit appliquer les permissions.

## 5. Les tests servent le risque

Le nombre de tests n’est pas un objectif en soi.

## 6. Les abstractions ont un coût

Une abstraction doit résoudre un problème réel.

## 7. Les données doivent être protégées à plusieurs niveaux

Application + base + infrastructure + secrets + logs.

## 8. Tout système distribué ajoute des problèmes

Réseau, retries, timeouts, ordre, duplication, observabilité, cohérence.

## 9. L’IA est faillible

Toujours :

```text
Understand
→ Review
→ Test
→ Verify
```

## 10. Un bon ingénieur sait dire « non »

Pas :

> « Non parce que je préfère autre chose. »

Mais :

> « Non, parce que cette solution ajoute X de complexité pour résoudre un problème que nous n’avons pas encore. »

---

# CAPSTONE — MISSION FINALE

Tu es maintenant responsable technique d’un SaaS de gestion de formations.

Le produit doit permettre :

```text
Organizations
Users
Roles
Courses
Lessons
Enrollments
Progress
Payments
Notifications
Admin
Analytics
```

Tu dois produire, dans cet ordre :

```text
01-product-discovery.md
02-requirements.md
03-user-stories.md
04-architecture.md
05-adrs/
06-threat-model.md
07-database-design.md
08-api-design.md
09-frontend-architecture.md
10-backlog.md
11-implementation/
12-testing-strategy.md
13-security-review.md
14-ci-cd.md
15-deployment.md
16-observability.md
17-runbook.md
18-readme.md
19-incident-report.md
20-final-audit.md
```

## Gate 1 — Product

Impossible de passer à l’architecture si les exigences critiques restent ambiguës.

## Gate 2 — Architecture

Impossible de coder une feature importante sans savoir :

- où elle vit ;
- quelles données elle manipule ;
- quelles permissions elle nécessite ;
- quels sont ses risques.

## Gate 3 — Tests

Impossible de considérer une feature critique comme terminée sans stratégie de validation.

## Gate 4 — Security

Impossible de déployer sans vérifier les surfaces d’attaque principales.

## Gate 5 — Production

Impossible de considérer l’application prête sans :

- logs ;
- monitoring ;
- backup ;
- rollback ;
- documentation opérationnelle.

---

# AUDIT FINAL — FORMAT

```markdown
# PROFESSIONAL SOFTWARE AUDIT

Date:

## Executive Summary

### Overall assessment
[summary]

### Main strengths
- ...

### Main risks
- ...

---

## Critical

### CRIT-001
Title:
Evidence:
Impact:
Recommendation:
Owner:
Priority:

---

## High

### HIGH-001
...

---

## Medium

### MED-001
...

---

## Low

### LOW-001
...

---

# Architecture

## Strengths
## Weaknesses
## Evolution risks

# Security

## Authentication
## Authorization
## Data protection
## Injection
## Browser security
## Secrets
## External integrations

# Quality

## Code
## Tests
## CI

# Performance

## Database
## API
## Frontend
## Background jobs

# Operations

## Deployment
## Monitoring
## Backup
## Recovery

# Documentation

# Technical debt

# Prioritized remediation plan

## P0
Immediate risk reduction.

## P1
Important engineering improvements.

## P2
Medium-term improvements.

## P3
Nice-to-have improvements.

# Final recommendation
```

---

# CONCLUSION

Le but de cette formation n’est pas de te rendre meilleur à produire rapidement du code.

Le but est de te rendre meilleur à **construire des systèmes dont tu peux expliquer et défendre les décisions**.

À chaque projet, pense :

```text
Pourquoi ?
↓
Pour qui ?
↓
Sous quelles contraintes ?
↓
Quelle architecture ?
↓
Quels risques ?
↓
Quelles données ?
↓
Quel contrat ?
↓
Comment tester ?
↓
Comment sécuriser ?
↓
Comment déployer ?
↓
Comment observer ?
↓
Comment réparer ?
↓
Comment faire évoluer ?
```

Et lorsque l’IA intervient :

```text
Je définis le problème
↓
Je fournis le contexte
↓
Je fixe les contraintes
↓
Je demande une proposition
↓
Je critique la proposition
↓
Je vérifie le code
↓
Je teste
↓
Je sécurise
↓
Je décide
↓
J’intègre
```

**L’IA peut accélérer ton ingénierie. Elle ne doit pas remplacer ton ingénierie.**

---

# PREMIÈRE SESSION DE TRAVAIL

Commence par le Module 1.

### Mission

Avant d’écrire la moindre ligne de code pour le SaaS de formation, produis :

```markdown
# Product Engineering Brief

## 1. Problème

## 2. Utilisateurs

## 3. Objectifs business

## 4. Fonctionnalités principales

## 5. Hors périmètre

## 6. Contraintes

## 7. Risques

## 8. Données sensibles

## 9. Questions ouvertes

## 10. Hypothèses
```

Puis réponds à cette question :

> **Quelle est, selon toi, la plus grosse erreur qu’un développeur pourrait faire en commençant immédiatement à coder ce projet ?**

Ne commence pas par coder.

Commence par **penser comme un Software Engineer**.
