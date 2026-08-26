# 📊 Dashboard Django + Tailwind CSS + Chart.js

> Guide pratique pour construire un dashboard de supervision réseau avec **Django**, **Tailwind CSS** et **Chart.js**.

![Django](https://img.shields.io/badge/Django-092E20?style=for-the-badge&logo=django&logoColor=white)
![Tailwind CSS](https://img.shields.io/badge/Tailwind_CSS-06B6D4?style=for-the-badge&logo=tailwindcss&logoColor=white)
![Chart.js](https://img.shields.io/badge/Chart.js-FF6384?style=for-the-badge&logo=chartdotjs&logoColor=white)
![Python](https://img.shields.io/badge/Python-3776AB?style=for-the-badge&logo=python&logoColor=white)

---

## 📌 Objectif

Construire progressivement un dashboard qui permet de visualiser :

- **100 équipements réseau**
- 🟢 équipements actifs
- 🔴 équipements inactifs
- ⚠️ équipements en alerte
- CPU
- mémoire
- température
- latence
- état global du réseau

L'objectif est d'obtenir une interface claire, responsive et facile à faire évoluer.

---

# 🧭 Sommaire

1. [Architecture](#-architecture)
2. [Installer Chart.js](#-installer-chartjs)
3. [Préparer les données Django](#-préparer-les-données-django)
4. [Structure du dashboard](#-structure-du-dashboard)
5. [Cartes statistiques](#-cartes-statistiques)
6. [Premier graphique Chart.js](#-premier-graphique-chartjs)
7. [Graphique CPU](#-graphique-cpu)
8. [Dashboard responsive](#-dashboard-responsive)
9. [UX/UI recommandée](#-uxui-recommandée)
10. [Version complète](#-version-complète)
11. [Évolutions possibles](#-évolutions-possibles)

---

# 🏗️ Architecture

Une organisation simple et maintenable :

```text
service/
│
├── models.py
├── views.py
├── urls.py
│
├── templates/
│   └── service/
│       └── dashboard.html
│
└── static/
    └── service/
        └── js/
            └── dashboard.js
```

Avec Tailwind CSS, la majorité du style reste directement dans le template.

Le JavaScript reste séparé :

```text
dashboard.html
      │
      ├── Tailwind CSS
      │
      ├── données Django
      │
      └── Chart.js
             │
             └── dashboard.js
```

---

# 📦 Installer / charger Chart.js

Si tu veux commencer simplement, utilise le CDN officiel :

```html
<script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
```

Dans `dashboard.html`, charge Chart.js **avant** ton fichier JavaScript :

```html
<script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
<script src="{% static 'service/js/dashboard.js' %}"></script>
```

> 💡 Si ton projet utilise déjà une installation npm/build avec Tailwind, tu peux ensuite installer Chart.js avec npm. Pour un premier dashboard Django, le CDN permet de démarrer rapidement.

---

# 🧠 Préparer les données Django

Dans `views.py`, on récupère les équipements de l'utilisateur connecté.

```python
def dashboard(request):

    equipements = Equipements.objects.filter(
        owner=request.user
    )

    equipement_inactif = 0
    equipement_actifs = 0

    for e in equipements:

        if e.statut:
            equipement_actifs += 1
        else:
            equipement_inactif += 1

    nbr_appareils = equipements.count()

    alerte = Alerte(equipements)

    cpu_data = [
        {
            "name": e.name,
            "cpu": e.cpu,
            "memoire": e.memoire,
            "temperature": e.temperature,
            "latence": e.latence,
        }
        for e in equipements
    ]

    context = {
        "equipement_innactif": equipement_inactif,
        "equipement_actifs": equipement_actifs,
        "nbr_appareils": nbr_appareils,
        "alerte": alerte,
        "cpu_data": cpu_data,
    }

    return render(
        request,
        "service/dashboard.html",
        context
    )
```

---

# ⚠️ Fonction `Alerte()`

Dans ton cas, une alerte correspond actuellement à un CPU supérieur à 80 %.

```python
def Alerte(equipements):

    alerte = 0

    for e in equipements:

        if e.cpu > 80:
            alerte += 1

    return alerte
```

Plus tard, tu pourras rendre le système plus intelligent :

```text
CPU > 80 %          → ⚠️ Alerte CPU
Mémoire > 80 %      → ⚠️ Alerte mémoire
Température > 70°C  → 🔥 Alerte température
Latence > 100 ms    → 🌐 Alerte réseau
statut = False      → 🔴 Équipement hors ligne
```

---

# 🎨 Structure du dashboard

L'interface peut être organisée en 4 niveaux :

```text
┌────────────────────────────────────────────────────────┐
│ Header / Navigation                                    │
├────────────────────────────────────────────────────────┤
│                                                        │
│ Dashboard réseau                         🔄 Actualiser │
│ Vue globale de votre infrastructure                    │
│                                                        │
├────────────┬────────────┬────────────┬─────────────────┤
│ Total      │ Actifs     │ Inactifs   │ Alertes         │
│ 100        │ 80         │ 20         │ 12              │
├────────────┴────────────┴────────────┴─────────────────┤
│                                                        │
│ État réseau                  │ Alertes                 │
│ Doughnut Chart               │ Liste des problèmes     │
│                              │                         │
├──────────────────────────────┴─────────────────────────┤
│                                                        │
│ Performance des équipements                           │
│ CPU / RAM / température / latence                     │
│                                                        │
├────────────────────────────────────────────────────────┤
│                                                        │
│ Équipements à surveiller                              │
│ Table                                                   │
└────────────────────────────────────────────────────────┘
```

Cette organisation permet de montrer d'abord les informations les plus importantes.

---

# 📊 Cartes statistiques

Avec Tailwind CSS :

```html
<div class="grid grid-cols-1 gap-4 sm:grid-cols-2 xl:grid-cols-4">

    <!-- Total -->
    <div class="rounded-2xl border border-slate-200 bg-white p-5 shadow-sm">
        <div class="flex items-center justify-between">

            <div>
                <p class="text-sm font-medium text-slate-500">
                    Total équipements
                </p>

                <p class="mt-2 text-3xl font-bold text-slate-900">
                    {{ nbr_appareils }}
                </p>
            </div>

            <div class="rounded-xl bg-slate-100 p-3">
                🖥️
            </div>

        </div>
    </div>


    <!-- Actifs -->
    <div class="rounded-2xl border border-emerald-200 bg-emerald-50 p-5 shadow-sm">
        <div class="flex items-center justify-between">

            <div>
                <p class="text-sm font-medium text-emerald-700">
                    Actifs
                </p>

                <p class="mt-2 text-3xl font-bold text-emerald-900">
                    {{ equipement_actifs }}
                </p>
            </div>

            <div class="rounded-xl bg-emerald-100 p-3">
                🟢
            </div>

        </div>
    </div>


    <!-- Inactifs -->
    <div class="rounded-2xl border border-red-200 bg-red-50 p-5 shadow-sm">
        <div class="flex items-center justify-between">

            <div>
                <p class="text-sm font-medium text-red-700">
                    Inactifs
                </p>

                <p class="mt-2 text-3xl font-bold text-red-900">
                    {{ equipement_innactif }}
                </p>
            </div>

            <div class="rounded-xl bg-red-100 p-3">
                🔴
            </div>

        </div>
    </div>


    <!-- Alertes -->
    <div class="rounded-2xl border border-amber-200 bg-amber-50 p-5 shadow-sm">
        <div class="flex items-center justify-between">

            <div>
                <p class="text-sm font-medium text-amber-700">
                    Alertes
                </p>

                <p class="mt-2 text-3xl font-bold text-amber-900">
                    {{ alerte }}
                </p>
            </div>

            <div class="rounded-xl bg-amber-100 p-3">
                ⚠️
            </div>

        </div>
    </div>

</div>
```

---

# 🍩 Premier graphique Chart.js

## HTML

```html
<div class="rounded-2xl border border-slate-200 bg-white p-6 shadow-sm">

    <div class="mb-6">
        <h2 class="text-lg font-semibold text-slate-900">
            État des équipements
        </h2>

        <p class="mt-1 text-sm text-slate-500">
            Répartition des équipements actifs et inactifs
        </p>
    </div>

    <div class="relative h-[320px] w-full">
        <canvas id="statutChart"></canvas>
    </div>

</div>
```

Le point important est :

```html
<div class="relative h-[320px] w-full">
```

La hauteur est donnée au **conteneur**, pas directement au canvas.

---

# 🔐 Transmettre les données Django à JavaScript

Utilise `json_script` :

```html
{{ equipement_actifs|json_script:"equipement-actifs" }}
{{ equipement_innactif|json_script:"equipement-inactifs" }}
```

Puis dans `dashboard.js` :

```javascript
const actifs = JSON.parse(
    document.getElementById("equipement-actifs").textContent
);

const inactifs = JSON.parse(
    document.getElementById("equipement-inactifs").textContent
);
```

C'est préférable à l'injection directe :

```javascript
const actifs = {{ equipement_actifs }};
```

---

# 📈 Créer le graphique

```javascript
const statutCanvas = document.getElementById("statutChart");

new Chart(statutCanvas, {

    type: "doughnut",

    data: {

        labels: [
            "Actifs",
            "Inactifs"
        ],

        datasets: [
            {
                data: [
                    actifs,
                    inactifs
                ],

                borderWidth: 0
            }
        ]

    },

    options: {

        responsive: true,

        maintainAspectRatio: false,

        cutout: "72%",

        plugins: {

            legend: {
                position: "bottom",

                labels: {
                    padding: 20,
                    usePointStyle: true
                }
            }

        }

    }

});
```

### Pourquoi `maintainAspectRatio: false` ?

Parce que notre conteneur Tailwind contrôle la hauteur :

```html
<div class="relative h-[320px] w-full">
```

Chart.js s'adapte donc à cet espace.

---

# 📊 Graphique CPU

## HTML

```html
<div class="rounded-2xl border border-slate-200 bg-white p-6 shadow-sm">

    <div class="mb-6">
        <h2 class="text-lg font-semibold text-slate-900">
            Utilisation CPU
        </h2>

        <p class="mt-1 text-sm text-slate-500">
            Utilisation processeur des équipements
        </p>
    </div>

    <div class="relative h-[360px] w-full">
        <canvas id="cpuChart"></canvas>
    </div>

</div>
```

---

# 🧩 Données CPU

Dans le template :

```html
{{ cpu_data|json_script:"cpu-data" }}
```

Dans JavaScript :

```javascript
const cpuData = JSON.parse(
    document.getElementById("cpu-data").textContent
);

const cpuLabels = cpuData.map(
    item => item.name
);

const cpuValues = cpuData.map(
    item => item.cpu
);
```

---

# 📊 Création du graphique CPU

```javascript
const cpuCanvas = document.getElementById("cpuChart");

new Chart(cpuCanvas, {

    type: "bar",

    data: {

        labels: cpuLabels,

        datasets: [
            {
                label: "CPU (%)",

                data: cpuValues,

                borderRadius: 8,

                borderSkipped: false
            }
        ]

    },

    options: {

        responsive: true,

        maintainAspectRatio: false,

        interaction: {
            mode: "index",
            intersect: false
        },

        scales: {

            y: {
                beginAtZero: true,
                max: 100,

                ticks: {
                    callback: value => `${value}%`
                }
            }

        },

        plugins: {

            legend: {
                display: false
            }

        }

    }

});
```

---

# 📱 Responsive avec Tailwind

Avec Tailwind, évite de définir une largeur fixe pour les graphiques.

❌ À éviter :

```html
<div class="w-[1000px]">
```

❌ À éviter :

```html
<canvas width="1000" height="500">
```

✅ Préférer :

```html
<div class="relative h-[300px] w-full sm:h-[350px] lg:h-[400px]">
    <canvas id="cpuChart"></canvas>
</div>
```

---

# 🖥️ Grille responsive

Deux graphiques côte à côte sur desktop :

```html
<div class="grid grid-cols-1 gap-6 xl:grid-cols-2">

    <div class="rounded-2xl border border-slate-200 bg-white p-6 shadow-sm">
        ...
    </div>

    <div class="rounded-2xl border border-slate-200 bg-white p-6 shadow-sm">
        ...
    </div>

</div>
```

Sur mobile :

```text
┌───────────────────────┐
│ Graphique 1           │
└───────────────────────┘

┌───────────────────────┐
│ Graphique 2           │
└───────────────────────┘
```

Sur desktop :

```text
┌────────────────────┐  ┌────────────────────┐
│ Graphique 1        │  │ Graphique 2        │
└────────────────────┘  └────────────────────┘
```

Tailwind fait automatiquement le changement grâce à :

```html
grid-cols-1 xl:grid-cols-2
```

---

# 🎯 UX/UI : principes à respecter

## 1. Hiérarchie visuelle

L'utilisateur doit comprendre l'état du réseau en quelques secondes.

Ordre recommandé :

```text
1. Total
2. Actifs / Inactifs
3. Alertes
4. Performance
5. Équipements problématiques
```

---

## 2. Ne pas surcharger le dashboard

Évite d'afficher immédiatement :

```text
100 équipements
100 CPU
100 RAM
100 températures
100 latences
100 adresses IP
100 modèles
...
```

Le dashboard doit être une **vue synthétique**.

Les détails peuvent être dans :

```text
/équipements/
```

---

## 3. Les couleurs doivent avoir une signification

Utilise une convention stable :

| État | Couleur | Signification |
|---|---|---|
| 🟢 | `emerald` | Normal / actif |
| 🟡 | `amber` | Attention |
| 🔴 | `red` | Critique / inactif |
| 🔵 | `blue` | Information |
| ⚪ | `slate` | Neutre |

Exemple :

```html
<span class="rounded-full bg-emerald-100 px-2.5 py-1 text-xs font-medium text-emerald-700">
    Actif
</span>
```

---

# ⚠️ Afficher les alertes

Une bonne UX consiste à afficher directement les équipements problématiques.

Exemple :

```html
<div class="rounded-2xl border border-slate-200 bg-white p-6 shadow-sm">

    <div class="mb-5 flex items-center justify-between">

        <div>
            <h2 class="text-lg font-semibold text-slate-900">
                Alertes
            </h2>

            <p class="text-sm text-slate-500">
                Équipements nécessitant votre attention
            </p>
        </div>

        <span class="rounded-full bg-amber-100 px-3 py-1 text-sm font-medium text-amber-700">
            {{ alerte }}
        </span>

    </div>

    <div class="space-y-3">

        <div class="flex items-center justify-between rounded-xl bg-red-50 p-4">

            <div>
                <p class="font-medium text-slate-900">
                    Switch Cisco 01
                </p>

                <p class="text-sm text-slate-500">
                    CPU élevé
                </p>
            </div>

            <span class="font-semibold text-red-600">
                92%
            </span>

        </div>

    </div>

</div>
```

---

# 🧱 Carte générique réutilisable

Pour garder une interface cohérente, utilise toujours cette base :

```html
<div class="rounded-2xl border border-slate-200 bg-white p-6 shadow-sm">

    <div class="mb-6">
        <h2 class="text-lg font-semibold text-slate-900">
            Titre
        </h2>

        <p class="mt-1 text-sm text-slate-500">
            Description courte
        </p>
    </div>

    <!-- contenu -->

</div>
```

Cela donne une interface beaucoup plus homogène.

---

# 🚀 Dashboard complet : structure recommandée

```html
<main class="min-h-screen bg-slate-50 p-4 sm:p-6 lg:p-8">

    <div class="mx-auto max-w-7xl space-y-6">

        <!-- Header -->

        <header class="flex flex-col gap-4 sm:flex-row sm:items-center sm:justify-between">

            <div>

                <p class="text-sm font-medium text-blue-600">
                    Supervision réseau
                </p>

                <h1 class="mt-1 text-2xl font-bold tracking-tight text-slate-900 sm:text-3xl">
                    Dashboard
                </h1>

                <p class="mt-1 text-sm text-slate-500">
                    Vue globale de votre infrastructure réseau.
                </p>

            </div>

            <button
                type="button"
                class="rounded-xl bg-slate-900 px-4 py-2.5 text-sm font-medium text-white shadow-sm transition hover:bg-slate-800"
            >
                ↻ Actualiser
            </button>

        </header>


        <!-- KPI -->

        <section class="grid grid-cols-1 gap-4 sm:grid-cols-2 xl:grid-cols-4">

            <!-- cartes statistiques -->

        </section>


        <!-- Charts -->

        <section class="grid grid-cols-1 gap-6 xl:grid-cols-2">

            <!-- Statut -->

            <div class="rounded-2xl border border-slate-200 bg-white p-6 shadow-sm">

                <div class="mb-6">

                    <h2 class="text-lg font-semibold text-slate-900">
                        État des équipements
                    </h2>

                    <p class="mt-1 text-sm text-slate-500">
                        Disponibilité de l'infrastructure
                    </p>

                </div>

                <div class="relative h-[320px] w-full">

                    <canvas id="statutChart"></canvas>

                </div>

            </div>


            <!-- CPU -->

            <div class="rounded-2xl border border-slate-200 bg-white p-6 shadow-sm">

                <div class="mb-6">

                    <h2 class="text-lg font-semibold text-slate-900">
                        CPU
                    </h2>

                    <p class="mt-1 text-sm text-slate-500">
                        Utilisation des processeurs
                    </p>

                </div>

                <div class="relative h-[320px] w-full">

                    <canvas id="cpuChart"></canvas>

                </div>

            </div>

        </section>


        <!-- Alertes -->

        <section class="grid grid-cols-1 gap-6 lg:grid-cols-2">

            <!-- Alertes -->

            <div class="rounded-2xl border border-slate-200 bg-white p-6 shadow-sm">

                <h2 class="text-lg font-semibold text-slate-900">
                    Alertes
                </h2>

                <p class="mt-1 text-sm text-slate-500">
                    Équipements nécessitant votre attention.
                </p>

                <!-- liste -->

            </div>


            <!-- Performance -->

            <div class="rounded-2xl border border-slate-200 bg-white p-6 shadow-sm">

                <h2 class="text-lg font-semibold text-slate-900">
                    Performance
                </h2>

                <p class="mt-1 text-sm text-slate-500">
                    Vue générale des performances réseau.
                </p>

                <!-- contenu -->

            </div>

        </section>

    </div>

</main>
```

---

# 🧩 Version finale du JavaScript

```javascript
document.addEventListener("DOMContentLoaded", () => {

    // ========================================================
    // DONNEES DJANGO
    // ========================================================

    const actifs = JSON.parse(
        document.getElementById("equipement-actifs").textContent
    );

    const inactifs = JSON.parse(
        document.getElementById("equipement-inactifs").textContent
    );

    const cpuData = JSON.parse(
        document.getElementById("cpu-data").textContent
    );


    // ========================================================
    // GRAPHIQUE STATUT
    // ========================================================

    const statutCanvas = document.getElementById("statutChart");

    if (statutCanvas) {

        new Chart(statutCanvas, {

            type: "doughnut",

            data: {

                labels: [
                    "Actifs",
                    "Inactifs"
                ],

                datasets: [
                    {
                        data: [
                            actifs,
                            inactifs
                        ],

                        borderWidth: 0
                    }
                ]

            },

            options: {

                responsive: true,

                maintainAspectRatio: false,

                cutout: "72%",

                plugins: {

                    legend: {

                        position: "bottom",

                        labels: {
                            padding: 20,
                            usePointStyle: true
                        }

                    }

                }

            }

        });

    }


    // ========================================================
    // GRAPHIQUE CPU
    // ========================================================

    const cpuCanvas = document.getElementById("cpuChart");

    if (cpuCanvas) {

        const cpuLabels = cpuData.map(
            item => item.name
        );

        const cpuValues = cpuData.map(
            item => item.cpu
        );


        new Chart(cpuCanvas, {

            type: "bar",

            data: {

                labels: cpuLabels,

                datasets: [

                    {
                        label: "CPU (%)",

                        data: cpuValues,

                        borderRadius: 8,

                        borderSkipped: false

                    }

                ]

            },

            options: {

                responsive: true,

                maintainAspectRatio: false,

                interaction: {

                    mode: "index",

                    intersect: false

                },

                scales: {

                    y: {

                        beginAtZero: true,

                        max: 100,

                        ticks: {

                            callback: value => `${value}%`

                        }

                    }

                },

                plugins: {

                    legend: {
                        display: false
                    }

                }

            }

        });

    }

});
```

---

# 📌 Bonnes pratiques Chart.js

### Toujours

```javascript
responsive: true
```

### Pour contrôler la hauteur avec Tailwind

```javascript
maintainAspectRatio: false
```

avec :

```html
<div class="relative h-[320px] w-full">
    <canvas></canvas>
</div>
```

### Pour les graphiques longs

Avec 100 équipements, un graphique contenant 100 barres devient vite difficile à lire.

Préférer :

- Top 10 CPU
- Top 10 mémoire
- équipements en alerte
- moyenne CPU
- moyenne mémoire
- évolution dans le temps

---

# 💡 Évolution du projet

Une fois ce dashboard fonctionnel, tu peux ajouter :

```text
┌─────────────────────────────────────────┐
│              DASHBOARD                  │
├─────────────────────────────────────────┤
│ KPI                                     │
├─────────────────────┬───────────────────┤
│ Disponibilité       │ Alertes           │
│ Doughnut             │ Liste             │
├─────────────────────┴───────────────────┤
│                                         │
│ Performance historique                  │
│ Line Chart                              │
│                                         │
├─────────────────────────────────────────┤
│ Top équipements problématiques          │
├─────────────────────────────────────────┤
│ Tableau des équipements                 │
└─────────────────────────────────────────┘
```

### Étape suivante recommandée

Pour ton projet de supervision réseau, le meilleur ajout après les graphiques statiques serait un **graphique historique** :

```text
CPU (%)
100 ┤
 80 ┤        ╭──╮
 60 ┤    ╭───╯  ╰──╮
 40 ┤───╯          ╰───
 20 ┤
  0 └────────────────────────
      10h  11h  12h  13h  14h
```

Cela nécessite de conserver les mesures CPU/RAM/température dans une table d'historique plutôt que de ne garder que la dernière valeur dans `Equipements`.

---

## ✅ Résumé

Pour ton dashboard :

**Django**

```text
Models → Views → Template
```

**Tailwind CSS**

```text
Layout → Cards → Responsive UI
```

**Chart.js**

```text
Données Django → JSON → Chart.js
```

**Architecture finale**

```text
                  Django
                    │
                    ▼
              ┌───────────┐
              │  views.py │
              └─────┬─────┘
                    │
                    │ JSON
                    ▼
              ┌─────────────┐
              │ dashboard   │
              │   .html     │
              └──────┬──────┘
                     │
          ┌──────────┴──────────┐
          ▼                     ▼
     Tailwind CSS           Chart.js
          │                     │
          ▼                     ▼
       UI/UX                Graphiques
```

> 🎯 **Objectif final :** un dashboard qui donne l'état du réseau en quelques secondes, puis permet d'aller vers le détail lorsqu'un problème est détecté.
