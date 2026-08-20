# Formation : Design System & Architecture Logicielle

> Formation complète pour développeur fullstack (Python / Flask / Django) — de la notion de base à l'application concrète en projet réel.

---

## Sommaire

**Partie 1 — Design System**
1. Qu'est-ce qu'un Design System
2. Les Design Tokens
3. Les composants UI et leurs états
4. Typographie, couleurs, espacement (les fondations)
5. Documentation et gouvernance d'un Design System
6. Outils du marché (Storybook, Figma, etc.)
7. Construire son propre Design System pas à pas

**Partie 2 — Architecture Logicielle**
8. Qu'est-ce que l'architecture logicielle
9. Architecture en couches (Layered Architecture)
10. MVC, MVT et leurs variantes
11. Architecture Hexagonale / Clean Architecture
12. Monolithe vs Microservices vs Serverless
13. Architecture Front-End (état, routing, composants)
14. Principes transverses : SOLID, DRY, KISS, séparation des responsabilités
15. Choisir son architecture selon le projet

**Partie 3 — Application pratique**
16. Étude de cas : structurer un projet Flask/Django avec une vraie architecture
17. Étude de cas : construire un mini Design System pour un projet perso
18. Checklist finale et ressources pour aller plus loin

---

# PARTIE 1 — DESIGN SYSTEM

## 1. Qu'est-ce qu'un Design System

Un **Design System** est bien plus qu'une charte graphique. C'est un **écosystème complet** composé de :

- Des **règles** (couleurs, typographie, espacements...)
- Des **composants réutilisables** (boutons, cards, inputs...)
- Une **documentation** qui explique comment et quand les utiliser
- Une **gouvernance** qui décide comment le système évolue dans le temps

### Design System vs UI Kit vs Charte graphique

Ces trois termes sont souvent confondus. Voici la différence :

| Élément | Contenu | Vivant ? |
|---|---|---|
| **Charte graphique** | Logo, couleurs, polices (souvent un PDF statique) | Non, figé |
| **UI Kit** | Bibliothèque de composants visuels (souvent juste dans Figma) | Peu évolutif |
| **Design System** | Charte + UI Kit + code + documentation + règles d'usage | Oui, versionné et maintenu comme un produit |

**Point clé à retenir** : un Design System est traité comme **un produit à part entière**, avec ses propres releases, sa documentation, ses contributeurs — pas comme un simple dossier de maquettes.

### Pourquoi ça existe

Imagine une équipe de 10 développeurs qui codent chacun leurs boutons différemment : tailles différentes, couleurs légèrement différentes, comportements au survol différents. Résultat : une application incohérente, du code dupliqué, et une expérience utilisateur dégradée.

Le Design System résout 4 problèmes majeurs :

1. **Incohérence visuelle** → un seul bouton "primary" existe, utilisé partout
2. **Perte de temps** → on ne réinvente pas la roue à chaque nouvelle page
3. **Dette technique** → moins de CSS dupliqué, plus facile à maintenir
4. **Communication designer ↔ développeur** → un langage commun, les mêmes noms des deux côtés

---

## 2. Les Design Tokens

Les **design tokens** sont les valeurs atomiques, les plus petites briques du Design System. Ce sont des variables nommées qui remplacent les valeurs "en dur" dans le code.

### Exemple concret

Au lieu d'écrire dans ton CSS :

```css
.button {
  background-color: #2563EB;
  padding: 12px 24px;
  border-radius: 8px;
}
```

Tu écris :

```css
.button {
  background-color: var(--color-primary-500);
  padding: var(--spacing-md) var(--spacing-lg);
  border-radius: var(--radius-md);
}
```

**Pourquoi c'est puissant** : si demain tu changes `--color-primary-500` de bleu à violet, **tout ton site change automatiquement**, partout où ce token est utilisé, sans toucher au reste du code.

### Les grandes catégories de tokens

**a) Couleurs**
```css
--color-primary-100: #DBEAFE;
--color-primary-500: #2563EB;
--color-primary-900: #1E3A8A;
--color-neutral-50:  #FAFAFA;
--color-neutral-900: #171717;
--color-success: #16A34A;
--color-warning: #D97706;
--color-error:   #DC2626;
```
On utilise une **échelle numérique** (50 → 900) pour avoir des nuances claires et foncées d'une même couleur, ce qui est indispensable pour gérer le mode sombre (dark mode) plus tard.

**b) Typographie**
```css
--font-family-base: 'Inter', sans-serif;
--font-size-xs: 12px;
--font-size-sm: 14px;
--font-size-base: 16px;
--font-size-lg: 20px;
--font-size-xl: 28px;
--font-weight-regular: 400;
--font-weight-bold: 700;
--line-height-tight: 1.2;
--line-height-normal: 1.5;
```

**c) Espacements (spacing)**
On utilise en général une échelle basée sur un multiple de 4 ou 8 pixels, c'est un standard de l'industrie :
```css
--spacing-xs: 4px;
--spacing-sm: 8px;
--spacing-md: 16px;
--spacing-lg: 24px;
--spacing-xl: 32px;
--spacing-2xl: 48px;
```

**d) Rayons de bordure, ombres, transitions**
```css
--radius-sm: 4px;
--radius-md: 8px;
--radius-full: 9999px;

--shadow-sm: 0 1px 2px rgba(0,0,0,0.05);
--shadow-md: 0 4px 6px rgba(0,0,0,0.1);

--transition-fast: 150ms ease-in-out;
```

### Tokens à deux niveaux (avancé)

Dans les Design Systems matures, on sépare souvent :

- **Tokens globaux** (raw tokens) : `blue-500: #2563EB`
- **Tokens sémantiques** (alias tokens) : `color-primary: {blue-500}`, `color-danger: {red-500}`

**L'intérêt** : le token sémantique décrit **l'intention** ("primary", "danger"), pas juste la couleur physique. Si tu changes ta couleur de marque, tu changes un seul alias, pas tous les usages dans le code.

---

## 3. Les composants UI et leurs états

Un composant n'est jamais "juste un bouton". Il faut penser à **tous ses états possibles** :

| État | Description |
|---|---|
| Default | État normal |
| Hover | Survol souris |
| Focus | Sélectionné au clavier (accessibilité) |
| Active/Pressed | Pendant le clic |
| Disabled | Non cliquable |
| Loading | En attente d'une action (spinner) |
| Error | Ex : un champ de formulaire invalide |

### Exemple : anatomie d'un composant Button

```
Bouton "Primary"
├── Variantes : primary, secondary, ghost, danger
├── Tailles : sm, md, lg
├── États : default, hover, focus, disabled, loading
├── Avec icône : gauche, droite, seule (icon-only)
└── Props : label, onClick, disabled, loading, fullWidth
```

**Bonne pratique avancée** : documenter chaque composant avec sa **matrice de variantes** (variante × taille × état) pour ne rien oublier — c'est ce que font les vrais Design Systems (Ant Design, Material UI...).

### Composants atomiques vs composés (Atomic Design)

Une méthode très utilisée, créée par Brad Frost, structure les composants en 5 niveaux :

1. **Atomes** : bouton, input, label, icône (les plus petits éléments)
2. **Molécules** : un champ de recherche = input + bouton (combinaison d'atomes)
3. **Organismes** : une barre de navigation complète, un formulaire entier
4. **Templates** : structure de page sans contenu réel
5. **Pages** : le template rempli avec du vrai contenu

Cette hiérarchie t'aide à savoir **où** placer un nouveau composant et à éviter la duplication.

---

## 4. Fondations : typographie, couleurs, espacement

### La hiérarchie typographique

Une bonne hiérarchie typographique guide l'œil sans effort. Exemple type :

```
H1 — 32px / bold      → titre de page (un seul par page)
H2 — 24px / bold      → sections principales
H3 — 20px / semibold  → sous-sections
Body — 16px / regular → texte courant
Small — 14px / regular→ légendes, notes
Caption — 12px        → mentions légales, métadonnées
```

**Règle d'or** : ne jamais dépasser 3-4 tailles de police différentes sur une même page.

### La règle du contraste (accessibilité)

Le ratio de contraste entre le texte et son fond doit respecter les normes **WCAG** :
- Texte normal : ratio minimum **4.5:1**
- Texte large (18px+ bold) : ratio minimum **3:1**

C'est un point souvent oublié par les développeurs autodidactes mais **indispensable** pour un niveau professionnel.

### Le système d'espacement en grille (8pt grid)

La majorité des Design Systems professionnels (Material Design, Apple HIG) utilisent une grille de **8px** comme unité de base. Cela garantit un alignement visuel cohérent partout, sans que le designer/développeur n'ait à "deviner" des valeurs comme 13px ou 22px.

---

## 5. Documentation et gouvernance

Un Design System sans documentation n'est pas un Design System — c'est juste une collection de fichiers.

### Ce que doit contenir la documentation

- **Vue d'ensemble** : philosophie, principes de design
- **Fondations** : couleurs, typo, spacing, grille
- **Composants** : chaque composant avec exemples de code + captures
- **Guidelines d'usage** : "Utiliser le bouton danger uniquement pour les actions destructives (suppression)"
- **Changelog** : historique des versions (comme un vrai package logiciel)

### La gouvernance : qui décide ?

Dans une équipe, il faut définir :
- Qui peut **proposer** un nouveau composant
- Qui **valide** son intégration au Design System
- Comment on gère le **versionnement sémantique** (semver : `1.2.0` → ajout non cassant, `2.0.0` → breaking change)

En solo (comme pour tes projets CVCraft ou NEXUS ACADEMY), la gouvernance se résume à : **documenter tes propres règles dans un fichier `DESIGN.md`** pour rester cohérent avec toi-même au fil du temps.

---

## 6. Outils du marché

| Outil | Usage |
|---|---|
| **Figma** | Conception visuelle, prototypage, source de vérité design |
| **Storybook** | Catalogue interactif des composants codés (React, Vue...) |
| **Style Dictionary** | Transforme les tokens (JSON) en CSS/SCSS/JS automatiquement |
| **Tailwind CSS** | Framework utilitaire qui peut *implémenter* un Design System via son fichier de config |
| **Zeroheight** | Documentation de Design System liée à Figma |

### Exemples de Design Systems open source à étudier

- **Material Design** (Google) — le plus complet et documenté
- **Ant Design** — excellent pour les back-offices/dashboards (proche de tes besoins Flask/Django admin)
- **Chakra UI / Radix UI** — approche moderne, accessible par défaut
- **Polaris** (Shopify) — très bien pensé pour les interfaces e-commerce

---

## 7. Construire son propre Design System, étape par étape

1. **Auditer** l'existant (captures d'écran de toutes tes pages actuelles, repérer les incohérences)
2. **Définir les tokens** (couleurs, typo, spacing) dans un fichier central (`tokens.css` ou `tokens.json`)
3. **Lister les composants** nécessaires (formulaire de contact, carte projet, navbar...)
4. **Créer les composants** un par un avec toutes leurs variantes/états
5. **Documenter** au fur et à mesure (même un simple fichier Markdown suffit au début)
6. **Itérer** : le Design System n'est jamais "fini", il évolue avec le produit

---

# PARTIE 2 — ARCHITECTURE LOGICIELLE

## 8. Qu'est-ce que l'architecture logicielle

L'architecture logicielle définit **comment le code est organisé** et **comment les différentes parties du système communiquent**. C'est la structure invisible qui détermine si un projet est facile ou difficile à faire évoluer.

### Les enjeux d'une bonne architecture

- **Maintenabilité** : facilité à corriger des bugs ou ajouter des fonctionnalités
- **Testabilité** : facilité à écrire des tests unitaires/fonctionnels
- **Scalabilité** : capacité à gérer plus d'utilisateurs/de données
- **Découplage** : les modules ne dépendent pas trop les uns des autres
- **Lisibilité** : un nouveau développeur doit comprendre où va son code

### Mauvaise architecture = "code spaghetti"

Signes qu'un projet a une mauvaise architecture :
- Toute la logique métier est dans les vues/routes (`views.py`, `app.py`)
- Impossible de tester une fonction sans lancer tout le serveur
- Un changement dans la base de données casse l'affichage
- Copier-coller du même code à plusieurs endroits

---

## 9. Architecture en couches (Layered Architecture)

C'est l'architecture la plus classique et la plus facile à comprendre. On sépare le code en **couches horizontales**, chacune avec une responsabilité précise.

```
┌─────────────────────────────┐
│   Présentation (UI/API)     │  ← Templates, routes Flask, vues Django
├─────────────────────────────┤
│   Logique métier (Service)  │  ← Règles business, calculs, validations
├─────────────────────────────┤
│   Accès aux données (Repo)  │  ← Requêtes SQL, ORM
├─────────────────────────────┤
│   Base de données           │  ← PostgreSQL, MySQL...
└─────────────────────────────┘
```

**Règle fondamentale** : une couche ne parle qu'à la couche immédiatement en dessous. La couche présentation ne doit **jamais** faire de requête SQL directement.

### Exemple concret en Flask

**Mauvaise pratique** (tout dans la route) :
```python
@app.route('/users/<id>')
def get_user(id):
    user = db.session.execute(f"SELECT * FROM users WHERE id={id}")
    if user.age < 18:
        return "Accès refusé", 403
    return render_template('user.html', user=user)
```

**Bonne pratique** (couches séparées) :
```python
# repository.py — couche accès données
def get_user_by_id(user_id):
    return User.query.get(user_id)

# services.py — couche logique métier
def can_access_content(user):
    return user.age >= 18

# routes.py — couche présentation
@app.route('/users/<id>')
def get_user(id):
    user = get_user_by_id(id)
    if not can_access_content(user):
        return "Accès refusé", 403
    return render_template('user.html', user=user)
```

**Avantage énorme** : tu peux maintenant tester `can_access_content(user)` avec un simple test unitaire, sans base de données, sans serveur web.

---

## 10. MVC, MVT et leurs variantes

### MVC (Model-View-Controller)

- **Model** : représente les données et la logique métier (ex : classes SQLAlchemy)
- **View** : ce que voit l'utilisateur (le HTML rendu)
- **Controller** : reçoit la requête, orchestre Model et View

### MVT (Model-View-Template) — spécifique à Django

Django utilise une variante nommée MVT, où les rôles sont légèrement redistribués :

| MVC classique | Django (MVT) |
|---|---|
| Model | Model (identique) |
| Controller | **View** (fichier `views.py`) |
| View | **Template** (fichier `.html`) |

**Piège classique** : en Django, ce que le cours "MVC classique" appelle une "View" s'appelle un "Template", et ce qu'on appelle un "Controller" s'appelle une "View". Cette confusion de vocabulaire perd beaucoup de développeurs débutants.

### Schéma du flux MVT en Django

```
Requête HTTP
   ↓
urls.py (routeur)
   ↓
views.py (View — logique de traitement)
   ↓                ↓
models.py       templates/*.html
(données)        (affichage)
   ↓
Réponse HTTP
```

---

## 11. Architecture Hexagonale / Clean Architecture

C'est un niveau **avancé**, utilisé dans les applications complexes ou critiques. L'idée centrale : **la logique métier ne doit dépendre d'aucun détail technique** (ni la base de données, ni le framework web, ni l'API externe).

### Le principe du hexagone

```
              ┌───────────────┐
   API REST → │               │ ← Base de données
              │   DOMAINE     │
   CLI      → │  (logique     │ → Email/SMS
              │   métier      │
   Tests    → │   pure)       │ ← API externe
              └───────────────┘
```

Le **domaine** (le cœur métier) ne connaît rien de Flask, de Django, ni de PostgreSQL. Il ne connaît que ses propres règles. Les technologies externes (web, BDD, email...) sont des "adaptateurs" branchés autour.

### Pourquoi c'est puissant

- Tu peux **changer de base de données** (PostgreSQL → MongoDB) sans toucher à la logique métier
- Tu peux **tester** toute la logique métier sans jamais démarrer une vraie base de données
- Tu peux **changer de framework web** (Flask → FastAPI) sans réécrire les règles business

### Exemple simplifié

```python
# domain/order.py — cœur métier, aucune dépendance externe
class Order:
    def __init__(self, items):
        self.items = items

    def total_price(self):
        return sum(item.price for item in self.items)

    def apply_discount(self, percent):
        return self.total_price() * (1 - percent / 100)

# infrastructure/db_repository.py — adaptateur BDD (détail technique)
class SQLOrderRepository:
    def save(self, order):
        # code SQLAlchemy ici
        pass

# infrastructure/flask_routes.py — adaptateur web (détail technique)
@app.route('/order/<id>/total')
def get_total(id):
    order = repository.find(id)
    return {"total": order.total_price()}
```

Remarque : `Order` ne sait pas qu'il existe une base de données ou une route Flask. C'est ça, l'architecture hexagonale.

**Note pour progresser** : ce niveau n'est pas nécessaire pour un petit projet perso (CVCraft), mais c'est un concept que les recruteurs valorisent énormément à l'entretien — savoir en parler donne un net avantage.

---

## 12. Monolithe vs Microservices vs Serverless

### Monolithe

Toute l'application (backend, logique métier, souvent le frontend) est dans **un seul projet**, déployée comme **un seul bloc**.

**Avantages** : simple à développer, à déployer, à débugger (un seul endroit à regarder). Idéal pour démarrer un projet ou une petite/moyenne équipe.

**Inconvénients** : difficile à scaler indépendamment (si une seule fonctionnalité est lente, il faut scaler toute l'app), un bug peut potentiellement tout faire tomber.

### Microservices

L'application est découpée en **plusieurs petits services indépendants**, chacun avec sa propre base de données, déployés séparément, qui communiquent via API (REST, gRPC) ou messages (Kafka, RabbitMQ).

```
[Service Auth] ←→ [Service Commandes] ←→ [Service Paiement]
      ↓                    ↓                     ↓
   BDD Auth          BDD Commandes           BDD Paiement
```

**Avantages** : chaque service peut être développé, déployé et scalé indépendamment ; une équipe par service ; résilience (la panne d'un service n'arrête pas forcément les autres).

**Inconvénients** : complexité énorme (réseau, cohérence des données, monitoring distribué). **Ce n'est pas fait pour les petits projets** — une erreur fréquente chez les débutants est de vouloir faire des microservices trop tôt.

### Serverless

Le code s'exécute sous forme de **fonctions déclenchées à la demande** (AWS Lambda, Google Cloud Functions), sans gérer de serveur. Tu payes uniquement à l'exécution.

**Avantages** : zéro gestion d'infrastructure, scalabilité automatique.
**Inconvénients** : "cold starts" (latence au démarrage), plus complexe pour des applications avec état (stateful), coûts imprévisibles à grande échelle.

### Quel choix pour toi aujourd'hui ?

Pour tes projets actuels (**CVCraft**, **NEXUS ACADEMY**) et pour décrocher ton premier emploi : **le monolithe bien structuré (en couches) est le bon choix**. C'est aussi ce qui est le plus attendu en entretien junior — savoir *pourquoi* on ne fait pas de microservices dès le départ est plus valorisé que d'en faire sans raison.

---

## 13. Architecture Front-End

Même côté client (JS), il existe une architecture à respecter :

- **Composants** : découper l'interface en blocs réutilisables (inspiré de l'Atomic Design vu en Partie 1)
- **State management** : où et comment stocker l'état de l'application (ex : Context API, Redux, Zustand en React ; ou simplement des variables bien organisées en JS vanilla)
- **Routing** : gestion des routes côté client (React Router) vs côté serveur (Flask/Django gèrent ça nativement)
- **Séparation logique/présentation** : éviter de mélanger les appels API directement dans le rendu visuel — utiliser des hooks/services dédiés

---

## 14. Principes transverses

Ces principes s'appliquent à **toute architecture**, quel que soit le style choisi.

### SOLID (les 5 principes de la POO)

- **S** — Single Responsibility : une classe = une seule responsabilité
- **O** — Open/Closed : le code doit être ouvert à l'extension, fermé à la modification
- **L** — Liskov Substitution : une sous-classe doit pouvoir remplacer sa classe mère sans casser le programme
- **I** — Interface Segregation : préférer plusieurs petites interfaces à une seule grande
- **D** — Dependency Inversion : dépendre d'abstractions, pas d'implémentations concrètes

### DRY — Don't Repeat Yourself
Ne jamais dupliquer une même logique à plusieurs endroits. Si tu copies-colles du code, c'est un signal qu'il faut créer une fonction/module partagé.

### KISS — Keep It Simple, Stupid
Ne pas sur-architecturer un petit projet. L'architecture hexagonale n'a aucun sens sur un formulaire de contact.

### Separation of Concerns
Chaque module doit avoir une responsabilité claire et ne pas empiéter sur celle des autres (c'est le principe qui sous-tend toutes les architectures en couches vues plus haut).

---

## 15. Choisir son architecture selon le projet

| Contexte | Architecture recommandée |
|---|---|
| Petit projet perso / MVP | Monolithe simple, MVC/MVT |
| Projet avec logique métier complexe | Monolithe en couches, voire Clean Architecture |
| Startup en croissance | Monolithe modulaire (facile à découper en microservices plus tard) |
| Grande entreprise, plusieurs équipes | Microservices |
| Traitement ponctuel/événementiel | Serverless |

**Règle d'or à retenir pour toute ta carrière** : *"Commence simple, complexifie seulement quand le besoin réel apparaît."* La sur-ingénierie est une des erreurs les plus fréquentes, même chez les développeurs expérimentés.

---

# PARTIE 3 — APPLICATION PRATIQUE

## 16. Étude de cas : structurer un projet Flask/Django

### Structure Flask recommandée (architecture en couches)

```
mon_projet/
├── app/
│   ├── __init__.py
│   ├── models/          ← Couche données (SQLAlchemy)
│   │   └── user.py
│   ├── repositories/    ← Requêtes BDD isolées
│   │   └── user_repository.py
│   ├── services/        ← Logique métier pure
│   │   └── user_service.py
│   ├── routes/          ← Couche présentation (endpoints)
│   │   └── user_routes.py
│   ├── templates/
│   └── static/
│       └── design-system/
│           ├── tokens.css
│           └── components.css
├── tests/
│   ├── test_services.py     ← Tests unitaires (rapides, sans BDD)
│   └── test_routes.py       ← Tests d'intégration
├── config.py
└── run.py
```

### Structure Django équivalente

Django impose déjà une structure par "app", mais on peut renforcer la séparation :

```
projet/
├── users/
│   ├── models.py       ← Model (données)
│   ├── views.py        ← View (orchestration, doit rester légère)
│   ├── services.py     ← Logique métier (à créer soi-même, bonne pratique)
│   ├── repositories.py ← Requêtes complexes isolées
│   ├── urls.py
│   └── templates/users/
└── static/design-system/
```

**Point clé** : Django ne force pas nativement la séparation logique métier / vue. C'est à toi de créer ce fichier `services.py` par convention — c'est une pratique de développeur senior.

---

## 17. Étude de cas : mini Design System pour un projet perso

Pour un projet comme **CVCraft** ou **NEXUS ACADEMY**, voici une structure minimale mais professionnelle :

```
static/design-system/
├── tokens.css        ← Toutes les variables CSS (couleurs, spacing, typo)
├── base.css          ← Reset CSS + styles globaux (body, h1-h6...)
├── components/
│   ├── button.css
│   ├── card.css
│   ├── form.css
│   └── navbar.css
└── DESIGN.md          ← Documentation : règles d'usage, exemples
```

**Contenu type de `DESIGN.md`** :
```markdown
# Design System — CVCraft

## Couleurs
- Primary : utilisé pour les CTA principaux uniquement
- Danger : réservé aux actions de suppression

## Boutons
- `btn-primary` : une seule action principale par écran
- `btn-secondary` : actions secondaires
- Toujours inclure un état :hover et :disabled

## Espacement
- Grille de 8px : toujours utiliser des multiples de 8px
```

Ce simple fichier, même pour un projet solo, te fait gagner un temps considérable et **prouve en entretien que tu penses comme un développeur senior**, pas juste comme quelqu'un qui "fait marcher le code".

---

## 18. Checklist finale — Niveau avancé atteint

Coche mentalement chaque point : si tu peux l'expliquer avec tes propres mots, tu as le niveau visé par cette formation.

**Design System**
- [ ] Différence entre charte graphique, UI Kit et Design System
- [ ] Rôle des design tokens et pourquoi ils remplacent les valeurs en dur
- [ ] Notion de tokens sémantiques vs tokens globaux
- [ ] Tous les états d'un composant (hover, focus, disabled, loading...)
- [ ] Principe de l'Atomic Design (atomes → pages)
- [ ] Notion de contraste et d'accessibilité (WCAG)
- [ ] Grille d'espacement 8pt

**Architecture**
- [ ] Architecture en couches et pourquoi séparer présentation/métier/données
- [ ] Différence MVC vs MVT (piège Django)
- [ ] Principe de l'architecture hexagonale (domaine indépendant des détails techniques)
- [ ] Différences Monolithe / Microservices / Serverless, et quand utiliser chacun
- [ ] Principes SOLID, DRY, KISS
- [ ] Savoir structurer un projet Flask/Django en couches

---

## Pour aller plus loin

- **Livre** : *Clean Architecture*, Robert C. Martin (référence absolue, même en résumé/vulgarisé)
- **Livre** : *Refactoring UI*, Adam Wathan & Steve Schoger (excellent pour le design pratique)
- **Site** : material.io/design (étudier un vrai Design System complet)
- **Pratique** : reprendre CVCraft ou NEXUS ACADEMY et refactorer une seule route selon l'architecture en couches vue en partie 16 — c'est le meilleur exercice pour ancrer la théorie
