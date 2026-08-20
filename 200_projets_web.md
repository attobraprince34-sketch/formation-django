# 200 idées de projets — de Django simple à DRF + React (+ outils Python)

Progression pensée pour monter en compétence par paliers : chaque niveau réutilise les acquis du précédent en ajoutant une difficulté (auth, relations DB, API, frontend séparé, architecture).

---

## Niveau 1 — Django simple, sans base de données complexe (1–30)
1. **To-do list** — CRUD basique avec formulaires Django.
2. **Bloc-notes** — création/édition/suppression de notes texte.
3. **Page portfolio dynamique** — contenu géré depuis l'admin Django.
4. **Livre d'or** — les visiteurs laissent un message affiché en liste.
5. **Convertisseur d'unités** — formulaire + calcul côté serveur.
6. **Générateur de mots de passe** — formulaire avec options de complexité.
7. **Compteur de visites** — session/cookie pour compter les vues d'une page.
8. **Blog minimal** — liste d'articles + page de détail, sans commentaires.
9. **Galerie d'images statique** — upload simple via l'admin.
10. **Citation aléatoire du jour** — affichage random depuis une liste en base.
11. **Formulaire de contact** — envoi d'email via Django.
12. **Horloge mondiale** — affichage de l'heure dans plusieurs fuseaux.
13. **Calculatrice IMC** — formulaire + résultat calculé.
14. **Landing page produit** — page vitrine simple avec sections statiques.
15. **Liste de courses partagée** — ajout/suppression d'articles sans compte.
16. **Générateur de QR code** — à partir d'un texte/URL saisi.
17. **Convertisseur de devises (taux fixes)** — calcul simple sans API externe.
18. **Page météo statique** — données saisies manuellement en base.
19. **Mini CV en ligne** — contenu injecté depuis un modèle Django.
20. **Sondage à choix unique** — vote + affichage du résultat en %.
21. **Journal de gratitude** — entrée quotidienne texte, liste chronologique.
22. **Calculateur de pourboire** — formulaire + calcul.
23. **Page FAQ dynamique** — questions/réponses gérées depuis l'admin.
24. **Compteur de calories basique** — liste d'aliments avec valeurs prédéfinies.
25. **Bottin téléphonique** — CRUD simple contacts (nom, tel, email).
26. **Générateur de factures PDF basique** — formulaire → PDF simple.
27. **Timer Pomodoro côté serveur** — simple minuteur avec état en session.
28. **Page d'annonces locales** — liste d'annonces sans authentification.
29. **Convertisseur Markdown → HTML** — zone de texte, rendu à l'affichage.
30. **Mini bibliothèque personnelle** — liste de livres lus avec notes.

## Niveau 2 — Django intermédiaire : comptes, relations, permissions (31–60)
31. **Blog avec comptes auteurs** — authentification + un auteur = plusieurs articles.
32. **Système de commentaires** — relation article → commentaires, modération admin.
33. **Gestionnaire de tâches multi-utilisateurs** — chaque utilisateur voit ses propres tâches.
34. **Forum simple** — catégories, sujets, réponses, relations imbriquées.
35. **Carnet d'adresses partagé en équipe** — permissions par groupe.
36. **Suivi de dépenses personnelles** — comptes utilisateurs, catégories liées.
37. **Plateforme d'avis produits** — relation produit → avis, note moyenne calculée.
38. **Système de réservation de créneaux** — relation utilisateur → créneau, contraintes d'unicité.
39. **Gestionnaire de recettes de cuisine** — recette liée à des ingrédients (many-to-many).
40. **Suivi de lecture collaboratif** — bibliothèque partagée avec statuts de lecture par utilisateur.
41. **Petit réseau social minimal** — profils, posts, likes (many-to-many).
42. **Gestion de projets simples** — projet → tâches → assignation à un utilisateur.
43. **Plateforme de petites annonces avec comptes** — vendeur lié à ses annonces.
44. **Suivi de candidatures d'emploi** — statuts personnalisés, historique par utilisateur.
45. **Carnet de santé personnel** — suivi de poids/mesures dans le temps, graphique simple.
46. **Système de vote pondéré** — plusieurs sondages, un utilisateur = une voix par sondage.
47. **Gestionnaire de mots de passe chiffrés** — chiffrement basique côté serveur, comptes utilisateurs.
48. **Plateforme d'entraide entre étudiants** — questions/réponses liées à des matières.
49. **Suivi d'habitudes (habit tracker)** — habitude → coche quotidienne liée à l'utilisateur.
50. **Gestion de bibliothèque avec emprunts** — livre → emprunt → utilisateur, dates de retour.
51. **Plateforme de covoiturage basique** — trajet proposé, relation avec passagers inscrits.
52. **Suivi budgétaire familial** — comptes multiples liés à un foyer.
53. **Système de tickets support** — ticket → statut → réponses, rôles agent/client.
54. **Générateur de plannings d'équipe** — créneaux liés à des membres, détection de conflits.
55. **Plateforme d'événements avec inscriptions** — événement → participants, places limitées.
56. **Carnet de suivi sportif** — séance liée à des exercices, progression dans le temps.
57. **Système de parrainage** — utilisateur → filleuls, suivi des récompenses.
58. **Gestion de stock simple** — produit, mouvement de stock, historique.
59. **Plateforme de dons/collectes** — campagne → dons liés, total calculé en temps réel.
60. **Suivi de maintenance d'équipements** — équipement → interventions planifiées et historisées.

## Niveau 3 — Django avancé : signals, Celery, cache, permissions fines (61–80)
61. **Notifications en temps différé** — signals Django + tâches Celery pour emails groupés.
62. **Système de rappels planifiés** — Celery beat pour tâches récurrentes.
63. **Plateforme avec rôles et permissions personnalisées** — groupes, permissions par objet.
64. **Générateur de rapports PDF programmés** — tâche Celery + envoi automatique par email.
65. **Système de cache de pages lourdes** — cache Django sur des vues à fort trafic simulé.
66. **Historique d'audit automatique** — signals pour tracer chaque modification en base.
67. **File d'attente de traitement d'images** — upload → traitement asynchrone via Celery.
68. **Système de recommandations simples** — logique de scoring basée sur l'historique utilisateur.
69. **Plateforme multi-tenant basique** — isolation de données par organisation.
70. **Import/export de données en masse** — traitement asynchrone de fichiers CSV volumineux.
71. **Système de versionning de contenu** — historique des versions d'un article/document.
72. **Générateur de newsletters automatiques** — agrégation de contenu + envoi programmé.
73. **Plateforme avec recherche full-text** — intégration PostgreSQL full-text search.
74. **Système de badges/gamification** — règles déclenchées par signals selon l'activité utilisateur.
75. **Gestion de workflow de validation** — statuts multiples avec transitions contrôlées.
76. **Plateforme de sauvegarde automatique** — tâche planifiée de backup de données utilisateur.
77. **Système d'abonnements avec expiration** — tâche périodique de vérification/désactivation.
78. **Détection d'anomalies dans les logs applicatifs** — analyse périodique + alertes.
79. **Plateforme avec limitation de débit (rate limiting)** — protection des endpoints sensibles.
80. **Système de files de modération** — contenu signalé mis en attente de validation humaine.

## Niveau 4 — Django REST Framework : API pures (81–110)
81. **API de gestion de tâches** — CRUD complet exposé en REST, sans frontend.
82. **API de blog avec authentification par token** — endpoints articles/commentaires.
83. **API météo agrégée** — combine plusieurs sources externes, expose un format unifié.
84. **API de gestion d'inventaire** — endpoints produits/stocks avec filtres avancés.
85. **API de réservation avec disponibilités** — vérification de créneaux en temps réel.
86. **API de messagerie interne** — endpoints conversations/messages avec pagination.
87. **API e-commerce simplifiée** — produits, panier, commandes.
88. **API de gestion d'utilisateurs avec rôles** — JWT, permissions par endpoint.
89. **API de statistiques agrégées** — endpoints de reporting avec agrégations complexes.
90. **API de géolocalisation de points d'intérêt** — recherche par proximité.
91. **API de gestion de fichiers/documents** — upload, versionning, permissions d'accès.
92. **API de suivi de commandes en temps réel** — statuts avec historique complet.
93. **API de quiz/évaluations** — questions, réponses, scoring côté serveur.
94. **API de gestion d'événements avec billetterie** — inscriptions, places limitées, validation.
95. **API de recommandations de contenu** — endpoint retournant des suggestions personnalisées.
96. **API de gestion de projets (type kanban)** — colonnes, cartes, déplacement d'état.
97. **API multi-langue** — contenu traduisible, sélection de langue par requête.
98. **API de paiement simulé** — workflow de transaction avec statuts et webhooks internes.
99. **API de gestion de contenu (headless CMS)** — endpoints génériques pour pages/blocs.
100. **API de suivi de santé/fitness** — endpoints séances, mesures, progression.
101. **API d'annuaire d'entreprises** — recherche, filtres, fiches détaillées.
102. **API de gestion de tickets support** — SLA, priorités, assignation automatique.
103. **API de covoiturage** — recherche de trajets compatibles par critères.
104. **API de gestion de bibliothèque** — emprunts, réservations, pénalités de retard.
105. **API de sondages avec résultats en temps réel** — agrégation des votes à la volée.
106. **API de gestion RH basique** — employés, congés, validation hiérarchique.
107. **API de suivi de budget d'entreprise** — dépenses, catégories, plafonds d'alerte.
108. **API de gestion de flotte de véhicules** — suivi, entretien, affectation.
109. **API de scoring/crédit simplifié** — calcul de score selon des règles métier.
110. **API de gestion de contenu éducatif** — cours, modules, progression apprenant.

## Niveau 5 — DRF + React : applications fullstack complètes (111–150)
111. **Gestionnaire de tâches fullstack** — API DRF + interface React avec filtres et drag & drop.
112. **Réseau social minimal** — fil d'actualité, likes, commentaires en React connecté à l'API.
113. **Plateforme e-commerce complète** — catalogue, panier, tunnel de commande côté React.
114. **Dashboard d'administration** — visualisation de données DRF avec graphiques React.
115. **Plateforme e-learning (type NEXUS ACADEMY avancé)** — cours, progression, quiz interactifs.
116. **Application de messagerie instantanée** — React + WebSockets/polling sur API DRF.
117. **Plateforme de gestion de projets kanban** — colonnes dynamiques, drag & drop React.
118. **Marketplace multi-vendeurs** — comptes vendeurs, produits, commandes séparées.
119. **Plateforme de réservation (salles, rendez-vous)** — calendrier interactif React + API.
120. **Application de suivi budgétaire personnel** — graphiques de dépenses, catégories dynamiques.
121. **Plateforme de forum moderne** — React pour l'UI, API DRF pour la logique de discussion.
122. **CRM simplifié** — gestion de contacts/opportunités avec pipeline visuel.
123. **Plateforme de covoiturage interactive** — carte, recherche, réservation en React.
124. **Application de gestion RH** — congés, plannings, validation avec interface fluide.
125. **Plateforme d'événements avec billetterie** — inscription, paiement simulé, QR code d'entrée.
126. **Application de sondages en temps réel** — résultats graphiques mis à jour dynamiquement.
127. **Plateforme de portfolio pour freelances** — profils, projets, système d'avis.
128. **Application de suivi sportif avec tableaux de bord** — progression visualisée en graphiques.
129. **Plateforme de petites annonces avec chat intégré** — React + API messagerie liée.
130. **Application de gestion de bibliothèque moderne** — recherche avancée, réservation, notifications.
131. **Dashboard de monitoring système** — API collectant des métriques, React pour la visualisation.
132. **Plateforme de gestion d'inventaire avec scanner** — React + API DRF, lecture code-barres.
133. **Application de recettes avec planificateur de repas** — glisser-déposer sur calendrier React.
134. **Plateforme de mise en relation freelances/clients** — profils, offres, système de matching.
135. **Application de gestion de flotte avec carte** — suivi position, historique trajets.
136. **Plateforme de crowdfunding** — campagnes, contributions, barre de progression en temps réel.
137. **Application de gestion de contenu (blog + CMS admin React)** — édition riche, prévisualisation.
138. **Plateforme de quiz compétitifs** — classement en temps réel, sessions multijoueurs simples.
139. **Application de suivi de projets avec Gantt** — visualisation temporelle React.
140. **Plateforme de gestion de tickets support avec chat** — file d'attente, priorisation visuelle.
141. **Application e-commerce avec recommandations** — moteur de suggestion basé sur l'historique.
142. **Plateforme de gestion d'abonnements SaaS** — facturation récurrente simulée, dashboard client.
143. **Application de suivi de candidatures (job tracker)** — pipeline visuel type kanban.
144. **Plateforme de mentorat en ligne** — mise en relation, prise de rendez-vous, suivi de sessions.
145. **Application de gestion de stock multi-entrepôts** — transferts, alertes de rupture.
146. **Plateforme de notation/avis vérifiés** — modération, système anti-fraude basique.
147. **Application de planification d'événements collaboratifs** — votes de disponibilités type Doodle.
148. **Plateforme de gestion de contenu multi-auteurs** — workflow éditorial avec statuts de validation.
149. **Application de suivi énergétique/consommation** — graphiques de tendances, alertes de seuil.
150. **Plateforme complète de type CVCraft avancée** — génération de CV dynamique, templates multiples, export PDF.

## Niveau 6 — Projets complexes : architecture avancée (151–170)
151. **Architecture microservices** — plusieurs services Django/DRF communiquant via API interne.
152. **Système de notifications temps réel** — WebSockets (Django Channels) + React.
153. **Plateforme avec authentification multi-fournisseurs (OAuth)** — Google/GitHub + JWT interne.
154. **Système de paiement réel intégré** — Stripe/PayPal sandbox avec webhooks sécurisés.
155. **Plateforme multi-tenant avancée** — isolation stricte des données par organisation, sous-domaines.
156. **Système de recherche avancée avec Elasticsearch** — indexation et recherche full-text performante.
157. **Pipeline CI/CD complet pour une application Django** — tests automatisés, déploiement continu.
158. **Application avec mise en cache distribuée (Redis)** — sessions, cache de requêtes lourdes.
159. **Plateforme avec files de tâches distribuées (Celery + RabbitMQ/Redis)** — traitement asynchrone à grande échelle.
160. **Système de logs centralisés et monitoring (type ELK simplifié)** — collecte et visualisation.
161. **Application avec API GraphQL** — alternative à REST pour des requêtes complexes côté client.
162. **Plateforme avec tests de charge et optimisation** — benchmarking et refactoring de performance.
163. **Système de déploiement conteneurisé (Docker + orchestration)** — environnements reproductibles.
164. **Plateforme avec chiffrement de bout en bout** — messagerie sécurisée, gestion de clés.
165. **Application avec architecture événementielle (event-driven)** — découplage via messages/événements.
166. **Système de recommandations avec machine learning basique** — scikit-learn intégré à une API DRF.
167. **Plateforme avec API versionnée et documentation automatique** — Swagger/OpenAPI complet.
168. **Application avec réplication de base de données** — lecture/écriture séparées pour la performance.
169. **Système de feature flags** — activation progressive de fonctionnalités par utilisateur/groupe.
170. **Plateforme SaaS complète avec facturation, rôles, API publique et documentation développeur.**

## Niveau 7 — Autres outils Python (scripts, automatisation, cybersécurité, data) (171–200)
171. **Scanner de ports réseau basique** — script Python utilisant socket.
172. **Générateur de rapports d'audit sécurité en PDF** — via ton générateur ReportLab existant.
173. **Outil de brute-force éducatif sur environnement contrôlé** — démonstration pédagogique de vulnérabilité.
174. **Analyseur de logs de sécurité** — détection de tentatives suspectes dans des fichiers logs.
175. **Script de sauvegarde automatique de fichiers/dossiers** — planifié via cron/Task Scheduler.
176. **Outil de vérification de mots de passe (force, fuites connues)** — croisement avec API de fuites.
177. **Scraper de veille technologique** — collecte automatique d'articles/tendances tech.
178. **Bot Telegram/Discord d'automatisation** — notifications, commandes utiles au quotidien.
179. **Outil d'automatisation de tâches administratives** — remplissage de formulaires, extraction de données.
180. **Analyseur de trafic réseau basique (packet sniffer pédagogique)** — avec Scapy.
181. **Générateur de rapports de vulnérabilités automatisé** — agrégation de résultats de scans.
182. **Script de durcissement automatique de serveur Linux** — application de bonnes pratiques CIS.
183. **Outil de détection de phishing basique** — analyse d'URLs suspectes par règles heuristiques.
184. **Automatisation de déploiement de configuration réseau (Cisco)** — scripts Python + Netmiko.
185. **Outil d'inventaire automatique du parc informatique** — scan et rapport du réseau local.
186. **Script d'analyse de fichiers malveillants basique (sandbox simple)** — analyse statique de fichiers.
187. **Générateur automatique de CV/portfolio à partir de données structurées** — extension de CVCraft.
188. **Outil de nettoyage et normalisation de données (data cleaning)** — pandas sur jeux de données réels.
189. **Tableau de bord d'analyse de données avec pandas + matplotlib** — visualisation de tendances.
190. **Bot de veille sur les offres d'emploi tech** — agrégation et alerte par mots-clés.
191. **Script d'automatisation de publication sur réseaux sociaux** — planification de contenu.
192. **Outil de génération de rapports d'incident sécurité** — structuration automatique post-incident.
193. **Analyseur de configuration firewall** — détection de règles trop permissives.
194. **Script d'automatisation de tests de pénétration basiques** — enchaînement d'outils existants.
195. **Outil de monitoring de disponibilité de sites web** — alertes en cas de panne détectée.
196. **Générateur automatique de contenu pédagogique (quiz, fiches)** — à partir de notes de cours.
197. **Script d'automatisation de sauvegarde vers le cloud** — synchronisation planifiée.
198. **Outil d'analyse de certificats SSL/TLS** — vérification d'expiration et de configuration.
199. **Assistant CLI personnel en Python** — automatisation de tâches répétitives quotidiennes.
200. **Framework interne réutilisable** — combine plusieurs scripts ci-dessus (scan, rapport, alerte) en un outil unique packagé, point d'orgue du parcours.

---

### Comment utiliser cette liste
- Progresse séquentiellement dans les niveaux 1 à 5 pour construire des compétences solides en Django puis en fullstack DRF + React.
- Les niveaux 6 et 7 ne sont pas obligatoirement "après" le niveau 5 — pioche dans le niveau 7 (cybersécurité/scripts) en parallèle dès que tu es à l'aise en Python, cela nourrit directement ton profil pentest.
- Chaque projet terminé et documenté (README, capture d'écran, cas d'usage) devient un contenu potentiel pour TikTok et une pièce de portfolio supplémentaire.
