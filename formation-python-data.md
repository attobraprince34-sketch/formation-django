<div align="center">

# 🐍 Formation Complète : Python pour l'Analyse de Données

### De zéro à autonome : coder, comprendre, construire, automatiser avec l'IA

![Python](https://img.shields.io/badge/Python-3776AB?style=for-the-badge&logo=python&logoColor=white)
![Pandas](https://img.shields.io/badge/Pandas-150458?style=for-the-badge&logo=pandas&logoColor=white)
![NumPy](https://img.shields.io/badge/NumPy-013243?style=for-the-badge&logo=numpy&logoColor=white)
![Jupyter](https://img.shields.io/badge/Jupyter-F37626?style=for-the-badge&logo=jupyter&logoColor=white)
![SQL](https://img.shields.io/badge/SQL-4479A1?style=for-the-badge&logo=postgresql&logoColor=white)
![Streamlit](https://img.shields.io/badge/Streamlit-FF4B4B?style=for-the-badge&logo=streamlit&logoColor=white)
![Git](https://img.shields.io/badge/Git-F05032?style=for-the-badge&logo=git&logoColor=white)

*Niveau : Grand débutant → Autonome sur projets réels*

</div>

---

## 📋 Table des matières

- [Comment utiliser cette formation](#-comment-utiliser-cette-formation)
- [Module 0 — Installer son environnement](#module-0--installer-son-environnement)
- [Module 1 — Fondamentaux de Python](#module-1--fondamentaux-de-python)
- [Module 2 — Travailler comme un professionnel](#module-2--travailler-comme-un-professionnel-venv-pip-git)
- [Module 3 — NumPy, le calcul numérique](#module-3--numpy-le-calcul-numérique)
- [Module 4 — Pandas, le cœur de l'analyse de données](#module-4--pandas-le-cœur-de-lanalyse-de-données)
- [Module 5 — Visualisation de données](#module-5--visualisation-de-données)
- [Module 6 — Statistiques descriptives appliquées](#module-6--statistiques-descriptives-appliquées)
- [Module 7 — Automatiser des rapports Excel](#module-7--automatiser-des-rapports-excel)
- [Module 8 — SQL avec Python](#module-8--sql-avec-python)
- [Module 9 — Dashboards interactifs avec Streamlit](#module-9--dashboards-interactifs-avec-streamlit)
- [Module 10 — Intégrer des agents IA à ses analyses](#module-10--intégrer-des-agents-ia-à-ses-analyses)
- [🚀 Projets pour s'entraîner](#-projets-pour-sentraîner)
- [📚 Pour aller plus loin](#-pour-aller-plus-loin)

---

## 🎯 Comment utiliser cette formation

Cette formation est construite comme un **enseignant** la donnerait en présentiel : chaque notion est expliquée avant d'être utilisée, jamais l'inverse. Tu ne copieras jamais un bout de code sans savoir pourquoi il fonctionne.

**Règles pour bien apprendre :**

1. **Ne saute aucun module.** Chaque module s'appuie sur le précédent.
2. **Tape le code toi-même.** Ne copie-colle jamais. Tes doigts doivent apprendre autant que ta tête.
3. **Casse volontairement ton code.** Change une valeur, supprime une ligne, regarde l'erreur, comprends-la. Les erreurs sont des professeurs, pas des ennemis.
4. **Fais les exercices avant de lire la correction.** Même si tu bloques 20 minutes.
5. **À la fin de chaque module, explique la notion à voix haute** comme si tu l'enseignais à quelqu'un. Si tu bafouilles, tu ne maîtrises pas encore.

**Outil recommandé :** installe [Jupyter Notebook](https://jupyter.org/) ou utilise [Google Colab](https://colab.research.google.com/) (gratuit, sans installation) pour exécuter le code au fur et à mesure de ta lecture.

---

## Module 0 — Installer son environnement

Avant d'écrire une seule ligne de Python, il faut un endroit où l'exécuter.

### Option A — Anaconda (recommandé pour la data)

Anaconda installe Python **et** toutes les bibliothèques de data (Pandas, NumPy, Matplotlib...) d'un coup, avec Jupyter Notebook inclus.

👉 Télécharge-le sur [anaconda.com/download](https://www.anaconda.com/download)

### Option B — Google Colab (zéro installation)

Si tu veux commencer immédiatement sans rien installer : [colab.research.google.com](https://colab.research.google.com/). C'est un notebook Jupyter dans ton navigateur, gratuit, avec Python déjà configuré.

### Vérifier que ça fonctionne

Ouvre un notebook et tape :

```python
print("Mon environnement fonctionne !")
```

Si ça affiche le texte, tu es prêt. Sinon, réinstalle Anaconda en cochant l'option "Add to PATH" pendant l'installation (Windows).

> 💡 **Pourquoi Jupyter et pas un simple fichier `.py` ?** En data, tu explores : tu regardes un bout de données, tu ajustes, tu regardes encore. Jupyter exécute le code **cellule par cellule**, donc tu vois le résultat immédiatement sans relancer tout le programme. C'est l'outil standard du métier.

---

## Module 1 — Fondamentaux de Python

### 1.1 Variables et types

Une variable est une boîte étiquetée qui contient une valeur.

```python
age = 25              # int (nombre entier)
prix = 19.99           # float (nombre décimal)
nom = "Alice"          # str (chaîne de caractères)
est_actif = True       # bool (Vrai/Faux)
```

**Pourquoi c'est important :** en data, une colonne entière de ton tableau aura un seul type. Si Python pense qu'une colonne de nombres est du texte, tu ne pourras pas faire de calcul dessus — c'est l'erreur n°1 des débutants en analyse de données.

Vérifier le type d'une variable :
```python
print(type(prix))   # <class 'float'>
```

### 1.2 Opérateurs

```python
# Arithmétiques
10 + 3   # 13
10 - 3   # 7
10 * 3   # 30
10 / 3   # 3.333... (division qui garde les décimales)
10 // 3  # 3 (division entière)
10 % 3   # 1 (le reste de la division)
10 ** 2  # 100 (puissance)

# Comparaison (renvoient True ou False)
5 > 3    # True
5 == 5   # True (== compare, = affecte)
5 != 3   # True

# Logiques
True and False  # False
True or False   # True
not True        # False
```

⚠️ **Piège classique :** `=` sert à donner une valeur, `==` sert à comparer. Confondre les deux est l'erreur n°1 des débutants en Python.

### 1.3 Structures de données natives

C'est ici que ça devient crucial pour la data — tu manipuleras ces structures **en permanence**.

#### Les listes `[]`
Une collection ordonnée et modifiable.

```python
ventes = [120, 340, 89, 400]
print(ventes[0])        # 120 (le premier élément, l'index commence à 0 !)
print(ventes[-1])       # 400 (le dernier élément)
ventes.append(250)      # ajoute 250 à la fin
print(len(ventes))      # 5, la longueur de la liste
print(ventes[1:3])      # [340, 89] : slicing, du 2e (inclus) au 4e (exclu)
```

#### Les dictionnaires `{}`
Une collection de paires clé-valeur. **C'est la structure la plus proche de ce que tu manipuleras en data** (une ligne de données = souvent un dictionnaire).

```python
client = {
    "nom": "Alice",
    "age": 28,
    "ville": "Paris"
}
print(client["nom"])       # Alice
client["age"] = 29         # on modifie une valeur
client["email"] = "a@x.com"  # on ajoute une nouvelle clé
```

#### Les tuples `()`
Comme une liste, mais **non modifiable** une fois créée. Utile pour des données qui ne doivent jamais changer (ex : coordonnées GPS).

```python
coordonnees = (48.8566, 2.3522)
```

### 1.4 Structures de contrôle

#### Les conditions
```python
note = 14
if note >= 16:
    print("Excellent")
elif note >= 10:
    print("Passable")
else:
    print("Insuffisant")
```

#### Les boucles
```python
# Boucle for : répète pour chaque élément
ventes = [120, 340, 89, 400]
for vente in ventes:
    print(vente * 1.2)  # applique une TVA à chaque vente

# Boucle while : répète tant qu'une condition est vraie
compteur = 0
while compteur < 5:
    print(compteur)
    compteur += 1
```

> 💡 **En pratique :** avec Pandas (module 4), tu écriras très rarement des boucles `for` sur tes données — on utilise des méthodes optimisées à la place. Mais il faut comprendre la logique des boucles pour comprendre *pourquoi* Pandas est plus rapide.

### 1.5 Fonctions

Une fonction est un bloc de code réutilisable.

```python
def calculer_tva(prix_ht, taux=0.20):
    """Calcule le prix TTC à partir du prix HT."""
    prix_ttc = prix_ht * (1 + taux)
    return prix_ttc

resultat = calculer_tva(100)         # 120.0
resultat2 = calculer_tva(100, 0.10)  # 110.0
```

- `prix_ht` et `taux` sont les **paramètres** (`taux` a une valeur par défaut : 0.20)
- `return` renvoie le résultat pour qu'on puisse l'utiliser ailleurs
- Le texte entre `"""..."""` est une **docstring** : elle documente ce que fait la fonction (bonne pratique professionnelle)

### 1.6 Manipulation de chaînes de caractères

Indispensable pour nettoyer des données texte (noms, adresses, catégories mal saisies...).

```python
texte = "  Bonjour Le Monde  "

texte.strip()          # "Bonjour Le Monde" (retire les espaces autour)
texte.lower()           # "  bonjour le monde  " (minuscules)
texte.upper()           # "  BONJOUR LE MONDE  " (majuscules)
texte.replace("Le", "Notre")  # remplace un mot
texte.split(" ")        # découpe en liste de mots

# f-strings : insérer des variables dans du texte (très utilisé)
nom = "Alice"
age = 28
print(f"{nom} a {age} ans")   # "Alice a 28 ans"
```

### 1.7 Lire et écrire des fichiers

```python
# Écrire dans un fichier
with open("notes.txt", "w") as fichier:
    fichier.write("Première ligne\n")
    fichier.write("Deuxième ligne")

# Lire un fichier
with open("notes.txt", "r") as fichier:
    contenu = fichier.read()
    print(contenu)
```

`with` garantit que le fichier se ferme correctement même en cas d'erreur — toujours l'utiliser.

### 1.8 Gérer les erreurs

```python
try:
    resultat = 10 / 0
except ZeroDivisionError:
    print("Impossible de diviser par zéro !")
except Exception as e:
    print(f"Une erreur est survenue : {e}")
```

`try/except` permet à ton programme de ne pas planter quand une erreur prévisible peut arriver (fichier manquant, donnée invalide...).

### ✅ Exercices Module 1

1. Crée un dictionnaire représentant un produit (nom, prix, quantité en stock). Affiche une phrase du type `"Le produit X coûte Y€, il en reste Z en stock."` avec une f-string.
2. Crée une liste de 5 notes. Écris une boucle qui affiche "Réussite" si la note est ≥ 10, sinon "Échec".
3. Écris une fonction `moyenne(liste_notes)` qui calcule et renvoie la moyenne d'une liste de nombres, sans utiliser de bibliothèque externe.
4. Écris un programme qui demande à l'utilisateur un nombre (`input()`) et affiche s'il est pair ou impair, en gérant le cas où l'utilisateur tape du texte au lieu d'un nombre (`try/except`).

---

## Module 2 — Travailler comme un professionnel (venv, pip, Git)

### 2.1 Les environnements virtuels

Un environnement virtuel isole les bibliothèques d'un projet pour éviter les conflits entre projets.

```bash
# Créer un environnement
python -m venv mon_projet_env

# L'activer (Windows)
mon_projet_env\Scripts\activate

# L'activer (Mac/Linux)
source mon_projet_env/bin/activate
```

> 💡 **Pourquoi c'est important :** imagine que le Projet A a besoin de Pandas version 1.0 et le Projet B de la version 2.0. Sans environnement virtuel, ils se marchent dessus. Avec, chaque projet a sa propre boîte à outils isolée.

### 2.2 Installer des bibliothèques avec pip

```bash
pip install pandas numpy matplotlib seaborn openpyxl
```

Une fois ton environnement configuré, tu ne réinstalles jamais Python en entier — tu ajoutes juste les bibliothèques dont tu as besoin.

### 2.3 Git et GitHub — les bases essentielles

Git garde un historique de toutes les versions de ton code. GitHub héberge ce code en ligne.

```bash
git init                        # initialise un dépôt Git dans ton dossier
git add .                       # prépare tous les fichiers modifiés
git commit -m "Premier import"  # enregistre une version avec un message
git push                        # envoie sur GitHub
```

Tu n'as pas besoin de tout maîtriser maintenant, mais retiens ces 4 commandes : elles couvrent 90% de ton usage quotidien.

---

## Module 3 — NumPy, le calcul numérique

NumPy (*Numerical Python*) est la bibliothèque sur laquelle **Pandas est construit**. Tu ne l'utiliseras pas autant que Pandas au quotidien, mais comprendre ses bases t'aide à comprendre pourquoi Pandas est rapide.

```python
import numpy as np
```

### 3.1 Pourquoi pas juste des listes Python ?

```python
# Avec une liste Python : lent, syntaxe lourde
prix = [10, 20, 30]
prix_remise = [p * 0.9 for p in prix]

# Avec NumPy : rapide, syntaxe naturelle
prix = np.array([10, 20, 30])
prix_remise = prix * 0.9   # opération appliquée à TOUS les éléments d'un coup
```

C'est ce qu'on appelle une **opération vectorisée** : au lieu de traiter les éléments un par un (boucle), NumPy les traite tous en même temps, en exploitant du code optimisé en C sous le capot. Sur 10 éléments la différence est invisible ; sur 10 millions de lignes, elle est énorme.

### 3.2 Créer et manipuler des tableaux

```python
a = np.array([1, 2, 3, 4, 5])

a.shape          # (5,) → la forme du tableau
a.mean()          # 3.0 → moyenne
a.sum()           # 15 → somme
a.max(), a.min()  # 5, 1
a[1:3]            # array([2, 3]) → slicing comme les listes

# Tableau à 2 dimensions (comme une mini feuille Excel)
matrice = np.array([[1, 2, 3], [4, 5, 6]])
matrice.shape     # (2, 3) → 2 lignes, 3 colonnes
```

### ✅ Exercices Module 3

1. Crée un tableau NumPy de 10 notes. Calcule la moyenne, le max, le min et l'écart-type (`.std()`).
2. Crée deux tableaux de même taille et additionne-les directement (`a + b`).
3. Filtre un tableau pour ne garder que les valeurs supérieures à 50 : `a[a > 50]`.

---

## Module 4 — Pandas, le cœur de l'analyse de données

C'est **le module le plus important de toute la formation**. Pandas te permet de manipuler des données tabulaires (comme un fichier Excel) directement en Python.

```python
import pandas as pd
```

### 4.1 Les deux structures fondamentales

- **`Series`** : une seule colonne de données (comme une colonne Excel)
- **`DataFrame`** : un tableau complet, plusieurs colonnes (comme une feuille Excel entière)

```python
# Une Series
notes = pd.Series([12, 15, 9, 18], name="Notes")

# Un DataFrame : un dictionnaire de listes devient un tableau
donnees = {
    "nom": ["Alice", "Bob", "Chloé"],
    "age": [28, 34, 22],
    "ville": ["Paris", "Lyon", "Marseille"]
}
df = pd.DataFrame(donnees)
print(df)
```

```
     nom  age      ville
0  Alice   28      Paris
1    Bob   34       Lyon
2  Chloé   22  Marseille
```

Chaque ligne a un **index** (0, 1, 2...) automatiquement, chaque colonne a un **nom**.

### 4.2 Importer et exporter des données

```python
df = pd.read_csv("ventes.csv")            # importer un CSV
df = pd.read_excel("ventes.xlsx")          # importer un Excel
df = pd.read_json("donnees.json")          # importer du JSON

df.to_csv("resultat.csv", index=False)     # exporter en CSV
df.to_excel("resultat.xlsx", index=False)  # exporter en Excel
```

`index=False` évite d'ajouter une colonne inutile avec les numéros de ligne au fichier exporté.

### 4.3 Explorer un jeu de données (le réflexe n°1)

**Avant toute analyse**, toujours commencer par comprendre ses données :

```python
df.head()        # affiche les 5 premières lignes
df.tail(3)        # affiche les 3 dernières lignes
df.shape          # (nombre de lignes, nombre de colonnes)
df.info()         # types de chaque colonne + valeurs manquantes
df.describe()     # statistiques (moyenne, min, max...) des colonnes numériques
df.columns        # liste des noms de colonnes
df.dtypes         # type de chaque colonne
```

> 💡 **Habitude de pro :** ne jamais analyser un jeu de données sans avoir lancé `df.info()` et `df.describe()` en premier. C'est ce qui te révèle les valeurs manquantes, les types incorrects, ou les valeurs aberrantes avant qu'elles ne faussent ton analyse.

### 4.4 Sélectionner et filtrer

```python
df["age"]                      # sélectionne une colonne (renvoie une Series)
df[["nom", "age"]]              # sélectionne plusieurs colonnes (renvoie un DataFrame)

df.loc[0]                       # sélectionne la ligne d'index 0
df.iloc[0]                       # sélectionne la 1ère ligne par position

# Filtrer selon une condition (le plus utilisé au quotidien)
df[df["age"] > 25]              # uniquement les lignes où age > 25
df[(df["age"] > 25) & (df["ville"] == "Paris")]  # plusieurs conditions (& = et, | = ou)
```

⚠️ **Piège classique :** en Python normal on utilise `and`/`or`, mais avec Pandas sur des conditions de colonnes, il faut utiliser `&`/`|` et bien mettre des parenthèses autour de chaque condition.

### 4.5 Nettoyer les données

C'est souvent **80% du travail réel** en analyse de données.

```python
df.isnull().sum()                    # compte les valeurs manquantes par colonne
df.dropna()                          # supprime les lignes avec des valeurs manquantes
df.fillna(0)                         # remplace les valeurs manquantes par 0
df["age"].fillna(df["age"].mean())   # remplace par la moyenne de la colonne

df.duplicated().sum()                # compte les doublons
df.drop_duplicates()                 # supprime les doublons

df["age"] = df["age"].astype(int)    # convertit le type d'une colonne
```

### 4.6 Créer et transformer des colonnes

```python
df["age_dans_10_ans"] = df["age"] + 10               # nouvelle colonne calculée
df["categorie_age"] = df["age"].apply(
    lambda x: "Jeune" if x < 30 else "Senior"
)  # applique une fonction personnalisée à chaque valeur
```

`apply()` avec une fonction `lambda` (fonction rapide sans nom) est un outil que tu utiliseras très souvent pour créer des colonnes basées sur des règles métier.

### 4.7 Agréger : groupby et pivot_table

C'est **la notion la plus puissante de Pandas** pour du reporting business — l'équivalent des tableaux croisés dynamiques d'Excel.

```python
# "Pour chaque ville, quelle est la moyenne d'âge ?"
df.groupby("ville")["age"].mean()

# Plusieurs statistiques d'un coup
df.groupby("ville")["age"].agg(["mean", "min", "max", "count"])

# Tableau croisé dynamique
pd.pivot_table(df, values="age", index="ville", columns="categorie_age", aggfunc="mean")
```

### 4.8 Fusionner des données

```python
# Fusionner deux tableaux sur une colonne commune (comme RECHERCHEV/VLOOKUP en Excel)
clients = pd.DataFrame({"id_client": [1, 2, 3], "nom": ["Alice", "Bob", "Chloé"]})
commandes = pd.DataFrame({"id_client": [1, 1, 2], "montant": [50, 30, 100]})

fusion = pd.merge(clients, commandes, on="id_client", how="left")

# Empiler deux tableaux de même structure
pd.concat([df1, df2])
```

### 4.9 Gérer les dates

```python
df["date"] = pd.to_datetime(df["date"])   # convertit du texte en vraie date
df["annee"] = df["date"].dt.year          # extrait l'année
df["mois"] = df["date"].dt.month          # extrait le mois
df["jour_semaine"] = df["date"].dt.day_name()  # extrait le jour de la semaine
```

### ✅ Exercices Module 4

1. Charge le [dataset Titanic](https://www.kaggle.com/c/titanic/data) (très utilisé pour débuter) avec `pd.read_csv()`. Affiche `.info()` et `.describe()`.
2. Trouve combien de valeurs manquantes il y a par colonne.
3. Calcule le taux de survie moyen par classe de billet (`groupby`).
4. Crée une colonne `"tranche_age"` qui classe les passagers en "Enfant" (<18), "Adulte" (18-60), "Senior" (>60).
5. Exporte le résultat nettoyé dans un nouveau fichier CSV.

---

## Module 5 — Visualisation de données

Une donnée bien visualisée raconte une histoire qu'un tableau de chiffres ne raconte pas.

```python
import matplotlib.pyplot as plt
import seaborn as sns
```

### 5.1 Matplotlib — les bases

```python
plt.plot(df["date"], df["ventes"])      # courbe (évolution dans le temps)
plt.bar(df["categorie"], df["ventes"])  # barres (comparaison entre catégories)
plt.hist(df["age"], bins=20)            # histogramme (distribution d'une variable)
plt.scatter(df["prix"], df["ventes"])   # nuage de points (relation entre 2 variables)

plt.title("Évolution des ventes")
plt.xlabel("Date")
plt.ylabel("Ventes (€)")
plt.show()
```

### 5.2 Seaborn — des graphiques statistiques plus élégants

```python
sns.barplot(data=df, x="categorie", y="ventes")
sns.boxplot(data=df, x="categorie", y="ventes")     # détecte les valeurs aberrantes
sns.heatmap(df.corr(), annot=True)                  # matrice de corrélation
sns.histplot(data=df, x="age", kde=True)             # distribution + courbe de densité
```

### 5.3 Choisir le bon graphique (réflexe de reporting)

| Ce que tu veux montrer | Graphique à utiliser |
|---|---|
| Évolution dans le temps | Courbe (`plot`) |
| Comparer des catégories | Barres (`bar`) |
| Répartition/distribution | Histogramme (`hist`) |
| Relation entre 2 variables numériques | Nuage de points (`scatter`) |
| Répartition en parts | Camembert (à utiliser avec parcimonie) |
| Détecter des valeurs aberrantes | Boîte à moustaches (`boxplot`) |

### ✅ Exercices Module 5

1. Sur le dataset Titanic, trace un histogramme de l'âge des passagers.
2. Trace un barplot du taux de survie par classe de billet.
3. Trace une heatmap de corrélation entre les variables numériques.

---

## Module 6 — Statistiques descriptives appliquées

Pas besoin d'être mathématicien, mais ces notions te permettent d'interpréter correctement ce que tu vois.

- **Moyenne** : la valeur "typique", très sensible aux valeurs extrêmes
- **Médiane** : la valeur du milieu, insensible aux valeurs extrêmes → souvent plus fiable qu'une moyenne pour des salaires ou des prix
- **Écart-type** : mesure la dispersion des données autour de la moyenne
- **Quartiles** : découpent les données en 4 groupes égaux (utile pour repérer les 25% les plus hauts/bas)
- **Corrélation** (entre -1 et 1) : mesure si deux variables évoluent ensemble. ⚠️ **Corrélation n'est pas causalité** — deux variables corrélées ne veulent pas dire que l'une cause l'autre.

```python
df["age"].mean()
df["age"].median()
df["age"].std()
df["age"].quantile([0.25, 0.5, 0.75])
df[["age", "prix"]].corr()
```

**Détecter des valeurs aberrantes (outliers)** avec la méthode de l'écart interquartile :

```python
Q1 = df["prix"].quantile(0.25)
Q3 = df["prix"].quantile(0.75)
IQR = Q3 - Q1
outliers = df[(df["prix"] < Q1 - 1.5 * IQR) | (df["prix"] > Q3 + 1.5 * IQR)]
```

---

## Module 7 — Automatiser des rapports Excel

C'est là que Python devient un outil de productivité redoutable pour le business : ce qui prenait 2 heures chaque lundi devient un script qui tourne en 10 secondes.

```python
# Lire plusieurs feuilles d'un classeur Excel
feuilles = pd.read_excel("rapport.xlsx", sheet_name=None)  # dict de DataFrames

# Écrire plusieurs feuilles dans un même fichier
with pd.ExcelWriter("rapport_final.xlsx") as writer:
    df_ventes.to_excel(writer, sheet_name="Ventes", index=False)
    df_clients.to_excel(writer, sheet_name="Clients", index=False)
```

**Exemple de script d'automatisation complet :**

```python
import pandas as pd
from datetime import date

# 1. Charger les données brutes
df = pd.read_csv("ventes_brutes.csv")

# 2. Nettoyer
df = df.dropna(subset=["montant"])
df["date"] = pd.to_datetime(df["date"])

# 3. Agréger
resume = df.groupby("categorie")["montant"].agg(["sum", "mean", "count"])

# 4. Exporter avec la date du jour dans le nom de fichier
nom_fichier = f"rapport_{date.today()}.xlsx"
resume.to_excel(nom_fichier)

print(f"Rapport généré : {nom_fichier}")
```

Ce script, une fois écrit, peut tourner automatiquement chaque semaine (via une tâche planifiée) sans intervention humaine.

---

## Module 8 — SQL avec Python

Les données d'entreprise vivent rarement dans des CSV — elles vivent dans des bases de données. Savoir interroger une base en SQL depuis Python est une compétence très demandée.

```python
import sqlite3
import pandas as pd

# Se connecter à une base de données
connexion = sqlite3.connect("ma_base.db")

# Exécuter une requête SQL et récupérer directement un DataFrame
df = pd.read_sql_query("SELECT * FROM ventes WHERE montant > 100", connexion)

# Envoyer un DataFrame Pandas vers une base de données
df.to_sql("nouvelle_table", connexion, if_exists="replace", index=False)

connexion.close()
```

**Les 4 commandes SQL à connaître en priorité :**

```sql
SELECT nom, montant FROM ventes WHERE montant > 100 ORDER BY montant DESC;
SELECT categorie, SUM(montant) FROM ventes GROUP BY categorie;
SELECT * FROM ventes JOIN clients ON ventes.id_client = clients.id;
```

---

## Module 9 — Dashboards interactifs avec Streamlit

Streamlit transforme un script Python en application web interactive, sans connaissances en développement web.

```python
# app.py
import streamlit as st
import pandas as pd

st.title("📊 Dashboard des ventes")

df = pd.read_csv("ventes.csv")

ville_choisie = st.selectbox("Choisir une ville", df["ville"].unique())
df_filtre = df[df["ville"] == ville_choisie]

st.metric("Total des ventes", f"{df_filtre['montant'].sum()} €")
st.bar_chart(df_filtre.groupby("categorie")["montant"].sum())
st.dataframe(df_filtre)
```

Pour lancer : `streamlit run app.py`. En quelques lignes, tu obtiens un vrai outil web que tes collègues peuvent utiliser sans savoir coder.

---

## Module 10 — Intégrer des agents IA à ses analyses

C'est la compétence qui distingue un analyste "classique" d'un analyste augmenté par l'IA en 2026 : connecter un modèle de langage à ses propres données pour automatiser des tâches de raisonnement.

### 10.1 Le principe

Un **agent IA** est un programme qui utilise un modèle de langage (comme Claude) non pas juste pour discuter, mais pour **exécuter des actions** : lire un fichier, écrire une requête SQL, décider quelle analyse faire ensuite, ou résumer un résultat en langage naturel.

### 10.2 Appeler un modèle depuis Python

```python
import anthropic

client = anthropic.Anthropic(api_key="TA_CLE_API")

message = client.messages.create(
    model="claude-sonnet-4-6",
    max_tokens=500,
    messages=[
        {"role": "user", "content": "Résume ces chiffres de ventes en 3 phrases : ..."}
    ]
)
print(message.content[0].text)
```

### 10.3 Un agent simple : résumer automatiquement un rapport Pandas

```python
import anthropic
import pandas as pd

client = anthropic.Anthropic(api_key="TA_CLE_API")
df = pd.read_csv("ventes.csv")

resume_stats = df.describe().to_string()

reponse = client.messages.create(
    model="claude-sonnet-4-6",
    max_tokens=400,
    messages=[{
        "role": "user",
        "content": f"Voici des statistiques de ventes :\n{resume_stats}\n"
                   f"Rédige un résumé exécutif de 4 lignes destiné à un directeur commercial."
    }]
)
print(reponse.content[0].text)
```

**Ce que tu viens de construire :** un script qui analyse tes données ET rédige le commentaire business automatiquement. C'est l'automatisation du dernier kilomètre entre "avoir des chiffres" et "prendre une décision".

### 10.4 Le concept de "function calling" (les agents avancés)

Les modèles récents peuvent **décider eux-mêmes** d'appeler une fonction Python que tu as définie (ex : "va chercher les ventes de mars" → le modèle génère l'appel de fonction correspondant, tu l'exécutes, tu lui renvoies le résultat). C'est le mécanisme derrière tous les "agents data" modernes : le modèle raisonne, ton code exécute.

> 💡 Pour aller plus loin sur ce sujet, explore la documentation officielle sur le *tool use* / *function calling* de l'API Claude, et des frameworks comme LangChain qui structurent ces agents.

### ✅ Exercices Module 10

1. Écris un script qui charge un CSV, calcule des statistiques, et demande à un modèle de rédiger 3 recommandations business basées dessus.
2. Améliore ton dashboard Streamlit (Module 9) en ajoutant un bouton "Générer un résumé IA" qui envoie les données filtrées au modèle et affiche sa réponse.

---

## 🚀 Projets pour s'entraîner

Fais-les **dans l'ordre**. Chaque projet réutilise les modules précédents et en ajoute un nouveau. Ne cherche pas la solution en ligne avant d'avoir bloqué au moins 30 minutes.

### Projet 1 — Analyseur de notes de classe (Modules 1)
Un fichier texte contient des notes séparées par des virgules. Écris un script qui calcule la moyenne, la note max, la note min, et affiche "Classe en réussite" si la moyenne dépasse 10.

### Projet 2 — Le Titanic (Modules 3-4-5)
Utilise le dataset [Titanic de Kaggle](https://www.kaggle.com/c/titanic/data). Nettoie les données, calcule le taux de survie par sexe et par classe, visualise tes résultats avec 3 graphiques différents.

### Projet 3 — Dashboard de ventes personnelles (Modules 4-5-6)
Crée toi-même un faux fichier CSV de 200 lignes de ventes (produit, date, montant, région). Calcule l'évolution mensuelle, la région la plus performante, détecte les valeurs aberrantes, et présente 3 visualisations claires.

### Projet 4 — Rapport hebdomadaire automatisé (Module 7)
Écris un script qui prend un CSV de ventes brutes en entrée et génère automatiquement un fichier Excel avec plusieurs feuilles (résumé, détail par catégorie, top 10 clients), nommé avec la date du jour.

### Projet 5 — Mini base de données clients (Module 8)
Crée une base SQLite avec 2 tables (clients, commandes). Écris des requêtes SQL pour répondre à : qui sont les 5 meilleurs clients ? Quel est le panier moyen par mois ?

### Projet 6 — Dashboard interactif public (Module 9)
Transforme le Projet 3 en application Streamlit avec des filtres interactifs (par région, par période) que n'importe qui peut utiliser sans coder.

### Projet 7 — Assistant d'analyse augmenté par l'IA (Module 10)
Combine tout : un dashboard Streamlit qui charge des données réelles (les tiennes, ou un dataset public), calcule des statistiques, et propose un bouton qui envoie ces statistiques à un modèle IA pour générer un commentaire exécutif en français, prêt à copier dans un email à un manager.

### Projet 8 — Projet libre (portfolio)
Trouve un dataset qui t'intéresse vraiment (sport, finance personnelle, musique, météo...) sur [Kaggle](https://www.kaggle.com/datasets) ou [data.gouv.fr](https://www.data.gouv.fr/), et mène une analyse complète de bout en bout : nettoyage → exploration → visualisation → rapport automatisé → dashboard. C'est ce projet que tu montres en entretien.

---

## 📚 Pour aller plus loin

- **Datasets pour t'entraîner :** [Kaggle Datasets](https://www.kaggle.com/datasets), [data.gouv.fr](https://www.data.gouv.fr/)
- **Documentation officielle :** [pandas.pydata.org](https://pandas.pydata.org/docs/), [numpy.org](https://numpy.org/doc/)
- **Pour approfondir le SQL :** [sqlzoo.net](https://sqlzoo.net/)
- **Pour approfondir les agents IA :** documentation officielle de l'API Anthropic sur le *tool use*

---

<div align="center">

**Tu as terminé la formation.** Tu sais maintenant coder, nettoyer, analyser, visualiser, automatiser et augmenter tes analyses avec l'IA.
La seule chose qui reste à faire : **pratiquer, encore et encore, sur de vraies données.**

</div>
