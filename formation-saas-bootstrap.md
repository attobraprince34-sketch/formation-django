# Bootstrapper un SaaS rentable sans réseaux sociaux

Formation pour développeur solo (Python/FastAPI/Django + React) qui veut construire un SaaS rentable via SEO, product-led growth, marketplaces et vente B2B directe — sans dépendre d'une audience sociale.

Règle de base pour toute la formation : **tu ne codes rien tant que tu n'as pas de preuve que quelqu'un paierait.** Le code est la partie facile de ton profil. La distribution est ce qui va te manquer. Cette formation est pondérée en conséquence : moins de temps sur la stack technique (tu maîtrises déjà), plus de temps sur validation et distribution.

---

## Module 1 — Valider avant de coder

### Le problème que tu dois trouver

Un problème "vendable" a ces caractéristiques cumulées, pas une seule suffit :

1. **Il coûte déjà de l'argent ou du temps mesurable** à la personne qui le vit (elle paie déjà une solution bancale — Excel, VA, agence, outil générique détourné — ou elle perd un nombre d'heures qu'elle peut chiffrer).
2. **Il revient régulièrement** (hebdo/mensuel), pas une fois tous les 3 ans. Un problème ponctuel ne justifie pas un abonnement.
3. **La personne qui souffre a un budget et l'autorité de payer** — évite les problèmes de particuliers fauchés ou de salariés sans carte pro.
4. **Tu peux nommer 50 à 500 entreprises/personnes précises** qui l'ont, pas "les PME en général". Si tu ne peux pas dresser une liste, le marché n'est pas assez défini pour être atteint sans budget pub.
5. **La niche a déjà des lieux de rassemblement identifiables** : un forum, un subreddit, une communauté Slack/Discord, un annuaire professionnel, une liste de logiciels compatibles (marketplace). Sans ça, ta distribution SEO/communauté n'a pas de point d'appui.

Évite les niches "à la mode indie hacker" (outils pour indie hackers, outils pour créateurs de contenu) sauf si tu es toi-même dans cette niche depuis longtemps — c'est saturé et les gens de cette niche ne paient pas facilement.

### Où chercher le problème

- **Ton propre historique professionnel.** Le problème le plus fiable est celui que tu as vécu comme salarié/freelance dans un secteur précis (compta, immobilier, santé, juridique, logistique, e-commerce, agences...). Tu connais le vocabulaire, tu sais qui a le budget.
- **Les logiciels existants avec des reviews négatives.** G2, Capterra, Trustpilot : cherche "trop cher", "manque cette fonctionnalité", "support inexistant". Chaque plainte répétée est une opportunité de wedge (angle d'attaque étroit contre un acteur établi — c'est exactement ce qu'a fait Plausible contre Google Analytics).
- **Les threads Reddit/forums de niche** où quelqu'un demande "est-ce qu'il existe un outil pour X" et personne ne répond bien.
- **Les intégrations manquantes** dans les marketplaces d'apps (Zapier, Shopify App Store, marketplace HubSpot/Salesforce) : cherche les catégories avec peu d'apps mais beaucoup de recherches.

### Comment valider AVANT de coder

L'ordre compte. Ne code rien avant l'étape 4.

1. **10 à 15 conversations avec des gens qui ont le problème.** Pas un sondage, une conversation. Objectif : leur faire raconter la dernière fois qu'ils ont été bloqués par ce problème, avec des détails concrets (combien de temps perdu, quel outil de contournement, combien ça coûte). Tu cherches un signal émotionnel — frustration, soupir, "c'est un cauchemar" — pas un "ouais ce serait cool".
2. **Demande le prix avant de montrer quoi que ce soit.** "Si un outil réglait X, tu paierais combien par mois ?" Une réponse vague ou un silence est un signal négatif. Un chiffre précis et rapide est un signal positif.
3. **Vends la solution avant qu'elle existe.** Page de vente simple (une landing page, pas de démo produit) avec CTA "Rejoindre la liste d'attente" ou mieux, **prends une pré-vente réelle** (paiement Stripe ou acompte, remboursable) auprès de 3 à 5 personnes. Si personne ne sort sa carte, le problème n'est pas assez douloureux.
4. **Construis seulement après avoir 3-5 pré-clients payants ou engagés fermement.** À ce stade tu codes pour un besoin confirmé, pas une hypothèse.

Signal d'alerte à prendre au sérieux : si tu dois "convaincre" les gens que le problème existe pendant l'entretien, il n'est pas assez douloureux. Un vrai problème se reconnaît parce que la personne en face de toi commence à te vendre son besoin, pas l'inverse.

### Gros SaaS vs micro-SaaS : comment choisir

| Critère | Micro-SaaS | SaaS "classique" |
|---|---|---|
| Revenu visé réaliste solo | 3 000 à 30 000 €/mois | 50 000 €+/mois, nécessite une équipe |
| Marché | Niche étroite, TAM 1 000-50 000 clients potentiels | Marché large, TAM 6-7 chiffres de clients |
| Complexité produit | Un seul problème résolu très bien, peu de features | Plateforme multi-fonctions, intégrations nombreuses |
| Concurrence | Souvent aucun concurrent direct dans la sous-niche | Concurrents financés, coût d'acquisition élevé |
| Temps avant premier euro | Semaines | Mois |
| Risque | Faible (peu de code, peu de cash brûlé) | Élevé si solo sans financement |

**Choisis micro-SaaS si :** tu es seul, tu n'as pas 12 mois de trésorerie devant toi, tu veux un revenu vivable rapidement, tu préfères posséder 100% d'un petit truc rentable.

**Vise plus gros seulement si :** tu as déjà un micro-SaaS rentable qui te donne un revenu de base, ET tu identifies un problème avec un TAM large où tu peux construire un avantage défendable (données propriétaires, réseau, intégrations profondes) — pas juste "une meilleure UI".

Par défaut, dans ta situation (solo, pas de budget pub, pas d'audience), **vise micro-SaaS.** C'est le format qui correspond à tes contraintes de distribution : les canaux que tu vas utiliser (SEO de niche, marketplaces, cold outreach, communautés) scalent bien sur un marché étroit et mal sur un marché de masse où il faut un volume énorme de trafic pour percer.

### Exercices — Module 1 (cette semaine)

1. Liste 5 problèmes issus de ton expérience professionnelle passée (pas d'idées abstraites — des situations vécues, les tiennes ou celles de collègues).
2. Pour chacun, réponds par écrit aux 5 critères de "problème vendable" ci-dessus. Élimine ceux qui ratent 2 critères ou plus.
3. Sur le problème restant le plus fort, trouve et liste 30 entreprises/personnes précises (nom, contact si possible) qui l'ont.
4. Contacte 10 d'entre elles cette semaine pour un appel de 15 minutes. Objectif : au moins 5 conversations réalisées avant la fin de la semaine.
5. Note dans un doc partagé, pour chaque conversation : le problème dans leurs mots exacts, le prix qu'ils annoncent spontanément, et s'ils demandent "ça sort quand ?" sans que tu le suggères (signal fort).

---

## Module 2 — Stack technique et MVP

### Construire l'MVP avec FastAPI/Django + React

Objectif : un MVP livrable en 3 à 6 semaines, pas 3 mois. Le code de ton MVP est jetable à 30% — ne sur-architecture pas.

**Backend :**
- **Django** si ton produit a besoin d'un admin robuste rapidement (gestion de contenu, CRUD complexe, back-office) — l'admin Django auto-généré te fait gagner des semaines.
- **FastAPI** si ton produit est plutôt orienté API/intégrations, temps réel, ou si tu veux une stack async légère. Bon choix aussi si le SaaS doit exposer une API publique dès le départ (ça facilite la distribution marketplace, voir Module 4).
- Pas besoin de microservices, pas besoin de Celery/Redis avant d'avoir un vrai besoin de tâches asynchrones lourdes (emails transactionnels simples ne comptent pas).

**Frontend :**
- React + Vite (pas Next.js sauf si tu as besoin de SSR pour le SEO du site marketing lui-même — dans ce cas Next.js pour le site public, React/Vite classique pour l'app derrière l'auth).
- Composants UI : shadcn/ui ou une lib de composants toute faite. Ne construis pas ton propre design system au début.

**Base de données :** PostgreSQL, toujours, sauf raison précise. Hébergé (Neon, Supabase, Railway) plutôt que géré par toi.

### Ce qu'il ne faut PAS construire au début

- Pas de gestion multi-tenant complexe avant d'avoir des clients qui le demandent réellement.
- Pas de système de permissions/rôles granulaires — un simple "admin/membre" suffit à 95% des micro-SaaS.
- Pas d'app mobile native.
- Pas de personnalisation poussée (thèmes, branding client) avant que ce soit une demande payante explicite.
- Pas d'intégrations à la carte "au cas où" — construis l'intégration que tes 3 premiers clients demandent, pas une liste de 15 intégrations théoriques.
- Pas de tests exhaustifs à 100% de couverture sur du code qui va peut-être être jeté dans 2 mois. Teste les chemins critiques (paiement, auth, la logique cœur du produit), pas tout.
- Pas de micro-optimisation de perf avant d'avoir un vrai volume d'utilisateurs.
- Pas de fonctionnalité "parce qu'un concurrent l'a" — seulement parce qu'un client te l'a demandée.

Règle pratique : si une fonctionnalité n'est pas nécessaire pour que ton tout premier client paie et utilise le produit chaque semaine, elle attend.

### Outils pour aller vite

- **Auth :** Clerk ou Auth.js/NextAuth si Next.js ; sinon `django-allauth` (Django) ou `fastapi-users` (FastAPI). Ne réécris pas l'auth toi-même.
- **Paiement :** Stripe, sans hésitation. Stripe Checkout + Customer Portal pour éviter de coder ta propre UI de facturation au début. Stripe Billing pour la gestion des abonnements/usage-based.
- **Hosting :** Railway ou Render pour le backend (déploiement en quelques clics, scaling simple) ; Vercel ou Netlify pour le frontend si séparé. Passe à AWS/GCP seulement quand tu as un besoin précis (conformité, volume, coût).
- **Emailing transactionnel :** Resend ou Postmark (bonne délivrabilité, API simple). Pour l'emailing marketing/newsletter (Module 4), un outil séparé : Loops, ConvertKit ou Listmonk (self-hosted, gratuit).
- **Monitoring/erreurs :** Sentry, gratuit jusqu'à un certain volume, indispensable dès le jour 1 en prod.
- **Analytics produit :** PostHog (self-hostable, généreux en gratuit) plutôt que GA — tu veux savoir quelles features sont utilisées, pas juste le trafic.

### Exercices — Module 2 (cette semaine)

1. Écris le cahier des charges du MVP en une page : liste les 3 à 5 fonctionnalités strictement nécessaires pour que ton premier client puisse résoudre son problème de bout en bout. Rien d'autre.
2. Pour chaque fonctionnalité listée, écris une ligne "pourquoi c'est indispensable au jour 1" — si tu ne peux pas justifier en une phrase, retire-la.
3. Configure le squelette technique : repo, hosting, Stripe en mode test, auth, monitoring Sentry. Objectif : un "hello world" déployé en prod avec auth fonctionnelle avant la fin de la semaine.
4. Fixe-toi une deadline ferme de mise en prod du MVP (3-6 semaines) et écris-la quelque part que tu vois tous les jours.

---

## Module 3 — Pricing et monétisation

### Fixer un prix dès le jour 1

Ne lance jamais un produit sans prix affiché, même en version bêta. Le prix fait partie de la validation, pas une décision que tu prends après.

**Méthode pour fixer le prix initial :**

1. Reprends les chiffres que tes prospects t'ont donnés en entretien (Module 1). Prends la médiane, pas la moyenne (évite qu'un outlier généreux fausse tout).
2. Compare à la valeur créée, pas au coût de développement. Si ton outil fait gagner 5h/mois à quelqu'un payé 40€/h, tu es dans le vrai si tu factures 50-150€/mois — tu captures une fraction de la valeur, pas juste "un peu plus cher qu'un café".
3. Regarde 3 concurrents ou substituts directs (même approximatifs) et positionne-toi consciemment au-dessus ou en dessous — jamais au hasard.
4. Vise un prix qui te fait légèrement mal à l'aise en le disant à voix haute. C'est généralement le signe que tu n'es pas en train de sous-facturer par peur.

**Erreur la plus commune des développeurs solo :** sous-facturer parce qu'on raisonne en "temps de dev" plutôt qu'en valeur pour le client. Un outil B2B qui résout un vrai problème récurrent vaut rarement moins de 29-49€/mois, même en micro-SaaS.

### Modèles qui marchent en micro-SaaS

- **Paid-only avec essai gratuit limité dans le temps (14 jours, carte non requise ou requise selon ta tolérance au risque).** C'est le modèle par défaut à recommander : pas de support gratuit à vie, signal de prix clair dès le début, pas de base d'utilisateurs gratuits à gérer sans en tirer de revenu.
- **Usage-based / par volume** (par appel API, par site tracké, par utilisateur actif) : pertinent si ton produit est un outil technique/API (dans l'esprit FastAPI). Avantage : le prix suit naturellement la valeur consommée. Inconvénient : moins prévisible en cashflow au début.
- **Freemium** : à éviter en solo sauf cas précis (produit avec effet réseau/viral fort, comme un outil qui génère un widget affiché publiquement — voir cas Senja en Module 6). En solo bootstrap, une base gratuite large = coût de support et d'infra sans revenu, pour un développeur seul c'est souvent la mort par mille support tickets.
- **Paiement annuel avec remise (2 mois offerts)** : propose-le dès le lancement, ça améliore ton cashflow immédiat et réduit le churn mécaniquement (les gens qui paient annuellement partent moins).

**Ce qui ne marche presque jamais en micro-SaaS solo :** les plans "Enterprise, contactez-nous" sans avoir de clients qui le demandent explicitement, le pricing à la feature cochée avec 15 cases (trop complexe à vendre seul), le lifetime deal en masse dès le lancement (tue ton MRR récurrent et ta trésorerie future contre du cash ponctuel).

### Exercices — Module 3 (cette semaine)

1. Fixe 3 tiers de prix (starter / pro / au-dessus si pertinent) avec les limites précises de chaque tier (nombre d'utilisateurs, d'appels API, de sites...).
2. Configure Stripe Checkout avec ces 3 tiers en mode test, et teste le parcours complet toi-même de bout en bout.
3. Écris la page pricing publique — même basique — et publie-la avant d'avoir fini le produit. Regarde s'il y a des clics/questions dessus.
4. Décide et écris noir sur blanc ta politique d'essai gratuit (durée, carte requise ou non) et ta politique de remboursement.

---

## Module 4 — Distribution sans réseaux sociaux

C'est le module qui déterminera si ton SaaS génère du revenu ou pas. Priorise dans cet ordre pour un développeur solo B2B : **cold outreach direct** (résultat le plus rapide) → **communautés de niche** (moyen terme) → **marketplaces/intégrations** (moyen terme, effet cumulatif) → **SEO de contenu** (long terme, mais devient ton canal dominant après 6-12 mois).

### SEO de contenu pour un SaaS technique

Le SEO générique ("meilleur logiciel de gestion de projet") ne marche pas pour toi — trop de concurrence, tu n'as pas le budget de contenu. La stratégie qui marche pour un SaaS de niche :

1. **Cible les mots-clés "alternative à X"** où X est un concurrent établi cher, complexe ou avec une faiblesse connue (RGPD, prix, support). C'est exactement la stratégie de Plausible contre Google Analytics : ils ont ciblé "alternative Google Analytics", "analytics sans cookies", "GA4 alternative" — des gens qui ont déjà décidé de switcher, pas des gens à convaincre depuis zéro.
2. **Cible les mots-clés "comment faire X" très spécifiques à ton use case**, pas des sujets larges. Un article "comment automatiser les factures Stripe pour agences" bat un article générique "gestion de facturation" — moins de volume de recherche, mais intention beaucoup plus proche de l'achat.
3. **Documentation publique = contenu SEO.** Si ton produit a une API ou des fonctionnalités techniques, une doc publique bien indexée (comme Bannerbear) capte du trafic développeur qualifié gratuitement, sans effort de "blogging".
4. **Programmatic SEO si ta niche le permet** : génère des pages pour chaque variante de ton use case (par ville, par secteur, par intégration — "connecter [ton outil] à [outil tiers]"). Fonctionne bien pour les SaaS qui touchent plusieurs verticales ou intégrations.
5. **Publie ta progression publiquement en "open startup"** (chiffres de revenu, apprentissages) — ce contenu-là attire naturellement des liens et du trafic Hacker News/Indie Hackers sans que ce soit un réseau social au sens où tu l'entends (pas de jeu d'audience/followers, juste du contenu qui vit de lui-même dans le temps).
6. Publie 1 à 2 articles solides par semaine pendant les 3 premiers mois, minimum. Le SEO ne produit rien avant 3-6 mois — commence-le en parallèle du cold outreach, pas après.

### Se faire lister sur les marketplaces pertinentes

- **Product Hunt** : un seul lancement bien préparé (pas plusieurs) — prépare une liste de 30-50 contacts qui upvoteront/commenteront de bonne foi (anciens clients, gens rencontrés en cold outreach, communautés). Le trafic généré est ponctuel mais le badge et le lien restent en SEO permanent.
- **Zapier / Make** : si ton produit peut se connecter à d'autres outils, construis l'intégration Zapier tôt. Chaque recherche "connecter [ton outil] à [outil populaire]" devient une source de trafic qualifié et une raison objective de te choisir plutôt qu'un concurrent sans intégration.
- **GitHub** : si une partie de ton produit peut être open source (SDK client, CLI, plugin), publie-la. Ça génère de la crédibilité technique et du trafic organique de développeurs, exactement le mécanisme qu'a utilisé Plausible en open-sourçant son cœur produit.
- **Annuaires spécialisés à ta niche** : chaque secteur a ses propres annuaires de logiciels (marketplace Shopify pour l'e-commerce, AppExchange Salesforce pour le CRM, annuaires spécifiques compta/immo/santé). Cherche "[ton secteur] software directory" — souvent moins connu que Product Hunt mais bien plus qualifié.
- **G2 / Capterra** : crée ta fiche dès le lancement même sans reviews, encourage tes 5-10 premiers clients satisfaits à laisser un avis — ça compte pour le SEO de ces plateformes elles-mêmes qui rankent bien sur Google.

### Cold outreach B2B : trouver et contacter tes 50 premiers clients

1. **Constitue une liste précise**, pas achetée en masse. Sources : LinkedIn Sales Navigator (filtre par poste + secteur + taille d'entreprise), annuaires professionnels de ta niche, sites d'entreprises qui utilisent déjà un outil concurrent (identifiable via BuiltWith ou en regardant qui utilise l'API/le badge d'un concurrent).
2. **Personnalise sur un fait précis**, pas sur le prénom en mail merge. Une ligne qui prouve que tu as regardé leur situation (un outil qu'ils utilisent, une actu de leur boîte, un problème visible sur leur site) change radicalement le taux de réponse par rapport à un template générique.
3. **Ne vends pas dans le premier email.** Demande une conversation de 15 minutes pour comprendre leur process actuel, pas pour pitcher. Les gens répondent à la curiosité, pas au pitch.
4. **Séquence courte** : email 1 (personnalisé, question ouverte) → relance 1 à J+3 (courte, apporte une info/ressource) → relance 2 à J+7 (dernier email, ton "je referme le dossier, dis-moi si ça t'intéresse encore" — cette relance de fermeture a souvent le meilleur taux de réponse).
5. **Objectif réaliste** : sur 200 emails bien ciblés et personnalisés, attends-toi à 15-30 réponses, 5-10 appels, et 1-3 clients payants au premier passage. Fais tourner plusieurs vagues de 50-100 par semaine plutôt qu'un envoi massif unique.
6. **Utilise les 5-10 premiers clients cold-outreach comme preuve sociale** pour la vague suivante ("j'accompagne déjà [secteur] chez X et Y sur ce sujet") — ça augmente mécaniquement le taux de réponse des vagues suivantes.

### Communautés de niche : où poster sans spammer

- **Repère les communautés où ta cible se trouve déjà** (subreddits spécifiques au métier, pas r/SaaS ; Slack/Discord professionnels du secteur ; forums spécialisés type compta, juridique, immo selon ta niche).
- **Règle non négociable : contribue avant de promouvoir.** Passe 2-3 semaines à répondre à des questions, donner des conseils gratuits, sans jamais mentionner ton produit. Construis une réputation de personne compétente sur le sujet.
- **Ne poste jamais "j'ai créé mon SaaS, allez voir".** Réponds à des questions précises où ton outil est littéralement la réponse ("j'ai eu ce problème, j'ai fini par construire un petit outil pour ça, je peux te montrer si ça t'intéresse"). Le contexte doit justifier la mention, pas l'inverse.
- **Demande la permission aux modérateurs** avant tout post explicitement promotionnel (AMA, "show and tell", thread dédié) — la plupart des communautés ont un espace prévu pour ça, l'ignorer te fait bannir et brûle le canal définitivement.
- **Un post qui aide sans vendre** génère souvent plus de clients qu'un post promotionnel — les gens cliquent sur ton profil/lien par eux-mêmes quand tu as démontré de la compétence.

### Exercices — Module 4 (cette semaine)

1. Identifie 3 communautés de niche précises où ta cible se trouve. Rejoins-les, ne poste rien encore, observe le ton et les règles.
2. Construis une liste de 100 prospects qualifiés (nom, entreprise, contact, une ligne de personnalisation par prospect).
3. Écris et envoie ta première vague de 30 emails de cold outreach personnalisés. Objectif : au moins 3 appels bookés d'ici 2 semaines.
4. Identifie 2 marketplaces/annuaires pertinents pour ta niche et commence le process d'inscription/listing.
5. Liste 10 mots-clés "alternative à X" ou "comment faire Y" pertinents pour ton produit — base de ton calendrier éditorial SEO des 3 prochains mois.

---

## Module 5 — Métriques et itération

### Les chiffres à suivre pendant les 90 premiers jours

Ignore les vanity metrics (visiteurs uniques, followers, inscriptions gratuites sans conversion). Suis à la place :

- **MRR (Monthly Recurring Revenue)** — le seul chiffre qui ne ment pas sur la viabilité.
- **Nombre de clients payants** en valeur absolue (pas de %, trop peu de volume au début pour que les pourcentages aient un sens).
- **Taux de conversion essai → payant** — te dit si ton onboarding/produit convainc une fois testé.
- **Churn mensuel** (en nombre de clients ET en MRR perdu — un gros client qui part pèse plus qu'un petit). Au-delà de 5-7%/mois en micro-SaaS B2B, tu as un problème de rétention à traiter avant tout le reste.
- **Temps jusqu'à la première valeur** (time-to-value) : combien de temps entre l'inscription et le moment où l'utilisateur obtient un résultat concret. Plus c'est long, plus ta conversion souffre.
- **Source d'acquisition par client payant** (pas juste par visiteur) — te dit quel canal de distribution mérite ton temps.
- **Coût de ton temps par client acquis** — même sans budget pub, ton temps a un coût. Si le cold outreach te prend 10h pour 1 client à 40€/mois, calcule si c'est soutenable.

Ce que tu ignores volontairement les 90 premiers jours : nombre de followers, "impressions", trafic brut sans intention d'achat, comparaisons à des concurrents financés.

### Décider : pivoter, persister ou arrêter

Utilise ce cadre après 60-90 jours de vrais efforts de distribution (pas 2 semaines) :

**Persiste si :**
- Tu as des clients payants qui utilisent activement le produit chaque semaine (pas juste "abonnés mais inactifs").
- Le churn est sous contrôle et tu comprends pourquoi les gens qui partent, partent.
- Au moins un canal de distribution montre un signal répétable (même à petite échelle — 2 clients par semaine sur cold outreach est un signal, pas un échec).

**Pivote (le produit, pas la niche) si :**
- Tu as des conversations et de l'intérêt réel, mais peu de conversions en payant — le problème est validé mais ta solution ne le résout pas assez bien. Retourne parler aux clients pour comprendre le vrai blocage plutôt que de changer de marché.

**Pivote (la niche, pas le produit) si :**
- Ton produit fonctionne bien techniquement et les rares clients qui l'utilisent l'adorent, mais tu n'arrives pas à en trouver assez dans cette niche précise (marché trop petit, ou pas assez accessible via tes canaux). Le mécanisme produit peut souvent se repositionner vers une niche adjacente plus grande ou plus atteignable.

**Arrête si :**
- Après 90 jours d'efforts sérieux et documentés sur au moins 2 canaux de distribution, tu n'as ni clients payants récurrents ni signal d'intérêt fort (pas de "peut-être plus tard", du vrai refus poli répété). C'est rare si la validation du Module 1 a été faite sérieusement — la plupart des échecs viennent d'un Module 1 sauté ou bâclé.

### Exercices — Module 5 (cette semaine)

1. Construis un dashboard simple (Google Sheet suffit au début) avec les 7 métriques listées ci-dessus, mis à jour chaque semaine.
2. Fixe-toi un seuil chiffré de décision à J+90 pour chacun des 3 scénarios (persister/pivoter/arrêter) — écris les chiffres précis maintenant, avant d'être émotionnellement investi dans le résultat.
3. Si tu as déjà des clients : appelle tes 3 clients les plus actifs et tes 2 clients qui ont churné, pose la même question aux deux groupes ("qu'est-ce qui t'a fait rester / partir") et note les réponses mot pour mot.

---

## Module 6 — Cas pratiques

### Plausible Analytics — alternative privacy-first à Google Analytics

Plausible a été entièrement bootstrapé et a atteint 3,1 millions de dollars d'ARR fin 2024, avec environ 5 personnes et plus de 12 000 abonnés payants. Le code initial a été écrit en décembre 2018 par Uku Täht, qui travaille seul au début. Marko Saric rejoint comme cofondateur en mars 2020 pour prendre en charge le marketing, moment à partir duquel la croissance du revenu s'accélère nettement.

**Distribution :** le SEO a ciblé les gens qui cherchaient activement des alternatives à Google Analytics — analytics privacy-friendly, tracking conforme RGPD, analytics sans cookies — c'est-à-dire des personnes ayant déjà décidé de changer d'outil. Le produit a aussi été open-sourcé, ce qui a servi d'asset de crédibilité et de canal d'acquisition plutôt que de cannibaliser les ventes — la majorité des utilisateurs préfèrent payer l'hébergement géré plutôt que de s'occuper eux-mêmes de l'infrastructure. Aucune levée de fonds, aucune équipe commerciale, quasiment pas de dépense publicitaire.

**Chiffres clés :** 324 jours pour atteindre les premiers 400$ de MRR après le lancement de l'offre payante en mai 2019. Le cap du million de dollars d'ARR est franchi publiquement en 2022, puis 3,1M$ fin 2024.

**Leçon pour toi :** un wedge SEO net contre un concurrent dominant mais impopulaire pour une raison précise (ici : la complexité de GA4 et les problèmes RGPD) fonctionne mieux qu'une stratégie SEO généraliste. Le contenu ne visait pas "tout le monde qui fait de l'analytics", mais des gens en phase active de recherche de solution de remplacement.

### Bannerbear — API de génération automatique d'images et vidéos

Jon Yongfook a construit Bannerbear après avoir été frustré par le temps perdu à éditer manuellement des visuels marketing pour ses propres projets indie hacker. Ce n'était pas sa première tentative : il avait auparavant lancé 12 startups en 12 mois sans succès avant que Bannerbear ne trouve son marché.

**Distribution :** Jon a partagé sa progression publiquement (chiffres de revenu, échecs, étapes) plutôt que de polir le produit en silence, et un article sur son parcours vers 10K$ MRR a généré un pic de trafic important via Hacker News. Les canaux principaux ont été le contenu technique et le SEO (guides et tutoriels ciblant les développeurs cherchant des solutions d'automatisation) ainsi qu'une présence active dans les communautés comme Indie Hackers. Le produit était facturé dès le début à un niveau de prix assumé, en évitant volontairement les points de prix bas qui attirent des clients à forte friction et faible valeur.

**Chiffres clés :** environ 630 000$ d'ARR en 2025, soit environ 52-53 000$ de MRR, pour un seul fondateur, en 3 ans depuis le lancement du produit qui a fonctionné.

**Leçon pour toi :** en tant qu'API/outil technique, ta documentation publique et ton contenu orienté développeurs SONT ton SEO — chaque intégration créée par un utilisateur devient une source de trafic organique supplémentaire. C'est le modèle le plus proche de ta stack (FastAPI orienté API).

### Senja — widgets de témoignages clients

Fondé par deux personnes qui se sont rencontrées en ligne, Senja a mis 3 ans et 9 mois pour atteindre 1 million de dollars d'ARR. Leur mécanisme de croissance principal : chaque widget de témoignage intégré sur le site d'un client devient une pièce de marketing gratuite pour Senja lui-même, combiné à une pratique de "build in public" (partage transparent de la progression).

**Leçon pour toi :** si ton produit génère un artefact visible publiquement (widget, badge, page publique, export partageable), chaque client actif devient un canal de distribution passif — pense à intégrer ce mécanisme dans ton MVP dès que ta niche le permet, sans en faire une dépendance à un réseau social.

### Ce que ces trois cas ont en commun

- Aucun n'a démarré avec une audience préexistante sur les réseaux sociaux.
- Tous ont mis entre 1 et 4 ans pour atteindre un revenu significatif — pas de succès en 3 mois.
- Tous ont ciblé une niche précise avec un wedge clair (un angle contre un concurrent ou un problème très spécifique), pas un marché générique.
- Tous ont publié du contenu de manière disciplinée et régulière sur une longue durée, pas en rafale ponctuelle.
- Tous ont facturé dès le premier jour, sans période gratuite illimitée.

### Exercices — Module 6 (cette semaine)

1. Pour ton propre projet, écris en une phrase ton "wedge" : contre quel statu quo précis (outil cher, complexe, générique, mal adapté) tu te positionnes.
2. Identifie si ton produit peut générer un artefact partageable publiquement (comme le widget Senja) qui deviendrait un canal de distribution passif — si oui, note comment l'intégrer au MVP.
3. Planifie ton calendrier de contenu/build-in-public des 90 prochains jours : fréquence, plateforme de publication (blog propre + Indie Hackers/Hacker News quand pertinent), sujets basés sur les mots-clés du Module 4.

---

## Résumé opérationnel — les 90 premiers jours

- **Semaines 1-2 :** validation (Module 1). Ne code rien.
- **Semaines 3-6 :** construction du MVP minimal (Module 2) en parallèle du démarrage du cold outreach (Module 4) et des premiers posts de contribution en communauté.
- **Semaines 6-8 :** lancement payant (Module 3), premiers clients cold outreach, inscription marketplaces pertinentes.
- **Semaines 8-12 :** montée en cadence du contenu SEO, itération produit sur retours clients réels, suivi hebdomadaire des métriques (Module 5).
- **Jour 90 :** décision documentée persister/pivoter/arrêter sur la base de chiffres, pas d'intuition.
